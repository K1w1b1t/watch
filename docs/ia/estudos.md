# Trilha de estudos

Estudar o necessário ao próximo incremento e converter leitura em teste, decisão ou demonstração. Não existe curso, calendário ou certificação obrigatória.

| Área | Estudar | Material oficial | Exercício |
| --- | --- | --- | --- |
| Contratos | JSON Schema, versionamento e canonicalização | [JSON Schema](https://json-schema.org/learn/getting-started-step-by-step) | Mesma fixture passa/falha em todas as linguagens |
| Ingresso | HTTP, limites e falhas de dependências | [Workers testing](https://developers.cloudflare.com/workers/testing/), [Vitest](https://vitest.dev/guide/) | Rejeitar antes de persistir e não dar falso sucesso |
| Identidade | API keys, JWT, autorização por objeto | [JWT BCP](https://www.rfc-editor.org/rfc/rfc8725), [OWASP API Security](https://owasp.org/www-project-api-security/) | Revogar e negar leitura cruzada A/B |
| Dados | SQLite, transação, cursor, expurgo | [SQLite](https://sqlite.org/docs.html) | Reinício e replay sem duplicar |
| Detecção | Janelas, falsos positivos e versões | [ExUnit](https://hexdocs.pm/ex_unit/ExUnit.html), [ATT&CK](https://attack.mitre.org/) | Quatro falhas→zero, cinco→medium |
| Core/UI | API autorizada, estado e reconexão | [Phoenix Channels](https://hexdocs.pm/phoenix/channels.html), [React](https://react.dev/learn), [Playwright](https://playwright.dev/docs/intro) | Reconhecer/resolver e recompor lista |
| SDK TS e exemplos | Pacote server-side e instrumentação explícita | [Next instrumentation](https://nextjs.org/docs/app/guides/instrumentation), [Nest interceptors](https://docs.nestjs.com/interceptors) | Enviar sem chave no browser ou dependência de hospedagem |
| SDK Python/Go | Pacotes, cliente HTTP e cancelamento | [Python packaging](https://packaging.python.org/en/latest/tutorials/packaging-projects/), [Go modules](https://go.dev/doc/tutorial/create-module) | Mesma fixture e resultados compatíveis |
| Notificações | Webhook de saída e retry | [Discord webhooks](https://discord.com/developers/docs/resources/webhook) | Fake 429, reinício e outbox recuperável |
| Exportação | Push Loki e provisionamento local | [Loki API](https://grafana.com/docs/loki/latest/reference/loki-http-api/), [Terraform Grafana](https://registry.terraform.io/providers/grafana/grafana/latest/docs) | Alerta no dashboard local protegido |
| CI | Segredos, SCA/SAST conforme artefato | [Gitleaks](https://github.com/gitleaks/gitleaks), [CodeQL](https://codeql.github.com/docs/), [Sobelow](https://github.com/nccgroup/sobelow), [Trivy](https://trivy.dev/) | Fixture inerte bloqueia e correção passa |
| Pesquisa Sigma | Semântica de seleção e mapeamento | [Sigma](https://sigmahq.io/docs/) | Subset explícito e rejeição de operador não suportado |

Começar por contratos/HTTP, identidade/dados, detecção, API/UI e demonstração integrada. Só depois ampliar versões/SDKs/saídas. Conectores de plataformas, sensores e apps não são estudos obrigatórios desta fase. IaC remoto depende de decisão/gate de custo, sem conta paga necessária para aprender localmente.

Referências são materiais de estudo; validar APIs/versões utilizadas na implementação. Mencionar ATT&CK/Sigma não comprova eficácia ou conformidade integral. Regras, scores e sinais não confirmam invasão por si sós.
