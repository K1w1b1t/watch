# O projeto em desenhos

**Use este documento para:** acompanhar visualmente uma mensagem. As regras exatas estão nos [contratos](contratos.md), e os programas/línguas estão na [arquitetura](arquitetura.md).

## 1. Do acontecimento ao alerta

Leia da esquerda para a direita. A aplicação decide relatar; o SDK envia; o Watch guarda; o detector analisa o que foi guardado; o painel consulta o alerta.

```mermaid
flowchart LR
    APP["Aplicação relata falha de login"] --> SDK["SDK monta e envia evento"]
    SDK --> ENT["Entrada verifica pedido e chave"]
    ENT --> SRV["Servidor principal confere fonte"]
    SRV --> DB[("Guarda evento no SQLite")]
    DB --> DET["Detector procura sequência de falhas"]
    DET --> ALERT[("Guarda alerta e evidência")]
    ALERT --> API["API confere usuário e organização"]
    API --> UI["Painel mostra e permite tratar alerta"]
```

O detector lê depois da gravação. Confirmar que recebeu um evento não significa que ele já foi analisado. Um alerta guarda os eventos que justificaram a regra; não é só uma luz vermelha sem explicação.

## 2. Por onde entram os dados

Todos os caminhos chegam ao mesmo ingresso por SDK server-side. React passa pelo próprio backend para que a chave fique protegida.

```mermaid
flowchart TD
    CLI["Programa de terminal"] --> TS["SDK TypeScript"]
    NEXT["Next / Nest no servidor"] --> TS
    REACT["React com consentimento"] --> BACK["Próprio backend valida sessão e sinal"]
    BACK --> TS
    PY["Programa Python"] --> PSDK["SDK Python"]
    GO["Programa Go"] --> GSDK["SDK Go"]
    TS --> ENT["Entrada do Watch"]
    PSDK --> ENT
    GSDK --> ENT
```

Não há coleta de dados da hospedagem ou de contas externas nesse desenho. A chave de SDK não aparece no React.

## 3. O que acontece se algo falhar no recebimento

```mermaid
sequenceDiagram
    participant App as Aplicação com SDK
    participant E as Entrada TypeScript
    participant S as Servidor Elixir
    participant DB as SQLite do servidor
    App->>E: Evento com chave de escrita
    E->>S: Verificar chave pela API interna protegida
    S-->>E: Contexto autorizado ou erro
    E->>S: Pedir gravação com contexto autenticado
    S->>DB: Conferir fonte e gravar sem duplicar
    alt Gravação confirmada
        DB-->>S: Novo ou já existente
        S-->>E: Resultado persistido
        E-->>App: 201 ou 200
    else Dependência indisponível
        S-->>E: Falha
        E-->>App: 503, sem confirmação falsa
    end
```

Pedidos inválidos são rejeitados antes de guardar. O servidor controla a organização e a fonte; um campo recebido do cliente não concede permissão.

## 4. Saídas do alerta e retomada

```mermaid
flowchart LR
    A[("Alerta no Watch")] --> P["API e painel"]
    A --> L["Leitor com posição salva"]
    L --> O[("Fila de notificações: outbox")]
    O --> D["Resumo para Discord falso nos testes"]
    A --> X["Exportador com posição salva"]
    X --> K["Loki local"]
    K --> G["Grafana local protegido"]
```

A fila mantém tarefas após reinício. O exportador guarda até onde confirmou envio. Uma falha no destino não apaga o alerta original. Após aceite externo e perda da confirmação, pode ocorrer duplicata de mensagem/cópia, conforme os limites do contrato.

## 5. Nossos dois resultados completos

```mermaid
flowchart LR
    B["DS03: SDK TS e cinco falhas"] --> M["Um medium e tratamento no painel"]
    V["DS04: Python + Go vinculados"] --> H["Um high com seis evidências"]
    H --> S["Discord falso e gráfico local"]
```

Esses desenhos não são um grafo completo de dependências. Para escolher a próxima tarefa, use a [ordem de execução](ordem-execucao.md); para dividir o trabalho, use o [guia da dupla](trabalho-em-dupla.md).
