# Gratuidade e operação

Gratuidade é requisito de projeto, dentro de cotas e termos vigentes; não é promessa de custo zero em qualquer escala. A demonstração local é o caminho obrigatório e permanece disponível quando um provedor mudar de plano.

## Decisões iniciais

| Serviço | Uso proposto | Condição |
| --- | --- | --- |
| Local + SQLite | Base de todos os módulos e E2E | Não depende de conta cloud |
| Workers Free + D1 | Ingestão/metadados opcionais | Conferir cotas atuais; sem upgrade automático |
| Oracle Always Free existente | Core Elixir opcional | Conferir shape, região, CPU, memória, disco e tráfego na conta; capacidade pode faltar e recursos ociosos podem ser retomados |
| Axiom Personal | Avaliação futura, não dependência | Plano pessoal sujeito a termos e limites; não confundir franquia de loading compute com retenção ilimitada |
| R2 | Adiado | Free allowance não elimina cobrança acima dela; retenção longa fora do escopo |
| Fly.io | Fora da base gratuita | Trial temporário não atende hospedagem gratuita contínua |
| Vercel Hobby | Não adotado como padrão do time | Restrito a uso pessoal/não comercial; avaliar elegibilidade antes de escolher |
| IdP | Escolher em ID03 | Validar MFA, organizações, usuários ativos e recursos do plano gratuito |

## Gate antes de provisionar

Registrar data de verificação, plano, cotas, consumo atual compartilhado, recurso pretendido e procedimento de remoção. Somar disco, IP público, snapshots, tráfego, logs e backups: não basta verificar CPU. Não ativar planos pagos, trials como base permanente nem depender de créditos promocionais. Se a conta não oferece um limite efetivo de gasto, manter o módulo local até existir alternativa compatível. Alertas de orçamento avisam, não bloqueiam cobrança.

Definir limites internos conservadores: 1 evento por envio, 16 KiB por evento, 1.000 eventos/dia no laboratório remoto e retenção de 24h por padrão. Esses limites são propostas da aplicação, não cotas dos provedores. Medir bytes e operações antes de aumentar. Pausar ingresso ao atingir limite em vez de ampliar plano.

Terraform plan revisado antes de apply, state protegido fora do Git, credencial com menor privilégio e destroy documentado. Não criar domínio pago nem modificar infraestrutura compartilhada de outros projetos. Oracle é opcional e não garante disponibilidade. Backup pequeno e restauração local devem existir antes do deploy remoto.

## Referências para revalidar

- [Workers limits](https://developers.cloudflare.com/workers/platform/limits/)
- [D1 pricing](https://developers.cloudflare.com/d1/platform/pricing/)
- [R2 pricing](https://developers.cloudflare.com/r2/pricing/)
- [Consistência do KV](https://developers.cloudflare.com/kv/concepts/how-kv-works/)
- [Oracle Always Free](https://docs.oracle.com/en-us/iaas/Content/FreeTier/freetier_topic-Always_Free_Resources.htm)
- [Axiom pricing](https://axiom.co/pricing)
- [Fly.io trial](https://fly.io/docs/about/free-trial/)
- [Vercel Hobby](https://vercel.com/docs/plans/hobby)

Referências consultadas no planejamento em 03/11/2026. Revalidar no momento de qualquer deploy.
