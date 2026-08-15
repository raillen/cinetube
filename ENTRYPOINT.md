# Project Atlas entrypoint — CineTube

1. Leia `atlas.json`, `PROJECT_STATE.md` e `docs/ATLAS.md`.
2. Leia o Goal ativo em `.ai/goals/` e preserve seus critérios de aceitação.
3. Resolva somente o workforce exigido pelo Goal/Task conforme `docs/developer/WORKFORCE_DEPENDENCIES.md`; manifests são referências, não conteúdo materializado.
4. Carregue apenas os documentos e recursos ligados à tarefa; use Lean Progressive Context.
5. Para decisões de produto, leia `docs/product/`; para limites técnicos, `docs/architecture/`; para implementação, `docs/implementation/`.
6. Não acople domínio a Wails, SuperFlix, TMDB, Turso Cloud ou qualquer runtime específico.
7. Não coloque segredos no frontend, repositório, logs ou arquivos de backup.
8. Não implemente hospedagem, redistribuição ou extração não documentada de mídia.
9. Preserve local-first: cloud nunca pode ser requisito para dados pessoais locais.
10. Trate performance como gate: medir antes/depois de mudanças relevantes.
11. Atualize apenas documentação canônica impactada por mudanças estáveis.
12. Registre evidência de testes/benchmarks antes de declarar Goal concluído.
13. Não crie comentários/notas públicas: comunidade é non-goal explícito.
14. Não altere o pin de workforce nem o roteamento de LLMs silenciosamente.
