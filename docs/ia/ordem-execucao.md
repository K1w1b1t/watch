# Em que ordem executar

**Este documento responde:** qual resultado fazer primeiro e quais tarefas podem andar juntas. Para saber o que uma tarefa contém, abra a issue pelo link; para repartir entre vocês, use a [sugestão em dupla](trabalho-em-dupla.md).

## Começo do trabalho

Começar por **C01 #1**, o formato dos dados, e **DS01 #23**, as verificações automáticas. São as duas tarefas sem pré-requisitos.

Depois de C01, priorizar C03 #72, que combina a comunicação entre TypeScript e Elixir. Também ficam liberados SDK, entrada local, regra simples e painel com exemplos. Não é necessário uma única pessoa terminar toda uma área antes de outra começar.

## Como ler as rodadas

Uma rodada agrupa tarefas sem dependência entre si. Ela não é sprint nem prazo e não obriga esperar todos da linha anterior. Confira os pré-requisitos da sua próxima issue: assim que eles terminarem, você pode avançar.

As 23 issues básicas estão em Ready no Projects por escolha do time. Ready é a seleção da entrega; não remove bloqueio técnico. Por exemplo, IN03 não pode concluir integração real antes de existir o armazenamento necessário.

## Primeiro resultado: fluxo básico

São 23 issues até DS03. O sucesso esperado é um alerta medium no painel após cinco falhas, com evidência e isolamento entre organizações.

### Rodada 1

Combinar o formato das mensagens. Todas as outras partes usam os mesmos exemplos.

