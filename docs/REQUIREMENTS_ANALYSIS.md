# ServeRest — Análise de Requisitos e Contrato da API

| Campo | Valor |
| --- | --- |
| Projeto | ServeRest — API Quality Engineering |
| Etapa | 01 — Discovery |
| Versão do documento | 1.1 |
| Análise original | 22/10/2026 |
| Revisão contratual | 26/10/2026 |
| Fonte principal | OpenAPI 3.0.0 oficial, `info.version = 3.2.2` |
| Estado | Documental: nenhuma chamada, teste, coleção, bug ou ticket foi criado |

## 1. Objetivo e regra de evidência

Consolidar o que pode ser afirmado a partir da documentação pública da ServeRest, separar requisitos confirmados de hipóteses e apontar riscos antes do planejamento de testes.

| Marca | Significado |
| --- | --- |
| **C** | Confirmado expressamente no contrato ou repositório oficial; ainda não observado em execução. |
| **D** | Divergência confirmada entre partes do contrato; o comportamento da implementação continua desconhecido. |
| **H** | Hipótese ou dúvida ainda não confirmada. |
| **P** | Decisão recomendada para o projeto; ainda não implementada. |

Um comportamento documentado não comprova que a implementação o cumpre.

## 2. Produto e ambiente

- **C-ENV-01:** a ServeRest é uma API REST de loja virtual para estudo de testes, com os módulos Login, Usuários, Produtos e Carrinhos.
- **C-ENV-02:** o repositório oficial disponibiliza uso online, local via NPM e local via Docker; declara mesmas rotas, regras, dados iniciais e documentação.
- **C-ENV-03:** os dados do ambiente online são removidos diariamente; o próprio projeto recomenda ambiente local quando for necessário evitar interferência de outros usuários.
- **C-ENV-04:** teste de carga deve ser feito somente em ambiente local. Desempenho não faz parte deste projeto nesta etapa.
- **P-ENV-01:** usar instância local isolada, versão fixada e massa controlada quando a execução for autorizada.

## 3. Inventário do contrato

A revisão do OpenAPI oficial confirmou **9 caminhos, 16 operações, 32 schemas reutilizáveis, 4 request bodies, 4 respostas reutilizáveis e 1 esquema de segurança** (`ApiKeyAuth`, header `Authorization`).

| ID | Método | Caminho | Finalidade | Respostas declaradas | Segurança declarada |
| --- | --- | --- | --- | --- | --- |
| API-01 | POST | `/login` | Realizar login | 200, 401 | Não declarada |
| API-02 | GET | `/usuarios` | Listar ou filtrar usuários | 200 | Não declarada |
| API-03 | POST | `/usuarios` | Cadastrar usuário | 201, 400 | Não declarada |
| API-04 | GET | `/usuarios/{_id}` | Consultar usuário por ID | 200, 400 | Não declarada |
| API-05 | PUT | `/usuarios/{_id}` | Alterar; criar se ID não existir | 200, 201, 400 | Não declarada |
| API-06 | DELETE | `/usuarios/{_id}` | Excluir usuário | 200, 400 | Não declarada |
| API-07 | GET | `/produtos` | Listar ou filtrar produtos | 200 | Não declarada |
| API-08 | POST | `/produtos` | Cadastrar produto | 201, 400, 401, 403 | `Authorization`; administrador |
| API-09 | GET | `/produtos/{_id}` | Consultar produto por ID | 200, 400 | Não declarada |
| API-10 | PUT | `/produtos/{_id}` | Alterar; criar se ID não existir | 200, 201, 400, 401, 403 | `Authorization`; administrador |
| API-11 | DELETE | `/produtos/{_id}` | Excluir produto | 200, 400, 401, 403 | `Authorization`; administrador |
| API-12 | GET | `/carrinhos` | Listar ou filtrar carrinhos | 200 | Não declarada |
| API-13 | POST | `/carrinhos` | Criar carrinho | 201, 400, 401 | `Authorization` |
| API-14 | GET | `/carrinhos/{_id}` | Consultar carrinho por ID | 200, 400 | Não declarada |
| API-15 | DELETE | `/carrinhos/concluir-compra` | Concluir compra e excluir carrinho do titular | 200, 401 | `Authorization` |
| API-16 | DELETE | `/carrinhos/cancelar-compra` | Cancelar compra, excluir carrinho e repor estoque | 200, 401 | `Authorization` |

