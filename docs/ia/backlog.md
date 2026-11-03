# Backlog modular

Cada código identifica um incremento. As primeiras issues de cada módulo podem começar com contratos e fixtures; as integrações reais têm dependências explícitas. Não há prazo, calendário ou responsável fixo.

| Código | Módulo | Incremento | Dependências |
| --- | --- | --- | --- |
| C01 | contratos | Formalizar contratos v1 e fixtures sintéticas | Nenhuma |
| IN01 | ingestao | Receber um evento por HTTP com armazenamento em memória | Nenhuma; usar o contrato v1 proposto |
| IN02 | ingestao | Aplicar autorização de máquina e limite de ingestão | IN01 |
| IN03 | ingestao | Integrar persistência real e reenvio idempotente | IN02, ST01 e ID01 para integração real |
| ID01 | identidade | Emitir e verificar chaves de máquina localmente | Nenhuma; usar porta v1 |
| ID02 | identidade | Revogar e rotacionar credenciais de máquina | ID01 |
| ID03 | identidade | Autorizar analistas com identidade humana e papéis | ID01 |
| ST01 | armazenamento | Persistir eventos com isolamento e idempotência em SQLite | Nenhuma; usar EventStore v1 |
| ST02 | armazenamento | Consultar e expurgar eventos com retenção limitada | ST01 |
| ST03 | armazenamento | Persistir alertas e evidências com consulta por tenant | ST01 |
| DE01 | deteccao | Detectar falha de autenticação com regra explicável | Nenhuma; usar evento/alerta v1 |
| DE02 | deteccao | Correlacionar falhas em janela temporal e deduplicar alertas | DE01 |
| DE03 | deteccao | Ligar detecção ao armazenamento com reprocessamento seguro | DE02 e ST03 para integração real |
| UI01 | painel | Exibir lista e detalhe de alertas com fixtures | Nenhuma; usar alerta v1 |
| UI02 | painel | Consultar API autenticada e reconhecer alertas | UI01; ID03 e ST03 para integração real |
| UI03 | painel | Atualizar alertas por Phoenix Channel com reconexão | UI02 |
| CO01 | coletor | Enviar um evento sintético por CLI | Nenhuma; usar ingestão v1 |
| CO02 | coletor | Reenviar com backoff e fila de tamanho limitado | CO01 |
| CO03 | coletor | Observar porta local Linux sem privilégios elevados | CO01 |
| IF01 | infraestrutura | Reproduzir laboratório local com rede e volumes isolados | Nenhuma; usar serviço de teste mínimo |
| IF02 | infraestrutura | Provisionar Worker e D1 mínimos dentro do plano gratuito | IF01; IN01 e ID01 para aplicação real |
| IF03 | infraestrutura | Preparar deploy opcional do core na Oracle Always Free | IF01; ST03 e ID03 para serviço real |
| DS01 | devsecops | Executar testes e verificar segredos em pull requests | Nenhuma; habilitar jobs por módulos existentes |
| DS02 | devsecops | Adicionar análise de dependências, código e IaC relevantes | DS01 |
| DS03 | devsecops | Demonstrar detecção e resposta no fluxo local completo | IN03, DE03, UI02 e CO01 |

## Como crescer

Após cada incremento, o módulo continua demonstrável isoladamente. Abrir melhorias separadas para regra de porta fora de allowlist, persistência de fila, exportação, mapeamento OCSF ou sensor Rust/Tauri somente quando existir um caso de uso e contrato verificável. Axiom, R2 e mobile não são pré-requisitos.

A documentação de cada issue no GitHub contém implementação esperada, critérios, testes unitários/E2E, exclusões e estudo. As primeiras issues não exigem aguardar C01: usam o contrato proposto; divergências devem ser resolvidas antes da integração.
