# ServeRest — Avaliação de Riscos de Qualidade

| Campo | Valor |
| --- | --- |
| Projeto | ServeRest — API Quality Engineering |
| Etapa | 02 — Planejamento |
| Versão | 1.1 — avaliação prospectiva revisada |
| Data | 26/10/2026 |
| Referência | `REQUIREMENTS_ANALYSIS.md` v1.1; `TEST_PLAN.md` v1.1 |
| Estado | Não há falha, incidente ou execução comprovada neste documento |

## 1. Método

Probabilidade e impacto usam escala ordinal de 1 (baixo) a 3 (alto). `Score = probabilidade × impacto`; P0 representa score 9 ou potencial crítico de segurança/integridade ainda não mensurado, P1 representa 4–6 e P2 representa 1–3. Prioridade organiza investigação; não é severidade de bug, estatística ou evidência de falha.

Os riscos são separados em **produto** (possível efeito funcional) e **execução** (risco de resultado não confiável, ambiente ou massa).

## 2. Riscos de produto

| ID | Risco potencial | Rastreabilidade | Prob. | Impacto | Prior. | Medida verificável no futuro |
| --- | --- | --- | ---: | ---: | --- | --- |
| RP-01 | Estoque inconsistente após criar, cancelar ou falhar carrinho | `REQ-CART-02/03/04`, `D-09` | 3 | 3 | P0 | Comparar estado antes/depois em massa exclusiva e observar efeito parcial. |
| RP-02 | Acesso indevido a escrita de produtos | `REQ-PRD-03`, `REQ-AUTH-02` | 2 | 3 | P1 | Comparar token ausente, expirado, comum e administrador com 401/403 documentados. |
| RP-03 | Exposição de `password` em consultas ou evidências | `D-06`, `REQ-USR-05` | A apurar | 3 | P0* | Usar dados sintéticos, acesso restrito e mascaramento; separar contrato de política desejada. |
| RP-04 | Carrinho inconsistente por produto repetido, segundo carrinho ou estoque insuficiente | `REQ-CART-02` | 2 | 3 | P1 | Verificar rejeição e ausência de efeitos não previstos. |
| RP-05 | Campos e limites divergirem ou serem pouco definidos | `D-02`, `D-04`, `D-07`, `D-08` | 2 | 2 | P1 | Levantar comportamento sem supor campos opcionais ou códigos não declarados. |
| RP-06 | Formato de `produtos` divergir do contrato | `D-01` | 3 | 2 | P1 | Confrontar schema, exemplo e instância de versão fixa antes de assert rígida. |
| RP-07 | PUT por ID inexistente ter efeito diferente do declarado | `REQ-USR-03`, `REQ-PRD-04`, `D-03` | 2 | 2 | P1 | Conferir status e persistência em ambiente controlado. |
| RP-08 | Conclusão de compra ter efeito de estoque/preço não especificado | `REQ-CART-05`, `D-05` | 2 | 3 | P1 | Documentar leitura antes/depois sem antecipar oráculo. |

\* P0 pelo potencial de impacto, com probabilidade indeterminada; não é alegação de vulnerabilidade.

## 3. Riscos de execução

| ID | Risco potencial | Rastreabilidade | Prob. | Impacto | Prior. | Controle verificável no futuro |
| --- | --- | --- | ---: | ---: | --- | --- |
| RE-01 | Massa compartilhada ou reset diário invalidar resultado | `C-ENV-03`, `P-ENV-01` | 3 | 3 | P0 | Executar em instância local versionada e usar massa única por execução. |
| RE-02 | Cenários alterarem estoque e contaminarem outros | `REQ-CART-03/04` | 3 | 3 | P0 | Usar reset, teardown e serialização de mutações. |
| RE-03 | Contrato e servidor terem versões diferentes | `D-01`, `C-ENV-02` | 2 | 3 | P1 | Versionar snapshot OpenAPI e registrar tag da instância. |
| RE-04 | Token expirar e gerar falso negativo | `REQ-AUTH-02` | 2 | 2 | P1 | Controlar reautenticação; isolar teste de expiração. |
| RE-05 | Logs ou evidências exporem tokens e dados | `D-06` | 2 | 3 | P1 | Usar variáveis não versionadas e revisão prévia do material. |
| RE-06 | CI divergir do ambiente local | `P-ENV-01` | 2 | 2 | P1 | Fixar versões e comparar execução somente quando CI existir. |

## 4. Ordem de investigação futura

1. Preservar snapshot OpenAPI, identificar versão e definir isolamento/proteção de evidências (`RE-01`, `RE-02`, `RE-03`, `RE-05`).
2. Investigar autenticação e permissões de produto (`RP-02`).
3. Investigar estoque, cancelamento, insuficiência e atomicidade (`RP-01`, `RP-04`).
4. Resolver as lacunas `D-01`, `D-02`, `D-04` a `D-09` antes de fixar expectativas rígidas.
5. Somente então desenhar regressão, smoke, automação e CI (`RE-04`, `RE-06`).

## 5. Aceitação, suspensão e reavaliação

- Os riscos de execução `RE-01` e `RE-02` precisam de controles definidos antes de uma execução confiável.
- Risco de produto confirmado por execução deverá ter evidência, versão, impacto e vínculo `REQ-*`/`D-*`; hipótese não vira bug.
- Reavaliar score, controle e prioridade após exploração controlada, primeiro ciclo manual e antes de qualquer relatório final.
- Toda mudança deverá indicar data, justificativa e rastreabilidade.

## 6. Pendências da próxima etapa

- Definir oráculos sem inventar regras.
- Projetar casos e rastreabilidade após a exploração autorizada.
- Preparar massa, lifecycle e política de evidência sem executar nesta etapa.
- Decidir gestão no Jira antes de criar tickets.

## Fontes

- [Discovery](REQUIREMENTS_ANALYSIS.md) v1.1.
- [Plano de Testes](TEST_PLAN.md) v1.1.
- https://serverest.dev/swagger.json?lang=pt-BR
