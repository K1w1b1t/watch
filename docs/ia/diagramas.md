# Diagramas gerais do Watch

Código Mermaid incorporado à documentação, editável e renderizado no GitHub. Estes diagramas consolidam os planos de arquitetura, detecção e identidade; descrevem o sistema pretendido, não componentes já implementados. O [plano geral](plano-geral.md) explica as decisões e o [backlog](backlog.md) identifica incrementos.

## Visão geral: origens, borda, core e saídas

Linhas contínuas representam fluxos planejados no backlog atual. Linhas tracejadas representam extensões em estudo, condicionadas a teste, privacidade e custo. SDKs/agentes enviam diretamente ao Watch; adaptadores externos ampliam a cobertura quando a plataforma oferece o sinal.

```mermaid
flowchart TD
    subgraph Clients ["1. SDKs, apps e fontes"]
        ServerSDK["SDKs servidor: TypeScript, Python e Go"]
        Apps["Next.js e NestJS: logger e eventos instrumentados"]
        Browser["React browser: sinal consentido via backend"]
        Desktop["App Desktop: Tauri e Rust"]
        Mobile["App Mobile: React Native, Android e iOS"]
        Connectors["Adaptadores: Supabase, Upstash, PostHog e Oracle"]
    end
    subgraph Edge ["2. Borda e controle de acesso"]
        Ingest["Worker de ingestao: limites, credencial e schema"]
        Registry["Sources e credenciais por tenant"]
        IdentityDB[("D1 ou adaptador local autoritativo")]
    end
    subgraph Data ["3. Eventos e evidencias"]
        Buffer[("Buffer curto de eventos minimizados")]
        Evidence[("Alertas e snapshots de evidencia")]
        Archive["Pesquisa futura: Axiom ou R2 e Parquet"]
    end
    subgraph Core ["4. Analise: Elixir e Phoenix"]
        Rules["Catalogo de regras versionadas"]
        Engine["Limiar, sequencia, correlacao e risco explicavel"]
        State[("Estado temporal consistente e limitado")]
        Scale["Pesquisa futura: Broadway e ETS"]
        API["API autorizada e Phoenix Channels"]
    end
    subgraph Output ["5. Visualizacao e notificacao"]
        React["Painel React"]
        MobileRead["Consulta de alertas no app mobile"]
        Outbox["Outbox: retry, roteamento e cooldown"]
        Discord["Discord: resumo saneado"]
        Exporter["Exportador no servidor"]
        Grafana["Loki e Grafana opcionais"]
    end
    subgraph Intel ["Evolucao de inteligencia e operacao"]
        Sigma["Sigma: subconjunto validado"]
        Feeds["Pesquisa futura: STIX e TAXII"]
        Catalog["Pesquisa futura: catalogo exato e Bloom filter"]
        Review["Fixtures, revisao e artefato versionado"]
        Terraform["Terraform e CI: infraestrutura e verificacoes"]
    end
    Browser --> Apps
    Apps --> ServerSDK
    ServerSDK --> Ingest
    Desktop --> Ingest
    Mobile --> Ingest
    Connectors --> Ingest
    Ingest --> Registry
    Registry --> IdentityDB
    Ingest --> Buffer
    Buffer --> Engine
    Rules --> Engine
    Engine <--> State
    Engine --> Evidence
    Evidence --> API
    API --> React
    API --> MobileRead
    Evidence --> Outbox
    Outbox --> Discord
    Evidence --> Exporter
    Exporter --> Grafana
    Sigma --> Review
    Review --> Rules
    Feeds -.-> Catalog
    Catalog -.-> Engine
    Scale -.-> Engine
    Evidence -.-> Archive
    Terraform --> Edge
    Terraform --> Output
```

O buffer protege o caminho de reprocessamento e mantém contexto limitado. A evolução para processamento por fila/stream exige aceite durável e falhas especificadas. Não encaminhar somente eventos que bateram em IoC: a correlação também precisa de eventos sem indicador conhecido.

## Autenticação humana, pareamento e ingestão de máquina

```mermaid
sequenceDiagram
    actor Analyst as Analista
    participant UI as Painel ou app de consulta
    participant IdP as Provedor de identidade
    participant Auth as API de identidade e fontes
    participant DB as Fonte autoritativa de credenciais
    participant Sensor as SDK ou app sensor
    participant Ingest as Ingresso Watch
    participant Store as Buffer de eventos
    Analyst->>UI: Iniciar sessao
    UI->>IdP: Autenticar com politica MFA
    IdP-->>UI: Token humano
    UI->>Auth: Cadastrar source com token
    Auth->>Auth: Validar JWT, membership e papel
    Auth->>DB: Registrar source no tenant autorizado
    UI->>Auth: Solicitar pareamento temporario
    Auth-->>UI: Codigo de uso unico e curta validade
    Analyst->>Sensor: Informar URL e codigo
    Sensor->>Auth: Resgatar codigo
    Auth->>DB: Consumir codigo atomicamente e salvar hash da chave
    Auth-->>Sensor: API key individual exibida uma vez
    Sensor->>Sensor: Armazenar segredo com mecanismo seguro
    Sensor->>Ingest: Evento permitido e Bearer API key
    Ingest->>DB: Verificar hash, status, source e escopo
    DB-->>Ingest: Contexto autorizado ou negacao
    alt Credencial e evento validos
        Ingest->>Store: Persistir evento idempotente por tenant
        Store-->>Ingest: Stored ou duplicate
        Ingest-->>Sensor: 201 ou 200 conforme contrato
    else Invalido ou dependencia indisponivel
        Ingest-->>Sensor: Erro explicito sem revelar segredo
    end
```

SDKs server-side podem receber URL e API key diretamente da configuração secreta do operador; pareamento é o caminho de onboarding dos dispositivos. O browser utiliza o backend da aplicação e não armazena essa chave. Revogação é conferida na fonte autoritativa; KV não garante invalidação global instantânea.

## Atualização de regras e inteligência

```mermaid
flowchart LR
    S["Regra Sigma ou catalogo local"] --> Q["Validar schema, licenca e proveniencia"]
    F["Feed STIX ou TAXII em estudo"] -.-> Q
    Q --> T["Fixtures positivas, negativas e de compatibilidade"]
    T --> R["Revisao de impacto, privacidade e custo"]
    R --> V["Artefato versionado e rollout controlado"]
    V --> E["Motor Watch"]
    E --> M["Medir ruido, lacunas e consumo"]
    M --> B["Ajustar ou reverter versao"]
    B --> T
```

Atualizar regras não equivale a executar Terraform recebido de um feed. Provisionamento de infraestrutura e promoção de conteúdo analítico são responsabilidades distintas. STIX/TAXII, Bloom filters e adoção de Broadway permanecem propostas de pesquisa descritas no plano geral.

## Ajustes em relação ao desenho inicial

- Incluir mobile, SDKs, Discord, Grafana e registro de fontes na mesma visão.
- API autorizada entre frontend e dados; remover acesso direto do React ao D1 e credenciais de busca.
- Validação autoritativa de credenciais; remover a hipótese de latência zero e revogação instantânea por KV.
- Separar esquema reduzido Watch de futura conversão OCSF e separar regras determinísticas de pesquisa avançada.
- Retenção curta/evidência seletiva como base; Axiom, R2 premium e recursos de monetização como alternativas futuras.
- Revisão e testes antes de promover regras/feeds; sem promessa de detecção infalível, escala ilimitada ou custo garantido em qualquer volume.
