# Backlog: encontre a tarefa pelo resultado

**Use este arquivo como índice das 52 issues.** Para escolher uma sequência, consulte a [ordem de execução](ordem-execucao.md). Para repartir entre vocês, use [trabalho em dupla](trabalho-em-dupla.md).

O código entre colchetes identifica a área; o número identifica a issue no GitHub. Nenhum dos dois, sozinho, indica prioridade.

| Código | Área |
| --- | --- |
| C | Contratos: formato e comunicação combinados |
| IN | Entrada: receber e validar pedidos |
| ID | Identidade: chaves e acesso humano |
| RG | Registro: fontes, contato e vínculos |
| ST | Armazenamento: eventos, alertas, evidências e API |
| DE | Detecção: regras e processamento |
| SK | SDK: bibliotecas de envio e exemplos de aplicação |
| CO | Demonstração por terminal e opções de envio do SDK |
| UI | Interface: painel para pessoas |
| NT | Notificação: mensagem, fila e destino |
| OB | Observação técnica: exportação, gráficos e medidas de funcionamento |
| IF | Infraestrutura: ambiente local e exercícios remotos opcionais |
| DS | Verificações: CI, segurança e demonstrações completas |

“Precisa de” lista as entregas necessárias para concluir a tarefa. É permitido começar com substitutos de teste; a integração final respeita os bloqueios reais. Não há prazo ou responsável fixo.

## Fluxo básico — 23 issues

O objetivo é fechar DS03: enviar pelo SDK TS e ver/tratar um alerta no painel.

