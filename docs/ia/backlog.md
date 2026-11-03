# Backlog modular

Cada código identifica um incremento. As primeiras issues de cada módulo podem começar com contratos e fixtures; as integrações reais têm dependências explícitas. Não há prazo, calendário ou responsável fixo.

| Código | Módulo | Incremento | Dependências |
| --- | --- | --- | --- |
| [C01](https://github.com/K1w1b1t/watch/issues/1) | contratos | Formalizar contratos v1 e fixtures sintéticas | Nenhuma |
| [IN01](https://github.com/K1w1b1t/watch/issues/2) | ingestao | Receber um evento por HTTP com armazenamento em memória | Nenhuma; usar o contrato v1 proposto |
| [IN02](https://github.com/K1w1b1t/watch/issues/3) | ingestao | Aplicar autorização de máquina e limite de ingestão | [IN01](https://github.com/K1w1b1t/watch/issues/2) |
| [IN03](https://github.com/K1w1b1t/watch/issues/4) | ingestao | Integrar persistência real e reenvio idempotente | [IN02](https://github.com/K1w1b1t/watch/issues/3), [ST01](https://github.com/K1w1b1t/watch/issues/8) e [ID01](https://github.com/K1w1b1t/watch/issues/5) para integração real |
| [ID01](https://github.com/K1w1b1t/watch/issues/5) | identidade | Emitir e verificar chaves de máquina localmente | Nenhuma; usar porta v1 |
| [ID02](https://github.com/K1w1b1t/watch/issues/6) | identidade | Revogar e rotacionar credenciais de máquina | [ID01](https://github.com/K1w1b1t/watch/issues/5) |
| [ID03](https://github.com/K1w1b1t/watch/issues/7) | identidade | Autorizar analistas com identidade humana e papéis | [ID01](https://github.com/K1w1b1t/watch/issues/5) |
| [ST01](https://github.com/K1w1b1t/watch/issues/8) | armazenamento | Persistir eventos com isolamento e idempotência em SQLite | Nenhuma; usar EventStore v1 |
| [ST02](https://github.com/K1w1b1t/watch/issues/9) | armazenamento | Consultar e expurgar eventos com retenção limitada | [ST01](https://github.com/K1w1b1t/watch/issues/8) |
| [ST03](https://github.com/K1w1b1t/watch/issues/10) | armazenamento | Persistir alertas e evidências com consulta por tenant | [ST01](https://github.com/K1w1b1t/watch/issues/8) |
| [DE01](https://github.com/K1w1b1t/watch/issues/11) | deteccao | Detectar falha de autenticação com regra explicável | Nenhuma; usar evento/alerta v1 |
| [DE02](https://github.com/K1w1b1t/watch/issues/12) | deteccao | Correlacionar falhas em janela temporal e deduplicar alertas | [DE01](https://github.com/K1w1b1t/watch/issues/11) |
| [DE03](https://github.com/K1w1b1t/watch/issues/13) | deteccao | Ligar detecção ao armazenamento com reprocessamento seguro | [DE02](https://github.com/K1w1b1t/watch/issues/12) e [ST03](https://github.com/K1w1b1t/watch/issues/10) para integração real |
| [UI01](https://github.com/K1w1b1t/watch/issues/14) | painel | Exibir lista e detalhe de alertas com fixtures | Nenhuma; usar alerta v1 |
| [UI02](https://github.com/K1w1b1t/watch/issues/15) | painel | Consultar API autenticada e reconhecer alertas | [UI01](https://github.com/K1w1b1t/watch/issues/14); [ID03](https://github.com/K1w1b1t/watch/issues/7) e [ST03](https://github.com/K1w1b1t/watch/issues/10) para integração real |
| [UI03](https://github.com/K1w1b1t/watch/issues/16) | painel | Atualizar alertas por Phoenix Channel com reconexão | [UI02](https://github.com/K1w1b1t/watch/issues/15) |
| [CO01](https://github.com/K1w1b1t/watch/issues/17) | coletor | Enviar um evento sintético por CLI | Nenhuma; usar ingestão v1 |
| [CO02](https://github.com/K1w1b1t/watch/issues/18) | coletor | Reenviar com backoff e fila de tamanho limitado | [CO01](https://github.com/K1w1b1t/watch/issues/17) |
| [CO03](https://github.com/K1w1b1t/watch/issues/19) | coletor | Observar porta local Linux sem privilégios elevados | [CO01](https://github.com/K1w1b1t/watch/issues/17) |
| [IF01](https://github.com/K1w1b1t/watch/issues/20) | infraestrutura | Reproduzir laboratório local com rede e volumes isolados | Nenhuma; usar serviço de teste mínimo |
| [IF02](https://github.com/K1w1b1t/watch/issues/21) | infraestrutura | Provisionar Worker e D1 mínimos dentro do plano gratuito | [IF01](https://github.com/K1w1b1t/watch/issues/20); [IN01](https://github.com/K1w1b1t/watch/issues/2) e [ID01](https://github.com/K1w1b1t/watch/issues/5) para aplicação real |
| [IF03](https://github.com/K1w1b1t/watch/issues/22) | infraestrutura | Preparar deploy opcional do core na Oracle Always Free | [IF01](https://github.com/K1w1b1t/watch/issues/20); [ST03](https://github.com/K1w1b1t/watch/issues/10) e [ID03](https://github.com/K1w1b1t/watch/issues/7) para serviço real |
| [DS01](https://github.com/K1w1b1t/watch/issues/23) | devsecops | Executar testes e verificar segredos em pull requests | Nenhuma; habilitar jobs por módulos existentes |
| [DS02](https://github.com/K1w1b1t/watch/issues/24) | devsecops | Adicionar análise de dependências, código e IaC relevantes | [DS01](https://github.com/K1w1b1t/watch/issues/23) |
| [DS03](https://github.com/K1w1b1t/watch/issues/25) | devsecops | Demonstrar detecção e resposta no fluxo local completo | [IN03](https://github.com/K1w1b1t/watch/issues/4), [DE03](https://github.com/K1w1b1t/watch/issues/13), [UI02](https://github.com/K1w1b1t/watch/issues/15) e [CO01](https://github.com/K1w1b1t/watch/issues/17) |

## Como crescer

Após cada incremento, o módulo continua demonstrável isoladamente. Abrir melhorias separadas para regra de porta fora de allowlist, persistência de fila, exportação, mapeamento OCSF ou sensor Rust/Tauri somente quando existir um caso de uso e contrato verificável. Axiom, R2 e mobile não são pré-requisitos.

A documentação de cada issue no GitHub contém implementação esperada, critérios, testes unitários/E2E, exclusões e estudo. As primeiras issues não exigem aguardar [C01](https://github.com/K1w1b1t/watch/issues/1): usam o contrato proposto; divergências devem ser resolvidas antes da integração.
