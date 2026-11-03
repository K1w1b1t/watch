# Glossário: os termos usados no Watch

**Use este arquivo para:** consultar uma palavra durante a leitura de uma issue. Os nomes em inglês que aparecem no código permanecem iguais; as explicações abaixo mostram seu significado no projeto.

## Dados e identidade

| Termo | Significado e exemplo |
| --- | --- |
| Evento / event | Registro de um acontecimento: uma falha de login em certo horário |
| Alerta / alert | Resultado de uma regra, com gravidade e explicação; não é confirmação automática de ataque |
| Evidência | Eventos usados para justificar o alerta |
| Organização / tenant | Grupo cujos dados devem ficar separados dos demais. `tenant_id` é seu identificador interno; usamos A e B nos testes |
| Fonte / source | Aplicação cadastrada que envia eventos, como `app-a`. Seu identificador é `source_id` |
| Pessoa / subject | Identificador observado no evento, como `pessoa-demo`. `subject_id` não é o login do analista do painel |
| Pseudônimo | Identificador no lugar de dado pessoal direto. Usamos valores fictícios; pseudonimizar não torna qualquer dado automaticamente anônimo |
| Namespace | Convenção de nomes. Duas fontes só interpretam o mesmo subject_id como a mesma pessoa quando essa convenção foi combinada e o vínculo foi autorizado |
| Credencial / API key | Segredo que autoriza uma aplicação a enviar por sua fonte |
| Autenticação | Conferir quem fez a chamada, por exemplo validando uma chave |
| Autorização | Conferir o que essa identidade pode fazer, como ler ou alterar um alerta |
| Papel / role | Conjunto de permissões. `viewer` lê; `analyst` pode reconhecer/resolver |
| Membership | Vínculo de um usuário com uma organização e seu papel nela |
| JWT | Token assinado usado para reconhecer a sessão humana. Sua assinatura e seus campos precisam ser verificados |
| JWKS | Publicação das chaves públicas usadas para verificar a assinatura dos tokens |
| Hash | Resumo calculado de um conteúdo. Guardamos hash da chave, não seu segredo; hash de evidência não prova que a origem disse a verdade |
| UUID | Formato de identificador, como `00000000-0000-4000-8000-000000000001` |
| Timestamp / RFC3339 / UTC | Horário em formato padronizado. `2026-11-03T12:00:00Z` é um exemplo em UTC |

## Programas e comunicação

| Termo | Significado e exemplo |
| --- | --- |
| SDK | Biblioteca que a aplicação importa para chamar `send` e enviar evento ao Watch |
| Server-side / backend | Código executado no servidor da aplicação, onde podemos manter o segredo do SDK |
| Frontend / browser | Código e tela no navegador do usuário; não deve receber chave secreta do Watch |
| API / endpoint | Interface de chamadas; um endpoint é uma operação por endereço/método, como `GET /v1/alerts` |
| HTTP / status HTTP | Protocolo das chamadas e seu código de resultado. `201` indica criação; `401`, credencial inválida |
| Payload / body | Conteúdo enviado na chamada, normalmente o JSON do evento |
| Core | Nome técnico do servidor principal do Watch, em Elixir/Phoenix |
| Worker / ingresso | Programa TypeScript que recebe os pedidos de envio e chama o servidor principal |
| CLI | Programa usado pelo terminal, como a demonstração que importa o SDK TS |
| Contrato | Acordo exato de campos, operações e respostas entre duas partes |
| Schema / JSON Schema | Arquivo que descreve e valida o formato permitido de um JSON |
| OpenAPI | Formato para documentar endereços, pedidos e respostas de uma API |
| Porta / interface | Operação que uma peça oferece para outra, como `EventStore.append`. Neste uso, não significa porta TCP |
| EventStore / AlertStore | Interfaces para guardar e consultar eventos ou alertas |
| Fixtures | Arquivos de exemplo com entradas e resultados conhecidos, usados nos testes |
| Fake / mock / stub | Substituto controlado de uma dependência no teste. Ajuda a desenvolver, mas não comprova integração com o serviço real |
| ADR | Registro curto de uma decisão técnica: problema, escolha, alternativas e consequências |
| Timeout | Tempo máximo de espera por uma operação |
| Retry / backoff / jitter | Nova tentativa, espera antes dela e variação dessa espera. Evitam repetição imediata sem controle |
| Webhook | Chamada HTTP para avisar um destino. No Watch, o webhook Discord é uma saída de notificação |
| Channel / WebSocket | Conexão para avisos em tempo real. O painel recebe o ID e consulta o estado atual pela API |
| TLS / HTTPS | Proteção da conexão para evitar transmitir credenciais e dados em texto aberto na rede |
| CSRF / mesma origem | Proteção contra um site externo forjar ação com a sessão do usuário; no exemplo React, conferir origem e prova CSRF no backend |

