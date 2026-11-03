# Contratos: o que cada parte pode enviar e receber

**Use este arquivo durante a implementação.** Ele é a referência dos campos, respostas e limites. Se ainda não conhece o projeto, comece pelo [exemplo geral](plano-geral.md); os termos estão no [glossário](glossario.md).

Um contrato evita que o SDK mande uma coisa e o servidor espere outra. C01 transforma a versão 1 abaixo em arquivos de validação e exemplos; C02 faz isso para 1.1; C03 documenta a API interna. Essas entregas ainda precisam ser implementadas.

## 1. Entenda os três identificadores

| Identificador | O que representa | De onde vem |
| --- | --- | --- |
| tenant_id | Organização cujos dados estamos tratando | Contexto interno obtido da credencial; nunca aceito do JSON público |
| source_id | Aplicação/fonte que envia, como app-a | Cadastro de fonte, vinculada à chave do SDK |
| subject_id | Pessoa fictícia observada no evento, como pessoa-demo | Instrumentação do exemplo, usando a convenção combinada |

O usuário do painel é outra identidade. Sua sessão humana permite consultar/tratar alertas; a chave de source só escreve eventos.

## 2. Como é um evento v1

Exemplo de falha de login com dados fictícios:

```json
{
  "schema_version": "1",
  "event_id": "00000000-0000-4000-8000-000000000001",
  "occurred_at": "2026-11-03T12:00:00Z",
  "kind": "auth.failure",
  "severity": "info",
  "source_id": "app-a",
  "subject_id": "pessoa-demo"
}
```

O horário é ilustrativo. No teste, fixar o relógio nesse contexto; num envio atual, usar horário válido em relação ao relógio do servidor.

| Campo | Regra que o validador deve aplicar |
| --- | --- |
| schema_version | String `1` nesta versão |
| event_id | UUID minúsculo; gerar uma vez e manter em todo reenvio |
| occurred_at | RFC3339 em UTC; aceitar até 24h no passado e 5min no futuro |
| kind | `auth.failure` ou `auth.success` |
| severity | `info`, `low`, `medium`, `high` ou `critical`; é contexto da aplicação, não decisão do detector |
| source_id | De 1 a 64 caracteres alfanuméricos, hífen ou sublinhado; deve corresponder à chave |
| subject_id | Mesmo formato de source_id; obrigatório em autenticação |

Rejeitar campos desconhecidos e conteúdo livre. Não emitir senha, token, email, nome, CPF, IP, hostname ou linha de comando. Nos exemplos, usar somente IDs artificiais. Um campo string dentro do tamanho certo ainda pode carregar dado pessoal; a instrumentação também precisa respeitar essa restrição.

## 3. Como enviar e interpretar a resposta

Enviar um único evento por `POST /v1/telemetry/ingest`, com `Content-Type: application/json` e `Authorization: Bearer <chave-do-SDK>`. A credencial vai no cabeçalho, não dentro do evento. Não aceitar compressão. O JSON UTF-8 deve caber em 16 KiB.

| Código HTTP | Significa | O que acontece com a gravação |
| --- | --- | --- |
| 201 | Evento novo guardado | O caminho persistente confirmou append no banco |
| 200 | Reenvio idêntico | Não cria outra cópia |
| 400 | Formato, tipo de evento ou versão inválidos | Rejeita |
| 401 | Chave ausente, inválida ou revogada | Rejeita |
| 403 | Fonte/permissão incompatível ou fonte desativada | Rejeita |
| 405 | Método HTTP não permitido | Rejeita |
| 409 | Mesmo event_id com conteúdo diferente | Não substitui o evento existente |
| 413 | Corpo acima do tamanho permitido | Rejeita |
| 415 | Tipo de conteúdo ou compressão não suportados | Rejeita |
| 429 | Limite de pedidos ou buffer sem capacidade | Não confirma nova gravação |
| 503 | Dependência necessária indisponível | Não confirma gravação |

Resposta de sucesso: `event_id`, `request_id` UUID e `status` igual a `stored` ou `duplicate`. Resposta de erro: `code` estável e `request_id`, sem devolver corpo/chave recebidos. Request_id ajuda a acompanhar um pedido nos registros saneados.

