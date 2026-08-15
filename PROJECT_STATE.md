# Current Project State

- Projeto: **CineTube**
- Framework: **Project Atlas 0.2.0 / Protocol 3**
- Fase: **P00 — Foundation**
- Goal ativo: **P00-G01 — Desktop Foundation & MVP Architecture**
- Estado do Goal: **PLANNED**
- Plataforma prioritária: **Desktop**
- Context methodology: **Lean Progressive Context (LPC)**
- Workforce: **on-demand, commit-pinned**
- LLM routing: **T0–T5, evidence-driven, cost-aware**
- Última atualização: `2026-08-15T21:05:00Z`

## Estado real

O repositório está na fase documental/arquitetural. A documentação define o produto e as fronteiras para implementação, mas não constitui evidência de que o aplicativo já esteja implementado.

O workforce necessário é resolvido sob demanda a partir do Project Atlas pinado. A política de modelos está definida em `.ai/orchestration/`: T0 evita LLM quando ferramentas determinísticas bastam; T1 usa pool gratuito não sensível; tiers pagos escalam somente por evidência/risco; Luna permanece sempre em `extra-high`; demais pagos em `high`; gratuitos no effort máximo suportado.

## Próxima ação

Executar e bloquear formalmente `P00-G01`, criando o esqueleto Wails v3/Go/Svelte e os contratos de domínio/providers/storage sem introduzir funcionalidades pós-v1. Antes da primeira execução agentic, o orchestrator deve verificar a disponibilidade live dos modelos aprovados e resolver apenas o workforce requerido pelo Goal.

## Recovery order

1. `ENTRYPOINT.md`
2. `atlas.json`
3. `PROJECT_STATE.md`
4. `docs/ATLAS.md`
5. `.ai/goals/P00/P00-G01.goal.json`
6. `.ai/orchestration/workforce-sources.json`
7. `.ai/orchestration/model-policy.json`
8. ADRs relevantes
9. somente os documentos/código/testes necessários à tarefa
