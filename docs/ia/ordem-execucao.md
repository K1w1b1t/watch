# Ordem de execução

O número da issue e o código do módulo não indicam prioridade. A sequência abaixo deriva dos bloqueios do [backlog](backlog.md), com dois resultados verificáveis: primeiro DS03, depois DS04.

## Por onde começar

Enquanto nenhuma issue estiver concluída, **C01 #1 e DS01 #23 são as únicas sem pré-requisitos**. C01 libera os contratos usados pelo restante; DS01 prepara CI em paralelo. Depois de C01, priorizar C03 #72 para liberar identidade, armazenamento e composição local; SDK, ingresso, regra pura e painel com fixtures podem seguir em paralelo.

Para escolher trabalho de integração, procurar uma issue sem bloqueios abertos. Protótipos com fakes podem começar antes, mas não encerram integração. Não é preciso esperar todo o quadro nem todo um módulo: bastam os pré-requisitos individuais da issue.

## Entrega 1 — SDK até alerta no painel

São 23 issues, incluindo DS03. Esta é a prioridade para demonstrar o caminho completo. Cada rodada agrupa tarefas independentes entre si; as rodadas não são sprints, datas ou responsáveis. A liberação segue as dependências individuais, sem obrigação de esperar todas as tarefas da rodada anterior.

