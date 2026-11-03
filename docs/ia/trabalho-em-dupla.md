# Gustavo e João: sugestões para trabalhar em dupla

**Este documento responde:** o que cada um pode pegar para que as entregas se complementem. É uma sugestão ajustável, sem prazo ou atribuição automática de responsáveis nas issues.

## Como as duas frentes se encontram

Gustavo acompanha principalmente a entrada dos dados: formato, identidade, fontes, recebimento e SDK. João acompanha os dados guardados: banco, regras, API de alertas e ambiente local. O painel é um ponto de encontro: João prepara a tela com exemplos e a API; Gustavo liga a tela ao servidor.

Essa distribuição não presume especialidade. Podem trocar tarefas por acordo, avisando na issue antes de começar para evitar trabalho duplicado.

## Peguem estas primeiro

- **Gustavo:** C01 #1, para combinar os campos dos eventos e exemplos que todos usarão.
- **João (`reaperkoji`):** DS01 #23, para preparar as verificações dos arquivos/módulos já existentes.
- Depois de C01, Gustavo segue com C03 #72, o acordo de comunicação; João segue com DE01 #11, a primeira regra usando os exemplos de C01.
- Depois de C03, Gustavo prepara chaves em ID01 #5; João prepara o banco de eventos em ST01 #8.

## Filas para chegar ao primeiro resultado completo

Cada coluna é uma sequência sugerida. O número é posição de escolha, não dia de trabalho. Não esperem a outra pessoa completar toda a linha: basta a sua próxima issue ter os pré-requisitos necessários. A sequência abaixo foi conferida para permitir as duas tarefas da linha em paralelo após as anteriores.

