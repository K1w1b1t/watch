# Materiais e trilha de estudos

Estudar o necessário para o próximo incremento; converter cada leitura em teste, decisão ou demonstração. Não é necessário concluir todos os materiais antes de contribuir.

| Módulo | Entender | Material oficial | Exercício de saída |
| --- | --- | --- | --- |
| Contratos | Eventos, versionamento, minimização e normalização | [OCSF](https://schema.ocsf.io/), [JSON Schema](https://json-schema.org/learn/getting-started-step-by-step) | Explicar por que envelope v1 não é implementação integral de OCSF |
| Ingestão | HTTP, validação, limite de corpo e dependências | [Workers testing](https://developers.cloudflare.com/workers/testing/), [Vitest](https://vitest.dev/guide/) | Rejeitar entrada inválida antes de persistir |
| Identidade | API keys, OAuth/OIDC, JWT, MFA e autorização por objeto | [OWASP API Security](https://owasp.org/www-project-api-security/), [JWT BCP RFC 8725](https://www.rfc-editor.org/rfc/rfc8725), [OIDC Core](https://openid.net/specs/openid-connect-core-1_0.html) | Demonstrar isolamento entre A e B e revogação |
| Dados | Transações, índices, parametrização, backup e expurgo | [SQLite](https://sqlite.org/docs.html), [D1](https://developers.cloudflare.com/d1/) | Provar persistência após reiniciar e consulta isolada |
| Detecção | Indicador vs incidente, falsos positivos, janelas e evidência | [MITRE ATT&CK](https://attack.mitre.org/), [Sigma](https://sigmahq.io/docs/), [ExUnit](https://hexdocs.pm/ex_unit/ExUnit.html) | Detectar sequência sintética e explicar um falso positivo |
| Painel/core | API, autorização, estado de UI e reconexão | [Phoenix Channels](https://hexdocs.pm/phoenix/channels.html), [React](https://react.dev/learn), [Playwright](https://playwright.dev/docs/intro) | Visualizar alerta e recuperar histórico após reconectar |
| Coletor | Privilégio mínimo, métricas do host e retry | [Rust Book](https://doc.rust-lang.org/book/), [Tauri](https://v2.tauri.app/) | Enviar só source pseudônimo e número de porta; Tauri apenas depois da CLI |
| Infraestrutura | IaC, state, IAM, firewall, TLS e restauração | [Terraform](https://developer.hashicorp.com/terraform/tutorials), [Oracle Free Tier](https://docs.oracle.com/en-us/iaas/Content/FreeTier/freetier.htm) | Recriar laboratório sem editar recurso manualmente |
| DevSecOps | CI, SAST, SCA, segredos, contêineres e IaC | [GitHub Actions security](https://docs.github.com/en/actions/security-for-github-actions), [CodeQL](https://codeql.github.com/docs/), [Gitleaks](https://github.com/gitleaks/gitleaks), [Trivy](https://trivy.dev/), [ZAP](https://www.zaproxy.org/docs/) | Scanner falha com fixture inerte e passa com correção |

## Ordem de aprofundamento

1. HTTP e testes; fixtures; ameaça e fronteira de confiança de cada módulo.
2. Autenticação de máquina, isolamento de tenant e armazenamento confiável.
3. Regras determinísticas, evidências e janela temporal.
4. Identidade humana, triagem e UX de investigação.
5. Provisionamento mínimo, segurança do host e exercício de recuperação.
6. Streaming e sensor real somente após funcionamento do caminho simples.

## Ferramentas sem acúmulo desnecessário

Começar com testes e scanner de segredos. Acrescentar CodeQL somente para linguagens suportadas; usar ferramentas adequadas para Elixir, como [Sobelow](https://github.com/nccgroup/sobelow), quando houver Phoenix. Trivy cobre dependências/contêineres/IaC conforme os artefatos surgirem. ZAP atua apenas no alvo local próprio. Revisar achados manualmente, justificar exceções com motivo e condição de revisão. Não ligar todos os scanners antes de existir artefato útil para examinar.

AWS Security pode fornecer teoria sobre IAM, logging, redes, resposta e proteção de dados; não exige provisionar serviços AWS pagos. A trilha do repositório não depende de curso, certificação ou agenda pessoal.

## Trilha dos módulos complementares

| Área | Estudar | Prática verificável |
| --- | --- | --- |
| SDKs e aplicações | [Next.js instrumentation](https://nextjs.org/docs/app/guides/instrumentation), [Nest interceptors](https://docs.nestjs.com/interceptors), [Python packaging](https://packaging.python.org/en/latest/tutorials/packaging-projects/), [Go modules](https://go.dev/doc/tutorial/create-module) | Enviar evento permitido em app de exemplo sem comprometer resposta da aplicação |
| Desktop | [Tauri capabilities](https://v2.tauri.app/security/capabilities/), [distribuição](https://v2.tauri.app/distribute/) | Instalar, enviar, pausar e remover sem coleta oculta |
| Mobile | [React Native](https://reactnative.dev/docs/getting-started), [SecureStore](https://docs.expo.dev/versions/latest/sdk/securestore/), [BackgroundTask](https://docs.expo.dev/versions/latest/sdk/background-task/) | Envio consentido em primeiro plano; distinguir atraso do SO de ataque |
| Origem externa | [PostHog webhook](https://posthog.com/docs/data-pipelines/destinations/webhook), [Upstash stats](https://upstash.com/docs/devops/developer-api/redis/get_database_stats), [Supabase webhooks](https://supabase.com/docs/guides/database/webhooks) | Normalizar fixture e rejeitar PII antes do Watch |
| Discord | [Webhooks](https://discord.com/developers/docs/resources/webhook), [rate limits](https://discord.com/developers/docs/topics/rate-limits) | Entrega fake com 429, retry limitado e menções desabilitadas |
| Grafana | [Loki HTTP API](https://grafana.com/docs/loki/latest/reference/loki-http-api/), [Terraform provider](https://registry.terraform.io/providers/grafana/grafana/latest/docs) | Exportar alerta saneado e reproduzir dashboard local |
| Correlação | [Sigma](https://sigmahq.io/docs/), [ATT&CK](https://attack.mitre.org/) | Regra versionada, teste benigno/malicioso e evidência de falso positivo |

Uma regra ou evento que menciona ATT&CK não certifica eficácia da detecção. Enriquecimento com IP/reputação e verificação de vazamentos dependem de decisão própria de privacidade/custo; ficam como pesquisa futura, sem enviar dados pessoais a terceiros no caminho inicial.
