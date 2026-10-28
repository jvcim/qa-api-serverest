# ServeRest — Plano de Testes de API

| Campo | Valor |
| --- | --- |
| Projeto | ServeRest — API Quality Engineering |
| Etapa | 02 — Planejamento |
| Versão | 1.1 — revisada documentalmente |
| Data | 26/10/2026 |
| Base | `REQUIREMENTS_ANALYSIS.md` v1.1; OpenAPI 3.0.0 / ServeRest 3.2.2 |
| Situação | Plano aprovado como artefato de planejamento; nenhum teste ou execução foi realizado |

## 1. Objetivo

Planejar a futura verificação funcional, contratual e de regras de negócio da API ServeRest. Este documento organiza escopo, riscos, ambiente e decisões de qualidade; não constitui evidência de comportamento observado.

## 2. Referência contratual e controle de mudanças

A Discovery registra 4 módulos, 9 caminhos, 16 operações, 32 schemas reutilizáveis, 4 request bodies, 4 respostas reutilizáveis e 1 esquema de segurança (`ApiKeyAuth`, no header `Authorization`). O inventário `API-01` a `API-16` e as lacunas `D-01` a `D-10` permanecem em [REQUIREMENTS_ANALYSIS.md](REQUIREMENTS_ANALYSIS.md).

Antes de qualquer execução, deve-se registrar a versão exata da instância, versionar um snapshot do OpenAPI e compará-lo com a referência 3.2.2. Mudanças contratuais exigem revisão do escopo e dos oráculos futuros.

## 3. Escopo planejado

### Incluído

- **Autenticação (`API-01`):** credenciais válidas e inválidas, formato do retorno e expiração documentada (`REQ-AUTH-01/02`).
- **Usuários (`API-02` a `API-06`):** cadastro, consulta, filtros, PUT, exclusão, duplicidade de e-mail e regra associada a carrinho (`REQ-USR-01` a `REQ-USR-05`).
- **Produtos (`API-07` a `API-11`):** CRUD, filtros, duplicidade, perfis autorizados, restrição de exclusão e PUT por ID inexistente (`REQ-PRD-01` a `REQ-PRD-06`).
- **Carrinhos (`API-12` a `API-16`):** criação, consulta, segundo carrinho, quantidade insuficiente, estoque, conclusão e cancelamento (`REQ-CART-01` a `REQ-CART-06`).
- **Contrato HTTP:** método, caminho, status declarados, JSON e campos de resposta; uma inconsistência do OpenAPI nunca será tratada como falha da implementação sem evidência.
- **Integração funcional:** cadeia usuário → autenticação → produto → carrinho, somente com massa controlada.

### Excluído nesta versão

Desempenho, carga, estresse, pentest, interface web/mobile, pagamento, banco de dados, PATCH, paginação, rate limiting e refresh token (`D-10`). Observações de exposição de dados serão tratadas de modo responsável, mas não equivalem a auditoria de segurança.

## 4. Estratégia planejada

| Camada | Objetivo | Abordagem futura |
| --- | --- | --- |
| Análise estática | Compreender contrato e ambiguidades | OpenAPI, repositório oficial e Discovery |
| Exploratória/manual | Esclarecer hipóteses com respostas reais | Postman, instância isolada e registros anonimizados |
| Funcional positiva/negativa | Verificar regras e erros | Equivalência, limites definidos, decisão e transição de estado |
| Contrato | Verificar interface publicada | Status, tipos e schemas OpenAPI após isolar inconsistências |
| Integração | Verificar efeitos entre recursos | Leitura antes/depois, limpeza e massa exclusiva |
| Regressão | Repetir verificações estáveis | Postman, JavaScript e Postman CLI após desenho e confirmação manual |

Somente regras documentadas ou observadas de forma controlada poderão virar oráculos. O perfil smoke será definido a partir de casos futuros; nenhum número mínimo artificial de cenários será imposto.

## 5. Ambiente, massa e evidências

- Preferir ServeRest local isolado, com versão exata registrada; não realizar mutações destrutivas no serviço público compartilhado.
- Usar usuários sintéticos únicos, perfis administrador e comum, produtos exclusivos e carrinhos vinculados à execução.
- Controlar setup, teardown, limpeza e serialização de mutações que alterem estoque.
- Não versionar token, senha ou dado real; mascarar evidências, em especial qualquer ocorrência de `password` (`D-06`).
- `D-01` impede fixar payload definitivo de carrinho até confirmação do formato real ou de fonte oficial complementar.

## 6. Priorização

A ordem de investigação segue [RISK_ASSESSMENT.md](RISK_ASSESSMENT.md). Como hipótese de planejamento, receberão atenção inicial: isolamento de massa e estoque, autorização de produtos, exposição de dados em evidências e divergências contratuais. Prioridade não é evidência de defeito ou severidade de bug.

## 7. Critérios

### Entrada para design

- Inventário `API-01…API-16`, requisitos `REQ-*` e dúvidas `D-01…D-10` revisados.
- Toda regra ambígua marcada como pendente, sem expectativa inventada.

### Entrada para execução manual futura

- Instância isolada e versão identificada; snapshot OpenAPI armazenado.
- Massa sintética, política de evidências e escopo autorizados.

### Entrada para automação/CI futuros

- Oráculos definidos; execução manual preliminar documentada; reset reprodutível; collection e ambiente sem segredos.

### Saída futura

- Casos prioritários terão resultado, evidência, data e rastreabilidade.
- Resultados `Pass`, `Fail`, `Blocked` e `Not Run` serão mantidos separados de status de trabalho.
- A recomendação de qualidade indicará escopo executado, riscos residuais e limitações; não representará decisão do mantenedor da ServeRest.

### Suspensão e retomada futuras

- Suspender quando a instância não for identificável, a massa estiver contaminada, o contrato divergir sem oráculo confiável ou houver risco de exposição de dados.
- Retomar apenas após restabelecer ambiente isolado, reset verificável, versão/snapshot e expectativa sustentada por fonte.

## 8. Gestão de desvios e evidências

Antes de registrar um defeito futuro, revisar contrato, repetir em instância limpa e isolar interferências. A evidência deverá conter método, caminho, massa sintética, versão, pré-condição, estado antes/depois, esperado e obtido. Não registrar bugs, métricas ou aprovações sem execução real.

## 9. Marcos e dependências

1. Discovery: concluída documentalmente.
2. Planejamento: este plano e a análise de riscos, concluídos documentalmente.
3. Design: catálogo, matriz e critérios detalhados — ainda não produzidos.
4. Organização: Jira — ainda não criado.
5. Exploração manual, automação, CI e relatório final — ainda não executados ou criados.

## 10. Dúvidas abertas

`D-01` a `D-10` continuam abertas conforme a Discovery: formato de carrinho, obrigatoriedade, texto do PUT de produtos, limites de cadastro, efeito da conclusão, exposição de `password`, valores de `administrador`, erros de entrada, atomicidade e recursos não publicados. Hipóteses não se tornam obrigações contratuais até haver fonte ou execução controlada.

## Fontes

- [Discovery](REQUIREMENTS_ANALYSIS.md) v1.1.
- https://serverest.dev/swagger.json?lang=pt-BR
- https://github.com/ServeRest/ServeRest
