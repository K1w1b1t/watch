# O que estudar para executar a próxima tarefa

**Use este documento para:** encontrar uma pergunta de aprendizagem e uma referência relacionada à issue que você pegou. Não é uma lista de cursos a terminar antes de começar.

Leia o suficiente para escrever o próximo teste, decidir uma interface ou demonstrar um comportamento. Se a dúvida é o significado de uma palavra, use primeiro o [glossário](glossario.md).

| Se você vai trabalhar em… | Pergunta que precisa responder | Referências oficiais | Pequena prova de entendimento |
| --- | --- | --- | --- |
| C01/C02: formato de dados | Como proibir campos extras e validar tipos diferentes? | [JSON Schema](https://json-schema.org/learn/getting-started-step-by-step) | Mostrar um JSON aceito e outro rejeitado, explicando o campo |
| C03/IN: comunicação e entrada | Quando o servidor pode afirmar que guardou? Como informar erro? | [Testes Workers](https://developers.cloudflare.com/workers/testing/), [Vitest](https://vitest.dev/guide/) | Simular falha de armazenamento e comprovar ausência de sucesso falso |
| ID/RG: identidade e permissão | Por que verificar assinatura não substitui autorizar a organização? | [Boas práticas JWT](https://www.rfc-editor.org/rfc/rfc8725), [OWASP API Security](https://owasp.org/www-project-api-security/) | Negar acesso de A ao objeto de B e revogar uma chave |
| ST: banco e retenção | Como impedir metade de uma gravação ou duplicação? | [SQLite](https://sqlite.org/docs.html) | Reiniciar e repetir operação sem duplicar evento/alerta |
| DE: regras e tempo | Como tratar quinto evento, atraso e repetição? | [ExUnit](https://hexdocs.pm/ex_unit/ExUnit.html), [MITRE ATT&CK](https://attack.mitre.org/) | Demonstrar quatro falhas sem alerta e cinco com explicação |
| UI/ST05: consulta e tela | Qual estado mostrar sem sessão, com erro ou evidência expirada? | [React](https://react.dev/learn), [Playwright](https://playwright.dev/docs/intro) | Percorrer lista/detalhe/status pelo navegador |
| UI03: avisos ao vivo | Como recuperar mudanças perdidas na desconexão? | [Phoenix Channels](https://hexdocs.pm/phoenix/channels.html) | Cortar conexão, criar mudança e recompor pela API |
| SDK TS/Next/Nest | Onde instrumentar sem expor segredo ao navegador? | [Next instrumentation](https://nextjs.org/docs/app/guides/instrumentation), [Nest interceptors](https://docs.nestjs.com/interceptors) | Evento sai do servidor e a resposta original da aplicação continua correta |
| SDK Python/Go | Como empacotar cliente reutilizável e controlar a espera? | [Python packaging](https://packaging.python.org/en/latest/tutorials/packaging-projects/), [Go modules](https://go.dev/doc/tutorial/create-module) | Executar exemplo que importa o pacote e passa pelos casos de C01 |
| NT: notificação | O que fazer se o destino aceitou, mas a confirmação se perdeu? | [Discord webhooks](https://discord.com/developers/docs/resources/webhook) | Explicar possível duplicata e testar retomada da fila |
| OB: cópia e gráfico | Qual dado foi exportado e quem pode consultá-lo? | [Loki HTTP API](https://grafana.com/docs/loki/latest/reference/loki-http-api/), [Provider Grafana](https://registry.terraform.io/providers/grafana/grafana/latest/docs) | Ver um alerta conhecido no gráfico local protegido |
| DS: verificações automáticas | Que material cada ferramenta realmente analisa? | [Gitleaks](https://github.com/gitleaks/gitleaks), [CodeQL](https://codeql.github.com/docs/), [Sobelow](https://github.com/nccgroup/sobelow), [Trivy](https://trivy.dev/) | Exemplo inerte bloqueia e correção passa no CI |
| DE08: Sigma | O que acontece se a regra usa recurso não suportado? | [Sigma](https://sigmahq.io/docs/) | Importar um exemplo compatível e rejeitar outro sem enfraquecer a regra |

As APIs e versões concretas são conferidas ao implementar. Referência a ATT&CK/Sigma não comprova que uma detecção é eficaz ou que suportamos o padrão inteiro. O resultado da leitura deve aparecer no teste, na decisão ou na demonstração da issue.
