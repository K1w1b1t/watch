# Sugestão de execução para Gustavo e João

Esta é uma divisão inicial para **Gustavo** e **João (`reaperkoji`)** escolherem trabalho complementar. Não pressupõe especialidade de nenhum dos dois, não fixa responsáveis, não altera assignees e não cria prazos. Podem trocar uma issue: avisem quem vai assumi-la e mantenham os pontos de integração combinados.

Gustavo acompanha a entrada dos eventos: contratos, credenciais/sources, ingresso e SDK TS. João acompanha o caminho dos dados: persistência, regras, API de alertas e composição local. O painel fecha a integração entre as duas frentes. Os dois revisam as interfaces que o outro vai consumir.

## O que pegar primeiro

- **Gustavo:** [C01 #1](https://github.com/K1w1b1t/watch/issues/1), contratos e fixtures compartilhadas.
- **João:** [DS01 #23](https://github.com/K1w1b1t/watch/issues/23), CI dos artefatos existentes. Enquanto não houver módulo executável, não declarar testes de aplicação como realizados.
- **Depois de C01:** Gustavo pega [C03 #72](https://github.com/K1w1b1t/watch/issues/72), transporte local; João pega [DE01 #11](https://github.com/K1w1b1t/watch/issues/11), regra pura usando as mesmas fixtures.
- **Depois de C03:** Gustavo segue com [ID01 #5](https://github.com/K1w1b1t/watch/issues/5), chaves; João segue com [ST01 #8](https://github.com/K1w1b1t/watch/issues/8), persistência.

As 23 issues do fluxo básico estão em **Ready** no Projects por escolha do time. Nesse quadro, Ready identifica trabalho selecionado para a entrega; **não remove bloqueios técnicos**. Antes de integrar ou concluir, conferir as dependências da issue. Protótipos com fakes continuam permitidos.

## Primeiro objetivo: fluxo básico completo

Cada coluna é uma fila sugerida para a pessoa. O número indica posição de escolha, não dia ou sprint. Não esperem a linha inteira terminar: avancem quando os pré-requisitos da próxima issue da própria coluna estiverem concluídos. As duas issues da mesma linha podem ser executadas em paralelo depois das anteriores.

| Posição | Gustavo | João (`reaperkoji`) |
| --- | --- | --- |
| 1 | [C01 #1](https://github.com/K1w1b1t/watch/issues/1) — schemas e fixtures | [DS01 #23](https://github.com/K1w1b1t/watch/issues/23) — CI inicial; apoio ao fluxo |
| 2 | [C03 #72](https://github.com/K1w1b1t/watch/issues/72) — contrato HTTP e topologia | [DE01 #11](https://github.com/K1w1b1t/watch/issues/11) — regra pura didática |
| 3 | [ID01 #5](https://github.com/K1w1b1t/watch/issues/5) — emissão/verificação de chaves | [ST01 #8](https://github.com/K1w1b1t/watch/issues/8) — EventStore SQLite |
| 4 | [ID03 #7](https://github.com/K1w1b1t/watch/issues/7) — identidade humana local | [ST02 #9](https://github.com/K1w1b1t/watch/issues/9) — retenção e limites |
| 5 | [RG01 #27](https://github.com/K1w1b1t/watch/issues/27) — cadastro/status de sources | [ST03 #10](https://github.com/K1w1b1t/watch/issues/10) — AlertStore |
| 6 | [IN01 #2](https://github.com/K1w1b1t/watch/issues/2) — ingresso HTTP local | [DE02 #12](https://github.com/K1w1b1t/watch/issues/12) — limiar de cinco falhas |
| 7 | [IN02 #3](https://github.com/K1w1b1t/watch/issues/3) — autorização e rate limit | [ST04 #61](https://github.com/K1w1b1t/watch/issues/61) — snapshots de evidência |
| 8 | [IN03 #4](https://github.com/K1w1b1t/watch/issues/4) — ligar ingresso ao core real | [DE03 #13](https://github.com/K1w1b1t/watch/issues/13) — executor/checkpoint |
| 9 | [SK01 #30](https://github.com/K1w1b1t/watch/issues/30) — SDK TypeScript | [ST05 #73](https://github.com/K1w1b1t/watch/issues/73) — API humana de alertas |
| 10 | [CO01 #17](https://github.com/K1w1b1t/watch/issues/17) — CLI que usa o SDK | [UI01 #14](https://github.com/K1w1b1t/watch/issues/14) — painel com fixtures |
| 11 | [ID02 #6](https://github.com/K1w1b1t/watch/issues/6) — revogação/rotação | [IF01 #20](https://github.com/K1w1b1t/watch/issues/20) — composição local com serviços reais |
| 12 | [UI02 #15](https://github.com/K1w1b1t/watch/issues/15) — painel consumindo API real | Revisar a integração UI/API e preparar o ambiente do exercício |

**Fechamento em dupla:** [DS03 #25](https://github.com/K1w1b1t/watch/issues/25). Gustavo conduz o envio pelo SDK/CLI, autenticação, revogação e ações no painel; João verifica persistência, detector, evidência e retomada. A demonstração precisa atravessar os serviços reais locais; não basta juntar duas demonstrações isoladas.

Essa tabela cobre as **23 issues do fluxo básico**, mais DS01 de apoio. A divisão prioriza liberar as fronteiras necessárias à outra pessoa, sem exigir que todos os módulos de uma mesma rodada geral terminem juntos.

## Pontos em que precisam combinar a entrega

| Antes de | O que Gustavo prepara | O que João confere ou entrega |
| --- | --- | --- |
| Começar módulos | C01 com fixtures e C03 com requisições/respostas, erros e autenticação interna | Revisar se os contratos são consumíveis em Elixir e se SQLite pertence apenas ao core |
| Integrar IN03 | Porta de identidade/source e cliente interno do ingresso | EventStore real, idempotência e erros; usar os mesmos casos de A/B e falha |
| Integrar UI02 | Cliente do painel e uso do token humano de ID03 | API ST05 e adaptador visual UI01 com campos/estados compatíveis |
| Rodar DS03 | Eventos sintéticos e fluxo de reconhecimento/resolução/revogação | Composição IF01, perfil básico sem regra low demo e evidência preservada |

Separar migrações e arquivos de identidade/sources dos de eventos/alertas. Combinar antes se ambos precisarem alterar o bootstrap Phoenix, a composição ou o contrato compartilhado; fazer PRs pequenas e revisar mudanças de interface antes de integrar. Não criar dois servidores ou duas autoridades SQLite para contornar uma espera.

## Se uma fila ficar bloqueada

Primeiro verificar se há uma issue anterior liberada na própria fila. Se a próxima depender da outra pessoa, usar uma alternativa liberada ou ajudar a revisar o pré-requisito; não inventar um contrato novo.

- **Gustavo:** SK01 e CO01 podem ser antecipadas após C01 e SK01, respectivamente; IN01 e UI01 também podem começar com fixtures após C01. Se João ainda não pegou UI01, combinem a troca antes. Para exercício adicional, SK02 depende apenas de SK01, mas não bloqueia DS03.
- **João:** DE02 pode ser antecipada após DE01; IF01 pode começar após C03. ST02 e ST03 ficam liberadas após ST01 e não dependem uma da outra. Se ainda estiver esperando C01, revisar o desenho e trabalhar em DS01.
- Avisar na issue que ela foi assumida antes de começar. Uma PR em revisão ainda não é dependência concluída; o consumidor pode usar fake ou branch combinada para desenvolver, mas precisa validar a integração final com a versão integrada.

## Depois: fluxo ampliado

Após demonstrar DS03, a sugestão continua com duas filas. Uma issue desta tabela pode ser antecipada se seus pré-requisitos já estiverem concluídos e isso não atrasar uma entrega necessária ao fluxo básico.

| Posição | Gustavo | João (`reaperkoji`) |
| --- | --- | --- |
| 1 | [C02 #26](https://github.com/K1w1b1t/watch/issues/26) — schemas 1.1 | [SK06 #35](https://github.com/K1w1b1t/watch/issues/35) — SDK Go v1 |
| 2 | [RG04 #76](https://github.com/K1w1b1t/watch/issues/76) — vínculos de sources | [DE05 #63](https://github.com/K1w1b1t/watch/issues/63) — estado atômico local |
| 3 | [IN04 #74](https://github.com/K1w1b1t/watch/issues/74) — ingresso 1.1 | [DE06 #64](https://github.com/K1w1b1t/watch/issues/64) — correlação multifonte |
| 4 | [SK05 #34](https://github.com/K1w1b1t/watch/issues/34) — SDK Python v1 | [OB01 #58](https://github.com/K1w1b1t/watch/issues/58) — exportação Loki |
| 5 | [NT01 #55](https://github.com/K1w1b1t/watch/issues/55) — formato/envio Discord fake | [OB02 #59](https://github.com/K1w1b1t/watch/issues/59) — dashboard Grafana |
| 6 | [SK07 #75](https://github.com/K1w1b1t/watch/issues/75) — SDK TS 1.1 | [NT02 #56](https://github.com/K1w1b1t/watch/issues/56) — ponte AlertStore/outbox |
| 7 | [RG03 #29](https://github.com/K1w1b1t/watch/issues/29) — último contato/cobertura | [NT03 #57](https://github.com/K1w1b1t/watch/issues/57) — roteamento/cooldown |

**Fechamento em dupla:** [DS04 #71](https://github.com/K1w1b1t/watch/issues/71). Gustavo fornece Python/TS, heartbeat e vínculos; João fornece Go, correlação, outbox e visualização. Revisem juntos a conformidade Python/Go, o vínculo real de subjects/sources e o perfil ampliado: um high, sem os alertas low/medium dos outros perfis.

Essa divisão cobre as **15 issues adicionais** do fluxo ampliado. Antes de João integrar DE06, RG04 precisa estar concluída; antes de integrar NT02, NT01 precisa estar concluída. Gustavo integra SK07 somente após IN04, e RG03 observa apenas eventos persistidos. Não substituir essas entregas por mocks no exercício final.

## Complementos quando fizer sentido

DS01 já foi colocada no início da fila de João. As outras 13 issues de apoio/complemento podem ser escolhidas conforme a necessidade, sem bloquear DS03/DS04:

| Sugestão | Issues | Dependência ou combinação importante |
| --- | --- | --- |
| Gustavo | [CO02 #18](https://github.com/K1w1b1t/watch/issues/18), [SK02 #31](https://github.com/K1w1b1t/watch/issues/31) | Ambas após SK01; retry/fila e exemplo Next usam o mesmo SDK |
| Gustavo | [SK03 #32](https://github.com/K1w1b1t/watch/issues/32), [SK04 #33](https://github.com/K1w1b1t/watch/issues/33) | Após SK07; exemplos Nest/React sem adaptador de hospedagem |
| Gustavo | [UI03 #16](https://github.com/K1w1b1t/watch/issues/16) | Após UI02; João revisa a autorização do Channel no core |
| João | [DS02 #24](https://github.com/K1w1b1t/watch/issues/24), [OB03 #60](https://github.com/K1w1b1t/watch/issues/60) | DS02 após DS01 e artefatos existentes; OB03 após IN03, DE03 e NT02 |
| João | [DE04 #62](https://github.com/K1w1b1t/watch/issues/62) → [DE07 #65](https://github.com/K1w1b1t/watch/issues/65) / [DE08 #66](https://github.com/K1w1b1t/watch/issues/66) | DE04 após DE02; depois score e importação Sigma podem seguir independentemente |
| Gustavo | [IF04 #69](https://github.com/K1w1b1t/watch/issues/69) → [IF02 #21](https://github.com/K1w1b1t/watch/issues/21) | Opcional: topologia/custos após C03, depois smoke Worker/D1 |
| João | [IF03 #22](https://github.com/K1w1b1t/watch/issues/22) | Opcional, após IF04 de Gustavo; preparar plano, sem apply |

Essas sugestões cobrem as **52 issues existentes**, sem novas funcionalidades, contas/aplicações reais ou requisitos de piloto empresarial. A ordem geral e os bloqueios continuam em [ordem de execução](ordem-execucao.md) e no [backlog](backlog.md).