| Issue | O que você constrói | Precisa de |
| --- | --- | --- |
| [C01 #1](https://github.com/K1w1b1t/watch/issues/1) | Definir o formato dos eventos e criar exemplos para todos os módulos | Nenhuma outra issue |
| [IN01 #2](https://github.com/K1w1b1t/watch/issues/2) | Receber um evento por HTTP e validar seus dados | [C01 #1](https://github.com/K1w1b1t/watch/issues/1) |
| [DE01 #11](https://github.com/K1w1b1t/watch/issues/11) | Criar uma primeira regra simples para aprender o caminho da detecção | [C01 #1](https://github.com/K1w1b1t/watch/issues/1) |
| [UI01 #14](https://github.com/K1w1b1t/watch/issues/14) | Montar o painel de alertas usando exemplos locais | [C01 #1](https://github.com/K1w1b1t/watch/issues/1) |
| [SK01 #30](https://github.com/K1w1b1t/watch/issues/30) | Criar o SDK TypeScript para enviar eventos de login | [C01 #1](https://github.com/K1w1b1t/watch/issues/1) |
| [C03 #72](https://github.com/K1w1b1t/watch/issues/72) | Definir como a entrada em TypeScript conversa com o servidor Elixir | [C01 #1](https://github.com/K1w1b1t/watch/issues/1) |
| [IN02 #3](https://github.com/K1w1b1t/watch/issues/3) | Controlar quem pode enviar eventos e limitar a quantidade de pedidos | [IN01 #2](https://github.com/K1w1b1t/watch/issues/2) |
| [ID01 #5](https://github.com/K1w1b1t/watch/issues/5) | Criar e verificar chaves de acesso dos SDKs | [C01 #1](https://github.com/K1w1b1t/watch/issues/1), [C03 #72](https://github.com/K1w1b1t/watch/issues/72) |
| [ID03 #7](https://github.com/K1w1b1t/watch/issues/7) | Permitir acesso de pessoas com papéis de leitura e análise | [C01 #1](https://github.com/K1w1b1t/watch/issues/1), [C03 #72](https://github.com/K1w1b1t/watch/issues/72) |
| [ST01 #8](https://github.com/K1w1b1t/watch/issues/8) | Guardar eventos no SQLite sem misturar organizações nem duplicar reenvios | [C01 #1](https://github.com/K1w1b1t/watch/issues/1), [C03 #72](https://github.com/K1w1b1t/watch/issues/72) |
| [DE02 #12](https://github.com/K1w1b1t/watch/issues/12) | Detectar cinco falhas de login em um intervalo de 60 segundos | [DE01 #11](https://github.com/K1w1b1t/watch/issues/11) |
| [CO01 #17](https://github.com/K1w1b1t/watch/issues/17) | Criar um comando de terminal para demonstrar o SDK TypeScript | [SK01 #30](https://github.com/K1w1b1t/watch/issues/30) |
| [IF01 #20](https://github.com/K1w1b1t/watch/issues/20) | Preparar um ambiente local que sobe os componentes do Watch juntos | [C03 #72](https://github.com/K1w1b1t/watch/issues/72) |
| [ID02 #6](https://github.com/K1w1b1t/watch/issues/6) | Revogar e substituir chaves de acesso | [ID01 #5](https://github.com/K1w1b1t/watch/issues/5) |
| [ST02 #9](https://github.com/K1w1b1t/watch/issues/9) | Limitar por quanto tempo e quanto volume de eventos ficam guardados | [ST01 #8](https://github.com/K1w1b1t/watch/issues/8) |
| [ST03 #10](https://github.com/K1w1b1t/watch/issues/10) | Guardar alertas e ligar cada alerta aos eventos que o explicam | [ST01 #8](https://github.com/K1w1b1t/watch/issues/8) |
| [RG01 #27](https://github.com/K1w1b1t/watch/issues/27) | Cadastrar e desativar as aplicações que podem enviar eventos | [ID03 #7](https://github.com/K1w1b1t/watch/issues/7), [C03 #72](https://github.com/K1w1b1t/watch/issues/72) |
| [IN03 #4](https://github.com/K1w1b1t/watch/issues/4) | Conectar a entrada ao servidor e ao banco reais | [IN02 #3](https://github.com/K1w1b1t/watch/issues/3), [ID01 #5](https://github.com/K1w1b1t/watch/issues/5), [ST01 #8](https://github.com/K1w1b1t/watch/issues/8), [RG01 #27](https://github.com/K1w1b1t/watch/issues/27), [C03 #72](https://github.com/K1w1b1t/watch/issues/72) |
| [DE03 #13](https://github.com/K1w1b1t/watch/issues/13) | Ler eventos do banco e retomar a análise depois de uma interrupção | [DE02 #12](https://github.com/K1w1b1t/watch/issues/12), [ST02 #9](https://github.com/K1w1b1t/watch/issues/9), [ST03 #10](https://github.com/K1w1b1t/watch/issues/10) |
| [ST04 #61](https://github.com/K1w1b1t/watch/issues/61) | Preservar os eventos que explicam um alerta mesmo após a limpeza do buffer | [ST02 #9](https://github.com/K1w1b1t/watch/issues/9), [ST03 #10](https://github.com/K1w1b1t/watch/issues/10) |
| [ST05 #73](https://github.com/K1w1b1t/watch/issues/73) | Criar a API que o painel usa para consultar e tratar alertas | [ST03 #10](https://github.com/K1w1b1t/watch/issues/10), [ID03 #7](https://github.com/K1w1b1t/watch/issues/7), [C03 #72](https://github.com/K1w1b1t/watch/issues/72) |
| [UI02 #15](https://github.com/K1w1b1t/watch/issues/15) | Ligar o painel à API e permitir reconhecer e resolver alertas | [UI01 #14](https://github.com/K1w1b1t/watch/issues/14), [ST05 #73](https://github.com/K1w1b1t/watch/issues/73) |
| [DS03 #25](https://github.com/K1w1b1t/watch/issues/25) | Demonstrar o caminho completo: enviar eventos e tratar o alerta no painel | [IN03 #4](https://github.com/K1w1b1t/watch/issues/4), [ID02 #6](https://github.com/K1w1b1t/watch/issues/6), [DE03 #13](https://github.com/K1w1b1t/watch/issues/13), [UI02 #15](https://github.com/K1w1b1t/watch/issues/15), [CO01 #17](https://github.com/K1w1b1t/watch/issues/17), [IF01 #20](https://github.com/K1w1b1t/watch/issues/20), [ST04 #61](https://github.com/K1w1b1t/watch/issues/61) |

## Fluxo ampliado — 15 issues

Depois do básico, fechar DS04: fontes Python/Go vinculadas, alerta high e saídas locais.

| Issue | O que você constrói | Precisa de |
| --- | --- | --- |
| [C02 #26](https://github.com/K1w1b1t/watch/issues/26) | Definir três novos tipos de evento para a versão 1.1 | [C01 #1](https://github.com/K1w1b1t/watch/issues/1) |
| [SK05 #34](https://github.com/K1w1b1t/watch/issues/34) | Criar o SDK Python para o mesmo contrato de login | [C01 #1](https://github.com/K1w1b1t/watch/issues/1) |
| [SK06 #35](https://github.com/K1w1b1t/watch/issues/35) | Criar o SDK Go com envio cancelável | [C01 #1](https://github.com/K1w1b1t/watch/issues/1) |
| [NT01 #55](https://github.com/K1w1b1t/watch/issues/55) | Preparar uma mensagem segura de alerta para o Discord | [C01 #1](https://github.com/K1w1b1t/watch/issues/1) |
| [OB01 #58](https://github.com/K1w1b1t/watch/issues/58) | Copiar resumos de alertas para consulta no Loki | [ST03 #10](https://github.com/K1w1b1t/watch/issues/10) |
| [DE05 #63](https://github.com/K1w1b1t/watch/issues/63) | Compartilhar o estado da detecção entre dois processos locais com segurança | [DE03 #13](https://github.com/K1w1b1t/watch/issues/13) |
| [RG04 #76](https://github.com/K1w1b1t/watch/issues/76) | Registrar quais aplicações podem correlacionar a mesma pessoa fictícia | [RG01 #27](https://github.com/K1w1b1t/watch/issues/27) |
| [NT02 #56](https://github.com/K1w1b1t/watch/issues/56) | Guardar notificações pendentes e retomar o envio após falhas | [NT01 #55](https://github.com/K1w1b1t/watch/issues/55), [ST03 #10](https://github.com/K1w1b1t/watch/issues/10) |
| [OB02 #59](https://github.com/K1w1b1t/watch/issues/59) | Montar um painel Grafana para os alertas enviados ao Loki | [OB01 #58](https://github.com/K1w1b1t/watch/issues/58) |
| [DE06 #64](https://github.com/K1w1b1t/watch/issues/64) | Detectar falhas seguidas de sucesso entre aplicações vinculadas | [DE03 #13](https://github.com/K1w1b1t/watch/issues/13), [RG04 #76](https://github.com/K1w1b1t/watch/issues/76) |
| [IN04 #74](https://github.com/K1w1b1t/watch/issues/74) | Fazer o servidor aceitar eventos 1.1 sem quebrar a versão 1 | [C02 #26](https://github.com/K1w1b1t/watch/issues/26), [IN03 #4](https://github.com/K1w1b1t/watch/issues/4) |
| [RG03 #29](https://github.com/K1w1b1t/watch/issues/29) | Mostrar quando uma fonte enviou dados pela última vez | [RG01 #27](https://github.com/K1w1b1t/watch/issues/27), [IN04 #74](https://github.com/K1w1b1t/watch/issues/74) |
| [NT03 #57](https://github.com/K1w1b1t/watch/issues/57) | Enviar cada alerta ao destino certo e reduzir avisos repetidos | [NT02 #56](https://github.com/K1w1b1t/watch/issues/56) |
| [SK07 #75](https://github.com/K1w1b1t/watch/issues/75) | Adicionar os eventos 1.1 ao SDK TypeScript e testar com o Watch real | [IN04 #74](https://github.com/K1w1b1t/watch/issues/74), [SK01 #30](https://github.com/K1w1b1t/watch/issues/30) |
| [DS04 #71](https://github.com/K1w1b1t/watch/issues/71) | Demonstrar SDKs diferentes gerando um alerta com notificação e gráfico | [IN03 #4](https://github.com/K1w1b1t/watch/issues/4), [ID02 #6](https://github.com/K1w1b1t/watch/issues/6), [DE03 #13](https://github.com/K1w1b1t/watch/issues/13), [IF01 #20](https://github.com/K1w1b1t/watch/issues/20), [RG03 #29](https://github.com/K1w1b1t/watch/issues/29), [SK05 #34](https://github.com/K1w1b1t/watch/issues/34), [SK06 #35](https://github.com/K1w1b1t/watch/issues/35), [NT03 #57](https://github.com/K1w1b1t/watch/issues/57), [OB02 #59](https://github.com/K1w1b1t/watch/issues/59), [ST04 #61](https://github.com/K1w1b1t/watch/issues/61), [DE05 #63](https://github.com/K1w1b1t/watch/issues/63), [DE06 #64](https://github.com/K1w1b1t/watch/issues/64), [SK07 #75](https://github.com/K1w1b1t/watch/issues/75) |

## Apoio e complementos — 14 issues

Estas tarefas ajudam o trabalho ou acrescentam exemplos/recursos, mas não são pré-requisitos dos roteiros DS03/DS04. DS01 pode começar em paralelo a C01.

| Issue | O que você constrói | Precisa de |
| --- | --- | --- |
| [UI03 #16](https://github.com/K1w1b1t/watch/issues/16) | Atualizar o painel quando um alerta mudar e recuperar após desconexão | [UI02 #15](https://github.com/K1w1b1t/watch/issues/15) |
| [CO02 #18](https://github.com/K1w1b1t/watch/issues/18) | Adicionar ao SDK TypeScript uma opção limitada de fila e reenvio | [SK01 #30](https://github.com/K1w1b1t/watch/issues/30) |
| [DS01 #23](https://github.com/K1w1b1t/watch/issues/23) | Fazer o GitHub executar testes e procurar segredos nas alterações | Nenhuma outra issue |
| [SK02 #31](https://github.com/K1w1b1t/watch/issues/31) | Mostrar como usar o SDK em uma rota Next.js | [SK01 #30](https://github.com/K1w1b1t/watch/issues/30) |
| [SK03 #32](https://github.com/K1w1b1t/watch/issues/32) | Mostrar como registrar um acesso negado em uma aplicação NestJS | [SK07 #75](https://github.com/K1w1b1t/watch/issues/75) |
| [SK04 #33](https://github.com/K1w1b1t/watch/issues/33) | Mostrar como o React envia um sinal consentido pelo próprio servidor | [SK07 #75](https://github.com/K1w1b1t/watch/issues/75) |
| [OB03 #60](https://github.com/K1w1b1t/watch/issues/60) | Mostrar se recebimento, análise e entrega estão funcionando | [IN03 #4](https://github.com/K1w1b1t/watch/issues/4), [DE03 #13](https://github.com/K1w1b1t/watch/issues/13), [NT02 #56](https://github.com/K1w1b1t/watch/issues/56) |
| [DE04 #62](https://github.com/K1w1b1t/watch/issues/62) | Carregar configurações de regras sem executar código recebido | [DE02 #12](https://github.com/K1w1b1t/watch/issues/12) |
| [IF04 #69](https://github.com/K1w1b1t/watch/issues/69) | Decidir onde cada componente remoto ficaria e conferir custos | [C03 #72](https://github.com/K1w1b1t/watch/issues/72) |
| [IF02 #21](https://github.com/K1w1b1t/watch/issues/21) | Testar Worker e D1 em um laboratório separado na Cloudflare | [IF04 #69](https://github.com/K1w1b1t/watch/issues/69) |
| [IF03 #22](https://github.com/K1w1b1t/watch/issues/22) | Preparar a configuração de uma máquina remota para o servidor | [IF04 #69](https://github.com/K1w1b1t/watch/issues/69) |
| [DS02 #24](https://github.com/K1w1b1t/watch/issues/24) | Verificar vulnerabilidades no código e nas dependências que já existem | [DS01 #23](https://github.com/K1w1b1t/watch/issues/23) |
| [DE07 #65](https://github.com/K1w1b1t/watch/issues/65) | Demonstrar uma pontuação de sinais com explicação de cada ponto | [DE04 #62](https://github.com/K1w1b1t/watch/issues/62) |
| [DE08 #66](https://github.com/K1w1b1t/watch/issues/66) | Converter um tipo simples de regra Sigma para o catálogo do Watch | [DE04 #62](https://github.com/K1w1b1t/watch/issues/62) |

## Como sair deste índice e começar

Abra a issue e leia entrega, exemplo, passos e demonstração. Se houver dependência aberta, confira se dá para usar um substituto no desenvolvimento ou escolha outra tarefa liberada. O [guia de execução](execucao.md) detalha como testar e apresentar a alteração.
