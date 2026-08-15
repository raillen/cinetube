# LLM Cost & Token Accounting

CineTube tracks model usage as engineering evidence, not as an afterthought.

For every LLM call, Project Intelligence should record provider, model, tier, effort, data-policy class, agent, task, input/output token counts when available, cached-token information, observed or estimated cost, fallback reason, evidence before/after, and the final result.

Reports are generated per task, per Goal and cumulatively. The report should flag unnecessary escalation, any use of restricted-data routes with sensitive context, and material deviation from the target tier distribution.

## Restricted-data economic routes

Two classes of routes are intentionally cheap but cannot receive sensitive context:

- OpenCode free models, while their free-period policy permits data use for model improvement.
- Meta Muse Spark 1.2 **Contributor**, whose discounted access tier permits activity to be used to improve Meta products.

These routes may be excellent for public/non-sensitive source code, tests, documentation, mechanical work and reproducible debugging, but they must be skipped when context includes secrets, credentials, private user data, real backups, payment/billing data, production dumps or other sensitive/proprietary material.

Muse Spark 1.2 Contributor is tracked separately with `cost_source = contributor-discount` so we can measure whether using it before ordinary paid T2/T3 models actually reduces marginal cost while preserving acceptance evidence.

The purpose is to answer continuously:

1. Could T0 deterministic tooling have avoided this call?
2. Could a cheaper tier have completed the same acceptance criterion?
3. Did Muse Spark 1.2 Contributor or another economic route pass the same gates before an expensive fallback was needed?
4. Did escalation materially improve evidence or merely increase cost?
5. Was any restricted-data route incorrectly given sensitive context?

Do not hard-code volatile provider prices into architectural decisions. Record the observed provider price/cost at execution time and retain it with the task evidence.

Canonical machine policy: `.ai/orchestration/model-telemetry.json`.
