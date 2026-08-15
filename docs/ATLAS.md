# CineTube Documentation Atlas

Este arquivo é o roteador canônico. Não leia toda a documentação por padrão.

## Estado e execução

- [`PROJECT_STATE.md`](../PROJECT_STATE.md) — estado real e próxima ação.
- [`atlas.json`](../atlas.json) — configuração Project Atlas.
- [Goal ativo](../.ai/goals/P00/P00-G01.goal.json) — critério mensurável de conclusão.

## Produto

- [`product/PRODUCT_VISION.md`](product/PRODUCT_VISION.md) — visão, princípios e posicionamento.
- [`product/REQUIREMENTS.md`](product/REQUIREMENTS.md) — requisitos funcionais/não funcionais.
- [`product/SCOPE_AND_ROADMAP.md`](product/SCOPE_AND_ROADMAP.md) — v1 desktop e pós-v1.
- [`product/USER_FLOWS.md`](product/USER_FLOWS.md) — fluxos críticos.
- [`product/RECOMMENDATION_ENGINE.md`](product/RECOMMENDATION_ENGINE.md) — recomendações locais.

## Arquitetura

- [`architecture/SYSTEM_ARCHITECTURE.md`](architecture/SYSTEM_ARCHITECTURE.md)
- [`architecture/DOMAIN_MODEL.md`](architecture/DOMAIN_MODEL.md)
- [`architecture/FRONTEND_BACKEND_BOUNDARY.md`](architecture/FRONTEND_BACKEND_BOUNDARY.md)
- [`architecture/PROVIDER_ARCHITECTURE.md`](architecture/PROVIDER_ARCHITECTURE.md)
- [`architecture/STORAGE_AND_SYNC.md`](architecture/STORAGE_AND_SYNC.md)
- [`architecture/MULTI_PROFILE.md`](architecture/MULTI_PROFILE.md)
- [`architecture/PLATFORM_STRATEGY.md`](architecture/PLATFORM_STRATEGY.md)
- [`architecture/adr/`](architecture/adr/) — decisões aceitas.

## UI/UX

- [`ui-ux/DESIGN_SYSTEM.md`](ui-ux/DESIGN_SYSTEM.md)
- [`ui-ux/INFORMATION_ARCHITECTURE.md`](ui-ux/INFORMATION_ARCHITECTURE.md)
- [`ui-ux/SCREENS.md`](ui-ux/SCREENS.md)
- [`ui-ux/PERFORMANCE_UI_RULES.md`](ui-ux/PERFORMANCE_UI_RULES.md)

## Implementação

- [`implementation/STACK.md`](implementation/STACK.md)
- [`implementation/REPOSITORY_STRUCTURE.md`](implementation/REPOSITORY_STRUCTURE.md)
- [`implementation/DATABASE_SCHEMA.md`](implementation/DATABASE_SCHEMA.md)
- [`implementation/API_CONTRACTS.md`](implementation/API_CONTRACTS.md)
- [`implementation/SUPERFLIX_INTEGRATION.md`](implementation/SUPERFLIX_INTEGRATION.md)
- [`implementation/TMDB_INTEGRATION.md`](implementation/TMDB_INTEGRATION.md)
- [`implementation/BACKUP_FORMAT.md`](implementation/BACKUP_FORMAT.md)
- [`implementation/PERFORMANCE_BUDGETS.md`](implementation/PERFORMANCE_BUDGETS.md)

## Qualidade e segurança

- [`quality/TEST_STRATEGY.md`](quality/TEST_STRATEGY.md)
- [`quality/ACCESSIBILITY.md`](quality/ACCESSIBILITY.md)
- [`security/THREAT_MODEL.md`](security/THREAT_MODEL.md)
- [`security/DATA_SECURITY.md`](security/DATA_SECURITY.md)
- [`security/CLOUD_AND_BILLING_SECURITY.md`](security/CLOUD_AND_BILLING_SECURITY.md)

## Operação, governança e futuro

