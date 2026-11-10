# ServeRest — Relatório de Execução

| Campo | Valor |
| --- | --- |
| Ambiente | ServeRest 3.2.2 local (`http://localhost:3000`) |
| Data | 09/11/2026 |
| Estado | Execução inicial; demais casos permanecem `Not Run` |

## D-01 — formato de `produtos` no carrinho

Foi criada massa sintética exclusiva (administrador, usuário comum e produto com estoque 5). A criação de carrinho com `produtos` como **array** contendo `{ idProduto, quantidade }` foi aceita. O estoque passou de 5 para 4; após `DELETE /carrinhos/cancelar-compra`, retornou a 5. Os três recursos sintéticos foram excluídos ao final.

| Casos relacionados | Resultado | Evidência |
| --- | --- | --- |
| TC-CART-001, TC-CART-008, TC-CART-009 | Pass | Carrinho criado e cancelado localmente; estoque 5 → 4 → 5 |

O schema OpenAPI ainda descreve `produtos` como objeto, embora o exemplo publicado e a execução local usem array. Isso permanece como divergência documental, não como defeito confirmado da API.

## CRUD e autorização

| Casos relacionados | Resultado observado |
| --- | --- |
| TC-AUTH-001, TC-AUTH-005 | Login de usuário sintético: `200`; criação de produto: administrador `201`, token ausente/inválido `401` e usuário comum `403`. |
| TC-USR-001, 002, 005, 006, 008 | Criar `201`, e-mail duplicado `400`, consultar `200`, atualizar `200` com persistência confirmada e excluir `200`. |
| TC-USR-009 | Exclusão de usuário com carrinho bloqueada com `400`. |
| TC-PRD-001, 003, 006, 007, 008, 009 | Criar `201`, nome duplicado `400`, consultar `200`, atualizar `200` com persistência confirmada, criar por PUT `201` e excluir `200`. |
| TC-PRD-010 | Exclusão de produto vinculado ao carrinho bloqueada com `400`. |

Cada fluxo usou recursos sintéticos exclusivos e realizou limpeza após a verificação. Não foram executados cenários exploratórios ou casos não listados nesta tabela.

## Rejeições de carrinho

| Casos relacionados | Resultado observado |
| --- | --- |
| TC-CART-003 | Produto inexistente recusado com `400`. |
| TC-CART-004 | Quantidade acima do estoque recusada com `400`. |
| TC-CART-005 | Produto duplicado no mesmo carrinho recusado com `400`. |
| TC-CART-006 | Segundo carrinho do mesmo usuário recusado com `400`; primeiro carrinho foi criado com `201` e cancelado na limpeza. |

## Casos adicionais concluídos

| Casos relacionados | Resultado observado |
| --- | --- |
| TC-CART-007, TC-CART-010 | Listar/consultar carrinho: `200`; concluir compra: `200`; consulta posterior: `400`. |
| TC-CART-011 | Um item de preço 10 retornou `quantidadeTotal: 1` e `precoTotal: 10`. |
| TC-CART-012 | Criação rejeitada por quantidade insuficiente: `400`; estoque permaneceu `2 → 2`. |
| TC-USR-003, TC-PRD-004, TC-PRD-005 | Campo ausente, enum inválido e valores zero/negativos foram recusados com `400`. |
| TC-USR-004, TC-USR-007 | Filtro por e-mail: `200` com um registro; PUT com ID inexistente: `201`. |
