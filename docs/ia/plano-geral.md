# O que vamos construir

**Este documento responde:** para que serve o Watch, o que acontece com um evento e quais resultados vamos demonstrar. Para decidir linguagens e componentes, veja a [arquitetura](arquitetura.md).

## A ideia em um exemplo

Imagine uma aplicação fictícia chamada `app-a`. Ela tem uma tela de login. Quando uma tentativa falha, o código do servidor chama uma biblioteca do Watch, o **SDK**, e informa: “houve uma falha de login para `pessoa-demo`”.

O SDK não descobre sozinho tudo que acontece. Quem desenvolve a aplicação escolhe onde chamar essa biblioteca e quais eventos permitidos enviar.

O Watch recebe os eventos, confere quem pode enviá-los e os guarda. Depois, uma regra procura cinco falhas da mesma fonte e pessoa em até 60 segundos. Quando encontra a sequência, cria um alerta. Uma pessoa abre o painel e vê por que ele foi criado.

## Evento, alerta e evidência

| Conceito | No nosso exemplo |
| --- | --- |
| Evento | Uma tentativa de login falhou em determinado horário |
| Regra | Cinco falhas da mesma pessoa/fonte em 60 segundos merecem atenção |
| Alerta | Resultado da regra: “sequência de falhas de login”, com gravidade média |
| Evidência | Os cinco eventos que sustentam esse resultado |
| Reconhecer | O analista marca que começou a olhar o alerta |
| Resolver | O analista encerra o tratamento do alerta na demonstração |

Um alerta indica uma situação que merece análise. Cinco falhas também podem ser uma pessoa com dificuldade para lembrar a senha; o sistema não deve afirmar que confirmou uma invasão.

## O caminho de um evento, passo a passo

1. **A aplicação relata o acontecimento.** O código chama o SDK no servidor com tipo, horário e identificadores fictícios. Não envia senha ou dados pessoais.
2. **O SDK monta e envia a mensagem.** Ele verifica o formato e faz uma chamada HTTP à entrada do Watch, usando sua chave de escrita.
3. **A entrada confere o pedido.** Valida tamanho, campos, chave, fonte e quantidade de pedidos. A organização é descoberta pela credencial, não escolhida no JSON.
4. **O servidor principal guarda o evento.** Ele verifica novamente a fonte e grava no SQLite. Só então a entrada responde que guardou. Reenvio igual não cria outra cópia.
5. **O detector lê os dados guardados.** Um processo aplica as regras e registra até onde terminou, para retomar se cair.
6. **A regra cria um alerta com explicação.** O alerta indica regra, versão, gravidade e eventos usados. Uma cópia mínima da evidência evita perder a explicação quando o buffer comum for limpo.
7. **O painel consulta a API.** Um usuário com permissão vê apenas os dados de sua organização e pode reconhecer/resolver conforme seu papel.
8. **As saídas opcionais avisam ou mostram gráficos.** Discord recebe resumo; Loki/Grafana recebem uma cópia saneada para consulta e visualização.

O sucesso do envio confirma armazenamento, não análise imediata. Se o servidor caiu antes de guardar, a resposta é de erro. O detector trabalha depois, lendo os dados persistidos.

## Primeira entrega: SDK até o painel

O roteiro [DS03 #25](https://github.com/K1w1b1t/watch/issues/25) será a prova da entrega básica:

- Uma CLI, ou programa de terminal, usa o SDK TypeScript para enviar eventos fictícios.
- Quatro falhas em 60 segundos produzem zero alertas; a quinta produz um alerta `medium` com cinco evidências.
- O painel permite abrir, reconhecer e resolver esse alerta.
- Reenviar evento não duplica alerta; organização B não vê dados de A; revogar chave impede novo envio.
- Interromper um processo permite testar retomada. Limpar eventos vencidos não apaga silenciosamente a evidência preservada.

Essa demonstração usa os serviços locais reais. Discord e Grafana não precisam estar prontos para concluí-la.

## Segunda entrega: aplicações diferentes e saídas

O roteiro [DS04 #71](https://github.com/K1w1b1t/watch/issues/71) amplia a prova:

1. Um programa Python envia cinco falhas.
2. Um programa Go envia um sucesso, usando a mesma convenção fictícia de pessoa.
3. As fontes foram vinculadas explicitamente dentro da mesma organização; sem esse vínculo, a regra não combina os dados.
4. A regra ampliada cria um alerta `high`, com os seis eventos. Só essa regra fica ativa nesse exercício.
5. A fila de notificações envia o resumo ao serviço que imita Discord. Loki e Grafana locais mostram a cópia exportada.
6. O SDK TS 1.1 envia um heartbeat explícito, um aviso de contato, para testar a consulta do último recebimento da fonte.

Também vamos desligar destinos e reiniciar processos para observar pendências e retomada. O [contrato](contratos.md) explica as condições exatas de tempo e os possíveis reenvios das saídas externas.

## O que mais faremos no projeto

As tarefas de apoio acrescentam CI, verificações de segurança, exemplos Next/Nest/React, atualização ao vivo do painel, configuração de regras, pontuação explicável e um importador limitado de Sigma. Há exercícios opcionais de infraestrutura remota, sempre depois de conferir topologia e custos.

Essas atividades estão no [backlog](backlog.md), mas não precisam virar uma fila obrigatória antes do primeiro resultado completo. O [guia de ordem](ordem-execucao.md) mostra o que bloqueia cada entrega.

## Limites desta fase

Vamos usar apenas aplicações de exemplo e dados fictícios. Não vamos captar eventos de contas de provedores, criar sensores desktop/mobile, mexer na VPS de outros projetos ou depender de plano pago para demonstrar localmente.

Também não fazem parte desta entrega: aplicação de cliente real, gestão empresarial de usuários/incidentes, ajuste operacional de regras em produção, backup/restauração integrados e metas empresariais de capacidade/latência. Esses limites mantêm o trabalho atual compreensível e verificável.

**Próximo passo:** leia [como as peças se conectam](arquitetura.md) ou escolha uma tarefa pela [ordem de execução](ordem-execucao.md).
