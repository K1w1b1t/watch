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

## Produtores, apps e análise ampliada

Os incrementos abaixo completam os originadores e as saídas do sistema. Mobile, desktop e SDKs podem começar com fixtures/receptores locais, sem aguardar o core. Conectores usam aplicações demonstrativas; a instalação em projetos/contas existentes fica para uma implementação com escopo próprio.

| Código | Módulo | Incremento | Dependências |
| --- | --- | --- | --- |
| [C02](https://github.com/K1w1b1t/watch/issues/26) | contratos | Formalizar extensão 1.1 para fontes, postura e serviços | Nenhuma; trabalhar contra proposta documentada |
| [RG01](https://github.com/K1w1b1t/watch/issues/27) | fontes | Cadastrar e listar fontes por tenant | Nenhuma; identidade humana pode ser fake local |
| [RG02](https://github.com/K1w1b1t/watch/issues/28) | fontes | Parear um dispositivo com código temporário de uso único | [RG01](https://github.com/K1w1b1t/watch/issues/27) e [ID01](https://github.com/K1w1b1t/watch/issues/5) para integração real |
| [RG03](https://github.com/K1w1b1t/watch/issues/29) | fontes | Mostrar último contato e cobertura desconhecida de uma fonte | [RG01](https://github.com/K1w1b1t/watch/issues/27) e [C02](https://github.com/K1w1b1t/watch/issues/26) para integração real |
| [SK01](https://github.com/K1w1b1t/watch/issues/30) | sdk-typescript | Enviar evento server-side com SDK TypeScript mínimo | Nenhuma; usar evento v1 e receptor fake |
| [SK02](https://github.com/K1w1b1t/watch/issues/31) | sdk-typescript | Instrumentar uma rota Next.js no servidor | [SK01](https://github.com/K1w1b1t/watch/issues/30) |
| [SK03](https://github.com/K1w1b1t/watch/issues/32) | sdk-typescript | Instrumentar acesso negado no NestJS | [SK01](https://github.com/K1w1b1t/watch/issues/30) e [C02](https://github.com/K1w1b1t/watch/issues/26) para kind 1.1 |
| [SK04](https://github.com/K1w1b1t/watch/issues/33) | sdk-typescript | Receber sinal do React pelo próprio backend com consentimento | Nenhuma; backend e transportes controlados locais |
| [SK05](https://github.com/K1w1b1t/watch/issues/34) | sdk-python | Enviar evento v1 com SDK Python mínimo | Nenhuma; contrato e fixtures v1 |
| [SK06](https://github.com/K1w1b1t/watch/issues/35) | sdk-go | Enviar evento v1 com SDK Go mínimo | Nenhuma; contrato e fixtures v1 |
| [CV01](https://github.com/K1w1b1t/watch/issues/36) | conector-vercel | Normalizar eventos instrumentados de projetos Vercel | Nenhuma; SDK fake e fixtures |
| [CS01](https://github.com/K1w1b1t/watch/issues/37) | conector-supabase | Converter resultado Supabase Auth observado no servidor | Nenhuma; cliente Auth simulado localmente |
| [CS02](https://github.com/K1w1b1t/watch/issues/38) | conector-supabase | Auditar mudança de uma tabela de laboratório sem exportar linhas | Nenhuma; Postgres local e evento 1.1 proposto |
| [CU01](https://github.com/K1w1b1t/watch/issues/39) | conector-upstash | Instrumentar falha Redis sem coletar comandos ou valores | Nenhuma; cliente Redis controlado |
| [CU02](https://github.com/K1w1b1t/watch/issues/40) | conector-upstash | Consultar estatísticas Upstash com polling limitado | [CU01](https://github.com/K1w1b1t/watch/issues/39); API de stats documentada pode ser simulada |
| [CP01](https://github.com/K1w1b1t/watch/issues/41) | conector-posthog | Receber destination webhook PostHog com allowlist | Nenhuma; webhook simulado local e contrato 1.1 |
| [OC01](https://github.com/K1w1b1t/watch/issues/42) | conector-oracle | Instrumentar falhas de operações de bucket em app de exemplo | Nenhuma; cliente Object Storage fake |
| [DT01](https://github.com/K1w1b1t/watch/issues/43) | desktop | Criar app Tauri Linux com envio sintético explícito | Nenhuma; transporte local e fixtures v1 |
| [DT02](https://github.com/K1w1b1t/watch/issues/44) | desktop | Parear desktop e guardar credencial fora da camada web | [DT01](https://github.com/K1w1b1t/watch/issues/43) e [RG02](https://github.com/K1w1b1t/watch/issues/28) para integração real |
| [DT03](https://github.com/K1w1b1t/watch/issues/45) | desktop | Acrescentar sensor de portas Linux ao app Tauri | [DT01](https://github.com/K1w1b1t/watch/issues/43); [CO03](https://github.com/K1w1b1t/watch/issues/19) e [DT02](https://github.com/K1w1b1t/watch/issues/44) para integração real |
| [DT04](https://github.com/K1w1b1t/watch/issues/46) | desktop | Adicionar adaptador de portas Windows com fixture e teste nativo | [DT01](https://github.com/K1w1b1t/watch/issues/43); contrato de sensor desktop |
| [DT05](https://github.com/K1w1b1t/watch/issues/47) | desktop | Adicionar adaptador de portas macOS com fixture e teste nativo | [DT01](https://github.com/K1w1b1t/watch/issues/43); contrato de sensor desktop |
| [DT06](https://github.com/K1w1b1t/watch/issues/48) | desktop | Pausar coleta e remover configuração de uma instalação | [DT02](https://github.com/K1w1b1t/watch/issues/44) |
| [MB01](https://github.com/K1w1b1t/watch/issues/49) | mobile | Criar app Android instalável com envio em primeiro plano | Nenhuma; receptor fake e evento v1 |
| [MB02](https://github.com/K1w1b1t/watch/issues/50) | mobile | Parear instalação mobile usando SecureStore | [MB01](https://github.com/K1w1b1t/watch/issues/49) e [RG02](https://github.com/K1w1b1t/watch/issues/28) para integração real |
| [MB03](https://github.com/K1w1b1t/watch/issues/51) | mobile | Enviar postura limitada com resultado desconhecido explícito | [MB01](https://github.com/K1w1b1t/watch/issues/49); [C02](https://github.com/K1w1b1t/watch/issues/26) e [MB02](https://github.com/K1w1b1t/watch/issues/50) para integração real |
| [MB04](https://github.com/K1w1b1t/watch/issues/52) | mobile | Enviar heartbeat em background oportunista com pausa | [MB02](https://github.com/K1w1b1t/watch/issues/50); [C02](https://github.com/K1w1b1t/watch/issues/26) para heartbeat |
| [MB05](https://github.com/K1w1b1t/watch/issues/53) | mobile | Validar build iOS e matriz de sinais disponíveis | [MB01](https://github.com/K1w1b1t/watch/issues/49) |
| [MB06](https://github.com/K1w1b1t/watch/issues/54) | mobile | Consultar alertas pelo app com identidade humana | [MB01](https://github.com/K1w1b1t/watch/issues/49); [ID03](https://github.com/K1w1b1t/watch/issues/7) e [ST03](https://github.com/K1w1b1t/watch/issues/10) para integração real |
| [NT01](https://github.com/K1w1b1t/watch/issues/55) | notificacoes | Formatar e enviar um alerta ao webhook Discord | Nenhuma; AlertStore fake e servidor Discord fake |
| [NT02](https://github.com/K1w1b1t/watch/issues/56) | notificacoes | Persistir outbox e reenviar notificações com limite | [NT01](https://github.com/K1w1b1t/watch/issues/55) |
| [NT03](https://github.com/K1w1b1t/watch/issues/57) | notificacoes | Rotear destino por tenant e reduzir alertas repetidos | [NT02](https://github.com/K1w1b1t/watch/issues/56) |
| [OB01](https://github.com/K1w1b1t/watch/issues/58) | observabilidade | Exportar alertas saneados para Loki pelo servidor | Nenhuma; alertas e Loki fake locais |
| [OB02](https://github.com/K1w1b1t/watch/issues/59) | observabilidade | Provisionar dashboard Grafana para alertas com Terraform | [OB01](https://github.com/K1w1b1t/watch/issues/58) para dados reais; Loki fixtures no início |
| [OB03](https://github.com/K1w1b1t/watch/issues/60) | observabilidade | Expor saúde da pipeline e falhas de entrega sem PII | Nenhuma; componentes podem usar adaptadores fake |
| [ST04](https://github.com/K1w1b1t/watch/issues/61) | armazenamento | Preservar snapshots mínimos de evidências após expurgo do buffer | [ST02](https://github.com/K1w1b1t/watch/issues/9) e [ST03](https://github.com/K1w1b1t/watch/issues/10) |
| [DE04](https://github.com/K1w1b1t/watch/issues/62) | deteccao | Carregar regras versionadas de catálogo com validação | [DE01](https://github.com/K1w1b1t/watch/issues/11) |
| [DE05](https://github.com/K1w1b1t/watch/issues/63) | deteccao | Garantir estado temporal consistente e limitado entre instâncias | [DE02](https://github.com/K1w1b1t/watch/issues/12) |
| [DE06](https://github.com/K1w1b1t/watch/issues/64) | deteccao | Correlacionar falha e sucesso entre duas fontes autorizadas | [DE02](https://github.com/K1w1b1t/watch/issues/12); evento v1 e registro de vínculos fake |
| [DE07](https://github.com/K1w1b1t/watch/issues/65) | deteccao | Pontuar sinais explicáveis sem confundir severidade de origem | [DE04](https://github.com/K1w1b1t/watch/issues/62) |
| [DE08](https://github.com/K1w1b1t/watch/issues/66) | deteccao | Importar um subconjunto explícito de regras Sigma | [DE04](https://github.com/K1w1b1t/watch/issues/62) |
| [AC01](https://github.com/K1w1b1t/watch/issues/67) | contas | Documentar sinais realmente disponíveis de contas e provedores | Nenhuma; pesquisa contra documentação oficial |
| [AC02](https://github.com/K1w1b1t/watch/issues/68) | contas | Normalizar um sinal de segurança de conta em laboratório | [AC01](https://github.com/K1w1b1t/watch/issues/67); [C02](https://github.com/K1w1b1t/watch/issues/26) para account.security_signal |
| [IF04](https://github.com/K1w1b1t/watch/issues/69) | infraestrutura | Registrar topologia remota completa e limites do modo gratuito | Nenhuma; decisão documental |
| [IF05](https://github.com/K1w1b1t/watch/issues/70) | infraestrutura | Empacotar release de laboratório de um app com checksums | [DT01](https://github.com/K1w1b1t/watch/issues/43) ou [MB01](https://github.com/K1w1b1t/watch/issues/49); um alvo por entrega |
| [DS04](https://github.com/K1w1b1t/watch/issues/71) | devsecops | Exercitar múltiplos originadores até Discord e Grafana locais | [IN03](https://github.com/K1w1b1t/watch/issues/4), [DE03](https://github.com/K1w1b1t/watch/issues/13), [DE06](https://github.com/K1w1b1t/watch/issues/64), [ST04](https://github.com/K1w1b1t/watch/issues/61), [NT02](https://github.com/K1w1b1t/watch/issues/56) e [OB01](https://github.com/K1w1b1t/watch/issues/58); produtores reais escolhidos entre [SK02](https://github.com/K1w1b1t/watch/issues/31), [DT02](https://github.com/K1w1b1t/watch/issues/44) e [MB02](https://github.com/K1w1b1t/watch/issues/50) |

## Como escolher uma entrega

Começar por uma primeira issue de módulo sem dependência funcional: SDK TypeScript/Python/Go, desktop Linux, Android em primeiro plano, conector com fixtures, Discord fake ou métricas locais. Os incrementos posteriores mantêm as capacidades anteriores funcionando. As issues de integração exercitam serviços reais; fixtures não encerram critérios de validação nativa ou deploy.

Cada issue contém comportamento esperado, critérios de aceite, testes unitários/E2E, exclusões e estudo. Não há prazo, estimativa em dias, calendário ou responsável fixo. Código C02 amplia o contrato sem alterar silenciosamente os eventos já planejados em C01.

Monetização, retenção de anos, coleta de SMS/localização, consulta de CPF/email em vazamentos, inteligência externa com dados pessoais, execução automática de contenção e IA autônoma de análise permanecem fora do escopo implementável atual. O mapa de origens distingue capacidades planejadas de visibilidade indisponível.
