# Contratos v1 propostos

São especificações de planejamento, não schemas executáveis. Cada módulo pode construir seu adaptador em memória e suas fixtures a partir deste texto. A issue C01 formaliza schemas e testes de contrato; qualquer mudança incompatível exige revisar produtores, consumidores e fixtures.

## Evento

| Campo | Regra |
| --- | --- |
| schema_version | String fixa `1` |
| event_id | UUID gerado na origem; estável durante retry |
| occurred_at | RFC3339 UTC; aceitar até 24h no passado e 5min no futuro usando relógio injetável |
| kind | `auth.failure`, `auth.success` ou `host.port_observed` |
| severity | `info`, `low`, `medium`, `high` ou `critical`; contexto fornecido pela origem, nunca prova de ataque |
| source_id | Identificador pseudônimo; 1–64 caracteres alfanuméricos, hífen ou sublinhado |
| subject_id | Identificador pseudônimo com as mesmas restrições; obrigatório nos eventos de autenticação |
| port | Inteiro de 1 a 65535; obrigatório somente em `host.port_observed` |

Rejeitar campos desconhecidos. Proibir payload livre, senha, token, e-mail, nome, CPF, IP real, hostname pessoal e linha de comando. O tenant é metadado interno derivado da credencial, não campo aceito do cliente. OCSF é referência para uma futura conversão, sem alegar conformidade deste envelope reduzido.

Limite inicial: um evento por POST, corpo até 16 KiB; JSON UTF-8; sem compressão. Tenant e source_id são vinculados pela credencial. Deduplicação por tenant + event_id; conteúdo diferente com o mesmo identificador é conflito.

## Ingestão HTTP

`POST /v1/telemetry/ingest`: Bearer de máquina, Content-Type application/json. 201 após persistir evento novo; 200 para repetição idêntica sem novo efeito; 400 schema inválido; 401 chave ausente/inválida/revogada; 403 escopo ou source não permitido; 409 conflito de ID; 413 excesso de tamanho; 415 tipo não suportado; 429 limite; 503 dependência indisponível. 405 para método não permitido.

Erros: code estável e request_id UUID, sem ecoar corpo ou segredo. Resposta de sucesso: event_id, request_id e status `stored` ou `duplicate`. Não responder sucesso se o armazenamento falhar.

## Portas entre módulos

- Identity.verify: segredo de máquina → tenant_id, credential_id, source_id e scopes, ou erro. Falha de dependência fecha o acesso. A credencial só recebe `events:write` inicialmente.
- EventStore.append: tenant validado + evento → stored/duplicate/conflict. Implementação em memória para desenvolvimento; SQLite para persistência local.
- EventStore.list: tenant autorizado + cursor + limit → itens e next_cursor. Ordenação estável por received_at e event_id. Limit padrão 20, máximo 100.
- Detector.evaluate: tenant + evento normalizado → zero ou mais alertas. Não executa HTTP nem acessa relógio global; estado e relógio são dependências explícitas.
- AlertStore: gravação idempotente por tenant + alert_id; listagem paginada, detalhe e atualização de status autorizados.

## Alerta e consulta

Alerta contém schema_version `1`, alert_id UUID, tenant_id interno, rule_id, rule_version, severity calculada pela regra, event_ids, detected_at UTC, status (`new`, `acknowledged`, `resolved`) e reason_code estável. Evidências referenciam IDs de eventos do próprio tenant; não embutir payload livre.

`GET /v1/alerts` retorna items e next_cursor; filtros opcionais status e severity; `GET /v1/alerts/{id}` inclui evidências permitidas. Recurso de outro tenant retorna 404. Sem sessão: 401. Papel viewer pode ler; analyst pode alterar status por `PATCH /v1/alerts/{id}` com campo status e transições new → acknowledged → resolved. Tentativa de outra transição: 409. Escrita sem papel: 403. Detalhes de identidade humana definidos em ID03.

Atualização opcional em Phoenix Channel: tópico derivado do tenant validado no servidor. Mensagem contém alert_id e tipo de mudança; cliente relê a API. Reconexão repõe a lista por HTTP.

## Fixtures que cada módulo deve conseguir reproduzir

1. Tenant A com source A, credencial válida e evento auth.failure válido.
2. Mesmo evento repetido e variante com conteúdo conflitante.
3. Tenant B com source B; tentativas de leitura e envio cruzados rejeitadas.
4. Chaves inválida, revogada e com source/escopo incompatíveis.
5. Cinco falhas do mesmo subject/source/tenant dentro de 60 segundos e sequência abaixo do limiar.
6. Histórico vazio, alerta novo, reconhecido e resolvido; dependência indisponível.

