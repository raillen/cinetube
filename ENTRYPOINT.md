# Project Atlas entrypoint — CineTube

1. Leia `atlas.json`, `PROJECT_STATE.md` e `docs/ATLAS.md`.
2. Leia o Goal ativo em `.ai/goals/` e preserve seus critérios de aceitação.
3. Se a tarefa usar agentes, resolva somente o workforce necessário via `.ai/orchestration/workforce-sources.json` e `workforce-bundles.json`.
4. Antes de chamar LLM, aplique `.ai/orchestration/model-policy.json`, `model-routing.json` e `fallbacks.json`; tente T0 determinístico quando aplicável.
5. GPT-5.6 Luna sempre usa `extra-high`; outros modelos pagos usam `high`; gratuitos usam o máximo effort suportado. Não reduza effort para economizar.
6. Nunca envie segredos, dados pessoais, dumps privados, credenciais ou conteúdo sensível ao pool gratuito do OpenCode.
7. Carregue apenas os documentos ligados à tarefa; use Lean Progressive Context.
8. Para decisões de produto, leia `docs/product/`; para limites técnicos, `docs/architecture/`; para implementação, `docs/implementation/`.
9. Não acople domínio a Wails, SuperFlix, TMDB, Turso Cloud ou qualquer runtime específico.
10. Não coloque segredos no frontend, repositório, logs ou arquivos de backup.
11. Não implemente hospedagem, redistribuição ou extração não documentada de mídia.
12. Preserve local-first: cloud nunca pode ser requisito para dados pessoais locais.
13. Trate performance como gate: medir antes/depois de mudanças relevantes.
14. Atualize apenas documentação canônica impactada por mudanças estáveis.
15. Registre evidência de testes/benchmarks e uso de modelo/tier antes de declarar Goal concluído.
16. Não crie comentários/notas públicas: comunidade é non-goal explícito.
