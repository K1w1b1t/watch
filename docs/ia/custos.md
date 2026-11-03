# Como executar sem depender de recurso pago

**Este documento responde:** quais recursos são necessários ao laboratório e quais cuidados precedem os exercícios remotos opcionais.

## O caminho principal é local

Para demonstrar DS03/DS04, a base é executar os programas do Watch, SQLite e, no segundo roteiro, Loki/Grafana na própria máquina. Os exemplos usam dados fictícios. O Discord é imitado por um servidor HTTP local nos testes.

Os READMEs dos módulos devem trazer os comandos reais de execução quando forem implementados. Neste momento não há comando único de instalação pronto: IF01 constrói a composição local.

| Recurso | Por que está no projeto | Precisa de conta externa para a demonstração local? |
| --- | --- | --- |
| Contêineres/processos locais | Subir entrada, servidor, detector e painel | Não |
| SQLite em volume privado | Manter dados após reinício | Não |
| Emissor humano local | Testar sessão/papéis do painel | Não |
| Servidor Discord falso | Testar mensagem e falha de entrega | Não |
| Loki/Grafana locais | Consultar e mostrar alertas exportados | Não |
| Worker/D1 remoto de IF02 | Exercício separado de infraestrutura | Sim, só se a conta for elegível |
| Máquina remota de IF03 | Preparar um plano de infraestrutura | Credenciais podem ser necessárias para plan; esta issue não cria a máquina |

## O que cada issue remota faz

1. **IF04:** decidir onde cada componente ficaria, consultar condições oficiais e documentar custos/limites antes de criar qualquer coisa.
2. **IF02:** testar apenas marcador fictício em Worker/D1 isolados. Isso não publica a entrada do Watch nem migra seu banco.
3. **IF03:** preparar a configuração de uma máquina nova dedicada e revisar o plano, sem executar apply.

Essas tarefas não bloqueiam o laboratório local. A VPS que já atende outros projetos não é um recurso disponível para o Watch.

## Antes de criar recurso remoto

Quem executar IF04 deve registrar data, plano/cotas, consumo já existente na conta, recurso pretendido, limite verificável e forma de remover. Conferir disco, IP, tráfego, registros, estado e outras dependências — não apenas CPU.

Não usar créditos/trials temporários como se fossem gratuidade permanente nem ativar upgrade automático. Se não houver opção elegível com limite compatível, continuar localmente. Alerta de orçamento avisa; não necessariamente impede cobrança.

Esta documentação não certifica preços atuais. A pessoa que implementar a issue precisa consultar e datar as condições oficiais do serviço naquele momento.

## Como trabalhar com Terraform

`plan` mostra a prévia; `apply` executa a mudança; `state` registra os recursos controlados. Revisar o plano antes de aplicar, manter state/segredos fora do Git e usar credencial de menor privilégio.

A remoção deve alcançar apenas os recursos Watch. Não criar domínio pago, alterar outros projetos ou declarar provisionamento testado com base apenas numa simulação.

## Por que temos limites também no ambiente local

Limites tornam o comportamento previsível e permitem testar saturação sem carga ilimitada. A referência exata está no [contrato](contratos.md#5-limites-de-pedidos-e-armazenamento).

| Recurso controlado | Valor inicial |
| --- | --- |
| Evento por pedido | 1, até 16 KiB |
| Pedidos autenticados por chave | 10/minuto no processo inicial |
| Buffer de cada organização | 24h, até 10.000 eventos ou 10 MiB de JSON |
| Cópias de evidência por organização | Até 7 dias ou 10 MiB |
| Espera do SDK por tentativa | 2 segundos |

O SDK TS ganha fila/reenvio opcional em CO02, com limites explícitos e perda de fila em reinício. Python/Go começam com envio unitário. Saturar um limite deve gerar resultado conhecido, não aumentar plano ou memória sem controle.
