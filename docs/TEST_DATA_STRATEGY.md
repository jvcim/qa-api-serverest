# ServeRest — Estratégia de Dados de Teste

| Campo | Valor |
| --- | --- |
| Etapa | 03 — Design de Testes |
| Estado | Estratégia planejada; nenhuma massa foi criada ou executada |
| Referências | `C-ENV-03`, `P-ENV-01`, `REQ-CART-03/04`, `D-01`, `D-06`, `D-09` |

## 1. Ambiente e versionamento

- Preferir instância ServeRest local e isolada, com versão fixada antes da primeira execução.
- Registrar origem, versão, data e snapshot do OpenAPI correspondente.
- Executar cada fluxo de modo independente; não depender de dados pré-cadastrados ou do ambiente público compartilhado.
- Preparar variáveis de ambiente para URL, credenciais sintéticas e parâmetros de execução futura; não versionar arquivos com segredos.
- O ambiente de CI só será definido depois que os oráculos e o lifecycle local forem estáveis.

## 2. Massa sintética planejada

| Recurso | Estratégia | Identificadores dinâmicos |
| --- | --- | --- |
| Administrador | Criar usuário próprio com `administrador: "true"` e e-mail único por execução | `_id`, e-mail e token guardados apenas em memória/variáveis seguras |
| Usuário comum | Criar usuário próprio com `administrador: "false"` e e-mail único | `_id`, e-mail e token |
| E-mail | Prefixo do cenário + timestamp/UUID, em domínio reservado de teste | Evita colisões e permite rastrear limpeza |
| Produto | Nome exclusivo por execução, preço inteiro positivo e quantidade controlada | `_id`, preço e estoque inicial |
| Valores inválidos | Gerar apenas no cenário negativo, uma variação por requisição | Não reutilizar recurso preparado se a requisição falhar |
| Carrinho | Associar ao usuário comum exclusivo e guardar IDs de produtos/estoques antes da criação | `_id` do carrinho, `idUsuario`, estoque antes/depois |

Valores de preço, quantidade, campos obrigatórios e formato de carrinho que não estejam definidos pelo contrato não serão transformados em massa “válida” sem investigação: permanecem ligados a `D-01`, `D-02`, `D-04` e `D-07`.

## 3. Isolamento, lifecycle e limpeza

1. Criar administrador e autenticar; criar somente produtos necessários ao cenário.
2. Criar usuário comum e autenticar; reservar um usuário sem carrinho para fluxos de criação.
3. Para cada cenário de carrinho, capturar estoque inicial e criar recursos próprios, sem paralelizar mutações no mesmo produto.
4. Quando o contrato permitir, cancelar o carrinho para remover o vínculo e restaurar estoque (`REQ-CART-04`).
5. Excluir produtos não vinculados e usuários sem carrinho; se a limpeza for bloqueada, registrar o recurso sintético e reinicializar a instância local em vez de contaminar o próximo cenário.
6. Se uma preparação falhar, marcar o cenário dependente como `Blocked` apenas durante execução futura; não continuar usando estado incerto.

`REQ-CART-03/04` exige que setup e teardown observem estoque antes/depois. `D-09` impede assumir atomicidade em falhas: toda rejeição de criação deverá preservar evidência de estado antes/depois quando a etapa de execução for autorizada.

## 4. Jornada integrada planejada

1. Criar administrador → autenticar → criar produto e reter `_id`/estoque.
2. Criar usuário comum → autenticar → criar carrinho com produto controlado.
3. Consultar carrinho e produto para registrar somente efeitos documentados.
4. Cancelar **ou** concluir compra com o token do titular.
5. Consultar o estado final de recurso e realizar limpeza possível.

O formato de `produtos` no carrinho só será escolhido após esclarecimento de `D-01`. A conclusão de compra só verificará remoção do carrinho até que `D-05` esclareça estoque/preço. Esta jornada é design, não registro de execução.

## 5. Segurança e evidências

- Não usar credenciais pessoais, dados reais ou tokens reais em documentação pública.
- Mascarar `Authorization`, `password`, e-mail e identificadores se forem exibidos em logs, relatórios ou capturas.
- Utilizar exclusivamente recursos sintéticos e variáveis locais/CI não versionadas.
- Revisar saída de ferramentas antes de commit; `.env` e artefatos de execução permanecem ignorados.
- Não tratar a presença do campo `password` no schema (`D-06`) como autorização para expor seu conteúdo.

## 6. Preparação para automação futura

Quando autorizada, a automação deverá criar dados por cenário, transportar IDs dinamicamente, manter coleções/ambientes sem segredos e permitir reset reprodutível. Cenários de estoque e carrinho deverão ser serializados ou receber produtos exclusivos. Nenhuma collection, script, variável de ambiente real ou configuração de CI é criada por este documento.
