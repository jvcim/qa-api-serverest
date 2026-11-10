# Collections Postman

`ServeRest-smoke.postman_collection.json` cria massa sintética local, valida login e criação autorizada de produto e remove os recursos ao fim. Use somente com `http://localhost:3000` ou outra instância local isolada. Tokens, IDs e credenciais são gerados em memória e não são versionados.

Os demais fluxos executados manualmente estão registrados em `docs/EXECUTION_REPORT.md`; serão incorporados à collection em uma próxima ampliação, após revisão de seus scripts de setup e teardown.

`ServeRest-negative.postman_collection.json` cobre login inválido e escrita de produto sem token ou com token inválido. Ela não cria massa persistente.
