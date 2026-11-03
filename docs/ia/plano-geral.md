# Plano geral do Watch

## Objetivo do sistema

Conectar aplicações, serviços, computadores e celulares a uma pipeline comum de telemetria de segurança. Receber eventos, normalizar, correlacionar sinais, preservar evidências selecionadas e apresentar alertas explicáveis no painel e no Discord. SDKs e agentes próprios são os produtores principais; Log Drains de terceiros não são requisito.

A primeira entrega de cada módulo é utilizável isoladamente. Incrementos acrescentam funcionalidades e integração sem exigir que todo o sistema esteja pronto. O [backlog](backlog.md) organiza as entregas sem calendário, duração ou responsável fixo; testes unitários, de contrato e E2E acompanham os incrementos.

## Origem e consolidação dos planos

Este documento consolida os materiais locais `projeto_soc/Arquitetura do Open SOC.txt`, `projeto_soc/Detecção de Invasões em SOC.md`, `projeto_soc/message.txt` e o código do diagrama geral fornecido pelo time. Esses materiais registram a concepção anterior ao nome Watch. Os [diagramas incorporados](diagramas.md) apresentam a visão consolidada em código Mermaid, renderizado no GitHub.

A consolidação preserva os objetivos e as linhas de pesquisa, ajustando hipóteses que precisam de validação. Afirmações de latência zero, escala ilimitada, detecção infalível e gratuidade de qualquer recurso não são critérios técnicos do projeto. As decisões atuais estão nos [contratos](contratos.md), na [arquitetura](arquitetura.md) e no [mapa de cobertura](origens-e-cobertura.md).

## 1. Produtores: SDKs e apps próprios

A integração server-side deve começar com URL de ingestão e API key vinculada à source. A aplicação emite eventos explicitamente ou conecta o SDK ao seu logger/interceptor. O SDK valida e minimiza o envelope antes de enviar; timeout, retry limitado, lote e fila são capacidades incrementais, sem bloquear indefinidamente o serviço monitorado.

Configurar o transporte não cria visibilidade automática de tudo que acontece no provedor: o SDK observa os eventos emitidos pela aplicação e pelos adaptadores instalados. Logs brutos não são encaminhados indiscriminadamente; a configuração simples não dispensa schemas, allowlists e exclusão de segredos.

| Produtor | Integração pretendida | Incrementos do backlog |
| --- | --- | --- |
| Node/TypeScript | Transporte comum, eventos e adapter do logger | SK01 |
| Next.js e NestJS | Instrumentação no servidor, resultado de autenticação e acesso negado | SK02, SK03 |
| React/browser | Sinal consentido ao próprio backend autenticado | SK04 |
| Python e Go | Mesmo protocolo de ingestão e fixtures de conformidade | SK05, SK06 |
| Desktop | Tauri/Rust, credencial por instalação, sinais permitidos pelo SO | DT01–DT06, CO01–CO03 |
| Android e iOS | React Native, consentimento, postura limitada, heartbeat e leitura de alertas | MB01–MB06 |
| Serviços e contas | Adaptadores de operações instrumentadas e sinais oficialmente disponíveis | CV01, CS01–CS02, CU01–CU02, CP01, OC01, AC01–AC02 |

Browser usa o backend da aplicação para guardar o segredo; não recebe API key de máquina. Apps nativos possuem credencial de escrita individual, armazenada com mecanismo seguro da plataforma. Mobile respeita sandbox, permissões e execução oportunista: não observa todos os apps, SMS ou localização e não garante diagnóstico global de comprometimento.

## 2. Borda e ingestão

Cloudflare Worker é a opção de borda: recebe HTTPS, limita tamanho/taxa, verifica credencial e source, valida schema e deriva tenant do servidor. O mesmo contrato deve funcionar no laboratório local. Rejeitar eventos inválidos antes de encaminhar e registrar somente metadados operacionais permitidos.

O ingresso encaminha eventos elegíveis ao motor de análise e ao buffer curto. Não condicionar análise comportamental à presença em uma lista de IoCs: eventos sem indicador conhecido também podem compor uma sequência suspeita. Resposta de sucesso deve refletir persistência/aceite durável conforme o contrato; execução em background não comprova entrega.

