# De onde vêm os eventos e o que eles permitem observar

**Use este documento para:** decidir onde chamar o SDK e entender o que o Watch consegue saber a partir desse envio.

## A aplicação precisa contar o que aconteceu

Instalar um SDK não dá ao Watch visão automática do servidor ou da hospedagem. É o código da aplicação que chama a biblioteca nos pontos escolhidos.

Exemplo: uma rota Next de login chama o SDK depois de calcular sucesso/falha. O Watch conhece esse resultado. Uma tentativa que não passa por essa instrumentação fica fora dessa observação.

## Exemplos que vamos implementar

| Aplicação | Onde a chave fica e como envia | O que a issue demonstra |
| --- | --- | --- |
| Node/TypeScript | Configuração do servidor → SDK TS | SK01: envio explícito de autenticação v1 |
| Next.js | Código da rota no servidor → SDK TS | SK02: resultado de login sem senha/cookie/URL |
| NestJS | Filtro/interceptor no servidor → SDK TS 1.1 | SK03: uma negação de ação de catálogo |
| React | Navegador → próprio backend com sessão/consentimento → SDK TS | SK04: sinal limitado e classificado como relato do cliente |
| Python | Configuração do servidor → SDK Python | SK05: mesmo formato de autenticação v1 |
| Go | Configuração do servidor → SDK Go | SK06: v1 com cancelamento de envio |
| Programa de terminal | Configuração local → importa SDK TS | CO01: demonstração sem criar outro transporte |

CO02 acrescenta fila/reenvio opcionais ao SDK TS. SK07 adiciona os tipos 1.1 ao SDK TS; Python e Go ficam em v1 nesta fase.

Next/Nest/React são exemplos de aplicações, não conectores de hospedagem. Não vamos chamar APIs de Vercel, Supabase, Upstash, PostHog ou Oracle para buscar dados nesta fase.

## Tipo de evento não é a mesma coisa que detecção pronta

| Sinal recebido | O que podemos afirmar |
| --- | --- |
| auth.failure | A aplicação relatou uma falha; DE02 procura a sequência combinada |
| auth.success | A aplicação relatou sucesso; DE06 pode correlacioná-lo com falhas de fontes vinculadas |
| source.heartbeat | A aplicação chamou o SDK para avisar contato; não é diagnóstico de segurança |
| app.access_denied | A aplicação relatou negação de uma ação de catálogo; não há regra obrigatória para todo evento desse tipo |
| analytics.signal | O backend encaminhou relato consentido do cliente; continua client_reported |

O servidor pode guardar um tipo que nenhuma regra usa. Isso não significa que ele foi classificado como seguro.

## Vincular duas aplicações exige acordo

No exercício Python/Go, os dois exemplos usam uma pessoa fictícia e uma convenção de IDs comuns. RG04 registra o vínculo das fontes na mesma organização. Só então DE06 pode combinar a sequência.

Ter a mesma string subject_id, email ou IP não é autorização para juntar pessoas. Nos exercícios, não há dados reais a descobrir ou deduzir.

## Para onde vão os resultados

O painel consulta a API autorizada do Watch. Discord recebe resumo do alerta; Loki/Grafana recebem uma cópia saneada para consulta/gráfico. Esses são destinos de saída, não novas entradas de eventos.

A evidência tem prazo de retenção. Depois de removida, o detalhe informa sua expiração. Não é possível prometer investigação completa sobre dados que já não existem.

## O que está fora da observação desta fase

Não há sensores de sistema operacional, apps Android/iOS, coleta de processos, monitoramento de SMS/localização, consulta de contas externas ou captura de todos os logs. A implementação usa exemplos dentro deste repositório, sem mudar aplicações/contas de outros projetos.
