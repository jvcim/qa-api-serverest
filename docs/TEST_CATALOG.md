# ServeRest — Catálogo de Casos de Teste de API

| Campo | Valor |
| --- | --- |
| Etapa | 03 — Design de Testes |
| Base | Discovery v1.1, Test Plan v1.1 e Risk Assessment v1.1 |
| Estado | Planejado; todos os casos estão `Not Run` |
| Convenção | `TC-<módulo>-<número>`; não há tickets Jira nesta etapa |

## Convenções de design

Cada cenário abaixo possui massa exclusivamente sintética, execução em instância local isolada como pré-condição geral e limpeza planejada conforme [TEST_DATA_STRATEGY.md](TEST_DATA_STRATEGY.md). `Smoke` identifica a jornada mínima proposta; `Regression` representa cobertura repetível planejada; `Exploratório` é usado quando o contrato não fornece oráculo suficiente. “Condicionado” significa que só pode ser automatizado após a dúvida indicada ser resolvida.

Em cenários exploratórios, o resultado esperado não é “aprovação”: a saída esperada é registrar a resposta e classificá-la como esclarecimento contratual, divergência documental ou possível defeito apenas após evidência reproduzível.

## Autenticação

| ID | Título e objetivo | Requisito / endpoint | Pré-condições e massa | Passos | Resultado esperado | Técnica | Pri. / classe / perfil | Automação / status / risco |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| TC-AUTH-001 | Login válido e emissão de token | `REQ-AUTH-01`; `POST /login` | Usuário sintético válido cadastrado | Enviar `email` e `password` válidos | `200` com `message` e `authorization` conforme schema | Partição de equivalência, contrato | P0; funcional/contrato; Smoke | Sim; Not Run; `RP-02`, `RE-04` |
| TC-AUTH-002 | Credenciais inválidas são recusadas | `REQ-AUTH-01`; `POST /login` | Usuário sintético existente; senha incorreta | Enviar credenciais inválidas | `401` documentado; não receber token válido | Partição de equivalência | P1; negativa; Regression | Sim; Not Run; `RP-02` |
| TC-AUTH-003 | Campos ausentes no login | `D-02`; `POST /login` | Nenhuma | Variar omissão de `email`, `password` e ambos | Sem oráculo contratual de obrigatoriedade; registrar status/mensagem sem classificar Pass/Fail | Equivalência, exploratório | P1; negativa; Exploratório | Condicionado; Not Run; `RP-05` |
| TC-AUTH-004 | Contrato da resposta de login | `REQ-AUTH-01`; `POST /login` | Mesma massa de TC-AUTH-001 | Executar login válido e validar tipos/campos publicados | `200`; `message` e `authorization` string conforme OpenAPI; sem expor token em evidência | Contrato | P1; contrato; Regression | Sim; Not Run; `RE-05` |
| TC-AUTH-005 | Autorização com token ausente, inválido e expirado | `REQ-AUTH-02`, `REQ-PRD-03`; `POST /produtos` | Admin sintético; token válido, ausente, inválido e expirado quando controlável | Tentar escrita de produto por partição de token | Ausente/inválido/expirado recebem `401` conforme contrato; expiração depende de controle de tempo | Tabela de decisão | P0; segurança funcional; Smoke | Parcial/condicionado para expiração; Not Run; `RP-02`, `RE-04` |

## Usuários