IN01 usa armazenamento em memória, identificado como demonstração. IN03 substitui pelo armazenamento persistente: depois disso, 201/200 só são retornados após confirmação real. Essa confirmação não significa que o detector já concluiu a análise.

## 4. O que torna um reenvio igual

A identidade do evento é o par organização + event_id. Mesmo par e mesmo conteúdo normalizado é repetição; conteúdo diferente é conflito. Mesmo ID em outra organização é independente.

C01 define a representação padronizada: propriedades ordenadas recursivamente, sem espaços desnecessários, strings UTF-8, sem floats/campos livres no envelope. Também fixa a normalização de UUIDs e a precisão UTC dos horários em exemplos compartilhados. Isso impede comparar dois JSONs equivalentes como diferentes apenas por sua escrita.

Todo reenvio passa novamente por autorização. Não pode contornar revogação de chave ou desativação da fonte. Guardar o hash de uma evidência ajuda a comparar conteúdo, mas não prova que a aplicação de origem é confiável.

## 5. Limites de pedidos e armazenamento

| Limite | Valor inicial | Comportamento |
| --- | --- | --- |
| Pedido individual | 1 evento e até 16 KiB | Rejeitar excesso |
| Taxa por credencial | 10 pedidos autenticados/minuto, incluindo reenvios | Janela fixa por recebimento; 429 com Retry-After em IN02 |
| Buffer de eventos | 24h por received_at | Remover vencidos por manutenção |
| Volume do buffer por organização | 10.000 eventos ou 10 MiB de JSON normalizado | Primeiro teto atingido; remover vencidos antes de tentar append |
| Buffer continua cheio | Sem expulsar evento recente para abrir espaço | 429 com code capacity_exceeded |
| Cópias de evidência | Até 7 dias ou 10 MiB por organização | Remover as mais antigas; preservar indicação de expiração |

Os valores de volume/prazo são configuráveis. MiB aqui mede conteúdo JSON, não o tamanho físico completo do arquivo SQLite. O limitador inicial de IN02 é por processo; reinício e múltiplos Workers são limitações documentadas. Um limite remoto de consumo exige decisão específica em IF04.

## 6. Como os módulos conversam dentro do Watch

O servidor principal Elixir é o único dono dos arquivos SQLite. A entrada TypeScript usa HTTP interno para falar com ele. C03 formaliza endereços, exemplos, autenticação própria, tempo máximo de 2s e propagação de request_id. Chave de SDK e token humano não substituem a credencial interna.

| Operação técnica | Em português | Entrada → saída |
| --- | --- | --- |
| Identity.verify | Conferir chave de envio | Chave → organização, credencial, fonte e permissões, ou erro |
| HumanIdentity.verify | Conferir sessão de uma pessoa | JWT → usuário interno, vínculo e papel, ou erro |
| EventStore.append | Guardar evento | Contexto autorizado + evento → stored/duplicate/conflict/capacity_exceeded |
| EventStore.list | Ler uma página de eventos | Organização, cursor e limite → itens e próximo cursor |
| Detector.evaluate | Aplicar uma regra | Organização, evento, relógio e estado explícitos → zero ou mais resultados |
| AlertStore | Guardar/consultar/tratar alertas | Operações por organização com ocorrência única e status válido |

Falha de verificação não libera acesso. Antes de gravar, o servidor confere novamente fonte e estado. Um chamador interno sem autenticação não pode escolher uma organização arbitrária.

EventStore atribui `received_at`, o horário de recebimento, e `ingest_seq`, sequência crescente. A lista usa cursor opaco por essa sequência, 20 itens por padrão e no máximo 100. Um evento atrasado pode ter occurred_at antigo e ingest_seq novo; a leitura não deve pulá-lo.

## 7. Como o detector conta e retoma

| Perfil escolhido | Regra ativa | Resultado do exercício |
| --- | --- | --- |
| Didático, apenas por escolha explícita | demo.auth_failure v1 | Uma falha → low |
| Básico de DS03 | auth.failure_threshold v1 | Cinco falhas da mesma organização/fonte/pessoa em 60s → medium |
| Ampliado de DS04 | auth.failure_then_success v1 | Cinco falhas em 60s e depois sucesso em até 120s da quinta falha, com fontes vinculadas → high |