- [C01 #1](https://github.com/K1w1b1t/watch/issues/1) — Definir o formato dos eventos e criar exemplos para todos os módulos.

### Rodada 2

Criar as primeiras peças independentes e combinar como os processos vão conversar.

- [IN01 #2](https://github.com/K1w1b1t/watch/issues/2) — Receber um evento por HTTP e validar seus dados.
- [DE01 #11](https://github.com/K1w1b1t/watch/issues/11) — Criar uma primeira regra simples para aprender o caminho da detecção.
- [UI01 #14](https://github.com/K1w1b1t/watch/issues/14) — Montar o painel de alertas usando exemplos locais.
- [SK01 #30](https://github.com/K1w1b1t/watch/issues/30) — Criar o SDK TypeScript para enviar eventos de login.
- [C03 #72](https://github.com/K1w1b1t/watch/issues/72) — Definir como a entrada em TypeScript conversa com o servidor Elixir.

### Rodada 3

Dar identidade, banco e execução local às peças, enquanto a regra aprende a contar a sequência.

- [IN02 #3](https://github.com/K1w1b1t/watch/issues/3) — Controlar quem pode enviar eventos e limitar a quantidade de pedidos.
- [ID01 #5](https://github.com/K1w1b1t/watch/issues/5) — Criar e verificar chaves de acesso dos SDKs.
- [ID03 #7](https://github.com/K1w1b1t/watch/issues/7) — Permitir acesso de pessoas com papéis de leitura e análise.
- [ST01 #8](https://github.com/K1w1b1t/watch/issues/8) — Guardar eventos no SQLite sem misturar organizações nem duplicar reenvios.
- [DE02 #12](https://github.com/K1w1b1t/watch/issues/12) — Detectar cinco falhas de login em um intervalo de 60 segundos.
- [CO01 #17](https://github.com/K1w1b1t/watch/issues/17) — Criar um comando de terminal para demonstrar o SDK TypeScript.
- [IF01 #20](https://github.com/K1w1b1t/watch/issues/20) — Preparar um ambiente local que sobe os componentes do Watch juntos.

### Rodada 4

Completar a administração de chave/fonte e a persistência dos alertas e limites.

- [ID02 #6](https://github.com/K1w1b1t/watch/issues/6) — Revogar e substituir chaves de acesso.
- [ST02 #9](https://github.com/K1w1b1t/watch/issues/9) — Limitar por quanto tempo e quanto volume de eventos ficam guardados.
- [ST03 #10](https://github.com/K1w1b1t/watch/issues/10) — Guardar alertas e ligar cada alerta aos eventos que o explicam.
- [RG01 #27](https://github.com/K1w1b1t/watch/issues/27) — Cadastrar e desativar as aplicações que podem enviar eventos.

### Rodada 5

Ligar os serviços reais e garantir que o alerta tenha explicação e possa ser consultado.

- [IN03 #4](https://github.com/K1w1b1t/watch/issues/4) — Conectar a entrada ao servidor e ao banco reais.
- [DE03 #13](https://github.com/K1w1b1t/watch/issues/13) — Ler eventos do banco e retomar a análise depois de uma interrupção.
- [ST04 #61](https://github.com/K1w1b1t/watch/issues/61) — Preservar os eventos que explicam um alerta mesmo após a limpeza do buffer.
- [ST05 #73](https://github.com/K1w1b1t/watch/issues/73) — Criar a API que o painel usa para consultar e tratar alertas.

### Rodada 6

Trocar os dados de exemplo do painel pelos dados reais da API.

- [UI02 #15](https://github.com/K1w1b1t/watch/issues/15) — Ligar o painel à API e permitir reconhecer e resolver alertas.

### Rodada 7

Provar o caminho inteiro, incluindo erro, reenvio, revogação e evidência.

- [DS03 #25](https://github.com/K1w1b1t/watch/issues/25) — Demonstrar o caminho completo: enviar eventos e tratar o alerta no painel.

**Quando parar para demonstrar:** DS03 precisa atravessar SDK → entrada → servidor/banco → detector → API → painel com as peças reais. Quatro falhas não alertam; a quinta gera um medium. Não esperar Discord/Grafana para mostrar esse primeiro resultado.

## Segundo resultado: fluxo ampliado

As 15 tarefas adicionais abaixo consideram a base pronta. Algumas podem começar antes se seus próprios bloqueios já terminaram, mas a prioridade é fechar DS03.

### Rodada 1 do ampliado

Preparar novos formatos, outros SDKs, vínculos e as primeiras saídas.

- [C02 #26](https://github.com/K1w1b1t/watch/issues/26) — Definir três novos tipos de evento para a versão 1.1.
- [SK05 #34](https://github.com/K1w1b1t/watch/issues/34) — Criar o SDK Python para o mesmo contrato de login.
- [SK06 #35](https://github.com/K1w1b1t/watch/issues/35) — Criar o SDK Go com envio cancelável.
- [NT01 #55](https://github.com/K1w1b1t/watch/issues/55) — Preparar uma mensagem segura de alerta para o Discord.
- [OB01 #58](https://github.com/K1w1b1t/watch/issues/58) — Copiar resumos de alertas para consulta no Loki.
- [DE05 #63](https://github.com/K1w1b1t/watch/issues/63) — Compartilhar o estado da detecção entre dois processos locais com segurança.
- [RG04 #76](https://github.com/K1w1b1t/watch/issues/76) — Registrar quais aplicações podem correlacionar a mesma pessoa fictícia.

### Rodada 2 do ampliado

Ligar os dados a notificações, gráficos, correlação e recebimento 1.1.

- [NT02 #56](https://github.com/K1w1b1t/watch/issues/56) — Guardar notificações pendentes e retomar o envio após falhas.
- [OB02 #59](https://github.com/K1w1b1t/watch/issues/59) — Montar um painel Grafana para os alertas enviados ao Loki.
- [DE06 #64](https://github.com/K1w1b1t/watch/issues/64) — Detectar falhas seguidas de sucesso entre aplicações vinculadas.
- [IN04 #74](https://github.com/K1w1b1t/watch/issues/74) — Fazer o servidor aceitar eventos 1.1 sem quebrar a versão 1.

### Rodada 3 do ampliado

Completar contato das fontes, roteamento e envio 1.1 por TypeScript.

- [RG03 #29](https://github.com/K1w1b1t/watch/issues/29) — Mostrar quando uma fonte enviou dados pela última vez.
- [NT03 #57](https://github.com/K1w1b1t/watch/issues/57) — Enviar cada alerta ao destino certo e reduzir avisos repetidos.
- [SK07 #75](https://github.com/K1w1b1t/watch/issues/75) — Adicionar os eventos 1.1 ao SDK TypeScript e testar com o Watch real.

### Rodada 4 do ampliado

Demonstrar os SDKs diferentes e as saídas trabalhando juntos.

- [DS04 #71](https://github.com/K1w1b1t/watch/issues/71) — Demonstrar SDKs diferentes gerando um alerta com notificação e gráfico.

**Quando parar para demonstrar:** DS04 usa cinco falhas Python e um sucesso Go, com fontes vinculadas, para criar um high com seis evidências. O resumo chega ao Discord falso; Loki/Grafana locais mostram a cópia; TS 1.1 testa contato da fonte.

## Apoio e complementos não são uma terceira barreira

DS01 começa junto de C01. DS02 é acrescentado quando houver material para os scanners. Atualização ao vivo, exemplos Next/Nest/React, opções do SDK, estudo de regras e experiências remotas entram conforme a necessidade e suas dependências.

Essas 14 tarefas estão na seção [Apoio e complementos do backlog](backlog.md#apoio-e-complementos--14-issues). Não precisam terminar todas antes de DS03/DS04. A tabela da [dupla](trabalho-em-dupla.md) sugere quem pode pegá-las.

## Como usar o quadro no dia a dia

1. Escolher a entrega em que estão trabalhando, começando pelo básico.
2. Conferir se os bloqueios da próxima issue já terminaram, mesmo que o cartão esteja Ready.
3. Avisar quem vai assumir, mover para o status de trabalho e entregar uma alteração revisável.
4. Validar a integração com a pessoa que consome a entrega e só então concluir.

Para visualizar melhor, a sugestão é mostrar Entrega, Rodada e Status nos cartões e ordenar por rodada dentro da entrega. Esses campos são uma recomendação de visualização; o estado já aplicado foi Ready nas 23 básicas.