Usar IDs artificiais, horários fixos e credenciais de teste sem validade externa. Cada módulo documenta como subir seu modo de demonstração isolado. Mocks validam o contrato, mas não substituem o E2E integrado antes da entrega conjunta.

## Extensão planejada v1.1 — novos produtores

C02 formaliza uma versão `1.1` mantendo os eventos `1` existentes válidos. Não aceitar novos campos no schema `1` nem reabrir payload livre. A rota HTTP permanece `/v1/telemetry/ingest`, escolhendo validador por schema_version. Sem alteração silenciosa dos códigos de resposta.

Eventos 1.1 preservam campos base e acrescentam kinds fechados: `source.heartbeat`, `device.posture`, `app.access_denied`, `service.operation_failed`, `db.change_observed`, `analytics.signal` e `account.security_signal`.

- `source.heartbeat`: platform em linux/windows/macos/android/ios/server; app_version string de versão limitada a 32 caracteres. Não envia nomes/identificadores de hardware.
- `device.posture`: platform, check_id de allowlist versionada, result `pass`/`fail`/`unknown`/`unsupported`. Disponibilidade de bloqueio de tela ou integridade só quando API pública permitir; unknown jamais significa seguro. Não transmitir SSID, localização, SMS ou lista de apps.
- `app.access_denied`: subject_id pseudônimo e action_id de catálogo da aplicação, sem URL/query/header/cookie.
- `service.operation_failed`: service `supabase`/`upstash`/`vercel`/`oracle`/`other`; operation `auth`/`read`/`write`/`delete`; outcome `denied`/`timeout`/`unavailable`/`quota`. Sem comandos Redis, SQL ou nomes de objetos.
- `db.change_observed`: resource_id pseudônimo de tabela permitida e operation `insert`/`update`/`delete`. Sem valores de colunas ou IDs de registros pessoais.
- `analytics.signal`: signal_id de catálogo, subject_id pseudônimo quando consentido e verificável; trust_level é atribuído pelo adaptador no servidor, nunca pelo browser.
- `account.security_signal`: signal_id de catálogo e provider de enumeração aprovada. Um login OAuth no Watch não cria esse evento por si só.

Catálogos iniciais com fixtures: action_id `demo.admin.read`; signal_id `demo.sensitive_action` para analytics e `demo.credential_changed` para conta; check_id `screen_lock_available` e `app_integrity`. Cada check é apenas observação da API documentada, não diagnóstico global. Ampliar catálogos exige schema e teste. Campos específicos são obrigatórios apenas no kind correspondente; rejeitar os demais. Sem qualquer token/credencial de provedor no envelope.

## Registro e pareamento de sources

Source tem source_id pseudônimo, tenant_id interno, tipo de produtor, status enabled/disabled, platform opcional e last_seen_at calculado no servidor. Cadastro e consulta exigem identidade humana e membership; a fonte só envia com credencial vinculada a ela.

RG02 define pareamento com código aleatório de uso único (mínimo 128 bits), validade de 5min e consumo atômico. A emissão exige analyst; o resgate tem rate limit e retorna credencial somente uma vez. QR/código nunca contém token do Grafana/Discord. URLs de destino são configuradas pelo operador: sem aceitar URL arbitrária de origem para o servidor visitar.

## Notificação e exportação

Notificação contém alert_id, rule_id/version, severity, reason_code e URL de detalhe em origem allowlisted; o tenant é obtido do roteamento interno. Conteúdo saneado, tamanho limitado, menções Discord desativadas; nenhum evento bruto. Chave de entrega: tenant + alert_id + destino. Estado pending/sent/failed, tentativas limitadas e próxima tentativa persistida. Uma falha após aceite do Discord pode duplicar mensagem: não prometer exactly-once.

Exportação Loki é derivada de AlertStore, com token apenas no servidor. Labels limitadas a ambiente, regra e severidade; nunca ID de usuário, source, tenant ou alert_id como label de cardinalidade não limitada. Isolamento é responsabilidade da stack/credencial e do acesso ao Grafana; filtro visual não implementa autorização.

Evidência de alerta inclui snapshot mínimo dos eventos selecionados, hash do conteúdo normalizado, regra/versão e horário de coleta. Hash não prova autenticidade de uma origem comprometida. Expiração do buffer bruto não pode apagar a explicação silenciosamente: indicar evidência expirada quando o snapshot também for expurgado.