| ID | Título e objetivo | Requisito / endpoint | Pré-condições e massa | Passos | Resultado esperado | Técnica | Pri. / classe / perfil | Automação / status / risco |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| TC-USR-001 | Cadastro válido de usuário | `REQ-USR-01`; `POST /usuarios` | E-mail sintético único; `administrador` string `"false"` | Enviar payload documentado | `201`, `message` e `_id` conforme contrato | Equivalência, contrato | P1; funcional/contrato; Regression | Sim; Not Run; `RE-01` |
| TC-USR-002 | E-mail duplicado é recusado | `REQ-USR-02`; `POST /usuarios` | Usuário sintético existente | Repetir cadastro com mesmo e-mail | `400` documentado; não criar segundo usuário | Tabela de decisão | P1; negativa; Regression | Sim; Not Run; `RP-05` |
| TC-USR-003 | Campos e valor de administrador fora do schema | `REQ-USR-01`, `D-02`, `D-07`; `POST /usuarios` | Payloads com campos ausentes, booleano e valor fora de enum | Variar uma entrada por vez | Campos obrigatórios/aceitação fora do enum não estão definidos; registrar comportamento | Equivalência, exploratório | P1; negativa; Exploratório | Condicionado; Not Run; `RP-05` |
| TC-USR-004 | Listagem e filtros de usuários | `REQ-USR-05`; `GET /usuarios` | Dois usuários sintéticos distinguíveis | Consultar sem filtro e com `_id`, `nome`, `email`, `password`, `administrador` | `200`; estrutura publicada e filtragem observada sem expor `password` | Equivalência, contrato | P2; contrato; Regression | Sim; Not Run; `RP-03`, `RE-05` |
| TC-USR-005 | Consulta por ID existente e inexistente | `REQ-USR-05`; `GET /usuarios/{_id}` | Usuário sintético criado; ID aleatório inexistente | Consultar as duas partições | Existente `200`; inexistente `400` conforme contrato | Equivalência | P2; funcional/negativa; Regression | Sim; Not Run; `RE-01` |
| TC-USR-006 | Atualização de usuário existente | `REQ-USR-03`; `PUT /usuarios/{_id}` | Usuário próprio existente e e-mail novo | Atualizar dados e consultar recurso | `200` documentado; alteração persistida | Transição de estado | P1; funcional/integração; Regression | Sim; Not Run; `RP-07` |
| TC-USR-007 | Criação por PUT com ID inexistente | `REQ-USR-03`; `PUT /usuarios/{_id}` | ID sintético inexistente; e-mail único | Enviar payload válido ao ID inexistente | `201` documentado; recurso criado | Tabela de decisão | P1; funcional; Regression | Sim; Not Run; `RP-07` |
| TC-USR-008 | Exclusão de usuário sem carrinho | `REQ-USR-04`; `DELETE /usuarios/{_id}` | Usuário próprio sem carrinho | Excluir e consultar ID | `200` documentado; observar resposta e estado posterior | Transição de estado | P1; funcional; Regression | Sim; Not Run; `RE-01` |
| TC-USR-009 | Exclusão bloqueada para usuário com carrinho | `REQ-USR-04`; `DELETE /usuarios/{_id}` | Usuário com carrinho próprio controlado | Tentar exclusão e consultar estado | `400` documentado; usuário/carrinho preservados | Tabela de decisão, integração | P0; negativa/integração; Regression | Sim; Not Run; `RP-01`, `RP-04` |

## Produtos

