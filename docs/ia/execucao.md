# Como pegar, executar e entregar uma issue

**Este documento responde:** o que fazer depois de escolher uma tarefa. Para escolher qual pegar, use a [ordem](ordem-execucao.md) ou a [divisão da dupla](trabalho-em-dupla.md).

## 1. Entenda o resultado antes de codar

Leia o início da issue e responda com suas palavras:

- O que vai existir depois desta tarefa que ainda não existe?
- Quem usa esse resultado: aplicação, entrada, detector, painel ou operador?
- Como vou provar que funciona?

Exemplo: em IN01, o resultado é uma rota HTTP que valida e guarda no componente local de teste. Não é necessário criar login, banco ou detector nessa mesma issue.

Se houver termo desconhecido, consulte o [glossário](glossario.md). Se faltar uma decisão de campo/resposta, consulte o [contrato](contratos.md) e combine a decisão antes de implementar uma interface diferente.

## 2. Confira as dependências e sinalize que vai pegar

Abra os pré-requisitos indicados na issue. É possível desenvolver com uma dependência de teste enquanto o outro módulo não ficou pronto, mas a integração só termina com os componentes reais exigidos.

Avisem quem vai assumir antes de começar, para não fazer duas implementações concorrentes. As filas de Gustavo/João são sugestões; podem ser trocadas por acordo. Não há calendário ou responsável fixo.

No quadro, as 23 issues básicas estão em Ready por seleção do time. Isso não significa que IN03 já possa integrar um EventStore que ninguém implementou. O bloqueio técnico continua na descrição e nas dependências nativas do GitHub.

## 3. Trabalhe com exemplos pequenos e conhecidos

Use as organizações fictícias A/B, eventos e relógios de C01/C02. Não teste com senhas, usuários ou projetos reais. Identifique claramente o que está substituído por um fake, mock ou stub — nomes para dependências controladas no teste.

Exemplo: o painel de UI01 pode ler arquivos de alertas. Em UI02 ele precisa consultar a API real. Ter uma tela bonita com os arquivos não comprova UI02.

## 4. Implemente pelo comportamento

Para código, comece com um teste que falha pelo comportamento ausente, implemente o necessário e melhore a estrutura mantendo os testes. Esse ciclo é chamado TDD. Não escreva teste que só repete as mesmas linhas da implementação.

| Tipo de verificação | Pergunta respondida | Exemplo |
| --- | --- | --- |
| Teste de lógica, ou unitário | A regra isolada calcula corretamente? | Quatro falhas não alertam e cinco alertam |
| Teste de contrato | As partes concordam com o formato? | SDK e servidor aceitam os mesmos JSONs de C01 |
| Teste pela entrada pública | O módulo funciona quando usado de verdade? | Fazer HTTP na rota local e conferir efeito no banco |
| Teste integrado, ou E2E completo | O conjunto de componentes reais funciona junto? | SDK → entrada → banco → detector → API → painel |

Inclua o caminho de erro relevante: credencial inválida, conflito, organização errada, falha de dependência, reinício ou concorrência, conforme a tarefa. Uma decisão documental exige revisão de exemplos/coerência, não testes artificiais de texto.

## 5. Demonstre e abra a proposta de alteração

Uma pull request, ou PR, é a proposta de alteração para o outro revisar. Inclua:

1. O problema resolvido e o comportamento entregue.
2. Comandos e pré-requisitos para executar localmente.
3. Um exemplo de sucesso e o erro mais importante da tarefa.
4. Testes executados e resultado, com componentes reais e simulados identificados.
5. Limitação que permanece e algum critério ainda pendente, se houver.

Não invente comandos de execução que ainda não foram criados. A PR que implementa o módulo deve atualizar seu README com os comandos que realmente funcionaram.

## 6. Revise junto à pessoa que vai consumir a entrega

Se Gustavo entrega o formato que João vai ler, João revisa os exemplos. Se João entrega a API do painel, Gustavo confere os pedidos e respostas antes de conectar a tela. Fazer isso cedo evita duas peças corretas isoladamente, mas incompatíveis.

Uma PR em revisão não é pré-requisito concluído. É possível combinar uma branch para desenvolver em paralelo, mas a prova final usa as versões integradas.

## 7. Quando encerrar

- Todos os critérios da issue têm evidência verificável.
- Os testes relevantes passam e o README permite repetir a demonstração.
- Os componentes reais exigidos estão integrados e as dependências de conclusão foram cumpridas.
- Não há segredo/dado pessoal em Git, erro, registro ou pacote do navegador.
- Dados de A não aparecem em B; formato e exemplos compartilhados continuam compatíveis.
- O CI foi atualizado para os artefatos que a alteração acrescentou.

O fechamento da issue descreve capacidade demonstrada, não só arquivos criados. Nos roteiros, DS03 usa apenas regra medium básica; DS04 usa apenas regra high ampliada. Não acrescentar piloto empresarial nem requisitos que a tarefa não pede.

## Para escrever uma nova issue

Explique, nessa ordem: entrega, motivo, exemplo, passos, dependências, critérios de término, testes, exclusões e leitura específica. Dê nome e link aos pré-requisitos. Evite siglas sem explicação e frases genéricas como “integrar tudo” sem dizer por onde entra e o que precisa sair.
