# Project Atlas entrypoint — CineTube

1. Leia `atlas.json`, `PROJECT_STATE.md` e `docs/ATLAS.md`.
2. Leia o Goal ativo em `.ai/goals/` e preserve seus critérios de aceitação.
3. Carregue apenas os documentos ligados à tarefa; use Lean Progressive Context.
4. Para decisões de produto, leia `docs/product/`; para limites técnicos, `docs/architecture/`; para implementação, `docs/implementation/`.
5. Não acople domínio a Wails, SuperFlix, TMDB, Turso Cloud ou qualquer runtime específico.
6. Não coloque segredos no frontend, repositório, logs ou arquivos de backup.
7. Não implemente hospedagem, redistribuição ou extração não documentada de mídia.
8. Preserve local-first: cloud nunca pode ser requisito para dados pessoais locais.
9. Trate performance como gate: medir antes/depois de mudanças relevantes.
10. Atualize apenas documentação canônica impactada por mudanças estáveis.
11. Registre evidência de testes/benchmarks antes de declarar Goal concluído.
12. Não crie comentários/notas públicas: comunidade é non-goal explícito.