> O OpenAPI não declara autenticação para as operações de usuários. Isso descreve o contrato publicado; não é avaliação ou aprovação de segurança.

## 4. Requisitos confirmados por módulo

### Login

- **REQ-AUTH-01 (C):** `POST /login` recebe `email` e `password`; sucesso é `200` com `message` e `authorization`, e credenciais inválidas recebem `401`.
- **REQ-AUTH-02 (C):** o token tem duração informada de 600 segundos (10 minutos); token expirado recebe `401` segundo a descrição da operação.

### Usuários

- **REQ-USR-01 (C):** o payload documentado possui `nome`, `email`, `password` e `administrador`; `administrador` é string enum `"true"`/`"false"`, não booleano.
- **REQ-USR-02 (C):** não é permitido cadastrar e-mail já utilizado; a resposta declarada é `400`.
- **REQ-USR-03 (C):** `PUT /usuarios/{_id}` retorna `200` ao alterar e `201` ao cadastrar quando não encontrar o ID.
- **REQ-USR-04 (C):** usuário com carrinho não pode ser excluído (`400`); para os demais casos, o contrato declara `200`, inclusive “nenhum registro excluído”.
- **REQ-USR-05 (C):** a listagem aceita os filtros `_id`, `nome`, `email`, `password` e `administrador`; consultas por ID inexistente declaram `400`.

### Produtos

- **REQ-PRD-01 (C):** o payload possui `nome`, `preco` inteiro, `descricao` e `quantidade` inteira.
- **REQ-PRD-02 (C):** produto com nome já utilizado é rejeitado com `400`.
- **REQ-PRD-03 (C):** criação, alteração e exclusão requerem token; o contrato declara `401` para token ausente, inválido ou expirado e `403` para rota exclusiva de administrador.
- **REQ-PRD-04 (C):** `PUT /produtos/{_id}` declara `200` para alteração e `201` para criação por ID inexistente.
- **REQ-PRD-05 (C):** produto pertencente a carrinho não pode ser excluído (`400`).
- **REQ-PRD-06 (C):** filtros de listagem: `_id`, `nome`, `preco` (inteiro ≥ 1), `descricao` e `quantidade` (inteiro ≥ 0).

### Carrinhos

- **REQ-CART-01 (C):** criação requer token e corpo com `produtos`, contendo `idProduto` e `quantidade` por item.
- **REQ-CART-02 (C):** existe no máximo um carrinho por usuário; produto repetido, segundo carrinho, produto inexistente ou quantidade insuficiente declaram `400`.
- **REQ-CART-03 (C):** criar carrinho reduz o estoque dos produtos adicionados.
- **REQ-CART-04 (C):** cancelamento exclui o carrinho do titular do token e repõe o estoque.
- **REQ-CART-05 (C):** conclusão exclui o carrinho do titular; o contrato não explicita o efeito sobre estoque e preço.
- **REQ-CART-06 (C):** a consulta documenta itens com `idProduto`, `quantidade` e `precoUnitario`, além de `precoTotal`, `quantidadeTotal`, `idUsuario` e `_id`.

## 5. Divergências e dúvidas abertas

