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