| ID | Título e objetivo | Requisito / endpoint | Pré-condições e massa | Passos | Resultado esperado | Técnica | Pri. / classe / perfil | Automação / status / risco |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| TC-PRD-001 | Administrador cadastra produto válido | `REQ-PRD-01`, `REQ-PRD-03`; `POST /produtos` | Admin sintético autenticado; nome único; preço/quantidade inteiros | Criar produto e guardar `_id` | `201`, `message` e `_id` conforme contrato | Equivalência, contrato | P0; funcional/contrato; Smoke | Sim; Not Run; `RP-02`, `RE-01` |
| TC-PRD-002 | Matriz de autorização de produto | `REQ-PRD-03`; `POST /produtos` | Admin, usuário comum, token ausente e inválido | Repetir escrita para cada perfil | Admin pode criar; token ausente/inválido `401`; usuário comum `403` | Tabela de decisão | P0; segurança funcional; Smoke | Sim; Not Run; `RP-02` |
| TC-PRD-003 | Nome de produto duplicado | `REQ-PRD-02`; `POST /produtos` | Admin e produto próprio existente | Tentar novo cadastro com mesmo nome | `400` documentado; não criar duplicata | Equivalência | P1; negativa; Regression | Sim; Not Run; `RP-05` |
| TC-PRD-004 | Campos e tipos de produto | `REQ-PRD-01`, `D-02`; `POST /produtos` | Admin; payloads com campo ausente ou tipo incompatível | Variar um campo por vez | Obrigatoriedade e mensagens não são definidas; registrar resultado sem oráculo rígido | Equivalência, exploratório | P1; negativa; Exploratório | Condicionado; Not Run; `RP-05` |
| TC-PRD-005 | Limites de preço e quantidade de produto | `D-04`; `POST /produtos` | Admin; valores zero, negativos e limites válidos conhecidos | Enviar uma variação por vez | Regras de cadastro não definidas; registrar resposta e efeito sem inferir aceitação | Valores-limite, exploratório | P1; negativa; Exploratório | Condicionado; Not Run; `RP-05` |
| TC-PRD-006 | Listagem, filtros e consulta por ID | `REQ-PRD-06`; `GET /produtos`, `GET /produtos/{_id}` | Produtos próprios distinguíveis; ID inexistente | Consultar lista/filtros e IDs existente/inexistente | Listagem `200`; ID existente `200`; inexistente `400`; filtros respeitam schema | Equivalência, contrato | P2; contrato; Regression | Sim; Not Run; `RE-01` |
| TC-PRD-007 | Atualização de produto existente | `REQ-PRD-04`; `PUT /produtos/{_id}` | Admin e produto próprio | Atualizar dados e consultar produto | `200` documentado; estado persistido | Transição de estado | P1; funcional/integração; Regression | Sim; Not Run; `RP-07` |
| TC-PRD-008 | Criação de produto por PUT | `REQ-PRD-04`, `D-03`; `PUT /produtos/{_id}` | Admin; ID inexistente; nome único | Enviar produto válido ao ID inexistente | `201` documentado; texto “usuário” é divergência documental, não oráculo funcional | Tabela de decisão | P1; funcional; Regression | Sim; Not Run; `RP-07` |
| TC-PRD-009 | Exclusão de produto sem carrinho | `REQ-PRD-05`; `DELETE /produtos/{_id}` | Admin e produto próprio fora de carrinho | Excluir e consultar ID | `200` documentado; estado posterior observado | Transição de estado | P1; funcional; Regression | Sim; Not Run; `RE-01` |
| TC-PRD-010 | Exclusão bloqueada para produto em carrinho | `REQ-PRD-05`; `DELETE /produtos/{_id}` | Admin, usuário comum, produto vinculado a carrinho | Tentar exclusão e consultar produto/carrinho | `400` documentado; recursos preservados | Tabela de decisão, integração | P0; negativa/integração; Regression | Sim; Not Run; `RP-01`, `RP-04` |

## Carrinhos

