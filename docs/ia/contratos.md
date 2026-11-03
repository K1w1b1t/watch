# Contratos propostos do laboratório

C01 (#1) transforma v1 em schemas/fixtures canônicos; C02 (#26) adiciona 1.1; C03 (#72) formaliza OpenAPI interna/topologia. São propostas até essas entregas. Consumidores compartilham os mesmos casos de conformidade. IN04 (#74) e SK07 (#75) comprovam integração 1.1 real.

## Evento v1

| Campo | Regra |
| --- | --- |
| schema_version | String `1` |
| event_id | UUID canônico minúsculo, gerado uma vez e preservado no retry |
| occurred_at | RFC3339 UTC; até 24h no passado e 5min no futuro, relógio injetável |
| kind | `auth.failure` ou `auth.success` |
| severity | `info`, `low`, `medium`, `high`, `critical`; contexto da origem |
| source_id | Pseudônimo 1–64 caracteres alfanuméricos, hífen ou sublinhado; vinculado à chave |
| subject_id | Pseudônimo com a mesma restrição; obrigatório em autenticação |

Rejeitar campos desconhecidos, tenant do cliente e payload livre. Não emitir senha, token, email, nome, CPF, IP, hostname ou linha de comando. Exemplos usam IDs artificiais de fixture. Em integração futura, geração de pseudônimo exige política explícita; restrição de string/schema não prova ausência universal de PII. SDK não coleta logs automaticamente.

JSON UTF-8 sem compressão, um evento/POST, até 16 KiB. C01 fixa canonicalização JSON compartilhada: ordenar propriedades recursivamente, sem whitespace, strings UTF-8 e sem floats/campos livres neste envelope. UUIDs e timestamps são normalizados antes de comparar; C01 fixa precisão UTC e fixtures em todas as linguagens. Deduplicação tenant+event_id por conteúdo normalizado; diferente é conflito. Hash é de conteúdo, não prova de autenticidade da origem.

## Ingestão

`POST /v1/telemetry/ingest`, Bearer de SDK e Content-Type application/json. 201 após append durável novo; 200 replay idêntico; 400 schema/versão inválidos; 401 chave ausente/inválida/revogada; 403 source/escopo inválido ou source disabled; 405 método; 409 ID conflitante; 413 corpo; 415 tipo/compressão; 429 limite; 503 dependência indisponível. Modo em memória IN01 é local e não comprova persistência em disco.

Sucesso: event_id, request_id UUID e status stored/duplicate. Erro: code estável e request_id, sem eco de body/segredo. Replays são reautorizados; não contornam revogação/status. Fonte inexistente ou disabled não grava. Resposta de sucesso não depende da execução do detector: evento aceito fica disponível para polling durável.

IN02: 10 requisições autenticadas/min por credencial, janela fixa de recebimento, incluindo replay, 429 com Retry-After; inicialmente por processo. ST02: 24h por received_at, teto 10.000 eventos ou 10 MiB JSON canônico/tenant, configurável. Expurgar expirados antes de append; sem espaço, rejeitar com 429 capacity_exceeded, sem expulsar recentes. Esses são limites do laboratório, não cotas ou capacidade prometida de fornecedor. Execução remota exige limite específico decidido em IF04.

## API interna e portas

C03 define paths, schemas e proteção dos endpoints internos de verify/append, com segredo próprio, timeout 2s e request_id. Core é único dono dos arquivos SQLite. Não aceitar contexto de tenant de um chamador não autenticado.

- Identity.verify: chave → tenant_id, credential_id, source_id, scopes; falha fechada. Chave inicial events:write, 256 bits, somente hash persistido. Source/status é revalidado pelo core antes de append.
- HumanIdentity.verify: JWT humano → user_id interno, membership e papel no tenant; viewer lê e analyst altera. Emissor/JWKS de demonstração só loopback, proibido no perfil remoto. Fixtures/CLI provisionam memberships locais.
- EventStore.append: contexto autorizado+evento → stored/duplicate/conflict/capacity_exceeded. Core atribui received_at e ingest_seq monotônico. List usa cursor opaco por ingest_seq, padrão 20/máximo 100. Occurred_at não é checkpoint.
- Detector.evaluate: tenant+evento normalizado+estado/relógio explícitos → sinais; sem HTTP/relógio global na função pura.
- AlertStore: gravação única tenant+alert_id e occurrence_key, lista/detalhe/status. Referências e snapshots pertencem ao mesmo tenant. Sequência monotônica de criação alimenta consumidores de notificação/exportação.

DE03 usa executor único e checkpoint recuperável, salva efeitos antes de avançar e reconstrói janela no restart. Lacuna de retenção vira coverage_gap; não alegar análise completa do período expurgado. DE05 acrescenta estado transacional para dois processos no mesmo host, não SQLite por rede.

## Alertas e perfis de regra

Alerta v1: schema_version `1`, alert_id UUID, tenant_id interno, rule_id/version, severity calculada, event_ids, detected_at, status new/acknowledged/resolved e reason_code. occurrence_key é chave interna determinística; repetição não reabre status.

- Demo opt-in DE01: demo.auth_failure, uma falha→low. Desabilitada nos exercícios integrados.
- Básico DE02/DS03: somente auth.failure_threshold, cinco falhas únicas por tenant/source/subject em janela inclusiva de 60s→medium. Ocorrência ancorada no quinto evento, cooldown 60s por chave. Quatro falhas→zero alertas.
- Ampliado DE06/DS04: somente auth.failure_then_success, cinco falhas em 60s e depois sucesso em até 120s desde a quinta falha, mesmo tenant/subject/namespace e ao menos duas sources vinculadas→high. Âncora no success event_id e cooldown 120s por conjunto de sources/subject. Regra medium desabilitada nesse perfil.

Eventos são ordenados por occurred_at/event_id para avaliar candidatos; replay não conta. DE02 mantém até 24h de histórico limitado e reavalia região afetada por atraso. Ocorrências já emitidas não são apagadas; chegada tardia pode revelar ocorrência adicional, sem reescrever a anterior. Fixtures fixam chegadas, fronteiras, âncoras e reinício. Não prometer mesma quantidade de alertas em diferentes ordens de chegada arbitrárias; exigir replay idempotente do mesmo histórico recebido.

DE04 suporta templates equality e auth_failure_threshold com operadores allowlisted. Sigma DE08 suporta somente mapa selection com igualdade escalar AND em kind/severity/source_id e condition `selection`. Rejeitar listas/modifiers/OR/not/regex/agregação/campos desconhecidos. DE07 emite documento de análise CLI separado (analysis_version, subject, janela, score 0..100 e fatores rule/version/occurrence_key/points), sem alterar alerta v1 ou persistir score.

## Consulta humana

ST03 persiste; ST05 (#73) expõe GET /v1/alerts (items/next_cursor, filtros status/severity), GET /v1/alerts/{id} (detalhe/evidência) e PATCH status. Sem sessão 401; viewer escreve 403; objeto de outro tenant 404; transição inválida 409. Somente new→acknowledged→resolved. Não há comentários, atribuição ou casos empresariais nesta fase.

UI02 recebe token do emissor demo por fluxo documentado, guarda só em memória e limpa sessão no logout/401. Channel opcional publica só ID/tipo e deriva tópico do tenant; expiração e perda de membership interrompem avisos. Reconexão relê HTTP.

## Extensão 1.1 emitida por SDK

Mesma rota, schema_version `1.1`, campos base v1. Mantém auth.failure/success com subject_id obrigatório e acrescenta somente:

| Kind | Campos específicos |
| --- | --- |
| source.heartbeat | platform fixo `server`; app_version 1–32 caracteres de versão; sem subject_id |
| app.access_denied | subject_id pseudônimo e action_id de catálogo (`demo.admin.read`) |
| analytics.signal | subject_id pseudônimo, signal_id de catálogo (`demo.sensitive_action`), trust_level fixo `client_reported` |

Campos específicos só são aceitos no kind correspondente. Analytics é relato consentido do browser construído pelo próprio backend, nunca prova autoritativa; só backend envia por SDK TS. Heartbeat exige chamada explícita ao SDK, sem coleta automática. Não há device.posture, evento de conta/provedor, SQL, Redis, bucket ou webhook de plataforma. TS ganha 1.1 em SK07; Python/Go permanecem v1 nesta fase.

## Sources, contato e vínculos

RG01: source_id pseudônimo, tenant interno, tipo sdk-server, status enabled/disabled; API create/list/detail/status autorizada por identidade humana. Chave é emitida via CLI ID01 e revogada/rotacionada via ID02, sem pareamento. Source reabilitada não reativa chave revogada.

RG03: last_seen_at só após append novo persistido, por received_at; replay/rejeitado não renova. Unknown sem evento, fresh até 30min inclusive, stale depois (limiar configurável). Silêncio não significa ataque nem segurança.

RG04 (#76): CLI administrativa cria/lista/remove vínculo persistente entre sources do mesmo tenant e namespace de subject compartilhado explicitamente. Detector não deduz vínculo por email/IP ou apenas por strings iguais. Desabilitar source impede novas correlações com ela.

## Evidências e saídas

ST04 estende o caminho obrigatório de criação do alerta: snapshot mínimo de eventos, hash canônico, regra/versão e horário em transação. Buffer expurgado mantém snapshot; snapshot até 7 dias ou 10 MiB JSON canônico/tenant, antigos primeiro. Depois preservar evidence_expired. Não prometer investigação completa após expurgo.

Outbox lê criação do AlertStore por sequência e insere tarefas+checkpoint em transação recuperável. Chave tenant+alert_id+destino; claim com lease 30s; cinco tentativas totais, teto Retry-After 30s e expiração 24h; 400/401 falha final. Default high/critical, por isso DS03 medium não exige Discord. NT03 roteia antes de enfileirar; cooldown 5min tenant/conjunto ordenado de sources/rule. Destino ausente/supressão são resultados persistidos, sem apagar alerta.

Discord recebe somente alert_id, regra/versão, severity, reason_code e link em origem allowlisted; sem body/evento bruto e allowed_mentions desativado. Segredo só no servidor. Fake externo nos testes; após aceite externo e falha de confirmação pode haver duplicata.

Loki é exportação de AlertStore por checkpoint próprio, timeout 2s e até três tentativas por ciclo, espera até 30s; falha não avança cursor. Labels somente ambiente/regra/severidade. Isolamento local por stack/credencial por tenant; Grafana não usa filtro visual como autorização. Retenção externa e duplicatas possíveis são independentes do expurgo Watch.

## Fixtures comuns

Tenant/source A e B, chaves válidas/inválidas/revogadas, source disabled, evento repetido/conflitante, janela abaixo/no limiar, sequência multifonte vinculada e sem vínculo, falha de dependência, evidência expirada e versões 1/1.1. IDs artificiais, relógio fixo e credenciais sem validade externa. Mocks ajudam a começar, não encerram E2E real exigido.
