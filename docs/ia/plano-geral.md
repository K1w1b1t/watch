# Plano geral do Watch

## Objetivo e primeira entrega

Aprender e construir uma pipeline de segurança: aplicação demonstra eventos explícitos por SDK, ingresso valida/autentica, core persiste e detecta sequência sintética, analista consulta evidência e altera status no painel. A primeira entrega integrada é DS03, executável localmente. DS04 amplia para múltiplos SDKs e saídas locais.

## Fontes e integrações

Os únicos emissores nesta fase são SDKs TypeScript, Python e Go server-side. A CLI reutiliza o SDK TS. Next.js/NestJS demonstram instrumentação de aplicação independente do provedor. React chama o próprio backend autenticado com consentimento; só o backend possui chave e chama SDK.

Não há descoberta automática de eventos: sem chamada ao SDK, não há visibilidade. Não há adaptadores de plataformas, polling de contas, webhook de ingresso de terceiros, sensor de SO, app desktop/mobile ou pareamento. Discord é um webhook de saída e não um novo ingestor.

## Componentes

1. **Contratos:** C01 formaliza v1; C02 formaliza 1.1; C03 define a conexão HTTP entre Worker e core e o dono de cada dado. Fixtures são compartilhadas entre linguagens.
2. **Ingresso:** Worker TS local recebe um evento por requisição, até 16 KiB, verifica chave/source, limita taxa e chama o core. Sucesso depende de persistência. IN04 integra 1.1 ao caminho real.
3. **Core:** Elixir/Phoenix é dono de SQLite, credenciais, sources, eventos, alertas e outbox. API interna exige segredo distinto; API humana exige JWT/membership. Worker não abre SQLite diretamente.
4. **Identidade e sources:** CLI local emite/rotaciona/revoga chave events:write. Usuários usam emissor/JWKS de demonstração local e papéis viewer/analyst. Sources podem ser desabilitadas; IN03 verifica isso no append. RG04 persiste vínculos explícitos para correlação multifonte.
5. **Detecção:** funções puras precedem executor com polling/checkpoint. Regra low de DE01 é opt-in didático. Perfil básico usa somente limiar de cinco falhas→medium; perfil ampliado usa somente falhas seguidas de sucesso→high. DE05 testa concorrência de processos no mesmo host, sem prometer escala distribuída.
6. **Retenção:** buffer até 24h, 10.000 eventos ou 10 MiB de JSON canônico por tenant. Expurgar expirados; se ainda cheio rejeitar novos com 429, sem expulsão silenciosa. Snapshots selecionados até 7 dias ou 10 MiB/tenant, expiração visível.
7. **Consulta:** ST03 persiste alertas; ST05 fornece HTTP autorizado; React oferece lista/detalhe/reconhecimento/resolução. Phoenix Channel é aviso opcional, com recuperação por HTTP.
8. **Saídas:** varredura recuperável AlertStore→outbox→Discord fake/local nos testes. Exportador com checkpoint envia resumo saneado a Loki; Grafana local protegido visualiza. Saídas externas podem duplicar após aceite e falha de confirmação.
9. **Verificação:** CI acompanha artefatos existentes, contratos e E2Es. OB03 instrumenta falhas técnicas do laboratório, sem introduzir projeto de operação empresarial.

## Entregas e decisões

O [backlog](backlog.md) é o mapa de issues e bloqueios. Cada descrição define valor, limite, teste observável e exclusões. Um módulo pode começar isolado; integração não fecha com mock de componente exigido como real. Alteração de contrato exige atualizar consumidores/fixtures afetados.

Modo remoto é opcional: IF04 decide topologia e custo antes de IF02/IF03. IF02 valida recursos Worker/D1 de smoke test isolado; não publica ingestão real. IF03 entrega plano Terraform revisável, sem apply. A VPS existente de outros projetos não é alvo.

## Fora desta fase

Adaptadores e integrações de plataformas, agentes/sensores/apps nativos, pareamento, sinais de contas externas, monetização, retenção longa, SMS/localização, consultas de vazamentos, resposta automática e IA autônoma. Também ficam adiados aplicação real de empresa, onboarding empresarial, casos de investigação, tuning operacional, backup/restore integrados e metas empresariais de capacidade/latência.

## Pesquisa preservada

OCSF, STIX/TAXII, Bloom filters, Broadway/ETS, baselines, Axiom e R2/Parquet permanecem ideias de estudo, sem dependência do laboratório. O subconjunto Sigma está delimitado em DE08; não implica compatibilidade integral. Nenhum indicador, score ou erro de aplicação confirma comprometimento. Pesquisa só vira execução após issue pequena com contrato, fixture e limite explícitos.

A [revisão inicial](revisao-planejamento-2026-11-03.md) é registro histórico anterior a este recorte; suas propostas de piloto empresarial não são requisitos atuais.