## 3. Identidade e isolamento

Separar acesso humano e acesso de máquina. Analistas usam IdP, JWT validado e memberships/papéis conferidos no servidor. O backend autoriza cada objeto e canal pelo tenant. OAuth de login não concede acesso universal às sessões de contas externas.

Máquinas usam segredo aleatório forte; persistir apenas hash e metadados de source/escopo/status. Exibir segredo uma vez; oferecer revogação, rotação e pareamento de uso único. D1 é a opção autoritativa na borda; no laboratório, usar o adaptador local. KV pode distribuir artefatos não críticos, sem decidir revogação imediata.

Um vazamento de hashes não revela diretamente chaves aleatórias fortes, mas comprometimento do banco/servidor continua sendo incidente grave. Isolamento não é garantido só pelo IdP ou pela existência de um JWT: deve ser verificado em cada leitura, escrita e assinatura.

## 4. Motor de análise

Elixir concentra regras, estado temporal, correlação e explicação. Phoenix acrescenta APIs e Channels quando houver um caso funcional. Começar por funções determinísticas e janelas pequenas com relógio injetável; incluir limite de memória, expiração, replay e falhas de dependência.

O plano evolui de regra simples para limiares, sequência entre fontes autorizadas, risco explicável e catálogo versionado. Severidade do cliente, erro HTTP e heurística de integridade são contexto, não confirmação de invasão. Distinguir sinal, alerta e incidente confirmado na investigação.

Estado global exige atualização consistente: memória de um Worker não é compartilhada entre instâncias e KV com consistência eventual não resolve incrementos concorrentes de um contador. O laboratório começa com transações SQLite; um backend remoto precisa de decisão e teste próprios. Broadway/ETS entram somente se medição de carga justificar, com recuperação e durabilidade especificadas.

## 5. Retenção e evidências

Guardar o mínimo necessário para a janela de análise e investigação proposta. O laboratório começa com buffer de eventos minimizados até 24h, limitado por volume; preservar snapshots selecionados em alertas até 7 dias ou 10 MiB por tenant, com expurgo e indicação de evidência expirada. Esses valores são parâmetros iniciais, não prazos de execução.

O plano original de Axiom para pesquisa e R2/Parquet para arquivo longo permanece como alternativa futura. Não exigir os dois junto com Loki, nem usar retenção de anos como pré-requisito do caminho gratuito. Revisar plano, custos, privacidade, exclusão e restauração antes de adotar outro destino.

## 6. Visualização e notificações

React consome a API autorizada do Watch para lista, evidência e triagem. Phoenix Channels pode avisar mudanças; após reconexão, o cliente recupera estado pela API. O frontend não acessa D1 diretamente nem recebe chave privilegiada de Axiom/Grafana.

Discord recebe resumo saneado do alerta, sem evento bruto ou menções ativas. A outbox persiste tentativas e limita retry/cooldown; atraso, falha e possível duplicata devem ser observáveis. O app mobile pode consultar alertas com identidade humana separada da credencial do sensor.

Grafana/Loki são uma saída opcional para alertas e saúde da pipeline. O dashboard complementa o painel do Watch. Aplicar isolamento real de acesso; filtro visual não é autorização. Retenção e quotas externas são independentes do expurgo local.

## 7. Infraestrutura, entrega e operação

Terraform descreve recursos suportados, bindings, permissões e dashboards. Build, empacotamento, assinatura, instalação, consentimento e pareamento têm processos próprios. CI executa testes e verificações relevantes de código, segredos, dependências e IaC.

Começar localmente, sem recursos pagos. Deploy de borda/core requer gate de custos e topologia completa, incluindo estado, evidência e outbox. A VPS Oracle usada por outros projetos não é alvo. Não usar créditos temporários ou upgrade de plano para declarar o sistema gratuito.

## 8. Engenharia de detecção e evolução avançada

