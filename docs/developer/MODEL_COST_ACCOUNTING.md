# LLM Cost & Token Accounting

CineTube tracks model usage as engineering evidence, not as an afterthought.

For every LLM call, Project Intelligence should record provider, model, tier, effort, agent, task, input/output token counts when available, cached-token information, observed or estimated cost, fallback reason, evidence before/after, and the final result.

Reports are generated per task, per Goal and cumulatively. The report should flag unnecessary escalation, use of sensitive context in the free pool, and material deviation from the target tier distribution.

The purpose is to answer three questions continuously:

1. Could T0 deterministic tooling have avoided this call?
2. Could a cheaper tier have completed the same acceptance criterion?
3. Did escalation materially improve evidence or merely increase cost?

Canonical machine policy: `.ai/orchestration/model-telemetry.json`.