| Rodada | Issues que podem executar em paralelo | Resultado |
| --- | --- | --- |
| 1 | [C01 #1](https://github.com/K1w1b1t/watch/issues/1) | Contrato compartilhado v1 |
| 2 | [IN01 #2](https://github.com/K1w1b1t/watch/issues/2), [DE01 #11](https://github.com/K1w1b1t/watch/issues/11), [UI01 #14](https://github.com/K1w1b1t/watch/issues/14), [SK01 #30](https://github.com/K1w1b1t/watch/issues/30), [C03 #72](https://github.com/K1w1b1t/watch/issues/72) | Ingresso/SDK/regra/UI isolados e transporte entre processos |
| 3 | [IN02 #3](https://github.com/K1w1b1t/watch/issues/3), [ID01 #5](https://github.com/K1w1b1t/watch/issues/5), [ID03 #7](https://github.com/K1w1b1t/watch/issues/7), [ST01 #8](https://github.com/K1w1b1t/watch/issues/8), [DE02 #12](https://github.com/K1w1b1t/watch/issues/12), [CO01 #17](https://github.com/K1w1b1t/watch/issues/17), [IF01 #20](https://github.com/K1w1b1t/watch/issues/20) | Autorização, persistência, limiar e composição local |
| 4 | [ID02 #6](https://github.com/K1w1b1t/watch/issues/6), [ST02 #9](https://github.com/K1w1b1t/watch/issues/9), [ST03 #10](https://github.com/K1w1b1t/watch/issues/10), [RG01 #27](https://github.com/K1w1b1t/watch/issues/27) | Revogação, retenção, alertas e sources |
| 5 | [IN03 #4](https://github.com/K1w1b1t/watch/issues/4), [DE03 #13](https://github.com/K1w1b1t/watch/issues/13), [ST04 #61](https://github.com/K1w1b1t/watch/issues/61), [ST05 #73](https://github.com/K1w1b1t/watch/issues/73) | Ingresso persistente, executor, evidências e API humana |
| 6 | [UI02 #15](https://github.com/K1w1b1t/watch/issues/15) | Painel integrado com reconhecimento/resolução |
| 7 | [DS03 #25](https://github.com/K1w1b1t/watch/issues/25) | Demonstração completa do fluxo básico |

**Fim da entrega:** DS03 demonstra SDK TS→ingresso→core→detector→API→painel, com um alerta medium após cinco falhas, isolamento, replay, revogação e evidência. Discord e Grafana não bloqueiam esse resultado.

## Entrega 2 — múltiplos SDKs e saídas

São mais 15 issues, incluindo DS04, considerando a entrega 1 concluída. Python/Go, contratos 1.1 e formato Discord podem começar antes assim que seus próprios pré-requisitos forem atendidos; a prioridade recomendada continua sendo fechar DS03 primeiro.

| Rodada dentro desta entrega | Issues que podem executar em paralelo | Resultado |
| --- | --- | --- |
| 1 | [C02 #26](https://github.com/K1w1b1t/watch/issues/26), [SK05 #34](https://github.com/K1w1b1t/watch/issues/34), [SK06 #35](https://github.com/K1w1b1t/watch/issues/35), [NT01 #55](https://github.com/K1w1b1t/watch/issues/55), [OB01 #58](https://github.com/K1w1b1t/watch/issues/58), [DE05 #63](https://github.com/K1w1b1t/watch/issues/63), [RG04 #76](https://github.com/K1w1b1t/watch/issues/76) | Versão 1.1, SDKs Python/Go, saídas e estado/vínculos locais |
| 2 | [NT02 #56](https://github.com/K1w1b1t/watch/issues/56), [OB02 #59](https://github.com/K1w1b1t/watch/issues/59), [DE06 #64](https://github.com/K1w1b1t/watch/issues/64), [IN04 #74](https://github.com/K1w1b1t/watch/issues/74) | Outbox, dashboard, correlação e ingresso 1.1 |
| 3 | [RG03 #29](https://github.com/K1w1b1t/watch/issues/29), [NT03 #57](https://github.com/K1w1b1t/watch/issues/57), [SK07 #75](https://github.com/K1w1b1t/watch/issues/75) | Cobertura de sources, roteamento e SDK TS 1.1 |
| 4 | [DS04 #71](https://github.com/K1w1b1t/watch/issues/71) | Demonstração ampliada até Discord fake e Grafana local |

**Fim da entrega:** DS04 demonstra cinco falhas pelo SDK Python e sucesso pelo SDK Go, sources vinculadas, um high com evidências, Discord fake e Grafana local. SDK TS envia heartbeat 1.1 para o exercício de cobertura.

## Complementos e apoio

Estas 14 issues continuam no escopo, mas não são bloqueios dos dois roteiros acima. Não precisam virar uma fila obrigatória antes de DS03/DS04. DS01 começa em paralelo com C01; o restante entra quando for útil e seus pré-requisitos estiverem concluídos. Não introduzir requisitos de piloto empresarial.

| Issue | Pode concluir depois de | Utilidade |
| --- | --- | --- |
| [UI03 #16](https://github.com/K1w1b1t/watch/issues/16) | [UI02 #15](https://github.com/K1w1b1t/watch/issues/15) | Atualizar alertas por Phoenix Channel autenticado |
| [CO02 #18](https://github.com/K1w1b1t/watch/issues/18) | [SK01 #30](https://github.com/K1w1b1t/watch/issues/30) | Adicionar retry e fila limitada ao SDK TypeScript |
| [IF02 #21](https://github.com/K1w1b1t/watch/issues/21) | [IF04 #69](https://github.com/K1w1b1t/watch/issues/69) | Validar laboratório isolado Worker e D1 com Terraform |
| [IF03 #22](https://github.com/K1w1b1t/watch/issues/22) | [IF04 #69](https://github.com/K1w1b1t/watch/issues/69) | Preparar plano Terraform opcional para core remoto |
| [DS01 #23](https://github.com/K1w1b1t/watch/issues/23) | Nenhum pré-requisito | Executar testes existentes e scanner de segredos em PRs |
| [DS02 #24](https://github.com/K1w1b1t/watch/issues/24) | [DS01 #23](https://github.com/K1w1b1t/watch/issues/23) | Adicionar SCA e análise dos artefatos presentes |
| [SK02 #31](https://github.com/K1w1b1t/watch/issues/31) | [SK01 #30](https://github.com/K1w1b1t/watch/issues/30) | Demonstrar SDK numa rota Next.js server-side |
| [SK03 #32](https://github.com/K1w1b1t/watch/issues/32) | [SK07 #75](https://github.com/K1w1b1t/watch/issues/75) | Demonstrar acesso negado NestJS pelo SDK |
| [SK04 #33](https://github.com/K1w1b1t/watch/issues/33) | [SK07 #75](https://github.com/K1w1b1t/watch/issues/75) | Demonstrar sinal React via backend e SDK server-side |
| [OB03 #60](https://github.com/K1w1b1t/watch/issues/60) | [IN03 #4](https://github.com/K1w1b1t/watch/issues/4), [DE03 #13](https://github.com/K1w1b1t/watch/issues/13), [NT02 #56](https://github.com/K1w1b1t/watch/issues/56) | Instrumentar métricas locais de falhas e entrega |
| [DE04 #62](https://github.com/K1w1b1t/watch/issues/62) | [DE02 #12](https://github.com/K1w1b1t/watch/issues/12) | Carregar catálogo fechado de regras versionadas |
| [DE07 #65](https://github.com/K1w1b1t/watch/issues/65) | [DE04 #62](https://github.com/K1w1b1t/watch/issues/62) | Calcular score explicável por regras locais |
| [DE08 #66](https://github.com/K1w1b1t/watch/issues/66) | [DE04 #62](https://github.com/K1w1b1t/watch/issues/62) | Importar seleção Sigma para template de igualdade |
| [IF04 #69](https://github.com/K1w1b1t/watch/issues/69) | [C03 #72](https://github.com/K1w1b1t/watch/issues/72) | Decidir topologia remota opcional e gate de custo |

IF04/IF02/IF03 são experimentos remotos opcionais dentro do escopo. Não é necessário provisionar cloud para demonstrar o laboratório local. DS02 aplica scanners apenas aos artefatos existentes; cada módulo mantém seus testes/CI na própria entrega.

## Leitura recomendada no Kanban

O Status acompanha execução; a entrega e a rodada explicam prioridade e dependências. Uma issue de rodada maior não está necessariamente em andamento ou pronta.

| Informação | Uso no quadro |
| --- | --- |
| Entrega | `1 - Fluxo básico`, `2 - Fluxo ampliado` ou `Apoio e complementos` |
| Rodada | Número da rodada dentro da entrega; complementos seguem os pré-requisitos da tabela |
| Status | A fazer, Em andamento e Concluído, preservando os nomes usados pelo time |
| Bloqueios | Conferir os pré-requisitos nativos antes de assumir integração; não usar número da issue como ordem |

Na visão diária, filtrar o fluxo básico primeiro e ordenar por rodada. Na visão geral, agrupar por entrega e mostrar rodada, módulo e status nos cartões. Quando DS03 concluir, usar o mesmo procedimento no fluxo ampliado. Essa é a configuração recomendada; alterações efetivamente aplicadas ao Projects dependem do acesso ao quadro.
