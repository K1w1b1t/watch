# Escopo e arquitetura

## Resultado do laboratório

SDK emite autenticação sintética, ingresso valida e autoriza, core persiste, detector explica sequência e painel consulta/reconhece/resolve alerta. O fluxo é demonstrável sem conta paga; não é uma declaração de prontidão empresarial.

| Componente | Responsabilidade | Limite |
| --- | --- | --- |
| SDK TS/Python/Go | Construir/validar evento e enviar por HTTPS/HTTP local | Só observa chamadas explícitas da aplicação |
| CLI/Next/Nest/backend React | Exemplificar uso do SDK | Sem transporte de ingestão próprio ou API de plataforma |
| Worker TypeScript | Método/corpo/schema, chave/source, rate limit | Não grava SQLite nem decide tenant pelo body |
| Core Elixir/Phoenix | API interna, identidade, sources, dados e API humana | Único dono de SQLite local |
| Detector Elixir | Regras, estado e executor recuperável | Um processo no perfil básico; DE05 testa concorrência local |
| React | Lista, detalhe e status | Só acessa API autorizada |
| Outbox | Roteamento e entrega Discord | Retentativa limitada, possível duplicata externa |
| Exportador/Loki/Grafana | Cópia saneada e visualização opcional | Não substitui AlertStore/autorização |
| CI/composição local | Reproduzir demonstrações e verificar artefatos | Sem deploy automático ou dados de terceiros |

## Transporte e autoridade

C03 (#72) formaliza OpenAPI e ADR local antes das integrações: Worker→core por HTTP interno autenticado, timeout 2s, request_id propagado e falha fechada. Credencial interna não é chave de SDK ou token humano. Eventos entram com tenant derivado da chave; o core revalida source/status. Tenant do envelope público é rejeitado.

SQLite contém autoridade de credenciais, sources, eventos, alertas e outbox no core; só processos do core no mesmo host/volume podem acessá-lo. Worker não compartilha arquivo SQLite e não mantém banco autoritativo paralelo. Em memória só nos modos de teste explicitamente identificados. ST03 é porta persistente, ST05 implementa HTTP humano.

Persistir evento e responder 201/200 precede a análise por executor polling local. Não é necessário executar detector dentro da requisição. DE03 lê por ingest_seq, controla checkpoint/replay e indica lacuna quando retenção remove contexto. Notificação nasce de leitura recuperável do AlertStore, não de callback volátil.

## Tecnologias propostas

TypeScript/Workers/Wrangler/Vitest; Elixir/Phoenix/ExUnit/SQLite; React/Vite/Playwright; SDK Python e Go com suíte compartilhada; contêineres locais e GitHub Actions. Terraform apenas nos recursos suportados dos laboratórios opcionais. Nova ferramenta exige ADR curto com problema, alternativas e consequência.

## Modos e extensões

Local é obrigatório; IF01 começa com perfil mínimo identificado e DS03 usa perfil integrado real. CLI CO01 usa SDK TS, CO02 adiciona retry/fila opcional ao SDK. V1 é autenticação; 1.1 acrescenta heartbeat, acesso negado e analytics explícitos por SDK, sem conectores. IN04 e SK07 comprovam integração dessas versões.

Remoto é opcional e começa em IF04. IF02 é smoke de Worker/D1 isolado, sem rota pública v1 nem migração dos dados do core. IF03 prepara Terraform de VM nova dedicada, sem apply. Nenhuma issue atual autoriza ocupar a VPS usada por outros projetos. Publicar smoke Worker não publica o Watch.

Apps nativos, sensores e adaptadores de plataformas estão adiados. Aplicação de cliente, onboarding empresarial, investigação/casos, tuning operacional, backup/restore integrados e testes de capacidade/latência de produção não são critérios desta fase.
