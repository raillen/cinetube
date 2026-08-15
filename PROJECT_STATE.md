# Current Project State

- Projeto: **CineTube**
- Framework: **Project Atlas 0.2.0 / Protocol 3**
- Fase: **P00 — Foundation**
- Goal ativo: **P00-G01 — Desktop Foundation & MVP Architecture**
- Estado do Goal: **PLANNED**
- Plataforma prioritária: **Desktop**
- Context methodology: **Lean Progressive Context (LPC)**
- Workforce: **on-demand, source pinada por commit**
- Model routing: **aguardando definição dedicada**
- Última atualização: `2026-08-15T20:56:00Z`

## Estado real

O repositório está na fase documental/arquitetural. A documentação define o produto e as fronteiras para implementação, mas não constitui evidência de que o aplicativo já esteja implementado.

O workforce do Project Atlas não é duplicado no CineTube: manifests referenciam uma source commit-pinned e o Goal resolve apenas Agents, Skills e Recipes necessários, usando cache derivado verificado.

## Próxima ação

Definir a política de LLMs/fallbacks por papel com foco em economia de tokens e custo; depois bloquear e executar `P00-G01`, criando o esqueleto Wails v3/Go/Svelte e os contratos de domínio/providers/storage sem introduzir funcionalidades pós-v1.

## Recovery order

1. `ENTRYPOINT.md`
2. `atlas.json`
3. `PROJECT_STATE.md`
4. `docs/ATLAS.md`
5. `.ai/goals/P00/P00-G01.goal.json`
6. `.ai/orchestration/workforce-sources.json`
7. `docs/developer/WORKFORCE_DEPENDENCIES.md`
8. ADRs relevantes
9. somente os documentos/código/testes/workforce necessários à tarefa
