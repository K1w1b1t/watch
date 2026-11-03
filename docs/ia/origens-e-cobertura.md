# Origens e cobertura vigente

| Origem | Caminho ao ingresso | Limite |
| --- | --- | --- |
| Aplicação Node/TypeScript | SDK TS SK01; 1.1 em SK07 | Só chamadas explícitas |
| Next.js de exemplo | SDK TS no servidor SK02 | Não é adaptador Vercel |
| NestJS de exemplo | SDK TS no servidor SK03 | Acesso negado instrumentado, sem coleta geral |
| React de exemplo | Backend próprio autenticado/consentido→SDK TS SK04 | Browser não possui chave nem chama Watch diretamente |
| Python | SDK SK05 v1 | Sem retry/fila/1.1 obrigatórios |
| Go | SDK SK06 v1 | Context cancelável; sem 1.1 obrigatório |
| CLI sintética | Importa SDK TS CO01 | Sem sensor ou transporte independente |

CO02 adiciona retry/fila opt-in ao SDK TS. SDKs operam com qualquer hospedagem compatível; o Watch não chama APIs de plataforma para buscar eventos. Sem instrumentação, visibilidade é ausente. Kinds sem regra são persistidos, sem alegação de detecção.

## Saídas

Painel React consulta API autorizada. Discord recebe resumo saneado (fake nos E2Es). Loki/Grafana locais são opcionais, com stacks/credenciais separadas para isolamento. São consumidores, não caminhos alternativos de ingestão.

## Adiados

Conectores/adaptadores Vercel, Supabase, Upstash, PostHog e Oracle; polling/webhooks de plataformas; pesquisa/importação de contas; agentes/sensores Linux/Windows/macOS; apps Android/iOS; pareamento e release de apps. As [issues adiadas](backlog.md#issues-adiadas) preservam o histórico, encerradas como not planned. Nenhum componente ativo depende delas.

## Laboratório e privacidade

Usar aplicações de exemplo no Watch, sem modificar projetos/contas existentes. Subjects e sources são artificiais; uma pseudonimização futura exige política explícita e não torna dados automaticamente anônimos. Tenant é derivado de credencial/membership no servidor. Source disabled, chave revogada e ausência de vínculo impedem os caminhos correspondentes.

O [contrato](contratos.md) define retenção e evidências mínimas. Este planejamento não inclui validação operacional em empresa; não existe promessa de monitoramento de todos os eventos, EDR, investigação completa ou detecção infalível.