- [`operations/BUILD_RELEASE.md`](operations/BUILD_RELEASE.md)
- [`operations/BACKUP_RECOVERY.md`](operations/BACKUP_RECOVERY.md)
- [`operations/PROVIDER_FAILURE.md`](operations/PROVIDER_FAILURE.md)
- [`governance/LEGAL_AND_CONTENT.md`](governance/LEGAL_AND_CONTENT.md)
- [`governance/PRIVACY.md`](governance/PRIVACY.md)
- [`governance/DECISIONS.md`](governance/DECISIONS.md)
- [`future/WEB_PWA.md`](future/WEB_PWA.md)
- [`future/ANDROID.md`](future/ANDROID.md)
- [`future/CLOUD_PREMIUM.md`](future/CLOUD_PREMIUM.md)

## Desenvolvedor e agentes

- [`developer/GETTING_STARTED.md`](developer/GETTING_STARTED.md)
- [`developer/CONTRIBUTING.md`](developer/CONTRIBUTING.md)
- [`developer/WORKFORCE_DEPENDENCIES.md`](developer/WORKFORCE_DEPENDENCIES.md) — resolução sob demanda de agents/skills/recipes.
- [`developer/MODEL_ROUTING.md`](developer/MODEL_ROUTING.md) — tiers, modelos, effort, fallback, restrições de dados e economia de tokens/créditos.
- [`developer/MODEL_COST_ACCOUNTING.md`](developer/MODEL_COST_ACCOUNTING.md) — telemetria de tokens/custo e avaliação dos tiers econômicos.
- [`developer/HERMES_INTEGRATION.md`](developer/HERMES_INTEGRATION.md) — bootstrap, provider mapping, main/auxiliary models, delegation e enforcement fail-closed no Hermes Agent.
- [`developer/TRAYCER_INTEGRATION.md`](developer/TRAYCER_INTEGRATION.md) — enforcement de provider/model/effort e handoff no Traycer.
- [`user/PRODUCT_GUIDE.md`](user/PRODUCT_GUIDE.md)

## Orquestração canônica

- [`.ai/orchestration/workforce-sources.json`](../.ai/orchestration/workforce-sources.json) — fonte pinada do Project Atlas.
- [`.ai/orchestration/workforce-bundles.json`](../.ai/orchestration/workforce-bundles.json) — workforce mínimo por classe de tarefa.
- [`.ai/orchestration/model-policy.json`](../.ai/orchestration/model-policy.json) — regras globais de tiers, effort e sensibilidade.
- [`.ai/orchestration/model-catalog.json`](../.ai/orchestration/model-catalog.json) — pool permitido de modelos, incluindo Command Code / Muse Spark 1.2 Contributor.
- [`.ai/orchestration/model-routing.json`](../.ai/orchestration/model-routing.json) — rotas por agent.
- [`.ai/orchestration/fallbacks.json`](../.ai/orchestration/fallbacks.json) — escalonamento baseado em evidência.
- [`.ai/orchestration/model-telemetry.json`](../.ai/orchestration/model-telemetry.json) — custo, tier, data-policy e resultado por chamada.
- [`.ai/orchestration/hermes-policy.json`](../.ai/orchestration/hermes-policy.json) — bridge Atlas ↔ Hermes, mapping explícito e validação de runtime identity.
- [`.ai/orchestration/traycer-policy.json`](../.ai/orchestration/traycer-policy.json) — bridge Atlas ↔ Traycer.
- [`.ai/orchestration/traycer-handoff-template.md`](../.ai/orchestration/traycer-handoff-template.md) — template canônico de handoff do Traycer.

## Para agentes

Comece pelo `ENTRYPOINT.md` e use somente os links necessários. Hermes entra por `.hermes.md`, mas esse arquivo apenas o encaminha para `ENTRYPOINT.md` e para a policy Atlas; ele não substitui o fluxo canônico.

Não reintroduza funcionalidades explicitamente removidas: comentários/notas públicas, guest mode, cloud obrigatório ou feed infinito.

Ao orquestrar LLMs, execute T0 quando aplicável, respeite o effort fixo e não escale sem evidência ou risco explícito. Nunca envie contexto sensível às rotas de dados restritos: OpenCode free pool e Command Code / Muse Spark 1.2 Contributor. Muse Contributor pode ser usado cedo em T2/T3 para código não sensível, mas não participa como autoridade de T4/T5.
