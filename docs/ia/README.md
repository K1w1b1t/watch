# Por onde começar no Watch

O Watch recebe eventos de aplicações, guarda esses eventos, procura padrões definidos por regras e apresenta alertas explicáveis. Um evento conta o que aconteceu; um alerta explica por que uma sequência merece atenção.

**Exemplo:** cinco tentativas de login falham para a mesma pessoa fictícia em até 60 segundos. A aplicação envia cinco eventos. O Watch cria um alerta de gravidade média e o painel mostra os cinco eventos usados na decisão.

## Se esta é sua primeira leitura

1. Leia o [plano geral](plano-geral.md) para entender o exemplo do começo ao fim e as duas entregas que vamos demonstrar.
2. Abra a [arquitetura](arquitetura.md) para entender qual parte do código faz cada trabalho.
3. Escolha uma tarefa pela [ordem de execução](ordem-execucao.md). Gustavo e João também podem usar a [sugestão de trabalho em dupla](trabalho-em-dupla.md).
4. Antes de implementar, leia os passos da issue e consulte no [contrato](contratos.md) os campos e respostas que ela usa.
5. Siga o [guia de execução de uma issue](execucao.md) para testar, demonstrar e entregar a alteração.

Não é necessário decorar todos os documentos. Consulte o [glossário](glossario.md) quando aparecer um termo desconhecido.

## Para que serve cada documento

| Sua dúvida | Documento | O que você encontra |
| --- | --- | --- |
| O que estamos construindo? | [Plano geral](plano-geral.md) | História de um evento, resultados das entregas e limites desta fase |
| Quem faz cada parte do sistema? | [Arquitetura](arquitetura.md) | Componentes, tecnologias e passagem dos dados |
| Como enxergar esse caminho? | [Diagramas](diagramas.md) | Desenhos comentados do recebimento, análise e notificação |
| O que significa esse termo? | [Glossário](glossario.md) | Termos técnicos com explicações e exemplos do Watch |
| Que tarefa pegar agora? | [Ordem de execução](ordem-execucao.md) | Pré-requisitos, paralelismo e caminho até cada demonstração |
| Como nós dois nos complementamos? | [Trabalho em dupla](trabalho-em-dupla.md) | Filas sugeridas para Gustavo e João e pontos de encontro |
| Quais tarefas existem? | [Backlog](backlog.md) | Índice das 52 issues, com entrega e dependências |
| Como executar e entregar uma issue? | [Execução](execucao.md) | Passo a passo de desenvolvimento, teste e revisão |
| Quais campos, códigos e limites usar? | [Contratos](contratos.md) | Referência técnica das mensagens e operações entre módulos |
| De onde vêm os dados e o que conseguimos observar? | [Origens e cobertura](origens-e-cobertura.md) | Uso dos SDKs e limites de visibilidade |
| O que precisamos proteger? | [Segurança](seguranca.md) | Credenciais, isolamento, dados permitidos e testes negativos |
| Preciso de conta ou recurso pago? | [Custos](custos.md) | Ambiente local e condições dos exercícios remotos opcionais |
| O que estudar para a próxima tarefa? | [Estudos](estudos.md) | Perguntas de aprendizagem e materiais por área |

## Combinados desta fase

- Construir e demonstrar localmente, com aplicações e usuários fictícios.
- Enviar eventos somente por SDKs usados no servidor. React passa pelo próprio backend; a chave não fica no navegador.
- Manter o mesmo contrato entre quem envia e quem recebe. Não inventar campos para desbloquear uma integração sem combinar a mudança.
- Usar as dependências das issues para concluir integrações. Ready no quadro básico significa que o trabalho foi selecionado, não que todos os pré-requisitos técnicos já terminaram.
- Não há datas ou responsáveis fixos. A divisão da dupla é uma sugestão que pode ser ajustada.

O objetivo desta fase é um laboratório demonstrável. Aplicação real de cliente, conectores de provedores, sensores de aparelhos e preparação empresarial não fazem parte das entregas atuais.
