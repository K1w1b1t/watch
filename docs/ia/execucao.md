# Execução das issues

## Seleção e paralelismo

Escolher uma issue sem responsável e sinalizar que vai assumi-la. A primeira issue de cada módulo usa contratos v1 e adaptadores locais; não depende da implementação de outro módulo. Incrementos seguintes dependem somente dos anteriores indicados. Integrações reais têm issues próprias. Não há prazos nem divisão por calendário.

Uma issue deve ter uma mudança observável, um limite claro e uma demonstração. Se surgirem dois resultados independentes, separar em novas issues. Não transformar uma melhoria de teste em reescrita do módulo inteiro.

## TDD e entrega

1. Ler objetivo, contrato, ameaças e critérios da issue.
2. Escrever um teste que falhe pelo comportamento ausente, incluindo caminho negativo relevante.
3. Implementar o menor comportamento que passa e refatorar mantendo os testes.
4. Exercitar o módulo pela entrada pública em E2E local com dependências controladas.
5. Documentar execução, decisão tomada, limitação e evidência de testes na PR.

Unitário verifica lógica isolada; teste de contrato verifica a fronteira; E2E do módulo entra por HTTP/CLI/UI reais e verifica um efeito observável. O E2E integrado atravessa os serviços reais locais. Simular um serviço externo é permitido no desenvolvimento, mas a PR deve dizer quais componentes foram simulados.

## Critério comum de conclusão

- Critérios específicos verificados; testes unitários e E2E passam.
- Sem regressão de contratos ou isolamento de tenant.
- Nenhum segredo, dado pessoal, estado Terraform ou artefato de laboratório no Git.
- README do módulo explica como executar e demonstrar, sem conta paga.
- Erros e falhas de dependências têm comportamento explícito.
- Issue/PR contém evidência e limitações; uma revisão do time verifica a mudança.

## Uso das descrições

As issues devem explicar o raciocínio e o que precisa ser implementado, permitindo que qualquer membro ou assistente ajude a compreender o problema. Elas não entregam a implementação pronta. Dúvidas de contrato devem gerar uma decisão compartilhada, sem inventar interfaces incompatíveis.

## Modelo para novas issues

Objetivo e resultado observável; módulo e incremento; dependências; contrato de entrada e saída; o que implementar; fora de escopo; critérios verificáveis; testes unitários e E2E; ameaça/defesa; material de estudo; demonstração e documentação. Não incluir prazo.
