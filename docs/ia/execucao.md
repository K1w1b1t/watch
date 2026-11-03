# Execução das issues

## Escolha e dependências

Escolher issue aberta sem responsável e sinalizar a intenção de assumi-la. Não há datas, duração ou responsáveis fixos. O [backlog](backlog.md) e os bloqueios nativos listam dependências **para concluir**; prototipar com fixtures antes delas é permitido, concluir integração sem componentes exigidos como reais não é.

Somente SDKs enviam ao ingresso nesta fase. CLI e apps de exemplo reutilizam SDK; nenhuma nova issue introduz transporte de ingestão paralelo, adaptador de plataforma ou sensor sem decisão explícita de escopo. Issues fora do escopo são removidas do backlog e não são dependências ativas.

Uma issue tem resultado observável, limite e demonstração. Separar resultados independentes; não dividir por arquivo ou por teste. ST03 persiste alertas, ST05 expõe HTTP humano; C02 cria schema, IN04 integra ingresso e SK07 integra SDK. Contratos/fixtures canônicos evitam interfaces concorrentes.

## TDD e entrega

1. Ler objetivo, contrato, ameaça e pré-requisitos de conclusão.
2. Para código, escrever teste que falha pelo comportamento ausente e caminho negativo relevante.
3. Implementar o menor comportamento, refatorar e executar os testes relevantes.
4. Exercitar entrada pública real HTTP/CLI/UI com efeito observável; explicitar cada serviço externo simulado.
5. Registrar comandos, evidência, decisões e limites na PR; revisão do time verifica a mudança.

Unitário testa lógica, contrato testa fronteira, E2E de módulo usa entrada real com dependências controladas e E2E integrado atravessa os componentes locais exigidos. Documento/ADR exige revisão de coerência e exemplos, sem testes artificiais. Aparelho/SO/conta não são exigidos pelo escopo SDK local; provisionamento opcional não pode ser declarado validado só com mock.

## Conclusão

- Critérios específicos e testes relevantes passam; README explica demonstração e falhas.
- Isolamento e contratos preservados, incluindo fixtures compartilhadas.
- Nenhum segredo, dado pessoal, state ou artefato de laboratório no Git/logs/bundle.
- Dependências de conclusão estão cumpridas; componente real exigido não foi substituído por mock.
- Perfil integrado é identificado: básico usa apenas limiar medium; ampliado usa apenas correlação high, sem regra low demo.
- Novo módulo atualiza CI/inventário pertinente; não é necessário instalar scanners para módulos ausentes.

Não acrescentar piloto/aplicação real, onboarding empresarial, casos de investigação, tuning, backup/restore integrados ou metas empresariais de capacidade/latência como critérios desta fase. OB03 cobre diagnóstico técnico local já previsto; não implica programa de operação empresarial.

## Modelo de descrição

Objetivo/valor; escopo; dependências para concluir; contratos/artefato canônico; comportamento; exclusões; critérios verificáveis; testes/demonstração; segurança; estudo. Linkar issues por número e manter bloqueios nativos coerentes. Mudança de contrato atualiza produtores/consumidores/fixtures na entrega correspondente, sem interface inventada silenciosamente.
