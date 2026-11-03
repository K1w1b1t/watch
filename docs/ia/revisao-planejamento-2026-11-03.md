# Revisão do planejamento — 03/11/2026

> **Registro histórico da revisão inicial.** O escopo vigente foi corrigido depois desta análise: somente SDKs, sem adaptadores/apps e sem piloto empresarial. As recomendações empresariais abaixo estão adiadas, não são requisitos das issues atuais. Consulte o [backlog vigente](backlog.md) e o [plano geral](plano-geral.md).

## Parecer

O Watch tem uma boa base para aprendizado e um laboratório de detecção de segurança. O planejamento ainda não fecha uma entrega operacional para uma empresa. As issues têm resultados locais majoritariamente claros, mas faltam integrações, decisões de contrato e critérios de operação. Concluir as 71 issues como escritas não comprovaria prontidão empresarial.

Revisão documental das 71 descrições atuais no GitHub e de todos os documentos de planejamento deste repositório. Na consulta: 71 issues abertas, nenhuma atribuída e nenhum pull request retornado pela listagem de todos os estados. O checkout contém documentação e licença, sem implementação, testes ou CI. Portanto, é possível avaliar a execução proposta, mas ainda não a consistência de uma implementação. Não foram alteradas issues, dependências ou decisões vigentes nesta revisão.

Dependências nativas foram verificadas por amostragem: os endpoints `issues/4/dependencies/blocked_by` e `issues/71/dependencies/blocked_by` retornaram listas vazias. Nesses casos os pré-requisitos existem no texto, mas não como bloqueios nativos. Não foi feita afirmação equivalente sobre as outras 69 issues. Planos/preços de fornecedores não foram revalidados: esta revisão avalia a consistência interna, sem recomendar contratação ou implantação específica.

## O que preservar

- Separação entre identidade humana e credenciais por source; tenant derivado no servidor; testes negativos entre tenants.
- Contratos, fixtures, demonstrações locais e distinção entre mocks e integração real.
- Aceite durável, idempotência, falhas explícitas, memória limitada e evidências com expiração visível.
- Segurança de SDKs/browser, consentimento, restrições dos sistemas móveis e ausência de promessas de EDR ou detecção infalível.
- Pesquisa avançada e saídas opcionais separadas do caminho mínimo.

## Achados prioritários