Só a regra do perfil escolhido fica ativa no exercício. A regra didática não gera low extras no básico; a regra medium não gera alertas extras no ampliado.

No básico, a janela de 60s é inclusiva e considera occurred_at; ordenar candidatos por occurred_at/event_id. A ocorrência é ancorada no quinto evento e tem espera de 60s por organização/fonte/pessoa. Reenvio não conta. Manter histórico limitado a 24h e 10.000 eventos/organização no estado de DE02.

Evento atrasado reavalia a parte afetada, sem apagar ocorrências já emitidas. Ele pode revelar nova ocorrência. Exigir repetição idempotente do mesmo histórico recebido, sem prometer quantidade igual de alertas para qualquer ordem arbitrária de chegada.

No ampliado, exigir mesmo subject_id, organização e namespace e pelo menos duas fontes explicitamente vinculadas. A ocorrência é ancorada no ID do sucesso; espera de 120s por pessoa/conjunto de fontes. Fonte desativada não participa de nova correlação.

DE03 lê por ingest_seq, guarda alerta antes de avançar seu checkpoint e reconstrói a janela após reinício. Uma occurrence_key identifica a mesma ocorrência e impede duplicar/reabrir alerta ao reler. Se a limpeza retirou contexto necessário, retornar coverage_gap e não afirmar análise completa. DE05 testa estado transacional entre dois processos no mesmo host, sem SQLite pela rede.

DE04 suporta modelos `equality` e `auth_failure_threshold`. O primeiro aceita combinação E de igualdades exatas em kind/severity/source_id; o segundo configura limiar/janela/espera dentro de limites documentados. A atualização de catálogo é completa: inválido não substitui parcialmente o anterior.

## 8. Como é um alerta e como a pessoa o consulta

| Campo de alerta v1 | Significado |
| --- | --- |
| schema_version | `1` |
| alert_id | UUID do alerta |
| tenant_id | Organização, apenas no contexto interno autorizado |
| rule_id / rule_version | Regra e versão usadas |
| severity | Gravidade calculada pela regra |
| event_ids | Eventos usados como evidência, todos da mesma organização |
| detected_at | Horário da detecção |
| status | `new`, `acknowledged` ou `resolved` |
| reason_code | Motivo estável que permite explicar a detecção |

O armazenamento usa também uma chave de ocorrência e sequência de criação internas. Repetir ocorrência não cria outro alerta nem reabre seu status. ST04 guarda o snapshot mínimo, seu hash, regra/versão e horário em transação com o alerta. Sem cópia após expiração, o detalhe sinaliza `evidence_expired`.

| Operação pública humana | Resultado |
| --- | --- |
| GET /v1/alerts | items/next_cursor; filtros opcionais status/severity |
| GET /v1/alerts/{id} | Detalhe com evidência permitida ou indicação de expiração |
| PATCH /v1/alerts/{id} | Alterar status na sequência new → acknowledged → resolved |

Sem sessão: 401. Viewer lê; escrita exige analyst, senão 403. Objeto de outra organização: 404. Mudança de status fora da sequência: 409. ST03 implementa armazenamento; ST05, esses endpoints.

ID03 fornece emissor/JWKS de demonstração só na máquina local, com vínculos cadastrados por exemplos/CLI. UI02 documenta obtenção do token, mantém sessão só em memória e a limpa em logout/401. O Channel opcional entrega só alert_id/tipo; expiração e perda de vínculo interrompem avisos. Reconectar relê a API.

## 9. O que muda na versão 1.1

Usar a mesma rota e `schema_version: "1.1"`. Manter campos básicos e autenticação de v1. Os campos específicos só podem aparecer no tipo correspondente:

| Tipo novo | Significado | Campos específicos |
| --- | --- | --- |
| source.heartbeat | A aplicação chamou explicitamente o SDK para avisar contato | platform fixo `server`, app_version de 1–32 caracteres de versão; sem subject_id |
| app.access_denied | Uma ação permitida no catálogo foi negada | subject_id e action_id `demo.admin.read` |
| analytics.signal | Relato consentido vindo do navegador, construído no próprio backend | subject_id, signal_id `demo.sensitive_action`, trust_level fixo `client_reported` |

