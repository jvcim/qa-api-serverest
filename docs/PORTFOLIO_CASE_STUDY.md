# ServeRest — API Quality Engineering

## Objetivo

Demonstrar um ciclo de QA de API: análise do OpenAPI, planejamento baseado em risco, dados sintéticos isolados, execução rastreável e automação reexecutável.

## O que foi entregue

- Discovery de 16 operações e 36 cenários planejados;
- Collections Postman para Smoke e regressão negativa;
- Execuções locais reais com Newman;
- Rastreabilidade entre requisitos, riscos, casos e evidência;
- Pipeline GitHub Actions que sobe uma instância ServeRest isolada antes das collections.

## Evidências reais

Foram confirmados login, autorização de produto, CRUD de usuários e produtos, regras de integridade de carrinho e restauração de estoque. O payload de carrinho aceito é um array, embora o schema OpenAPI ainda o descreva como objeto; a divergência está registrada como limitação documental, não como defeito confirmado.

## Limites responsáveis

Não há dados reais, tokens persistidos ou execução contra o ambiente público compartilhado. Casos não executados permanecem `Not Run`; desempenho e segurança ofensiva estão fora deste escopo.