| ID | Classificação | Item | Impacto | Próximo esclarecimento autorizado |
| --- | --- | --- | --- | --- |
| D-01 | **D** | No schema `requestBodyCarrinho`, `produtos` é declarado como `object`; o exemplo associado é um array de produtos. | Alto | Comparar snapshot versionado, código oficial e resposta real em ambiente isolado. |
| D-02 | **H** | Schemas de request não declaram propriedades individuais como `required`. | Alto | Avaliar validações reais antes de considerar campos opcionais. |
| D-03 | **D** | A descrição de `PUT /produtos/{_id}` menciona “usuário” ao explicar o cadastro por ID inexistente. | Médio | Tratar como inconsistência textual; confirmar o comportamento sem presumir defeito. |
| D-04 | **H** | Limites de preço e quantidade no cadastro não são definidos; limites de filtros GET não substituem essa regra. | Alto | Validar valores zero e negativos em etapa autorizada. |
| D-05 | **H** | A conclusão da compra não descreve explicitamente efeito em estoque, preço persistido ou alterações posteriores de preço. | Alto | Observar leituras antes/depois com massa controlada. |
| D-06 | **H / risco** | Schemas de resposta de usuários incluem `password`; não há garantia documental sobre representação ou política de exposição. | Alto | Tratar dados como sensíveis; não expor evidências públicas. |
| D-07 | **H** | Aceitação de booleano, caixa diferente ou outros valores para `administrador` não é especificada. | Médio | Validar em ambiente isolado. |
| D-08 | **H** | Nem todos os formatos e códigos de erro de campos inválidos são enumerados. | Médio | Separar regra ausente de desvio de contrato quando houver execução. |
| D-09 | **H** | A atomicidade do estoque após falha na criação de carrinho não está documentada. | Alto | Verificar ausência de efeito parcial. |
| D-10 | **H** | PATCH, paginação, rate limiting e refresh token não constam no OpenAPI revisado. | Médio | Manter fora do escopo até fonte oficial complementar. |

## 6. Riscos preliminares

| Risco | Probabilidade inicial | Impacto | Prioridade | Mitigação planejada |
| --- | --- | --- | --- | --- |
| Dados públicos interferirem no resultado | Alta | Alto | P0 | Instância local e massa própria |
| Carrinho contaminar estoque de cenários seguintes | Alta | Alto | P0 | Isolamento e limpeza futura |
| Divergência entre OpenAPI e implementação | Média | Alto | P1 | Fixar versão e guardar snapshot contratual |
| Token expirar durante execução | Média | Médio | P1 | Reautenticação controlada |
| Exposição de dados em respostas/evidências | A apurar | Alto | P0 | Dados sintéticos e mascaramento |
| Ambiente de CI divergir do local | Média | Médio | P1 | Versão e ambiente reprodutíveis |

As probabilidades são avaliações prospectivas de QA, não estatísticas observadas.

## 7. Dependências para investigação futura

1. Criar usuário administrador e autenticar para gerenciar produtos.
2. Criar usuário cliente e carrinho com produto controlado.
3. Consultar carrinho e estoque antes/depois de cancelar.
4. Recriar carrinho e concluir compra para observar o estado final.
5. Verificar bloqueios de exclusão com carrinho existente.

Esta lista descreve dependências de levantamento; não é plano de testes nem registro de execução.

## 8. Critério de saída da Discovery

- [x] Contrato revisado: 4 módulos, 9 caminhos e 16 operações.
- [x] Contagens de componentes registradas: 32 schemas, 4 request bodies, 4 respostas reutilizáveis e 1 esquema de segurança.
- [x] Regras documentadas separadas de comportamento ainda não observado.
- [x] Divergências, hipóteses e riscos mantidos sem classificá-los como bugs.
- [ ] Fixar versão da instância futura e versionar um snapshot do OpenAPI.
- [ ] Investigar ambiguidades por execução exploratória explicitamente autorizada.

## 9. Conclusão

A documentação está madura para iniciar a **Etapa 02 — Planejamento de Testes**, pois possui inventário completo, regras identificadas, riscos priorizados e lacunas explícitas. Ela não permite afirmar conformidade da implementação até que uma etapa futura autorize execução controlada.

## Fontes oficiais consultadas

- [Contrato OpenAPI ServeRest](https://serverest.dev/swagger.json?lang=pt-BR) — consultado em 26/10/2026.
- [Documentação ServeRest](https://serverest.dev/).
- [Repositório ServeRest](https://github.com/ServeRest/ServeRest) — execução local e política de uso.
