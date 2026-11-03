# Backlog modular vigente

Escopo aprovado em 03/11/2026: ingestão somente por SDKs server-side. CLI usa SDK TS; Next/Nest e React via backend são exemplos. Sem adaptadores de plataformas, agentes, sensores ou apps nativos. Dados sintéticos e laboratório local; piloto empresarial permanece fora desta fase.

52 issues ativas e 24 adiadas. Não há prazo, calendário ou responsável fixo. Protótipos podem começar com contratos/fakes; os pré-requisitos abaixo bloqueiam a **conclusão**, incluindo os E2Es reais explicitados. As descrições e bloqueios nativos no GitHub reproduzem este grafo.

## Issues ativas

| Código | Módulo | Resultado | Pré-requisitos para concluir |
| --- | --- | --- | --- |
| [C01](https://github.com/K1w1b1t/watch/issues/1) | contratos | Formalizar contratos v1 e fixtures compartilhadas | Nenhum |
| [IN01](https://github.com/K1w1b1t/watch/issues/2) | ingestao | Receber evento HTTP v1 com adaptadores locais | [C01](https://github.com/K1w1b1t/watch/issues/1) |
| [IN02](https://github.com/K1w1b1t/watch/issues/3) | ingestao | Aplicar autorização e rate limit no ingresso | [IN01](https://github.com/K1w1b1t/watch/issues/2) |
| [IN03](https://github.com/K1w1b1t/watch/issues/4) | ingestao | Integrar ingresso aos serviços persistentes do core | [IN02](https://github.com/K1w1b1t/watch/issues/3), [ID01](https://github.com/K1w1b1t/watch/issues/5), [ST01](https://github.com/K1w1b1t/watch/issues/8), [RG01](https://github.com/K1w1b1t/watch/issues/27), [C03](https://github.com/K1w1b1t/watch/issues/72) |
| [ID01](https://github.com/K1w1b1t/watch/issues/5) | identidade | Emitir e verificar chaves de SDK no core local | [C01](https://github.com/K1w1b1t/watch/issues/1), [C03](https://github.com/K1w1b1t/watch/issues/72) |
| [ID02](https://github.com/K1w1b1t/watch/issues/6) | identidade | Revogar e rotacionar chaves de SDK | [ID01](https://github.com/K1w1b1t/watch/issues/5) |
| [ID03](https://github.com/K1w1b1t/watch/issues/7) | identidade | Validar identidade humana e papéis no laboratório | [C01](https://github.com/K1w1b1t/watch/issues/1), [C03](https://github.com/K1w1b1t/watch/issues/72) |
| [ST01](https://github.com/K1w1b1t/watch/issues/8) | armazenamento | Persistir eventos isolados e idempotentes em SQLite | [C01](https://github.com/K1w1b1t/watch/issues/1), [C03](https://github.com/K1w1b1t/watch/issues/72) |
| [ST02](https://github.com/K1w1b1t/watch/issues/9) | armazenamento | Aplicar retenção e limites de volume ao buffer | [ST01](https://github.com/K1w1b1t/watch/issues/8) |
| [ST03](https://github.com/K1w1b1t/watch/issues/10) | armazenamento | Persistir alertas e referências de evidência por tenant | [ST01](https://github.com/K1w1b1t/watch/issues/8) |
| [DE01](https://github.com/K1w1b1t/watch/issues/11) | deteccao | Demonstrar regra pura de falha de autenticação | [C01](https://github.com/K1w1b1t/watch/issues/1) |
| [DE02](https://github.com/K1w1b1t/watch/issues/12) | deteccao | Detectar cinco falhas em janela e deduplicar sinais | [DE01](https://github.com/K1w1b1t/watch/issues/11) |
| [DE03](https://github.com/K1w1b1t/watch/issues/13) | deteccao | Processar eventos persistidos com checkpoint e replay seguro | [DE02](https://github.com/K1w1b1t/watch/issues/12), [ST02](https://github.com/K1w1b1t/watch/issues/9), [ST03](https://github.com/K1w1b1t/watch/issues/10) |
| [UI01](https://github.com/K1w1b1t/watch/issues/14) | painel | Exibir lista e detalhe de alertas com fixtures | [C01](https://github.com/K1w1b1t/watch/issues/1) |
| [UI02](https://github.com/K1w1b1t/watch/issues/15) | painel | Consultar API autenticada e alterar status de alertas | [UI01](https://github.com/K1w1b1t/watch/issues/14), [ST05](https://github.com/K1w1b1t/watch/issues/73) |
| [UI03](https://github.com/K1w1b1t/watch/issues/16) | painel | Atualizar alertas por Phoenix Channel autenticado | [UI02](https://github.com/K1w1b1t/watch/issues/15) |
| [CO01](https://github.com/K1w1b1t/watch/issues/17) | sdk-typescript | Demonstrar envio pela CLI usando o SDK TypeScript | [SK01](https://github.com/K1w1b1t/watch/issues/30) |
| [CO02](https://github.com/K1w1b1t/watch/issues/18) | sdk-typescript | Adicionar retry e fila limitada ao SDK TypeScript | [SK01](https://github.com/K1w1b1t/watch/issues/30) |
| [IF01](https://github.com/K1w1b1t/watch/issues/20) | infraestrutura | Reproduzir composição local do Watch com volumes isolados | [C03](https://github.com/K1w1b1t/watch/issues/72) |
| [IF02](https://github.com/K1w1b1t/watch/issues/21) | infraestrutura | Validar laboratório isolado Worker e D1 com Terraform | [IF04](https://github.com/K1w1b1t/watch/issues/69) |
| [IF03](https://github.com/K1w1b1t/watch/issues/22) | infraestrutura | Preparar plano Terraform opcional para core remoto | [IF04](https://github.com/K1w1b1t/watch/issues/69) |
| [DS01](https://github.com/K1w1b1t/watch/issues/23) | devsecops | Executar testes existentes e scanner de segredos em PRs | Nenhum |
| [DS02](https://github.com/K1w1b1t/watch/issues/24) | devsecops | Adicionar SCA e análise dos artefatos presentes | [DS01](https://github.com/K1w1b1t/watch/issues/23) |
| [DS03](https://github.com/K1w1b1t/watch/issues/25) | devsecops | Validar fluxo local do SDK até triagem do alerta | [IN03](https://github.com/K1w1b1t/watch/issues/4), [ID02](https://github.com/K1w1b1t/watch/issues/6), [DE03](https://github.com/K1w1b1t/watch/issues/13), [UI02](https://github.com/K1w1b1t/watch/issues/15), [CO01](https://github.com/K1w1b1t/watch/issues/17), [IF01](https://github.com/K1w1b1t/watch/issues/20), [ST04](https://github.com/K1w1b1t/watch/issues/61) |
| [C02](https://github.com/K1w1b1t/watch/issues/26) | contratos | Formalizar eventos 1.1 emitidos por SDKs | [C01](https://github.com/K1w1b1t/watch/issues/1) |
| [RG01](https://github.com/K1w1b1t/watch/issues/27) | fontes | Cadastrar consultar e desabilitar sources de SDK | [ID03](https://github.com/K1w1b1t/watch/issues/7), [C03](https://github.com/K1w1b1t/watch/issues/72) |
| [RG03](https://github.com/K1w1b1t/watch/issues/29) | fontes | Mostrar último contato e cobertura de sources de SDK | [RG01](https://github.com/K1w1b1t/watch/issues/27), [IN04](https://github.com/K1w1b1t/watch/issues/74) |
| [SK01](https://github.com/K1w1b1t/watch/issues/30) | sdk-typescript | Enviar eventos v1 com SDK TypeScript server-side | [C01](https://github.com/K1w1b1t/watch/issues/1) |
| [SK02](https://github.com/K1w1b1t/watch/issues/31) | sdk-typescript | Demonstrar SDK numa rota Next.js server-side | [SK01](https://github.com/K1w1b1t/watch/issues/30) |
| [SK03](https://github.com/K1w1b1t/watch/issues/32) | sdk-typescript | Demonstrar acesso negado NestJS pelo SDK | [SK07](https://github.com/K1w1b1t/watch/issues/75) |
| [SK04](https://github.com/K1w1b1t/watch/issues/33) | sdk-typescript | Demonstrar sinal React via backend e SDK server-side | [SK07](https://github.com/K1w1b1t/watch/issues/75) |
| [SK05](https://github.com/K1w1b1t/watch/issues/34) | sdk-python | Enviar eventos v1 com SDK Python | [C01](https://github.com/K1w1b1t/watch/issues/1) |
| [SK06](https://github.com/K1w1b1t/watch/issues/35) | sdk-go | Enviar eventos v1 com SDK Go | [C01](https://github.com/K1w1b1t/watch/issues/1) |
| [NT01](https://github.com/K1w1b1t/watch/issues/55) | notificacoes | Formatar e enviar alerta resumido ao Discord | [C01](https://github.com/K1w1b1t/watch/issues/1) |
| [NT02](https://github.com/K1w1b1t/watch/issues/56) | notificacoes | Integrar AlertStore à outbox durável de notificações | [NT01](https://github.com/K1w1b1t/watch/issues/55), [ST03](https://github.com/K1w1b1t/watch/issues/10) |
| [NT03](https://github.com/K1w1b1t/watch/issues/57) | notificacoes | Rotear notificações por tenant com cooldown | [NT02](https://github.com/K1w1b1t/watch/issues/56) |
| [OB01](https://github.com/K1w1b1t/watch/issues/58) | observabilidade | Exportar alertas saneados do Watch para Loki | [ST03](https://github.com/K1w1b1t/watch/issues/10) |
| [OB02](https://github.com/K1w1b1t/watch/issues/59) | observabilidade | Provisionar dashboard Grafana para Loki local | [OB01](https://github.com/K1w1b1t/watch/issues/58) |
| [OB03](https://github.com/K1w1b1t/watch/issues/60) | observabilidade | Instrumentar métricas locais de falhas e entrega | [IN03](https://github.com/K1w1b1t/watch/issues/4), [DE03](https://github.com/K1w1b1t/watch/issues/13), [NT02](https://github.com/K1w1b1t/watch/issues/56) |
| [ST04](https://github.com/K1w1b1t/watch/issues/61) | armazenamento | Preservar snapshots mínimos no caminho de criação de alerta | [ST02](https://github.com/K1w1b1t/watch/issues/9), [ST03](https://github.com/K1w1b1t/watch/issues/10) |
| [DE04](https://github.com/K1w1b1t/watch/issues/62) | deteccao | Carregar catálogo fechado de regras versionadas | [DE02](https://github.com/K1w1b1t/watch/issues/12) |
| [DE05](https://github.com/K1w1b1t/watch/issues/63) | deteccao | Persistir estado temporal atômico entre processos locais | [DE03](https://github.com/K1w1b1t/watch/issues/13) |
| [DE06](https://github.com/K1w1b1t/watch/issues/64) | deteccao | Correlacionar falhas e sucesso entre sources de SDK | [DE03](https://github.com/K1w1b1t/watch/issues/13), [RG04](https://github.com/K1w1b1t/watch/issues/76) |
| [DE07](https://github.com/K1w1b1t/watch/issues/65) | deteccao | Calcular score explicável por regras locais | [DE04](https://github.com/K1w1b1t/watch/issues/62) |
| [DE08](https://github.com/K1w1b1t/watch/issues/66) | deteccao | Importar seleção Sigma para template de igualdade | [DE04](https://github.com/K1w1b1t/watch/issues/62) |
| [IF04](https://github.com/K1w1b1t/watch/issues/69) | infraestrutura | Decidir topologia remota opcional e gate de custo | [C03](https://github.com/K1w1b1t/watch/issues/72) |
| [DS04](https://github.com/K1w1b1t/watch/issues/71) | devsecops | Validar múltiplos SDKs até Discord fake e Grafana locais | [IN03](https://github.com/K1w1b1t/watch/issues/4), [ID02](https://github.com/K1w1b1t/watch/issues/6), [DE03](https://github.com/K1w1b1t/watch/issues/13), [IF01](https://github.com/K1w1b1t/watch/issues/20), [RG03](https://github.com/K1w1b1t/watch/issues/29), [SK05](https://github.com/K1w1b1t/watch/issues/34), [SK06](https://github.com/K1w1b1t/watch/issues/35), [NT03](https://github.com/K1w1b1t/watch/issues/57), [OB02](https://github.com/K1w1b1t/watch/issues/59), [ST04](https://github.com/K1w1b1t/watch/issues/61), [DE05](https://github.com/K1w1b1t/watch/issues/63), [DE06](https://github.com/K1w1b1t/watch/issues/64), [SK07](https://github.com/K1w1b1t/watch/issues/75) |
| [C03](https://github.com/K1w1b1t/watch/issues/72) | contratos | Definir topologia local e contratos HTTP entre ingresso e core | [C01](https://github.com/K1w1b1t/watch/issues/1) |
| [ST05](https://github.com/K1w1b1t/watch/issues/73) | armazenamento | Expor API autorizada de alertas e triagem local | [ST03](https://github.com/K1w1b1t/watch/issues/10), [ID03](https://github.com/K1w1b1t/watch/issues/7), [C03](https://github.com/K1w1b1t/watch/issues/72) |
| [IN04](https://github.com/K1w1b1t/watch/issues/74) | ingestao | Aceitar contrato 1.1 no ingresso e no armazenamento | [C02](https://github.com/K1w1b1t/watch/issues/26), [IN03](https://github.com/K1w1b1t/watch/issues/4) |
| [SK07](https://github.com/K1w1b1t/watch/issues/75) | sdk-typescript | Enviar eventos 1.1 pelo SDK TypeScript | [IN04](https://github.com/K1w1b1t/watch/issues/74), [SK01](https://github.com/K1w1b1t/watch/issues/30) |
| [RG04](https://github.com/K1w1b1t/watch/issues/76) | fontes | Configurar vínculos autorizados entre sources de SDK | [RG01](https://github.com/K1w1b1t/watch/issues/27) |

## Marcos por capacidade

- **Contrato compartilhado:** C01 e C03; evolução 1.1 em C02, IN04 e SK07.
- **Fluxo básico demonstrável:** DS03, SDK TS→ingresso→core→detector medium→API→painel; inclui revogação, replay e evidência mínima.
- **Fluxo ampliado demonstrável:** DS04, SDKs Python/Go com fontes vinculadas→high→Discord fake e Loki/Grafana locais; heartbeat TS para cobertura.
- **Remoto opcional:** IF04 decide; IF02 faz smoke isolado; IF03 prepara plano sem apply. Não são pré-requisitos para concluir o laboratório local.

Cada módulo atualiza testes/CI relevantes e documentação na própria PR. Não inserir metas de piloto/aplicação real, onboarding empresarial, casos, tuning, backup/restore integrados ou capacidade/latência empresarial. Diagnóstico local OB03 mantém apenas o escopo técnico já previsto.

## Issues adiadas

Encerradas como `not_planned`, sem implementação. A descrição histórica é preservada abaixo do aviso de adiamento no GitHub. Não são dependências de nenhuma issue ativa; reabrir exige decisão explícita de escopo.

| Issue | Motivo |
| --- | --- |
| [[CO03] Observar porta local Linux sem privilégios elevados](https://github.com/K1w1b1t/watch/issues/19) | Agente/sensor fora do recorte SDK |
| [[RG02] Parear um dispositivo com código temporário de uso único](https://github.com/K1w1b1t/watch/issues/28) | Pareamento adiado; SDK recebe chave por configuração server-side |
| [[CV01] Normalizar eventos instrumentados de projetos Vercel](https://github.com/K1w1b1t/watch/issues/36) | Adaptador de plataforma adiado |
| [[CS01] Converter resultado Supabase Auth observado no servidor](https://github.com/K1w1b1t/watch/issues/37) | Adaptador de plataforma adiado |
| [[CS02] Auditar mudança de uma tabela de laboratório sem exportar linhas](https://github.com/K1w1b1t/watch/issues/38) | Adaptador de plataforma adiado |
| [[CU01] Instrumentar falha Redis sem coletar comandos ou valores](https://github.com/K1w1b1t/watch/issues/39) | Adaptador de plataforma adiado |
| [[CU02] Consultar estatísticas Upstash com polling limitado](https://github.com/K1w1b1t/watch/issues/40) | Adaptador de plataforma adiado |
| [[CP01] Receber destination webhook PostHog com allowlist](https://github.com/K1w1b1t/watch/issues/41) | Adaptador de plataforma adiado |
| [[OC01] Instrumentar falhas de operações de bucket em app de exemplo](https://github.com/K1w1b1t/watch/issues/42) | Adaptador de plataforma adiado |
| [[DT01] Criar app Tauri Linux com envio sintético explícito](https://github.com/K1w1b1t/watch/issues/43) | Agente/sensor fora do recorte SDK |
| [[DT02] Parear desktop e guardar credencial fora da camada web](https://github.com/K1w1b1t/watch/issues/44) | Agente/sensor fora do recorte SDK |
| [[DT03] Acrescentar sensor de portas Linux ao app Tauri](https://github.com/K1w1b1t/watch/issues/45) | Agente/sensor fora do recorte SDK |
| [[DT04] Adicionar adaptador de portas Windows com fixture e teste nativo](https://github.com/K1w1b1t/watch/issues/46) | Agente/sensor fora do recorte SDK |
| [[DT05] Adicionar adaptador de portas macOS com fixture e teste nativo](https://github.com/K1w1b1t/watch/issues/47) | Agente/sensor fora do recorte SDK |
| [[DT06] Pausar coleta e remover configuração de uma instalação](https://github.com/K1w1b1t/watch/issues/48) | Agente/sensor fora do recorte SDK |
| [[MB01] Criar app Android instalável com envio em primeiro plano](https://github.com/K1w1b1t/watch/issues/49) | Agente/sensor fora do recorte SDK |
| [[MB02] Parear instalação mobile usando SecureStore](https://github.com/K1w1b1t/watch/issues/50) | Agente/sensor fora do recorte SDK |
| [[MB03] Enviar postura limitada com resultado desconhecido explícito](https://github.com/K1w1b1t/watch/issues/51) | Agente/sensor fora do recorte SDK |
| [[MB04] Enviar heartbeat em background oportunista com pausa](https://github.com/K1w1b1t/watch/issues/52) | Agente/sensor fora do recorte SDK |
| [[MB05] Validar build iOS e matriz de sinais disponíveis](https://github.com/K1w1b1t/watch/issues/53) | Agente/sensor fora do recorte SDK |
| [[MB06] Consultar alertas pelo app com identidade humana](https://github.com/K1w1b1t/watch/issues/54) | Agente/sensor fora do recorte SDK |
| [[AC01] Documentar sinais realmente disponíveis de contas e provedores](https://github.com/K1w1b1t/watch/issues/67) | Pesquisa/importação de contas externas adiada |
| [[AC02] Normalizar um sinal de segurança de conta em laboratório](https://github.com/K1w1b1t/watch/issues/68) | Pesquisa/importação de contas externas adiada |
| [[IF05] Empacotar release de laboratório de um app com checksums](https://github.com/K1w1b1t/watch/issues/70) | Release de apps adiado |

## Histórico da revisão

CO01/CO02 mantêm códigos/números históricos, mas agora pertencem ao módulo SDK TypeScript: CLI que importa SDK e retry/fila do próprio SDK. ST03 foi reduzida à persistência e ST05 separa API humana. C03, IN04, SK07 e RG04 fecham transporte, integração 1.1 e vínculos. Regras demo/básica/ampliada têm ativação explícita. A revisão inicial é histórica e não adiciona requisitos empresariais ao backlog vigente.
