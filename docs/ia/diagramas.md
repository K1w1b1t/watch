# Diagramas do escopo vigente

Descrevem o laboratório planejado, não capacidades já implementadas. Somente SDKs server-side enviam ao ingresso; setas de Discord/Loki são saídas.

```mermaid
flowchart LR
    NEXT[Next e Nest exemplos] --> TS[SDK TypeScript]
    CLI[CLI demo] --> TS
    REACT[React consentido] --> BACK[Backend proprio autenticado]
    BACK --> TS
    PY[App demo Python] --> PS[SDK Python]
    GO[App demo Go] --> GS[SDK Go]
    TS --> W[Worker ingresso]
    PS --> W
    GS --> W
    W -->|HTTP interno autenticado| CORE[Core Elixir Phoenix]
    CORE --> DB[(SQLite exclusivo do core)]
    DB --> DET[Executor detector com checkpoint]
    DET --> ALERT[(AlertStore e snapshots)]
    ALERT --> API[API humana autorizada]
    API --> UI[Painel React]
    ALERT --> OUT[Outbox recuperavel]
    OUT --> DISC[Discord fake no E2E]
    ALERT --> EXP[Exportador com checkpoint]
    EXP --> LOKI[Loki local]
    LOKI --> GRAF[Grafana local protegido]
```

```mermaid
sequenceDiagram
    participant App as Aplicacao exemplo
    participant SDK as SDK server-side
    participant Worker as Ingresso
    participant Core as API interna core
    participant DB as SQLite do core
    App->>SDK: Evento explicito sem segredo no envelope
    SDK->>Worker: POST com chave events:write
    Worker->>Core: Verify autenticado por credencial interna
    Core->>DB: Consultar hash e contexto
    Core-->>Worker: Identidade ou erro
    Worker->>Core: Append com contexto autenticado
    Core->>DB: Validar source enabled e gravar idempotente
    DB-->>Core: Stored, duplicate ou erro
    Core-->>Worker: Resultado persistente
    Worker-->>SDK: 201, 200 ou erro explicito
    SDK-->>App: Resultado tipado
```

O core revalida source/status na gravação; contexto arbitrário no body não autoriza tenant. Detector lê o buffer depois do aceite, por ingest_seq; sucesso HTTP não promete detecção imediata. Queda de processo é tratada por checkpoint/replay e falha de entrega por outbox. Expurgo anterior à análise gera lacuna explícita.

```mermaid
flowchart TD
    C[C01 schemas v1] --> T[C03 transporte local]
    C --> E[C02 extensao 1.1]
    T --> BASE[IN03 caminho persistente v1]
    BASE --> V[IN04 ingresso 1.1]
    E --> V
    V --> SDK[SK07 SDK TS 1.1]
    BASE --> D3[DS03 SDK TS ate painel]
    SDK --> D4[DS04 SDKs ate Discord e Grafana]
    D3 --> CHECK[Perfil basico demonstrado]
    D4 --> CHECK2[Perfil ampliado demonstrado]
```

Este desenho resume capacidades; a lista completa de bloqueios está no backlog e nas issues. IF04 decide remoto antes de IF02 smoke e IF03 plano; nenhum deles publica automaticamente o SOC completo.
