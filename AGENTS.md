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

## Arquitetura alvo

`UI Svelte -> MediaClient -> Wails adapter -> Application Services -> Domain/Ports -> Adapters (providers, DB, filesystem)`.

## Workforce e modelos

- Agents/skills/recipes são dependências resolvidas sob demanda a partir da source pinada em `.ai/orchestration/workforce-sources.json`.
- Nunca copie todo o workforce para o projeto por conveniência; use o menor bundle necessário ao Goal/Task.
- Antes de chamar LLM, tente evidência T0 determinística quando aplicável.
- Roteamento de LLM obedece `.ai/orchestration/model-policy.json`, `model-catalog.json`, `model-routing.json` e `fallbacks.json`.
- GPT-5.6 Luna usa sempre `extra-high`; outros modelos pagos usam `high`; gratuitos usam effort máximo suportado.
- OpenCode free pool e Command Code Muse Spark 1.2 Contributor são rotas de contexto restrito: somente material público/não sensível.
- Muse Spark 1.2 Contributor deve ser preferido cedo em T2/T3 para coding não sensível quando reduzir custo, mas nunca é autoridade T4/T5.
- Escalone por evidência, gate ou risco explícito; nunca por insegurança declarada do modelo.
- T5 crítico exige revisão independente cross-provider quando disponível.

## Qualidade

- Tests: unitários de domínio, contratos de adapters, integração DB/providers, UI, E2E dos fluxos críticos.
- Performance: medir startup, memória idle, Home, Search, cache e playback handoff.
- Segurança: threat model e security review obrigatórios para storage, backup, player remoto e qualquer cloud futura.
- Acessibilidade: teclado completo, foco visível, contraste e reduced motion.

## Dependências

Não adicionar framework/biblioteca apenas por conveniência. Para cada dependência de runtime registre: problema resolvido, custo de bundle/memória, manutenção, licença e alternativa nativa.

## Git/PR

Mudanças de arquitetura exigem ADR. Mudanças de schema exigem migration + rollback/compatibilidade testada. Mudanças de comportamento estável exigem Documentation Delta correspondente.
