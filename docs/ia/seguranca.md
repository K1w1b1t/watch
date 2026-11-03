# Segurança do laboratório

## Fronteiras de confiança

SDK/emissor é não confiável mesmo autenticado. Worker valida método/corpo/schema e chama core com credencial interna distinta. Core determina tenant pela chave e verifica source/status antes de persistir. Contexto fornecido pelo cliente não autoriza acesso. Falha de dependência fecha acesso e não produz sucesso falso.

Chaves de SDK: 256 bits aleatórios, somente hash SHA-256/metadados, comparação segura, exibição única e revogação consultada na autoridade do core. Chave de SDK só escreve eventos; não lê alertas nem autentica chamadas internas. Emissão/revogação são CLIs locais; sem pareamento. Nenhum segredo entra em erro/body/log.

JWT humano: assinatura/algoritmo/issuer/audience/expiração e membership/papel validados. Emissor/JWKS de desenvolvimento só loopback; nunca perfil remoto. Fixtures/CLI definem memberships. Viewer lê e analyst altera; API e Channel autorizam cada tenant/objeto. Token expirado ou membership retirada interrompe acesso correspondente.

React de exemplo envia somente sinal de catálogo ao backend same-origin com sessão, CSRF/origem, limite e consentimento. Backend constrói envelope e envia pelo SDK TS; chave não vai ao browser. Não há APIs de plataformas, agentes, sensores ou coleta oculta.

## Exercícios

| Exercício sintético | Resultado verificável |
| --- | --- |
| Body/schema inválido e excesso de tamanho | 400/413/415, sem eco de entrada |
| BOLA entre A/B | 404 no detalhe, zero vazamento em lista/canal |
| Cinco falhas em 60s | Perfil básico: um medium; quatro: zero |
| Cinco falhas e sucesso multifonte vinculado | Perfil ampliado: um high; sem vínculo: zero |
| Replay/conflito | 200/409, sem duplicar efeitos |
| Rajada ou buffer cheio | 429, limite explícito e sem falso sucesso |
| Revogar chave ou desabilitar source | 401/403 no próximo envio |
| Core/store/sink offline | Falha observável, retomada por checkpoint/outbox |
| Expurgo de evento/snapshot | Snapshot preservado ou evidence_expired explícito |
| Fixture inerte de segredo/vulnerabilidade | CI vermelho/verde com evidência |

Somente ambientes próprios, dados sintéticos e carga local limitada. Sem malware, exploração de terceiros, varredura da Internet ou alteração em contas/projetos existentes.

## Dados e demonstração

Schemas fechados, sem payload livre, senha, token, dados pessoais ou identificadores de host. IDs artificiais em fixtures. Limites de string não garantem por si sós ausência de PII; política de geração pertence à instrumentação. Evidência/hash não provam que origem comprometida diz a verdade; alerta/score não confirma invasão.

Composição local com loopback, processo sem root e volume privado do core. Logs só request_id, resultado e code, sem bodies/Authorization. Métricas técnicas locais de OB03 usam labels limitados, sem IDs. Retenção e saturação seguem contratos; atraso/expurgo pode criar coverage_gap.

Outbox e exportação só usam segredos no servidor. Discord sem menções/eventos brutos. Loki/Grafana com isolamento por stack/credencial, não filtro visual. E2Es usam Discord fake, nenhum envio real é necessário.

Roteiro DS03 cobre reconhecer/resolver, revogar e retomar processo local; DS04 cobre saídas e múltiplos SDKs. Não ampliar esse roteiro para onboarding empresarial, casos, tuning, backup/restauração integrados ou metas de capacidade/latência nesta fase.
