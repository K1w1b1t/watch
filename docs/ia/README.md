# Planejamento do Watch

Construir um laboratório de telemetria e detecção em incrementos pequenos, funcionais e verificáveis. O aprendizado de DevSecOps acompanha cada funcionalidade: entender a ameaça, implementar a defesa, observar o comportamento e demonstrar a detecção.

## Escopo vigente

Somente SDKs server-side TypeScript, Python e Go enviam eventos ao ingresso. A CLI importa o SDK TS. Next.js e NestJS são apps de exemplo; React envia sinal consentido ao próprio backend, que usa o SDK TS. Não existem adaptadores de Vercel, Supabase, Upstash, PostHog, Oracle ou outras plataformas nesta fase. Agentes, sensores, apps desktop/mobile e pareamento estão adiados.

O laboratório usa dados sintéticos e serviços locais. Não exige aplicação de cliente, onboarding empresarial completo, investigação/casos, tuning operacional, backup/restauração integrados ou validação de capacidade/latência empresarial. Discord é saída de notificação; Loki/Grafana são saídas opcionais de consulta.

## Leitura

Para escolher a próxima tarefa, começar pela [ordem de execução](ordem-execucao.md), que separa o fluxo básico, o ampliado e os complementos.

Gustavo e João (`reaperkoji`) têm também uma [sugestão de divisão em dupla](trabalho-em-dupla.md), com sequências complementares e pontos de integração, sem atribuição fixa de responsáveis.

1. [Plano geral](plano-geral.md)
2. [Arquitetura e limites](arquitetura.md)
3. [Contratos entre módulos](contratos.md)
4. [Como executar uma issue](execucao.md)
5. [Backlog com dependências](backlog.md)
6. [Segurança e exercícios](seguranca.md)
7. [Custos e operação do laboratório](custos.md)
8. [Trilha de estudos](estudos.md)
9. [Origens e cobertura](origens-e-cobertura.md)
10. [Diagramas](diagramas.md)

O backlog não usa datas, duração ou responsáveis fixos. É permitido iniciar protótipos com contratos e fakes; para concluir, respeitar os pré-requisitos e E2Es reais de cada issue. Dependências de conclusão são cadastradas como bloqueios no GitHub.

Os contratos são propostas até C01/C02/C03 entregarem schemas, fixtures e transporte executáveis. O repositório começa com documentação e licença; nenhuma capacidade está implementada por estar descrita aqui.
