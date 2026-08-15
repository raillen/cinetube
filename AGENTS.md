# AGENTS.md — CineTube

## Missão

Construir um media center desktop local-first, leve, seguro e provider-agnostic. O desktop v1.0 é obrigatório; Web/PWA, Android, cloud sync e billing são pós-v1.

## Ordem de leitura

1. `ENTRYPOINT.md`
2. `atlas.json`
3. `PROJECT_STATE.md`
4. `docs/ATLAS.md`
5. Goal ativo
6. ADRs/contratos diretamente relacionados
7. resolver somente o workforce exigido pelo Goal/Task

## Invariantes

- Dados pessoais do usuário são locais por padrão.
- Nenhum cadastro é necessário para perfis locais.
- Wails é adapter de transporte/plataforma; domínio Go não importa Wails.
- Componentes Svelte não chamam bindings Wails diretamente; usam contratos/clientes de aplicação.
- Providers são capabilities independentes e substituíveis.
- SuperFlix não é modelo de domínio nem fonte única de metadata.
- IDs internos permanecem estáveis mesmo que providers mudem.
- Recomendações são locais, determinísticas e sem IA.
- Home é finita; não implementar feed infinito.
- Não usar autoplay de trailer/preview.
- Não hospedar/retransmitir mídia nem extrair streams não documentados.
- Comentários, notas públicas e comunidade estão fora de escopo.
- Backups portáteis não incluem cache reconstruível.
- Nunca armazenar chaves de criptografia junto do banco/backup.
- Nenhum segredo em frontend, Git ou logs.

## Workforce sob demanda

- `.ai/*/manifest.json` são manifests de dependência, não cópias locais de workforce.
- Leia `.ai/orchestration/workforce-sources.json` e `docs/developer/WORKFORCE_DEPENDENCIES.md`.
- Resolva somente Agents, Skills e Recipes requeridos pelo Goal/Task.
- Use apenas source fixada por commit SHA e cache previamente verificado.
- Não baixar ou injetar o catálogo completo por conveniência.
- Cache derivado fica fora do Git em `.atlas/cache/` ou no cache global do Project Atlas.
- Falha de integridade é fail-closed.
- Upgrade de workforce exige mudança explícita do pin e revisão.
- Papel de Agent e escolha de LLM são independentes; modelos/fallbacks são definidos em policy própria.

## Arquitetura alvo

`UI Svelte -> MediaClient -> Wails adapter -> Application Services -> Domain/Ports -> Adapters (providers, DB, filesystem)`.

## Qualidade

- Tests: unitários de domínio, contratos de adapters, integração DB/providers, UI, E2E dos fluxos críticos.
- Performance: medir startup, memória idle, Home, Search, cache e playback handoff.
- Segurança: threat model e security review obrigatórios para storage, backup, player remoto e qualquer cloud futura.
- Acessibilidade: teclado completo, foco visível, contraste e reduced motion.

## Dependências de runtime

Não adicionar framework/biblioteca apenas por conveniência. Para cada dependência de runtime registre: problema resolvido, custo de bundle/memória, manutenção, licença e alternativa nativa.

## Git/PR

Mudanças de arquitetura exigem ADR. Mudanças de schema exigem migration + rollback/compatibilidade testada. Mudanças de comportamento estável exigem Documentation Delta correspondente.