| ID | Título e objetivo | Requisito / endpoint | Pré-condições e massa | Passos | Resultado esperado | Técnica | Pri. / classe / perfil | Automação / status / risco |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| TC-CART-001 | Criação válida de carrinho | `REQ-CART-01`, `REQ-CART-02`; `POST /carrinhos` | Usuário comum autenticado, sem carrinho; produto com estoque | Enviar formato de payload confirmado em investigação | `201` é documentado; execução depende de resolver `D-01` | Transição de estado | P0; funcional/integração; Smoke | Condicionado; Not Run; `D-01`, `RP-01` |
| TC-CART-002 | Formato de `produtos` no payload | `D-01`; `POST /carrinhos` | Usuário sem carrinho; produto próprio | Comparar schema `object` e exemplo array em ambiente isolado | Sem oráculo de formato; produzir esclarecimento contratual, não Pass/Fail | Contrato exploratório | P0; contrato; Exploratório | Não até esclarecimento; Not Run; `RP-06`, `RE-03` |
| TC-CART-003 | Produto inexistente no carrinho | `REQ-CART-02`; `POST /carrinhos` | Usuário sem carrinho; ID inexistente; formato resolvido | Criar carrinho com ID inválido | `400` documentado; não criar carrinho | Equivalência | P1; negativa; Regression | Condicionado por `D-01`; Not Run; `RP-04` |
| TC-CART-004 | Quantidade insuficiente | `REQ-CART-02`; `POST /carrinhos` | Usuário sem carrinho; produto com estoque conhecido | Solicitar quantidade acima do estoque | `400` documentado; estoque não deve ser reduzido além de regra confirmada | Valores-limite, integração | P0; negativa/integração; Regression | Condicionado por `D-01`; Not Run; `RP-01`, `RP-04` |
| TC-CART-005 | Produto repetido no mesmo carrinho | `REQ-CART-02`; `POST /carrinhos` | Usuário sem carrinho; mesmo produto em duas entradas | Criar payload repetindo `idProduto` | `400` documentado; não criar carrinho | Tabela de decisão | P1; negativa; Regression | Condicionado por `D-01`; Not Run; `RP-04` |
| TC-CART-006 | Segundo carrinho do mesmo usuário | `REQ-CART-02`; `POST /carrinhos` | Usuário com um carrinho válido; outro produto | Tentar criar segundo carrinho | `400` documentado; primeiro carrinho preservado | Transição de estado | P1; negativa/integração; Regression | Condicionado por `D-01`; Not Run; `RP-04` |
| TC-CART-007 | Listagem, filtros e consulta por ID | `REQ-CART-06`; `GET /carrinhos`, `GET /carrinhos/{_id}` | Carrinho próprio; ID inexistente | Consultar lista/filtros e IDs | Lista `200`; ID existente `200`; inexistente `400`; validar schema publicado | Equivalência, contrato | P2; contrato; Regression | Sim após massa; Not Run; `RE-01` |
| TC-CART-008 | Redução de estoque ao criar carrinho | `REQ-CART-03`; `POST /carrinhos`, `GET /produtos/{_id}` | Produto próprio com quantidade inicial conhecida; usuário sem carrinho | Ler estoque, criar carrinho, reler estoque | Quantidade final = inicial − quantidade solicitada | Transição de estado, integração | P0; integração; Smoke | Condicionado por `D-01`; Not Run; `RP-01` |
| TC-CART-009 | Cancelamento remove carrinho e repõe estoque | `REQ-CART-04`; `DELETE /carrinhos/cancelar-compra` | Carrinho próprio criado; token do titular; estoque antes conhecido | Cancelar, consultar carrinho e produto | `200`; carrinho do titular removido e estoque restaurado | Transição de estado, integração | P0; integração; Smoke | Condicionado por `D-01`; Not Run; `RP-01` |
| TC-CART-010 | Conclusão remove carrinho do titular | `REQ-CART-05`; `DELETE /carrinhos/concluir-compra` | Carrinho próprio; token do titular | Concluir e consultar carrinho | `200`; carrinho removido; não pressupor regra de estoque/preço | Transição de estado | P1; funcional/integração; Regression | Condicionado por `D-01`; Not Run; `RP-08` |
| TC-CART-011 | Totais e preços do carrinho | `REQ-CART-06`, `D-05`; `GET /carrinhos/{_id}` | Carrinho com produtos/preços controlados | Comparar itens, `precoUnitario`, `quantidadeTotal` e `precoTotal` | Schema é verificável; fórmula/semântica de preço depende de esclarecimento de `D-05` | Contrato, exploratório | P1; contrato; Exploratório | Condicionado; Not Run; `RP-08` |
| TC-CART-012 | Integridade do estoque em criação rejeitada | `D-09`, `REQ-CART-02`, `REQ-CART-03`; `POST /carrinhos`, `GET /produtos/{_id}` | Estoque inicial registrado; payload que deve falhar | Enviar solicitação rejeitada e reler estoque | Atomicidade não é documentada; registrar efeito sem assumir ausência de alteração parcial | Transição de estado, exploratório | P0; integração; Exploratório | Condicionado; Not Run; `RP-01` |

## Resumo de design

| Módulo | Casos | Smoke proposto | Regression proposto | Exploratório | Candidatos à automação imediata |
| --- | ---: | ---: | ---: | ---: | ---: |
| Autenticação | 5 | 2 | 2 | 1 | 3 |
| Usuários | 9 | 0 | 8 | 1 | 8 |
| Produtos | 10 | 2 | 6 | 2 | 8 |
| Carrinhos | 12 | 3 | 6 | 3 | 1 |
| **Total** | **36** | **7** | **22** | **7** | **20** |

Há ainda 15 candidatos condicionados a esclarecimentos contratuais e um cenário (`TC-CART-002`) que não deve ser automatizado antes de resolver `D-01`. Perfis e candidaturas serão reavaliados após resolver `D-01`, `D-02`, `D-04`, `D-05` e `D-09`; nenhum caso foi executado.
