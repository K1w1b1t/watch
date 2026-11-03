# Origens, apps e cobertura do Watch

O sistema completo inclui produtores de eventos, provisionamento de fontes, análise, evidências, notificação e visualização. O fluxo local inicial é apenas um recorte desse sistema. Cada linha abaixo corresponde a incrementos no backlog, sem calendário.

## Integração principal via SDK próprio

URL de ingestão e API key individual configuram o transporte server-side ou do sensor nativo. Os produtores enviam diretamente ao Watch, sem Log Drains. Logger, interceptor e adaptador definem quais eventos serão emitidos; não há coleta automática de todos os logs de uma conta. No browser, usar backend autorizado para manter o segredo protegido. Ver [plano geral](plano-geral.md).

## Mapa de cobertura

| Origem ou capacidade | Entrada inicial | Incrementos planejados | Limitação de cobertura |
| --- | --- | --- | --- |
| Aplicações Node | SDK TypeScript server-side | Next.js e NestJS | Enxerga eventos instrumentados, não toda a conta cloud |
| React/browser | Relato pelo backend da própria aplicação | Consentimento e validação estrita | Evento do browser é não confiável; sem API key de máquina no bundle |
| Outros serviços | SDKs Python e Go | Conformidade com mesmo contrato | Sem duplicar lógica de detecção nos SDKs |
| Vercel | Eventos server-side via SDK | Adaptador de origem e cadastro de projetos | Drains nativos exigem plano pago; não são base gratuita |
| Supabase Auth | Resultado de autenticação observado no servidor | Conversão de resultado em evento pseudônimo | Não vê acessos que contornam a aplicação; SDK do cliente não é auditoria autoritativa |
| Supabase Postgres | Auditoria mínima de tabela de laboratório | Outbox local e redaction antes da saída | Webhook de linha não equivale a logs de Postgres/Auth; Drains não são Free |
| Upstash Redis | Resultado de operações instrumentadas | Poll opcional de estatísticas documentadas | Métrica de erro/uso não comprova intrusão; sem inventar webhook de segurança |
| PostHog | Destination webhook com allowlist | Normalização sem distinct_id, URL ou propriedades livres | Eventos analytics são contexto; dependem de consentimento e plano/cotas |
| Oracle/buckets | Operações instrumentadas do próprio serviço | Matriz de viabilidade de auditoria da conta | Não reutilizar VPS existente nem baixar conteúdo dos buckets |
| PCs Linux/Windows/macOS | CLI e adaptadores específicos do SO | App Tauri instalável e coleta consentida | Sem privilégio oculto, inventário indiscriminado ou promessa de EDR completo |
| Android | App React Native instalável com envio explícito | Credencial por instalação, postura e background oportunista | Sandbox impede visão global de outros apps; tarefa pode atrasar ou parar |
| iOS | Mesmo contrato, build local e matriz de capacidade | Postura permitida e leitura de alertas | Distribuição/assinatura têm restrições; background não é contínuo |
| Contas externas | Inventário de APIs/eventos oficialmente disponíveis | Importação de sinal de laboratório | OAuth de login não concede acesso universal às sessões de uma conta |
| Discord | Notificação resumida do alerta | Retry, outbox, roteamento e cooldown | Entrega pode atrasar/duplicar; push depende do usuário/Discord/SO |
| Grafana | Exportação de alertas saneados para Loki | Dashboard e saúde da pipeline | Não substituir autorização do Watch por filtro de dashboard |
| Motor de análise | Regra determinística e janela temporal | Regras versionadas, estado limitado, risco, correlação e subconjunto Sigma | Sinal suspeito não equivale a invasão confirmada |

## Onde ficam as responsabilidades

Todos os produtores enviam ao ingresso autenticado do Watch. Apenas adaptadores no servidor possuem credenciais de provedores, Grafana e Discord. Aplicações instaladas recebem credencial individual revogável limitada à sua própria source. Browser envia ao próprio backend, com sessão, autorização e proteção contra abuso; o backend constrói o evento permitido.

Registrar projeto, serviço e dispositivo como sources distintas no mesmo tenant. Vincular registros de múltiplas contas exige autorização explícita; nunca deduzir que duas pessoas/dispositivos são a mesma entidade por e-mail/IP. Correlação entre fontes usa subject_id pseudônimo obtido por política do servidor e vínculo autorizado. IDs pseudônimos não tornam dados automaticamente anônimos: restringir acesso e retenção.

O app mobile possui duas funções separadas: enviar sinais do próprio aparelho e consultar alertas do tenant como usuário autenticado. Credencial do sensor não permite ler incidentes. Discord oferece uma saída de alerta independente, sem substituir o app sensor.

## Retenção e estado de análise

Não é necessário guardar todos os logs brutos. Começar com eventos minimizados em buffer de até 24h, expurgado e limitado por bytes/quantidade. As regras temporais precisam de estado compartilhado consistente quando houver mais de uma instância: memória do Worker e read-modify-write em KV não garantem contadores globais corretos.

Preservar snapshots mínimos dos eventos que sustentam um alerta, junto da regra/versão e resultado. Proposta de laboratório: evidências até 7 dias ou 10 MiB por tenant, com expurgo explícito e indicação de evidência expirada. Não exportar logs brutos por padrão. Não prometer investigação completa após descartar contexto.

O Loki recebe apenas alertas e saúde operacional saneados. Sua retenção efetiva depende do serviço: não afirmar que expurgo no Watch apaga cópias externas nem que TTL de 7 dias é garantido no Grafana Cloud Free. O módulo de exportação deve documentar isso antes de usar dados reais.

## Referências verificadas no planejamento

- [Vercel Drains: planos e cobrança](https://vercel.com/docs/drains)
- [Supabase Log Drains: disponibilidade](https://supabase.com/docs/guides/observability/log-drains)
- [Upstash: estatísticas de banco](https://upstash.com/docs/devops/developer-api/redis/get_database_stats)
- [PostHog: destination webhook](https://posthog.com/docs/data-pipelines/destinations/webhook)
- [PostHog: preços de pipelines](https://posthog.com/data-pipelines/pricing)
- [Android: sandbox de aplicações](https://source.android.com/docs/security/app-sandbox)
- [Expo: tarefas em background](https://docs.expo.dev/versions/latest/sdk/background-task/)
- [Apple: conta gratuita e distribuição](https://developer.apple.com/help/account/basics/about-your-developer-account/)
- [Grafana Cloud: planos](https://grafana.com/pricing/)
- [Loki: API HTTP](https://grafana.com/docs/loki/latest/reference/loki-http-api/)
- [Discord: webhooks](https://discord.com/developers/docs/resources/webhook)

Essas capacidades serão revalidadas no incremento correspondente. Não alterar aplicações de outros repositórios para demonstrar os módulos: usar exemplos isolados dentro do Watch.

## Pontos de entrada do backlog

- [Mobile Android e evolução iOS](https://github.com/K1w1b1t/watch/issues/49)
- [Desktop Tauri](https://github.com/K1w1b1t/watch/issues/43)
- [SDK TypeScript](https://github.com/K1w1b1t/watch/issues/30), [Python](https://github.com/K1w1b1t/watch/issues/34) e [Go](https://github.com/K1w1b1t/watch/issues/35)
- [Fontes e pareamento](https://github.com/K1w1b1t/watch/issues/27)
- [Notificações Discord](https://github.com/K1w1b1t/watch/issues/55)
- [Exportação Loki/Grafana](https://github.com/K1w1b1t/watch/issues/58)
- [Backlog completo com dependências](backlog.md)
