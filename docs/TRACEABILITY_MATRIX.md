# ServeRest — Matriz de Rastreabilidade

| Campo | Valor |
| --- | --- |
| Etapa | 03 — Design de Testes |
| Estado de execução | Todos os casos: `Not Run` |
| Escopo | Cobertura planejada; não representa aprovação nem conformidade da API |

## Requisito → Endpoint → Risco → Casos

| Requisito / lacuna | Operação | Riscos | Casos planejados | Perfil / automação | Execução |
| --- | --- | --- | --- | --- | --- |
| `REQ-AUTH-01` | `API-01` `POST /login` | `RP-02`, `RE-05` | TC-AUTH-001, 002, 004 | Smoke/Regression; sim | Not Run |
| `REQ-AUTH-02`, `D-02` | `API-01`, `API-08` | `RP-02`, `RE-04`, `RP-05` | TC-AUTH-003, 005 | Exploratório/Smoke; condicionado | Not Run |
| `REQ-USR-01`, `REQ-USR-02` | `API-03` | `RP-05`, `RE-01` | TC-USR-001, 002, 003 | Regression/Exploratório; parcial | Not Run |
| `REQ-USR-05`, `D-06` | `API-02`, `API-04` | `RP-03`, `RE-05` | TC-USR-004, 005 | Regression; sim com mascaramento | Not Run |
| `REQ-USR-03` | `API-05` | `RP-07` | TC-USR-006, 007 | Regression; sim | Not Run |
| `REQ-USR-04` | `API-06` | `RP-01`, `RP-04` | TC-USR-008, 009 | Regression; sim | Not Run |
| `REQ-PRD-01`, `REQ-PRD-02`, `REQ-PRD-03`, `D-02`, `D-04` | `API-08` | `RP-02`, `RP-05` | TC-PRD-001 a 005 | Smoke/Regression/Exploratório; parcial | Not Run |
| `REQ-PRD-06` | `API-07`, `API-09` | `RE-01` | TC-PRD-006 | Regression; sim | Not Run |
| `REQ-PRD-04`, `D-03` | `API-10` | `RP-07` | TC-PRD-007, 008 | Regression; sim | Not Run |
| `REQ-PRD-05` | `API-11` | `RP-01`, `RP-04` | TC-PRD-009, 010 | Regression; sim | Not Run |
| `REQ-CART-01`, `REQ-CART-02`, `D-01` | `API-13` | `RP-01`, `RP-04`, `RP-06`, `RE-03` | TC-CART-001 a 006 | Smoke/Regression/Exploratório; condicionado | Not Run |
| `REQ-CART-06`, `D-05` | `API-12`, `API-14` | `RP-08`, `RE-01` | TC-CART-007, 011 | Regression/Exploratório; parcial | Not Run |
| `REQ-CART-03`, `D-09` | `API-13`, `API-09` | `RP-01` | TC-CART-008, 012 | Smoke/Exploratório; condicionado | Not Run |
| `REQ-CART-04` | `API-16` | `RP-01` | TC-CART-009 | Smoke; condicionado | Not Run |
| `REQ-CART-05`, `D-05` | `API-15` | `RP-08` | TC-CART-010, 011 | Regression/Exploratório; condicionado | Not Run |

## Cobertura planejada das operações

| Operação | Coberta por | Estado |
| --- | --- | --- |
| API-01 Login | TC-AUTH-001 a 004 | Planejada |
| API-02 Listar usuários | TC-USR-004 | Planejada |
| API-03 Cadastrar usuário | TC-USR-001 a 003 | Planejada |
| API-04 Consultar usuário | TC-USR-005 | Planejada |
| API-05 PUT usuário | TC-USR-006, 007 | Planejada |
| API-06 Excluir usuário | TC-USR-008, 009 | Planejada |
| API-07 Listar produtos | TC-PRD-006 | Planejada |
| API-08 Cadastrar produto | TC-AUTH-005, TC-PRD-001 a 005 | Planejada |
| API-09 Consultar produto | TC-PRD-006, TC-CART-008, 012 | Planejada |
| API-10 PUT produto | TC-PRD-007, 008 | Planejada |
| API-11 Excluir produto | TC-PRD-009, 010 | Planejada |
| API-12 Listar carrinhos | TC-CART-007 | Planejada |
| API-13 Criar carrinho | TC-CART-001 a 006, 008, 012 | Planejada, condicionada a `D-01` |
| API-14 Consultar carrinho | TC-CART-007, 010, 011 | Planejada |
| API-15 Concluir compra | TC-CART-010 | Planejada, efeito de estoque pendente em `D-05` |
| API-16 Cancelar compra | TC-CART-009 | Planejada |

## Lacunas que impedem critérios fechados

| Dúvida | Casos afetados | Tratamento |
| --- | --- | --- |
| `D-01` formato de carrinho | TC-CART-001 a 006, 008 a 012 | Condicionar payload e automação à confirmação |
| `D-02` obrigatoriedade | TC-AUTH-003, TC-USR-003, TC-PRD-004 | Explorar e registrar sem Pass/Fail prematuro |
| `D-04` limites de cadastro | TC-PRD-005 | Explorar limites sem inferir regra |
| `D-05` conclusão/totais | TC-CART-010, 011 | Validar somente efeitos documentados |
| `D-06` campo password | TC-USR-004 | Mascarar dados e avaliar responsavelmente |
| `D-09` atomicidade | TC-CART-012 | Explorar estado antes/depois |

## Resumo

As 16 operações possuem pelo menos um cenário planejado. Das 36 propostas, 20 são candidatas imediatas à automação, 15 são candidatas condicionadas a esclarecimentos contratuais, 1 não deve ser automatizada antes de resolver `D-01` e 7 possuem perfil exploratório. Cobertura planejada não é cobertura executada; o resultado é `Not Run` para todos os casos.
