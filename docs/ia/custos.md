# Custos e operação do laboratório

Execução local sem conta paga é requisito. Gratuidade remota depende de plano/cota elegível e não garante custo zero em qualquer escala. Esta documentação não certifica preços atuais; revalidar e datar fontes oficiais durante IF04.

| Recurso | Uso atual | Condição |
| --- | --- | --- |
| Local, contêineres e SQLite | Base obrigatória | Volume privado e recursos limitados |
| Workers/D1 | Smoke test opcional IF02 | Isolado, sem migrar dados/identidade nem publicar ingestão real |
| VM remota nova dedicada | Plano opcional IF03 | Sem apply nesta issue; nunca ocupar VPS existente |
| Emissor/JWKS local | Identidade demo ID03 | Não equivale a IdP/MFA empresarial |
| Discord fake | Notificação nos E2Es | Nenhuma conta real exigida |
| Loki/Grafana locais | Saída opcional | Sem serviço cloud obrigatório |

## Gate remoto

IF04 decide topologia completa antes de IF02/IF03: componente, autoridade dos dados, TLS, credencial, plano/cota, consumo compartilhado e remoção. Somar disco, IP, tráfego, state, logs e qualquer dependência; não depender de créditos/trials. Sem limite compatível ou capacidade elegível, manter local. Alertas de orçamento não bloqueiam cobrança.

Terraform plan revisável, state/segredos fora do Git, menor privilégio e teardown restrito aos recursos Watch. Nenhum domínio pago, alteração de outros projetos ou upgrade automático. IF02 só declara provisionamento testado após E2E remoto; inelegibilidade deixa essa validação pendente. IF03 entrega preparação validada, não deploy ou restore implementado.

## Limites internos

Um evento/POST até 16 KiB; 10 requisições/min/credencial por processo inicialmente; buffer 24h, até 10.000 eventos ou 10 MiB JSON canônico/tenant; snapshots 7 dias ou 10 MiB/tenant. São limites de laboratório detalhados nos contratos, sem promessa de capacidade empresarial. O antigo teto genérico de 1.000 eventos/dia foi retirado para evitar um limite sem implementação atribuída; IF04 define eventual orçamento remoto verificável antes de execução.

SDK TS tem timeout 2s e fila/retry opcionais limitados; fila em memória perde ao reiniciar. Python/Go começam sem fila/retry. Não ampliar plano ao saturar: rejeitar explicitamente conforme contrato. Nenhum SDK exige plataforma específica de hospedagem.

Axiom, R2 e outros destinos cloud continuam pesquisa futura. Adaptadores de plataformas e apps nativos estão adiados, portanto seus planos/distribuição não são pré-requisitos. Backup/restore integrados e prontidão empresarial não fazem parte desta fase.
