# Segurança desde o primeiro módulo

## Fronteiras de confiança

A origem é não confiável mesmo autenticada. A ingestão valida tamanho, esquema, fonte e credencial; o servidor determina tenant. Severidade declarada não aciona resposta automática. API e canais verificam autorização de cada objeto, sem confiar em tenant enviado pelo navegador.

Credenciais aleatórias com pelo menos 256 bits de entropia; somente hash SHA-256 e identificador público persistidos. Hash de chave aleatória não é estratégia de hash para senhas humanas. Comparação segura; exibição única; rotação e revogação auditadas sem registrar segredo. D1 é fonte de verdade na borda. KV não é autoridade de revogação: sua consistência eventual permite valores antigos após alteração.

Chaves de máquina não autenticam analistas. Identidade humana exige validação de assinatura, algoritmo permitido, issuer, audience e expiração; MFA e organizações dependem do provedor efetivamente escolhido. Tokens inválidos ou infraestrutura de auth indisponível não liberam acesso. Sessões de demonstração são estritamente locais.

## Catálogo de exercícios progressivos

| Sinal/ameaça | Exercício sintético | Defesa e evidência |
| --- | --- | --- |
| Injeção e payload malformado | Campos extras e conteúdo inválido | Rejeição por schema, query parametrizada, nenhum eco do corpo |
| BOLA / acesso entre tenants | Tentar consultar alerta de B com sessão de A | 404 e zero vazamento em lista, detalhe e canal |
| Brute force, ATT&CK T1110 | Cinco falhas sintéticas em 60s | Um alerta com regra, versão e IDs de evidência |
| Replay / poisoning | Reenviar ID e depois alterar conteúdo | Deduplicação ou conflito sem duplicar alerta |
| Exaustão de recursos | Rajada local limitada e corpo acima de 16 KiB | 429/413, filas e memória limitadas, medição |
| Abuso de credencial | Revogar chave e reutilizá-la | 401 no próximo acesso; segredo ausente de logs |
| Descoberta de serviços, ATT&CK T1046 como contexto | Porta sintética fora de allowlist | Sinal explicável; porta aberta isolada não prova ataque |
| Indisponibilidade de sink | Armazenamento retorna falha | Sem falso sucesso; retry limitado e recuperação demonstrada |
| Risco de supply chain | Fixture inerte detectável pelo scanner | CI bloqueia a fixture e passa após sua remoção |

Os exercícios usam exclusivamente ambiente próprio e fixtures. Não há malware, exploração de terceiros ou varredura da Internet. Testes de carga são locais, com volume limitado.

## Operação mínima

Métricas de aceitos, rejeitados, latência, erros de dependência e alertas por regra, sem rótulos com event_id/source_id de cardinalidade ilimitada. Logs estruturados com request_id, resultado e código; excluir bodies e Authorization. Usuário do processo sem privilégios, portas mínimas, TLS remoto, painel privado durante laboratório, patches e restauração testada.

Runbook: reconhecer incidente; preservar evidência sintética; revogar credencial; conter serviço comprometido; restaurar backup; verificar integridade e registrar aprendizado. Não implementar bloqueio automático de hosts no MVP.
