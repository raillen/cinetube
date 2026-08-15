# CineTube — Traycer Strict Handoff Template

> Canonical source for the Custom Hand-off Template configured in Traycer. The Traycer UI/template must remain semantically equivalent to this file.

## Mandatory execution header

Before handing work to a coding agent, Traycer MUST resolve and include every field below. Do not hand off with unresolved placeholders.

```text
GOAL_OR_TASK_ID: <resolved>
ATLAS_AGENT: <resolved>
ATLAS_TIER: <T0|T1|T2|T3|T4|T5>
PROVIDER: <google|opencode|command-code>
MODEL: <exact approved model>
EFFORT: <maximum-supported|high|extra-high>
SENSITIVITY: <non-sensitive|sensitive>
ACCEPTANCE_EVIDENCE: <tests/gates/criteria to prove completion>
```

If any field cannot be resolved, STOP with:

`TRAYCER_HANDOFF_PREFLIGHT_FAILED`

## Closed provider + model allowlist

Only these exact pairs are authorized:

```text
GOOGLE
- google / Gemini 3.7 Flash
- google / Gemini 3.1 Pro

OPENCODE
- opencode / DeepSeek V4 Flash Free
- opencode / MiMo V2.5 Free
- opencode / Big Pickle

COMMAND CODE
- command-code / Muse Spark 1.2 Contributor
- command-code / DeepSeek V4 Flash
- command-code / MiniMax M3
- command-code / GPT-5.6 Luna
- command-code / GLM 5.2
```

Provider + model is an indivisible identity. The same model through another provider is forbidden.

## Mandatory policy sources

Read before execution:

1. `AGENTS.md`
2. active Goal/Task
3. `.ai/orchestration/traycer-policy.json`
4. `.ai/orchestration/model-policy.json`
5. `.ai/orchestration/model-catalog.json`
6. `.ai/orchestration/model-routing.json`
7. `.ai/orchestration/fallbacks.json`
8. only the smallest additional context required by the task

## Effort lock

- `command-code / GPT-5.6 Luna` -> always `extra-high`.
- all other paid approved models -> always `high`.
- OpenCode free models -> always maximum supported effort.
- Never lower effort after a model has been selected. Save cost by choosing a cheaper valid tier/model, reducing context, using T0 tools, and stopping on passing evidence.

## Runtime identity check

The coding agent MUST already be configured by its host/provider to use the exact pair written in the execution header. A prompt cannot change the actual model/provider selected by the platform.

If the runtime/provider identity does not match the resolved header, do not execute the task. Return:

`MODEL_PROVIDER_POLICY_VIOLATION`

Never silently substitute a model or provider.

## Sensitive-context gate

The following routes are restricted to non-sensitive material:

- OpenCode / DeepSeek V4 Flash Free
- OpenCode / MiMo V2.5 Free
- OpenCode / Big Pickle
- Command Code / Muse Spark 1.2 Contributor

If `SENSITIVITY: sensitive`, these routes MUST be removed before handoff. Continue only with the next approved unrestricted route in the canonical routing policy.

## Execution rules

- Attempt deterministic T0 evidence first when applicable.
- Execute only the current Goal/Task.
- Do not broaden scope without an accepted Goal amendment.
- Do not perform automatic model/provider substitution.
- Do not use platform `Auto`, `Smart`, `Best Model`, implicit routing, or unlisted fallback behavior.
- One serious attempt per model is the default; retry only when new evidence identifies a narrow repair.
- Escalation requires objective failed evidence or explicit risk classification.
- Model self-reported uncertainty is not escalation evidence.
- If acceptance evidence passes, STOP. Do not call a stronger model ceremonially.
- T5 critical verification requires independent approved providers as defined by `model-routing.json`/`fallbacks.json`.

## Failure behavior

If the required exact approved pair is unavailable and no policy-approved fallback remains, STOP with:

`NO_APPROVED_MODEL_PROVIDER_AVAILABLE`

For critical T5 work where cross-provider verification cannot be achieved, STOP with:

`CROSS_PROVIDER_VERIFICATION_UNAVAILABLE`

Do not invent or discover a replacement model outside the checked-in allowlist.

## Completion record

Return/record at minimum:

```text
RESULT: PASS|FAIL|PARTIAL|UNAVAILABLE|ABORTED
PROVIDER: ...
MODEL: ...
EFFORT: ...
TIER: ...
EVIDENCE: ...
FALLBACK_USED: yes|no
FALLBACK_REASON: ...
```