| Prioridade | Evidência | Problema e correção proposta |
| --- | --- | --- |
| Alta | C01 #1, C02 #26 e contratos | C02 deve preservar o validador 1, mas não depende de C01. Permitir exploração paralela é válido; conclusão deve exigir compatibilidade com o artefato canônico e fixtures comuns. |
| Alta | IN01–IN03 #2–#4, C02 #26, SK03 #32 e produtores 1.1 | C02 entrega schemas/CLI; falta atribuir atualização do ingresso, persistência e consumidores para aceitar 1.1. Criar integração explícita com evento 1.1 real, rejeição de versões inválidas e regressão v1. Definir também a atualização do SDK TS que começa validando apenas v1. |
| Alta | DE01 #11, DE02 #12, DS03 #25 | DE01 alerta em cada falha; DE02 diz que quatro falhas não alertam e a quinta produz um alerta; DS03 espera um alerta. Especificar regra ativa por padrão, substituição ou coexistência e contagem por regra. Caso contrário, preservar o incremento anterior pode gerar cinco alertas low mais um medium. |
| Alta | DS03 #25 | Exige revogação e recuperação, mas não depende de ID02 #6 nem define quais dados/estado são recuperados. Incluir ID02, cenário concreto de recuperação e ambiente integrado reproduzível. IF01 #20 é candidato a pré-requisito de execução desse ambiente. |
| Alta | DS04 #71 | Exige source revogada, origem silenciosa unknown e dashboard, mas não declara ID02 #6, RG03 #29 e OB02 #59. Decidir também o papel de NT03 #57 para demonstrar roteamento entre tenants. Dois produtores devem emitir kinds compatíveis com a sequência de autenticação: apps com envio sintético genérico ou heartbeat não bastam. |
| Alta | DE06 #64, DS04 #71 | Correlação usa registro de vínculos fake. Não há entrega explícita para criar, autorizar, persistir e remover vínculos reais entre sources/subjects. Definir política de pseudonimização compatível e integrar o registro ao detector; não afirmar correlação operacional com um vínculo apenas simulado. |
| Alta | RG01 #27, RG02 #28, IN03 #4, ID03 #7 | Source disabled deve deixar de enviar, mas Identity.verify não define consulta ao status da source; RG02 exige analyst, mas sua integração não declara ID03. Atribuir verificação de status ao caminho real e incluir autorização humana na integração. Fake local continua permitido antes dela. |
| Alta | NT02 #56, ST03 #10, DE03 #13 | A outbox tem claim e retry, mas não define como cada alerta elegível vira tarefa de forma recuperável. Escolher transação compartilhada ou leitor com checkpoint/reconciliação; testar queda após salvar alerta e antes de criar tarefa. Não exigir a mesma tecnologia de banco para obter essa garantia. |
| Alta | IN03 #4, ST01 #8, ID01 #5, IF04 #69 | Worker TS e serviços/core precisam de contrato de transporte, autenticação interna, timeout e propriedade dos dados. Portas conceituais não definem como processos de linguagens diferentes se conectam. Registrar topologia local primeiro e remota antes do provisionamento, com uma autoridade por dado. |
| Média | IF02 #21, IF03 #22, IF04 #69 | IF04 não bloqueia os deploys. IF02 exercita revogação sem declarar ID02 e parte de IN01, cujo store é em memória. Deixar explícito se é smoke test descartável ou ingestão durável. IF03 combina decisão de capacidade, provisionamento, hardening, deploy e restore; separar resultados independentes. |
| Média | custos.md, arquitetura.md, IF03 #22 | A tabela de custos cita Oracle Always Free existente, enquanto a arquitetura e o fim do próprio documento excluem a VPS existente. Uniformizar o alvo e os pré-requisitos de capacidade. |
| Média | contratos, CU02 #40, AC02 #68, DE07 #65 | CU02 não define envelope/destino das métricas agregadas; AC02 pede trust_level que o kind account.security_signal não especifica; score/fatores de DE07 não têm saída contratada. Definir schema, enumerações e persistência quando aplicável, antes de integrar. |
| Média | C01 #1, C02 #26 e contratos | “PII rejeitada” é amplo demais para schemas: email/nome podem caber em strings permitidas como source_id. Definir geração dos pseudônimos, catálogos, limites e testes verificáveis. Schema fechado reduz exposição, mas não comprova ausência universal de dados pessoais. |
| Média | ST01 #8, ST02 #9, DE02 #12, DE03 #13 | Definir canonicalização do conteúdo, desempate de paginação, atraso aceito e reprocessamento. DE03 deve esclarecer reconstrução de janela no reinício, checkpoint sem perda e tratamento de expurgo antes da análise. DE05 #63 não deve corrigir silenciosamente uma garantia exigida antes dela. |
| Média | IN02 #3, custos.md e ST02 #9 | Rate limit de 10/min/credencial, orçamento remoto de 1.000/dia e limite de volume do buffer não têm um contrato integrado de aplicação/contabilização. ST02 implementa TTL, sem teto explícito de bytes/quantidade. Definir limites por tenant, comportamento ao saturar e efeito sobre a cobertura. |
| Média | DS04 #71 e contratos | Apenas alertas high/critical são notificados por padrão. O brute force de DE02 é medium. O roteiro que espera Discord deve usar DE06 high ou configurar explicitamente outro limiar; cinco falhas isoladas não satisfazem esse caminho. |

