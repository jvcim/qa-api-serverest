# ServeRest — API Quality Engineering

Projeto de portfólio dedicado a Quality Assurance para APIs REST: análise de requisitos, planejamento, execução local com Postman/Newman, evidências e integração contínua. A API avaliada é a [ServeRest](https://serverest.dev/).

## Objetivo

Demonstrar um ciclo de QA para APIs: Discovery, planejamento, design, execução rastreável, automação reexecutável e CI. Os resultados registrados são reais e obtidos em uma instância local isolada.

## Estágio atual

**Etapa 04 — Execução inicial e automação.** Discovery, planejamento e design foram concluídos; Smoke e regressão negativa já foram executados com Newman.

O contrato OpenAPI 3.0.0 informa versão `3.2.2`, 9 caminhos e 16 operações. A execução usa `http://localhost:3000`, dados sintéticos e limpeza automática; casos não executados seguem como `Not Run`.

## Documentação disponível

- [Análise de requisitos — Etapa 01: Discovery](docs/REQUIREMENTS_ANALYSIS.md)
- [Plano de testes de API — Etapa 02](docs/TEST_PLAN.md)
- [Avaliação de riscos de qualidade — Etapa 02](docs/RISK_ASSESSMENT.md)
- [Catálogo de casos — Etapa 03](docs/TEST_CATALOG.md)
- [Matriz de rastreabilidade — Etapa 03](docs/TRACEABILITY_MATRIX.md)
- [Estratégia de dados de teste — Etapa 03](docs/TEST_DATA_STRATEGY.md)
- [Relatório de execução](docs/EXECUTION_REPORT.md)
- [Caso de estudo](docs/PORTFOLIO_CASE_STUDY.md)
- [Collections Postman](postman/README.md)

## Próximas etapas planejadas

1. Ampliar as collections para os casos restantes.
2. Criar o backlog no projeto Jira pessoal assim que ele existir.
3. Consolidar relatório e apresentação final.

## Tecnologias planejadas

- API REST ServeRest e contrato OpenAPI 3.0;
- Postman collections e Newman;
- Swagger/OpenAPI para análise e rastreabilidade contratual;
- GitHub Actions para executar as collections em instância local isolada;
- Jira para futura gestão de trabalho e rastreabilidade, ainda não criado;
- Git e GitHub para versionamento e futura publicação;

Nenhuma collection, script de automação, dependência de execução, pipeline ou ticket foi criado nesta etapa.

## Estrutura inicial

```text
qa-api-serverest/
├── docs/
│   ├── REQUIREMENTS_ANALYSIS.md  # Discovery revisada e rastreável
│   ├── TEST_PLAN.md              # Planejamento, sem execução
│   ├── RISK_ASSESSMENT.md        # Riscos prospectivos e controles
│   ├── TEST_CATALOG.md           # Design de cenários, todos Not Run
│   ├── TRACEABILITY_MATRIX.md    # Cobertura planejada e dependências
│   └── TEST_DATA_STRATEGY.md     # Massa, isolamento e limpeza planejados
├── .gitignore
└── README.md
```

## Limites desta etapa

- Não foram feitas chamadas à API nem criados resultados de execução.
- Não há testes automatizados, collections Postman ou scripts JavaScript.
- Não foram criados bugs, tickets Jira ou relatórios de teste.
- O repositório permanece local; nenhuma publicação no GitHub foi realizada.

## Fonte primária

- [Contrato OpenAPI oficial](https://serverest.dev/swagger.json?lang=pt-BR)
- [Documentação ServeRest](https://serverest.dev/)
- [Repositório oficial ServeRest](https://github.com/ServeRest/ServeRest)
