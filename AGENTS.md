# AGENTS.md — CineTube

## Missão

Construir um media center desktop local-first, leve, seguro e provider-agnostic. O desktop v1.0 é obrigatório; Web/PWA, Android, cloud sync e billing são pós-v1.

## Ordem de leitura

Para qualquer agente que entre no projeto a partir do repositório:

1. `ENTRYPOINT.md`
2. `AGENTS.md`
3. `atlas.json`
4. `PROJECT_STATE.md`
5. `docs/ATLAS.md`
6. Goal ativo
7. ADRs/contratos diretamente relacionados

Hermes usa `.hermes.md` como bootstrap nativo de maior prioridade, mas `.hermes.md` deve imediatamente encaminhá-lo para `ENTRYPOINT.md`; não é uma segunda fonte de arquitetura.

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
- `provider + model + effort` é identidade indivisível; o mesmo modelo por outro provider é outra rota e é proibido se não estiver na allowlist.
- GPT-5.6 Luna usa sempre `extra-high`; outros modelos pagos usam `high`; gratuitos usam effort máximo suportado.
- OpenCode free pool e Command Code Muse Spark 1.2 Contributor são rotas de contexto restrito: somente material público/não sensível.
- Muse Spark 1.2 Contributor deve ser preferido cedo em T2/T3 para coding não sensível quando reduzir custo, mas nunca é autoridade T4/T5.
- Escalone por evidência, gate ou risco explícito; nunca por insegurança declarada do modelo.
- T5 crítico exige revisão independente cross-provider quando disponível.

## Hermes Agent

- `.hermes.md` é o bootstrap nativo; ele deve começar por `ENTRYPOINT.md`.
- Hermes deve obedecer `.ai/orchestration/hermes-policy.json` antes de selecionar main model, auxiliary model ou delegation.
- Nunca usar `provider: auto`, provider default, portal/agregador ou fallback implícito como substituto de um par Atlas não disponível.
- Nomes lógicos Atlas (`google`, `opencode`, `command-code`) não são automaticamente IDs de provider do Hermes; o mapping precisa ser explícito e verificado.
- Se o provider/model exato não puder ser executado no Hermes, abortar com `HERMES_NO_APPROVED_MODEL_PROVIDER_AVAILABLE`.
- Delegation e auxiliary models também obedecem à allowlist e à sensibilidade; saída com runtime identity errado não conta como evidência do Goal.
- Guia: `docs/developer/HERMES_INTEGRATION.md`.

## Traycer

Se Traycer for usado, obedecer `.ai/orchestration/traycer-policy.json` e `.ai/orchestration/traycer-handoff-template.md`. O handoff deve resolver provider/model/effort antes de executar e falhar fechado quando o par não estiver disponível.

## Qualidade

- Tests: unitários de domínio, contratos de adapters, integração DB/providers, UI, E2E dos fluxos críticos.
- Performance: medir startup, memória idle, Home, Search, cache e playback handoff.
- Segurança: threat model e security review obrigatórios para storage, backup, player remoto e qualquer cloud futura.
- Acessibilidade: teclado completo, foco visível, contraste e reduced motion.

## Dependências

Não adicionar framework/biblioteca apenas por conveniência. Para cada dependência de runtime registre: problema resolvido, custo de bundle/memória, manutenção, licença e alternativa nativa.

## Git/PR

Mudanças de arquitetura exigem ADR. Mudanças de schema exigem migration + rollback/compatibilidade testada. Mudanças de comportamento estável exigem Documentation Delta correspondente.