## Atomicidade, valor e clareza por grupo

Os grupos abaixo cobrem as 71 issues. “Bom recorte” significa resultado observável adequado para um incremento; não significa que o módulo está pronto para produção. Persistência com transação, idempotência e isolamento constitui um único resultado coerente, mesmo exigindo várias alterações de código.

| Issues | Avaliação e ajuste |
| --- | --- |
| C01, C02 (#1, #26) | Valor claro como infraestrutura de integração. Fixar artefatos canônicos, compatibilidade e precisão do aceite sobre PII; C02 deve convergir com C01. |
| IN01, IN02, IN03 (#2–#4) | Bons recortes: receber, autorizar/limitar e persistir de verdade. Fechar transporte interno, 1.1, status de source e limites operacionais. |
| ID01, ID02 (#5–#6) | Bons recortes. Emissão/verificação e ciclo de credencial têm valor claro; explicitar proteção do serviço interno e auditoria consultável. |
| ID03 (#7) | Escolha de IdP/MFA e implementação de autorização são resultados separáveis. Separar decisão do adaptador; definir bootstrap e administração de memberships. Dependência de ID01 merece justificativa, pois identidade humana é conceitualmente independente. |
| ST01, ST02 (#8–#9) | Bons recortes. Append/list de ST01 e paginação de ST02 precisam delimitar qual entrega introduz o quê; especificar volume máximo e canonicalização. |
| ST03 (#10) | Persistência de alertas, API de leitura e triagem podem ser separadas se houver PRs independentes. Se permanecer única, assumir explicitamente um primeiro fluxo de consulta/triagem, com responsável técnico pela API e transporte definidos. |
| ST04 (#61) | Bom recorte de preservação; deve integrar o caminho de criação de alerta, não apenas expor uma função demonstrável isolada. |
| DE01, DE02, DE03 (#11–#13) | Boa progressão didática. Resolver ativação de regras, semântica temporal, reconstrução de estado e expurgo versus checkpoint antes de cobrar um alerta único. |
| DE04, DE05, DE06 (#62–#64) | Valores distintos e complementares. Fixar operadores de DE04, garantias de estado de DE05 e integração de vínculos de DE06. |
| DE07, DE08 (#65–#66) | Experimentos delimitados, sem necessidade no primeiro piloto. DE07 precisa de contrato para score/fatores e estado; DE08 deve listar exatamente o subconjunto de operadores e campos aceitos. |
| UI01, UI02, UI03 (#14–#16) | Bons recortes locais e integrados. Falta atribuir login/logout/renovação de sessão e resolução do alerta pela UI; reconhecer apenas não completa triagem. UI03 deve especificar sessão revogada e perda de membership, além de expiração do token. |
| CO01, CO02, CO03 (#17–#19) | Bons recortes. CO02 explicita perda ao reiniciar. CO03 gera telemetria, mas falta regra para transformar porta fora de allowlist em alerta, como proposto no catálogo de exercícios. |
| IF01 (#20) | Bom recorte de laboratório. Definir quem atualiza a composição quando módulos reais entram, para não encerrar apenas com um serviço mínimo desconectado. |
| IF02, IF03 (#21–#22) | Concentram muitos resultados independentes e decisões condicionais. Separar plano elegível, provisionamento e validação de serviço real; distinguir impedimento de deploy de entrega documental concluída. |
| IF04, IF05 (#69–#70) | IF04 é decisão útil, antecipar aos deploys. IF05 é razoavelmente atômica se escolher um alvo concreto antes de começar. |
| DS01, DS02 (#23–#24) | DS01 bem delimitada. DS02 deve ganhar scanners por artefato existente em incrementos, com gates e exceções precisos; evita uma issue indefinida de todas as linguagens. |
| DS03, DS04 (#25, #71) | Valor claro como testes de sistema. Integração ampla é aceitável se os recursos já existirem: hoje os critérios revelam funcionalidades e dependências faltantes. |
| RG01, RG02, RG03 (#27–#29) | Bons resultados de cadastro, pareamento e cobertura. Resolver autorização real, status disabled, ingresso 1.1 e integração do heartbeat. RG01 testa detalhe, embora implementação descreva create/list: incluir a rota ou retirar o critério. |
| SK01, SK02, SK03 (#30–#32) | Bons recortes. Definir evolução do validador TS para 1.1 e comprovar integração com ingresso real após o desenvolvimento isolado. |
| SK04 (#33) | Resultado vertical válido, porém envolve consentimento, backend, auth, CSRF e transporte. Fixar kind, catálogos e integração antes da execução; separar infraestrutura reutilizável se surgir outro resultado independente. |
| SK05, SK06 (#34–#35) | Bons recortes por linguagem. Reutilizar a mesma suíte de conformidade; podem esperar o primeiro produtor integrado para evitar multiplicar protocolo instável. |
| CV01 (#36) | Potencial sobreposição com SK02. Definir valor exclusivo: mapeamento autorizado de projeto/source e testes de separação; se não houver diferença real, incorporar ao exemplo Next. |
| CS01, CS02 (#37–#38) | CS01 é adaptador bem delimitado; CS02 inclui captura, outbox e exportação. Pode permanecer uma fatia vertical se limitada à tabela demo, com checkpoints e retry claros; dividir se a outbox virar componente genérico. |
| CU01, CU02 (#39–#40) | CU01 bem delimitada. CU02 tem produto/contrato de saída indefinido e dependência funcional de CU01 pouco justificada. |
| CP01, OC01 (#41–#42) | Bons adaptadores locais, mas conclusão não comprova conta/provedor integrado. Atribuir integração real posterior ou classificar explicitamente como demonstração de contrato. |
| DT01, DT02, DT03 (#43–#45) | Boa progressão de app, credencial e sensor. Definir interoperabilidade CLI TS/adaptador Rust e testes reais de revogação. |
| DT04, DT05 (#46–#47) | Bons recortes por SO. Dependem do contrato do sensor ainda sem entrega canônica claramente indicada; referenciar DT03 quando necessário. |
| DT06 (#48) | Pausa e remoção compõem ciclo de instalação coerente. Se coleta periódica existir em DT03, pausa/consentimento devem estar disponíveis antes do piloto, ainda que a remoção venha depois. |
| MB01, MB02, MB03, MB04 (#49–#52) | Bons incrementos com limites honestos. Esclarecer coleta de postura, identidade do sensor e ingresso 1.1. Acrescentar remoção da credencial/instalação, para além de pausa do background. |
| MB05, MB06 (#53–#54) | Bons recortes, com validação nativa e identidade separada. MB05 precisa limitar matriz aos recursos existentes; MB06 deve definir login e lifecycle da sessão real. |
| NT01, NT02, NT03 (#55–#57) | Boa divisão de formato, entrega e roteamento. Completar ponte recuperável AlertStore→outbox e testes de isolamento desde a primeira integração entre tenants. |
| OB01, OB02, OB03 (#58–#60) | Bons recortes. OB01 precisa definir retomada/cursor; OB03 deve ser instrumentada nos módulos reais antes de comprovar saúde da pipeline. OB02 é dependência do teste de dashboard. |
| AC01, AC02 (#67–#68) | Boa separação entre pesquisa e adaptador. AC02 deve selecionar um provedor/sinal concreto e esclarecer trust_level antes de começar. |

## Lacunas para um piloto em empresa

O escopo atual exclui dados reais de clientes e se apoia em aplicações demonstrativas. Isso é coerente com laboratório. Mudar para uso empresarial exige uma entrega explícita, autorizada e validada para uma aplicação real, com limites conhecidos de cobertura. Mais SDKs e plataformas não substituem essa validação.

1. **Usuário e cenário:** escolher uma equipe, uma aplicação e uma ameaça detectável; documentar o que fica invisível quando o SDK não emite sinais. Não exigir EDR, SIEM universal ou alta disponibilidade para esse primeiro piloto.
2. **Onboarding completo:** criar tenant e primeiro administrador, gerir entrada/saída de analistas e memberships, cadastrar source, emitir/revogar credencial e desativar fonte. Pode começar por CLI/documentação, sem painel administrativo completo.
3. **Operação contínua:** iniciar ingestão, detector e notificações automaticamente, aplicar migrações, monitorar backlog/atraso, reiniciar sem perder trabalho e alertar quando a própria pipeline falhar.
4. **Investigação e resposta:** permitir atribuição ou procedimento explícito de responsável, comentário/registro de decisão, classificação de falso positivo, resolução e trilha de auditoria. Um workflow mínimo pode integrar ferramenta externa; não precisa de um sistema completo de casos próprio.
5. **Qualidade de detecção:** regras úteis ao cenário, habilitação/configuração por tenant, fixtures benignas/maliciosas e avaliação de ruído. Uma falha de autenticação isolada pode ensinar o protocolo, mas não justifica notificar uma equipe continuamente.
6. **Recuperação e manutenção:** backup do conjunto necessário (eventos, alertas, identidade, estado e outbox conforme desenho), restore integrado, atualização/rollback e objetivos mensuráveis de perda e tempo de recuperação.
7. **Capacidade e dados:** definir volume esperado, latência máxima de alerta, comportamento em saturação, retenção suficiente ao caso de uso, finalidade e acesso aos dados. Os limites atuais são de laboratório; não há demonstração de que atendam uma empresa específica.
8. **Aceite do piloto:** instalação reproduzível e exercício com telemetria da aplicação real, mostrando detecção, evidência, notificação, triagem, revogação, falha de sink e recuperação. Registrar exclusões; não usar o encerramento de issues como substituto dessa prova.

## Sequência recomendada, sem calendário

1. Consolidar contratos e topologia local; resolver regras ativas, transportes, canonicalização e dependências dos testes de sistema. Manter protótipos paralelos, mas exigir conformidade compartilhada para concluir integrações.
2. Entregar o caminho mínimo C01 + IN01–IN03 + ID01–ID03 + ST01–ST04 + DE01–DE03 + UI01–UI02 + CO01 + IF01 + DS01. Incorporar retenção e revogação ao aceite DS03 e explicitar quais regras estarão ativas.
3. Trocar emissor sintético por uma aplicação de exemplo via SK01/SK02; integrar RG01, NT01–NT03 e OB03, com vínculo real entre alerta e outbox e diagnóstico de perda/atraso.
4. Acrescentar as lacunas de operação/triagem/recuperação e executar um piloto delimitado. Validar identidade real e onboarding; se necessário apenas CLI, documentá-la integralmente.
5. Expandir produtores, 1.1, correlação entre fontes, apps e observabilidade opcional depois de estabilizar o caminho utilizado. Para implantar remotamente, IF04 precede IF02/IF03 e seus testes de custo/topologia.

## Padrão recomendado para as descrições

Manter o modelo atual e acrescentar, onde faltar: consumidor beneficiado; exemplo concreto de entrada/saída; artefato/interface canônica; números dos pré-requisitos para concluir; componentes que podem ser simulados durante desenvolvimento; cenário de demonstração e valores esperados; falhas/reinício/concorrência relevantes; integração posterior atribuída a uma issue. Não é necessário decompor cada teste ou arquivo em uma issue.

Tratar pré-requisito de conclusão como bloqueio; referência ou estudo não deve virar bloqueio. A capacidade de começar com fake não elimina dependência para concluir uma integração real. Evitar encerrar integração apenas com mock. Não impor datas ou responsáveis fixos: marcos podem ser definidos por capacidade demonstrada.
