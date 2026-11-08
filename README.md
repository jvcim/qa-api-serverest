# ServeRest — API Quality Engineering

Projeto de portfólio dedicado ao ciclo completo de Quality Assurance para APIs REST: análise de requisitos, planejamento de testes, análise de riscos, futura automação com Postman e integração contínua. A API pública de demonstração avaliada é a [ServeRest](https://serverest.dev/).

## Objetivo

Demonstrar um ciclo de QA para APIs: Discovery, planejamento, design de testes, automação, integração contínua, evidências e relatório final. O projeto está em desenvolvimento: Discovery e Planejamento foram concluídos documentalmente; ainda não há testes, execução, collections, bugs ou tickets.

## Estágio atual

**Etapa 03 — Design de Testes em desenvolvimento.** Discovery e Planejamento foram concluídos documentalmente; não há execução registrada.

O contrato OpenAPI 3.0.0 publicado pela ServeRest informa versão `3.2.2`, 9 caminhos, 16 operações, 32 schemas reutilizáveis, 4 request bodies e 4 respostas reutilizáveis. A Discovery, o plano e os riscos permanecem documentais: ainda não há chamadas, testes ou resultados.

## Documentação disponível

- [Análise de requisitos — Etapa 01: Discovery](docs/REQUIREMENTS_ANALYSIS.md)
- [Plano de testes de API — Etapa 02](docs/TEST_PLAN.md)
- [Avaliação de riscos de qualidade — Etapa 02](docs/RISK_ASSESSMENT.md)
- [Catálogo de casos — Etapa 03](docs/TEST_CATALOG.md)
- [Matriz de rastreabilidade — Etapa 03](docs/TRACEABILITY_MATRIX.md)
- [Estratégia de dados de teste — Etapa 03](docs/TEST_DATA_STRATEGY.md)

## Próximas etapas planejadas

1. Revisar o design e esclarecer lacunas do contrato em ambiente isolado.
2. Preparar a futura gestão de trabalho e rastreabilidade no Jira.
3. Criar collections Postman e automação somente após a definição dos oráculos.
4. Executar, analisar evidências e integrar a validação contínua quando autorizado.

## Tecnologias planejadas

- API REST ServeRest e contrato OpenAPI 3.0;
- Postman e scripts JavaScript para futura exploração e automação, ainda não criados;
- Postman CLI para futura execução local e em CI, ainda não configurado;
- Swagger/OpenAPI para análise e rastreabilidade contratual;
- GitHub Actions como alternativa de integração contínua futura, ainda não configurada;
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