| Posição | Gustavo | João (`reaperkoji`) |
| --- | --- | --- |
| 1 | [C01 #1](https://github.com/K1w1b1t/watch/issues/1) — Definir o formato dos eventos e criar exemplos para todos os módulos | [DS01 #23](https://github.com/K1w1b1t/watch/issues/23) — Fazer o GitHub executar testes e procurar segredos nas alterações |
| 2 | [C03 #72](https://github.com/K1w1b1t/watch/issues/72) — Definir como a entrada em TypeScript conversa com o servidor Elixir | [DE01 #11](https://github.com/K1w1b1t/watch/issues/11) — Criar uma primeira regra simples para aprender o caminho da detecção |
| 3 | [ID01 #5](https://github.com/K1w1b1t/watch/issues/5) — Criar e verificar chaves de acesso dos SDKs | [ST01 #8](https://github.com/K1w1b1t/watch/issues/8) — Guardar eventos no SQLite sem misturar organizações nem duplicar reenvios |
| 4 | [ID03 #7](https://github.com/K1w1b1t/watch/issues/7) — Permitir acesso de pessoas com papéis de leitura e análise | [ST02 #9](https://github.com/K1w1b1t/watch/issues/9) — Limitar por quanto tempo e quanto volume de eventos ficam guardados |
| 5 | [RG01 #27](https://github.com/K1w1b1t/watch/issues/27) — Cadastrar e desativar as aplicações que podem enviar eventos | [ST03 #10](https://github.com/K1w1b1t/watch/issues/10) — Guardar alertas e ligar cada alerta aos eventos que o explicam |
| 6 | [IN01 #2](https://github.com/K1w1b1t/watch/issues/2) — Receber um evento por HTTP e validar seus dados | [DE02 #12](https://github.com/K1w1b1t/watch/issues/12) — Detectar cinco falhas de login em um intervalo de 60 segundos |
| 7 | [IN02 #3](https://github.com/K1w1b1t/watch/issues/3) — Controlar quem pode enviar eventos e limitar a quantidade de pedidos | [ST04 #61](https://github.com/K1w1b1t/watch/issues/61) — Preservar os eventos que explicam um alerta mesmo após a limpeza do buffer |
| 8 | [IN03 #4](https://github.com/K1w1b1t/watch/issues/4) — Conectar a entrada ao servidor e ao banco reais | [DE03 #13](https://github.com/K1w1b1t/watch/issues/13) — Ler eventos do banco e retomar a análise depois de uma interrupção |
| 9 | [SK01 #30](https://github.com/K1w1b1t/watch/issues/30) — Criar o SDK TypeScript para enviar eventos de login | [ST05 #73](https://github.com/K1w1b1t/watch/issues/73) — Criar a API que o painel usa para consultar e tratar alertas |
| 10 | [CO01 #17](https://github.com/K1w1b1t/watch/issues/17) — Criar um comando de terminal para demonstrar o SDK TypeScript | [UI01 #14](https://github.com/K1w1b1t/watch/issues/14) — Montar o painel de alertas usando exemplos locais |
| 11 | [ID02 #6](https://github.com/K1w1b1t/watch/issues/6) — Revogar e substituir chaves de acesso | [IF01 #20](https://github.com/K1w1b1t/watch/issues/20) — Preparar um ambiente local que sobe os componentes do Watch juntos |
| 12 | [UI02 #15](https://github.com/K1w1b1t/watch/issues/15) — Ligar o painel à API e permitir reconhecer e resolver alertas | Conferir API/painel com Gustavo e preparar a demonstração |

**Agora os dois executam [DS03 #25](https://github.com/K1w1b1t/watch/issues/25) juntos.** Gustavo conduz envio, chave, painel e revogação. João confere banco, regra, evidência e retomada. O exercício precisa mostrar a mesma ocorrência atravessando o sistema inteiro.

Essa fila cobre as 23 issues básicas e acrescenta DS01 de apoio. Ready no quadro seleciona essas tarefas, mas não elimina dependências abertas.

## Combinem estes pontos antes de integrar

| Encontro | O que conferir juntos |
| --- | --- |
| C01 e C03 | Um evento válido e um inválido são entendidos igualmente por TypeScript e Elixir? Estão combinadas as mensagens e respostas internas? |
| IN03 + ST01 | O pedido real chega ao mesmo banco, sem duplicar reenvio? Fonte/organização são conferidas no servidor? |
| UI02 + ST05/UI01 | A tela usa os campos reais da API e trata sem sessão, falta de permissão, erro e evidência expirada? |
| DS03 | A regra básica está sozinha, a composição usa serviços reais e os dois conseguem repetir o roteiro? |

Separar alterações de identidade/fontes das de eventos/alertas. Se ambos precisarem alterar a inicialização do Phoenix, a composição ou um contrato, combinem a interface antes e façam propostas pequenas. Não criar dois servidores ou duas autoridades de banco para contornar uma espera.

## Se um de vocês estiver esperando

Gustavo pode antecipar SK01 após C01 e CO01 após SK01, ou trabalhar no ingresso local IN01. João pode antecipar DE02 após DE01 e IF01 após C03; ST02 e ST03 ficam independentes depois de ST01.

Se quiserem trocar UI01 ou outra tarefa, avisem antes de começar. Uma revisão do pré-requisito costuma liberar mais trabalho que abrir outro módulo sem necessidade. É possível usar um substituto ou uma branch combinada para desenvolver; a prova final precisa da versão integrada.

## Filas para a demonstração ampliada

Depois de mostrar DS03, continuar com fontes Python/Go, tipos 1.1, vínculos e saídas. Podem antecipar uma tarefa liberada se isso não atrasar a base.

| Posição | Gustavo | João (`reaperkoji`) |
| --- | --- | --- |
| 1 | [C02 #26](https://github.com/K1w1b1t/watch/issues/26) — Definir três novos tipos de evento para a versão 1.1 | [SK06 #35](https://github.com/K1w1b1t/watch/issues/35) — Criar o SDK Go com envio cancelável |
| 2 | [RG04 #76](https://github.com/K1w1b1t/watch/issues/76) — Registrar quais aplicações podem correlacionar a mesma pessoa fictícia | [DE05 #63](https://github.com/K1w1b1t/watch/issues/63) — Compartilhar o estado da detecção entre dois processos locais com segurança |
| 3 | [IN04 #74](https://github.com/K1w1b1t/watch/issues/74) — Fazer o servidor aceitar eventos 1.1 sem quebrar a versão 1 | [DE06 #64](https://github.com/K1w1b1t/watch/issues/64) — Detectar falhas seguidas de sucesso entre aplicações vinculadas |
| 4 | [SK05 #34](https://github.com/K1w1b1t/watch/issues/34) — Criar o SDK Python para o mesmo contrato de login | [OB01 #58](https://github.com/K1w1b1t/watch/issues/58) — Copiar resumos de alertas para consulta no Loki |
| 5 | [NT01 #55](https://github.com/K1w1b1t/watch/issues/55) — Preparar uma mensagem segura de alerta para o Discord | [OB02 #59](https://github.com/K1w1b1t/watch/issues/59) — Montar um painel Grafana para os alertas enviados ao Loki |
| 6 | [SK07 #75](https://github.com/K1w1b1t/watch/issues/75) — Adicionar os eventos 1.1 ao SDK TypeScript e testar com o Watch real | [NT02 #56](https://github.com/K1w1b1t/watch/issues/56) — Guardar notificações pendentes e retomar o envio após falhas |
| 7 | [RG03 #29](https://github.com/K1w1b1t/watch/issues/29) — Mostrar quando uma fonte enviou dados pela última vez | [NT03 #57](https://github.com/K1w1b1t/watch/issues/57) — Enviar cada alerta ao destino certo e reduzir avisos repetidos |

**Os dois executam [DS04 #71](https://github.com/K1w1b1t/watch/issues/71) juntos.** Gustavo fornece os exemplos Python/TS, contato e vínculos; João fornece Go, correlação, fila e gráficos. Confiram um high com seis evidências e as saídas, sem os alertas low/medium dos outros perfis.

Antes de João integrar DE06, RG04 precisa estar pronta. Antes de integrar NT02, ele usa o formato/envio NT01. Gustavo só integra SK07 após IN04; RG03 só renova contato de evento novo realmente guardado. São pontos de dependência, não combinações informais para pular teste.

## Complementos que cada um pode puxar

DS01 já aparece no início. As 13 tarefas restantes de apoio/complemento abaixo continuam no escopo, sem bloquear os dois roteiros completos.

| Sugestão | Tarefas | Quando faz sentido |
| --- | --- | --- |
| Gustavo | [CO02 #18](https://github.com/K1w1b1t/watch/issues/18) — Adicionar ao SDK TypeScript uma opção limitada de fila e reenvio; [SK02 #31](https://github.com/K1w1b1t/watch/issues/31) — Mostrar como usar o SDK em uma rota Next.js | Depois de SK01: opções de envio e exemplo Next. |
| Gustavo | [SK03 #32](https://github.com/K1w1b1t/watch/issues/32) — Mostrar como registrar um acesso negado em uma aplicação NestJS; [SK04 #33](https://github.com/K1w1b1t/watch/issues/33) — Mostrar como o React envia um sinal consentido pelo próprio servidor | Depois de SK07: exemplos Nest e React pelo próprio backend. |
| Gustavo | [UI03 #16](https://github.com/K1w1b1t/watch/issues/16) — Atualizar o painel quando um alerta mudar e recuperar após desconexão | Depois de UI02: avisos ao vivo; João revisa autorização do canal no servidor. |
| João | [DS02 #24](https://github.com/K1w1b1t/watch/issues/24) — Verificar vulnerabilidades no código e nas dependências que já existem; [OB03 #60](https://github.com/K1w1b1t/watch/issues/60) — Mostrar se recebimento, análise e entrega estão funcionando | DS02 após DS01 e material real para analisar; OB03 após IN03, DE03 e NT02. |
| João | [DE04 #62](https://github.com/K1w1b1t/watch/issues/62) — Carregar configurações de regras sem executar código recebido; [DE07 #65](https://github.com/K1w1b1t/watch/issues/65) — Demonstrar uma pontuação de sinais com explicação de cada ponto; [DE08 #66](https://github.com/K1w1b1t/watch/issues/66) — Converter um tipo simples de regra Sigma para o catálogo do Watch | DE04 após DE02; depois pontuação e importação podem andar separadas. |
| Gustavo | [IF04 #69](https://github.com/K1w1b1t/watch/issues/69) — Decidir onde cada componente remoto ficaria e conferir custos; [IF02 #21](https://github.com/K1w1b1t/watch/issues/21) — Testar Worker e D1 em um laboratório separado na Cloudflare | Opcional: decisão de custo/topologia após C03 e então teste Worker/D1. |
| João | [IF03 #22](https://github.com/K1w1b1t/watch/issues/22) — Preparar a configuração de uma máquina remota para o servidor | Opcional, após IF04 de Gustavo: preparar plano de máquina sem criar recurso. |

## Como confirmar que a divisão está funcionando

Ao terminar uma tarefa que o outro vai usar, demonstrem juntos um pedido e uma resposta reais ou os arquivos de exemplo combinados. O consumidor deve conseguir explicar como usar a entrega sem depender de uma explicação oral que não está no README.

As filas e os complementos cobrem as 52 issues existentes. Nenhuma nova funcionalidade ou preparação empresarial foi acrescentada. Para detalhes de campos, consulte [contratos](contratos.md); para a proposta de alteração e seus testes, consulte [execução](execucao.md).
