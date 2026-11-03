# O que precisamos proteger e como testar

**Use este documento para:** entender os riscos da sua tarefa e escolher o teste negativo adequado. As regras de campos e respostas estão nos [contratos](contratos.md).

## 1. Uma aplicação autenticada ainda pode mandar dado incorreto

Uma chave válida só diz que a chamada tem uma identidade reconhecida. A entrada ainda precisa validar tamanho, campos e vínculo com a fonte. O servidor confere fonte/estado antes de guardar.

Exemplo: a chave de app-a não permite escrever como app-b. Se o evento tentar trazer tenant_id para escolher outra organização, o campo é rejeitado.

## 2. Não misturar as três credenciais

| Credencial | Proteção necessária |
| --- | --- |
| Chave do SDK | 256 bits aleatórios; exibir uma vez; guardar só hash SHA-256/metadados; verificar estado atual e comparar de forma segura |
| Credencial interna | Só usada entre entrada e servidor; não deve estar no navegador nem ser substituída por chave de SDK |
| Token humano | Verificar assinatura, algoritmo, emissor, destinatário, expiração e vínculo/papel em cada organização |

A emissão/revogação de chaves é por comando local. O emissor humano de demonstração só roda na própria máquina e não pode ser ativado em perfil remoto. Falha na verificação não libera acesso.

## 3. Cada organização só acessa seus dados

Listagem, detalhe, alteração e canal ao vivo precisam aplicar a mesma separação. Filtrar a tela não resolve: alguém pode chamar a API diretamente.

Exemplo obrigatório: viewer de A tenta abrir alerta de B e recebe 404. Analyst de A também não pode alterá-lo. Viewer de A não altera alerta nem de A. Sessão expirada ou vínculo retirado interrompe os acessos correspondentes.

## 4. O navegador não recebe a chave do SDK

No exemplo React, o navegador manda apenas um sinal permitido ao próprio backend. O backend confere sessão, consentimento, origem, proteção CSRF e limite de pedidos, constrói o evento e chama o SDK.

Esse relato continua classificado como client_reported: o usuário pode controlar seu navegador. Não permitir propriedades livres nem apresentar analytics como prova de invasão.

## 5. Guardar só os dados necessários

Usar identificadores fictícios e formatos fechados. Não enviar senha, token, email, nome, CPF, IP, hostname, comando ou corpo livre. Não copiar Authorization/body para mensagens de erro ou registros.

Hash e pseudônimo não eliminam todos os riscos. A regra do formato não consegue descobrir se qualquer string é um nome real; a instrumentação deve construir apenas os campos permitidos. Uma fonte comprometida pode mentir mesmo se seu hash de conteúdo estiver correto.

A cópia de evidência precisa seguir a retenção e mostrar expiração. Medidas técnicas não usam IDs ilimitados como rótulos. No Discord, desativar menções e enviar só o resumo. Tokens de Discord/Loki/Grafana ficam no servidor.

## 6. Falhas não podem parecer sucesso

Se não foi possível guardar um evento, não devolver stored. Se a análise perdeu eventos por retenção, mostrar coverage_gap. Se um destino de notificação caiu, manter pendência e último sucesso real. As filas e as tentativas têm limites para não crescer ou repetir indefinidamente.

## 7. Exercícios que acompanham as entregas

| O que tentar no laboratório | O que precisa acontecer |
| --- | --- |
| Campo extra, corpo grande ou conteúdo inválido | Rejeição 400/413/415 sem ecoar o conteúdo |
| Chave inválida/revogada ou fonte desativada | Negação 401/403 antes de guardar |
| Mesmo ID com conteúdo igual ou diferente | Reenvio 200 sem duplicar ou conflito 409 sem substituir |
| Usuário A consultando/alterando B | Nenhum dado vazado, mesmo chamando API diretamente |
| Mais pedidos ou dados que o limite | 429 e indicação compreensível da causa |
| Quatro e depois cinco falhas em 60s | Perfil básico: zero e depois um medium |
| Falhas/sucesso de fontes sem vínculo | Sem correlação indevida entre fontes/organizações |
| Queda entre salvar alerta e criar aviso | Outbox recupera o trabalho após reinício |
| Limpeza de buffer e depois da cópia de evidência | Cópia preservada primeiro, expiração explícita depois |
| Segredo fictício detectável em teste de CI | Verificador falha e volta a passar após correção |

Todos esses exercícios usam ambiente próprio e dados fictícios. DS03 e DS04 definem a prova integrada; não exigem malware, varredura da internet ou conta real de cliente.

## 8. Configuração do ambiente local

Usar portas acessíveis só pela máquina local, processos sem root e volume privado do servidor. O banco não fica aberto na rede. Para exercícios remotos opcionais, conferir custo, autenticação e TLS antes de provisionar, conforme [custos](custos.md).

Isolar cópias de organizações no Loki/Grafana por instância/credencial. Um filtro visual no dashboard não é controle de acesso.