Exemplo de heartbeat:

```json
{
  "schema_version": "1.1",
  "event_id": "00000000-0000-4000-8000-000000000002",
  "occurred_at": "2026-11-03T12:00:10Z",
  "kind": "source.heartbeat",
  "severity": "info",
  "source_id": "app-a",
  "platform": "server",
  "app_version": "0.1.0"
}
```

IN04 integra o servidor; SK07 integra SDK TS. Python/Go permanecem v1. O backend constrói analytics com sessão/consentimento, sem aceitar propriedades livres. Não há sensor automático ou evento de conta/provedor. Guardar um tipo sem regra não equivale a detectar ameaça nele.

## 10. Cadastro, contato e vínculo de fontes

RG01 guarda source_id, organização interna, tipo sdk-server e estado enabled/disabled. API humana permite cadastro, lista, detalhe e estado. ID01 cria chave por CLI; ID02 revoga/troca. Reativar fonte não revive chave revogada.

RG03 atualiza last_seen_at por received_at somente para evento novo realmente guardado. Reenvio/rejeição não renova contato. Sem evento: unknown; até 30min inclusive: fresh; depois: stale, com limiar configurável. O painel/consulta deve explicar “sem contato conhecido”, “recente” ou “atrasado”, sem interpretar silêncio como invasão/segurança.

RG04 cadastra vínculos explícitos entre fontes da mesma organização e namespace compartilhado de pessoa fictícia. Consultas do detector usam esse registro real. Igualdade de email/IP ou da string do identificador, sozinha, não cria vínculo.

## 11. Como sai uma notificação

NT02 lê a sequência de criação de AlertStore e grava tarefas + posição de leitura em transação. Assim, uma queda após salvar o alerta não perde o aviso. A tarefa tem chave organização/alerta/destino e reserva de trabalho de 30s.

Estados: pending, sent e failed, equivalentes a pendente, enviada e falha. Até cinco tentativas totais, Retry-After até 30s e expiração em 24h. 400/401 são falha final; reabertura exige comando administrativo. O padrão envia high/critical, por isso o básico medium não exige Discord.

NT03 define destino antes de enfileirar e aplica espera de 5min por organização/conjunto ordenado de fontes/regra. Aviso suprimido ou sem destino tem resultado registrado, mas não apaga alerta. Troca de configuração não reenvia histórico automaticamente.

Discord recebe apenas alert_id, regra/versão, gravidade, reason_code e link de origem permitida. Sem evento bruto e com allowed_mentions desativado. Chave/URL secreta só no servidor. Após aceite externo e perda de confirmação pode haver duplicata.

## 12. Exportação, pontuação e importação limitada

Loki recebe resumos por exportador com posição própria, timeout 2s, até três tentativas por ciclo e espera até 30s. Falha não avança a posição. Rótulos apenas ambiente/regra/severidade. Isolar organizações por instância/credencial; filtro no Grafana não é autorização. Retenção externa é independente da limpeza do Watch; duplicata externa é possível.

DE07 produz análise separada pela CLI: analysis_version=1, organização interna, subject_id, início/fim da janela, score inteiro 0..100 e fatores rule_id/version/occurrence_key/points. Janela padrão 120s, sem persistência e sem alterar alerta v1.

DE08 aceita Sigma apenas com detection.selection em mapa de igualdade escalar, combinação E em kind/severity/source_id e condition `selection`. Listas, modificadores, OU, negação, regex, agregação e campo desconhecido são erro. Preservar origem/licença; não executar código recebido.

## 13. Exemplos compartilhados obrigatórios

Todo módulo usa os mesmos dados fictícios: organizações/fontes A e B, chaves válidas/inválidas/revogadas, fonte desativada, evento repetido/conflitante, sequência abaixo/no limiar, fontes vinculadas/sem vínculo, dependência indisponível, evidência expirada e versões 1/1.1.

Fixar IDs e relógio nos testes. Um substituto controlado ajuda a desenvolver, mas não substitui os serviços reais exigidos nas issues de integração.