Estas linhas de pesquisa foram preservadas dos planos gerais. Não são capacidades implementadas nem dependências obrigatórias das 71 issues atuais. Transformar cada hipótese em uma issue pequena com fixtures, critério mensurável e limite operacional antes de desenvolver.

| Tema | Objetivo de estudo | Experimento mínimo e gate de adoção |
| --- | --- | --- |
| OCSF | Mapear eventos do Watch para classes e atributos oficiais | Escolher uma classe/versão e testar conversão; campos do texto original devem ser conferidos no schema oficial |
| Sigma / Detection-as-Code | Versionar e revisar conhecimento de detecção | Importar um subconjunto e rejeitar operador/campo não suportado; DE04 e DE08 iniciam esse caminho |
| STIX 2.1 | Representar indicadores, contexto e proveniência | Ler bundle de fixture e extrair um tipo de indicador; validar validade, revogação e origem |
| TAXII 2.1 | Transportar coleções de inteligência | Cliente contra servidor local com paginação, timeout e cursor persistido; feed real exige licença/cota e autorização |
| Bloom filter | Acelerar consulta de pertencimento a um catálogo de IoCs | Comparar conjunto exato e filtro com taxa de falsos positivos medida; confirmar hits no catálogo exato |
| Broadway / ETS | Processar concorrência com backpressure e estado | Medir pipeline local com carga sintética limitada; testar reinício, mensagens duplicadas e recuperação de estado |
| Baselines e anomalias | Estudar desvios além de limiares fixos | Dataset sintético benigno/malicioso, métrica de falsos positivos/negativos e estado unknown sem dados suficientes |
| Axiom/APL/monitores | Avaliar pesquisa e monitoramento como alternativa de backend | Confirmar plano, API/provider e campos; comparar com caminho local sem adicionar dependência gratuita obrigatória |
| R2/Parquet | Arquivo e exportação de evidências em outra escala | Medir lote/bytes/custos e restauração; não adotar retenção longa no escopo inicial |
| Atualização de inteligência | Manter regras e IoCs com segurança | Preparar mudança revisável, validar fixtures e artefato versionado, promover com rollback; sem apply automático de conteúdo não validado |

Um Bloom filter negativo só indica ausência no conjunto inserido, sob as condições corretas de construção e consulta; não indica tráfego benigno nem ausência de ameaça. Um positivo pode ser falso e não confirma comprometimento. Inteligência desatualizada e dados ausentes continuam produzindo lacunas de detecção.

STIX/TAXII definem formatos/transporte, sem garantir qualidade do feed ou acesso gratuito. Sigma precisa de mapeamento de campos, backend e semântica compatíveis; conversão automática para qualquer destino não é pressuposto. ETS é estado em memória, não armazenamento durável por si só.

## 9. Ideias preservadas fora do escopo inicial

Stripe/assinaturas, planos premium, arquivo de anos, vazamentos por CPF/email, monitoramento genérico de sessões OAuth, SMS/localização, e-mail/Telegram e agentes autônomos de investigação foram citados nos materiais de origem. Permanecem registrados como ideias, sem ativação, promessa de gratuidade ou autorização para coletar dados. Uma proposta futura precisa definir finalidade, permissões, privacidade, plano/custo e entregas independentes.

## Referências técnicas para aprofundar

- [OCSF e classes oficiais](https://schema.ocsf.io/)
- [Sigma: conceitos e conversão](https://sigmahq.io/docs/guide/getting-started.html)
- [STIX 2.1 — OASIS](https://docs.oasis-open.org/cti/stix/v2.1/os/stix-v2.1-os.html)
- [TAXII 2.1 — OASIS](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html)
- [Broadway](https://hexdocs.pm/broadway/Broadway.html)
- [ETS](https://www.erlang.org/doc/apps/stdlib/ets.html)
- [Bloom filters: desenho e aplicações](https://arxiv.org/abs/0908.3574)

A adoção de tecnologia depende de teste e decisão do time; os materiais de origem não são uma especificação executável. Ver [execução](execucao.md), [estudos](estudos.md) e [custos](custos.md) para os critérios comuns.