## Persistência e detecção

| Termo | Significado e exemplo |
| --- | --- |
| Persistir / durável | Guardar de forma que o dado continue disponível depois de reiniciar o processo |
| Buffer | Armazenamento temporário dos eventos antes de sua limpeza por prazo/limite |
| Retenção / TTL | Por quanto tempo um dado pode ficar guardado; o buffer começa com prazo de 24 horas |
| Expurgo | Remoção controlada de dados vencidos ou de cópias de evidência acima do limite |
| Idempotência / replay | Repetir a mesma operação sem duplicar o efeito. Reenviar o mesmo evento igual não cria outra cópia |
| Normalização / canonicalização | Transformar conteúdo equivalente numa representação padronizada antes de comparar ou calcular hash |
| Transação / atomicidade | Um conjunto de alterações é aplicado inteiro ou desfeito inteiro; não deixa metade do alerta salva |
| Cursor / paginação | Marcador da próxima página de resultados; o cliente devolve o cursor sem interpretar seu conteúdo |
| occurred_at / received_at | Horário em que o evento aconteceu / horário em que o servidor recebeu |
| ingest_seq | Número crescente atribuído na chegada; ajuda a ler eventos sem pular os que chegaram atrasados |
| Determinístico | Mesmo conjunto de entradas e condições produz o mesmo resultado |
| Janela temporal | Intervalo de tempo em que uma regra agrupa eventos, como 60 segundos |
| Cooldown | Espera antes de gerar novo resultado equivalente, reduzindo repetição |
| occurrence_key | Chave que identifica a mesma ocorrência de uma regra para não duplicar alerta ao reprocessar |
| Checkpoint | Marcador da posição até onde um leitor concluiu seu trabalho |
| Polling | Consulta repetida de tempos em tempos, como o detector lendo eventos novos |
| coverage_gap | Informação de que faltam dados para afirmar que todo um trecho foi analisado |
| Snapshot | Cópia mínima de evidência preservada junto do alerta |
| Outbox | Tabela de tarefas de notificação pendentes, confirmadas ou falhas |
| Claim / lease | Reserva temporária de uma tarefa por um processo. Se ele cair, a reserva vence e outro pode retomá-la |
| Rótulo / label e cardinalidade | Nome usado para agrupar medidas. IDs diferentes sem limite criam muitos grupos; por isso não os usamos como labels no Loki/métricas |

## Desenvolvimento e infraestrutura

| Termo | Significado e exemplo |
| --- | --- |
| Issue / PR | Tarefa no GitHub / proposta de alteração de código para revisão |
| CI | Verificações automáticas executadas pelo GitHub para cada alteração |
| Teste unitário | Verifica uma regra de lógica isolada, como a contagem de cinco falhas |
| Teste de contrato | Confere que os participantes aceitam/rejeitam o formato combinado |
| E2E / ponta a ponta | Exercita uma entrada real e observa o resultado; uma integração completa atravessa os serviços reais exigidos |
| Contêiner / composição | Processo empacotado com dependências / configuração para subir os processos juntos |
| Loopback / localhost | Acesso pela própria máquina, como `127.0.0.1`; evita expor o laboratório na rede |
| Volume | Área persistente usada para dados dos contêineres |
| Root | Usuário de privilégios elevados; os processos do laboratório devem usar permissões menores |
| Terraform plan / apply / state | Prévia das mudanças / execução das mudanças / registro de recursos controlados pela ferramenta |
| Provisionar / teardown | Criar recursos de infraestrutura / remover os recursos do laboratório |
| Smoke test | Teste mínimo de funcionamento; IF02 testa marcador em Worker/D1, não o Watch completo |
| SAST / SCA / IaC | Análise de código / análise de dependências / infraestrutura descrita como código |
| Sigma | Formato de regras de detecção; DE08 suporta só uma parte explícita dele |
