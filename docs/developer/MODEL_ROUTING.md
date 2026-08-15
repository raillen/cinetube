# LLM Routing & Fallback Policy — CineTube

> Canonical operational explanation for `.ai/orchestration/model-policy.json`, `model-catalog.json`, `model-routing.json` and `fallbacks.json`.

## Objective

Minimize token consumption and marginal monetary cost without weakening correctness, security or release evidence. Model strength is not used as a status symbol: the orchestrator starts with deterministic evidence or the cheapest capable worker and escalates only when measurable evidence or task risk justifies it.

Model routing is operational infrastructure. It must not leak provider-specific assumptions into the CineTube domain architecture.

## Fixed effort rules

These rules are project-owner decisions and are not dynamically reduced to save money:

- **GPT-5.6 Luna:** always `extra-high` reasoning effort.
- **All other paid models:** always `high` reasoning effort.
- **OpenCode free models:** always the maximum reasoning effort supported by that model/provider.
- The orchestrator may choose a cheaper model or a lower tier, but it must not lower the configured effort of a selected model.

This deliberately separates **model selection** from **reasoning effort**. Economy comes primarily from avoiding unnecessary calls, restricting context, choosing cheaper tiers and stopping when evidence passes.

## Approved model pool

### Google

- `Gemini 3.7 Flash` — efficient agentic/coding and multimodal work; default Google workhorse.
- `Gemini 3.1 Pro` — deep/critical reasoning and cross-provider verification.

`Gemini 3.7 Flash` is a newly released/provider-visible model. The orchestrator must verify the live provider identifier before invocation because provider catalogs can update faster than checked-in documentation.

### OpenCode — free pool

- `DeepSeek V4 Flash Free`
- `MiMo-V2.5 Free`
- `Big Pickle`

These are T1 workers only. Availability and free status are runtime concerns and may change.

**Security restriction:** OpenCode documents that free-period data for these models may be collected/used for model improvement. Therefore they may receive only public or non-sensitive project context. Never send credentials, secrets, private user data, production dumps, private backups, payment data or similarly sensitive context to the free pool.

`Big Pickle` is a stealth model. It can provide a cheap alternative attempt, but it must never be the final authority for security, cryptography, architecture locks, privacy, billing or irreversible migrations.

### Command Code

- `DeepSeek V4 Flash` — economical paid daily worker.
- `MiniMax M3` — economical alternative worker.
- `GPT-5.6 Luna` — senior coding/debugging/review worker, always `extra-high`.
- `GLM 5.2` — senior long-horizon architecture/reasoning/review worker.

The live provider catalog must be checked before invocation. An unavailable model causes lateral fallback; it does not authorize an unlisted model automatically.

## Tier model

### T0 — deterministic tools / no LLM

Cost target: zero tokens, zero model credits.

Use compilers, tests, linters, schema validators, SQL queries, benchmarks, repository search, AST tools and other deterministic evidence first whenever they can answer the question.

Examples:

- `go test`, build and race/static checks;
- Svelte/TypeScript checks;
- database migration verification;
- benchmark comparison;
- `git diff`/repository search;
- JSON/schema validation;
- provider contract fixtures.

A failing deterministic check should be supplied to the next tier as focused evidence rather than sending a broad repository dump.

### T1 — free worker

Default route:

1. OpenCode DeepSeek V4 Flash Free
2. OpenCode MiMo-V2.5 Free
3. OpenCode Big Pickle

Use for repository exploration, summaries, documentation maintenance, issue work, low-risk implementation, test generation and mechanical refactors where automated evidence can verify the result.

Skip T1 entirely for sensitive context.

### T2 — economic paid worker

Default general route:

1. Command Code DeepSeek V4 Flash (`high`)
2. Command Code MiniMax M3 (`high`)
3. Google Gemini 3.7 Flash (`high`)

This is the normal paid implementation tier. The exact order may be specialized by agent: Gemini 3.7 Flash is preferred earlier for visual/multimodal UI, UX and architecture work; DeepSeek/MiniMax are preferred earlier for ordinary code to preserve stronger/provider-specific quota.

### T3 — senior worker

Default route:

1. Command Code GPT-5.6 Luna (`extra-high`)
2. Command Code GLM 5.2 (`high`)
3. Google Gemini 3.7 Flash (`high`)

Use for difficult debugging, multi-file refactors, complex implementation, long-horizon work and failures that survived a serious T2 attempt.

T3 must remain uncommon. Passing evidence ends the task; do not send a successful T2 patch to Luna merely for reassurance.

### T4 — critical reasoning

Pool:

- Google Gemini 3.1 Pro (`high`)
- Command Code GPT-5.6 Luna (`extra-high`)
- Command Code GLM 5.2 (`high`)

Use for critical architecture, security design, cryptography boundaries, backup integrity, authentication, billing, privacy, irreversible migrations and other tasks where a wrong decision has high recovery cost.

The primary model is role-specific rather than globally fixed. For code-heavy critical work, Luna may author; for deep architecture/security analysis, Gemini Pro or GLM may author.

### T5 — independent cross-provider verification

T5 is not simply “a stronger model.” It requires two independent model passes from different providers when available.

Preferred pairs:

1. Google Gemini 3.1 Pro + Command Code GPT-5.6 Luna
2. Google Gemini 3.1 Pro + Command Code GLM 5.2

Typical workflow:

1. primary author produces design/patch with evidence;
2. independent reviewer receives the smallest sufficient context plus acceptance criteria/evidence;
3. deterministic gates run;
4. material disagreement escalates to human review instead of endless model voting.

T5 is mandatory when the active Goal/security policy marks a change as critical.

## Evidence-driven escalation

Do **not** escalate because an LLM says “I am unsure.” Escalation requires one or more of:

- compile/build failure;
- unit/integration/E2E failure;
- contract/schema validation failure;
- benchmark regression outside the accepted budget;
- architecture invariant violation;
- security gate failure;
- two bounded patches failing the same acceptance criterion;
- task classification explicitly requiring a higher tier.

Before escalation, add only the missing evidence/context. Do not resend the entire repository by default.

## Role defaults

| Agent | Default | Normal route | Critical/escalation |
|---|---|---|---|
| explorer | T1 | DS Free → MiMo Free → Big Pickle | DS paid / Gemini 3.7 for complex multimodal exploration |
| architect | T2 | Gemini 3.7 → GLM 5.2 → Luna | Gemini 3.1 Pro + Luna (T5) |
| implementer | T1 | DS Free → MiMo Free → DS paid → MiniMax | Luna → GLM |
| frontend-engineer | T1 | MiMo Free → DS Free → MiniMax → DS paid | Gemini 3.7 for visual work; Luna for hard implementation |
| backend-engineer | T1 | DS Free → MiMo Free → DS paid → MiniMax | Luna → GLM |
| database-engineer | T1 | DS Free → DS paid | Luna/GLM; T5 for destructive/critical migration |
| networking-engineer | T2 | DS paid → MiniMax → Gemini 3.7 | Luna |
| ux-architect | T2 | Gemini 3.7 → MiMo Free (text-only) → MiniMax | Gemini 3.1 Pro |
| design-system-engineer | T2 | Gemini 3.7 → MiMo Free → MiniMax | Luna |
| accessibility-reviewer | T1 | DS Free for source checks | Gemini 3.7 for visual audit; DS paid fallback |
| security-architect | T4 | Gemini 3.1 Pro → GLM → Luna | independent Luna/Google T5 review |
| security-reviewer | T4 | Luna → Gemini 3.1 Pro → GLM | T5; must preserve independence |
| tester | T0 | tools → DS Free → MiMo Free → Big Pickle | DS paid; Luna only for hard investigation |
| quality-reviewer | T1 | DS Free → MiMo Free → Gemini 3.7 | Luna |
| reviewer | T1 | DS Free → Gemini 3.7 → DS paid | Luna/GLM; T5 if critical |
| debugger | T2 | DS paid → MiniMax | Luna → GLM → Gemini 3.7 |
| documentation-maintainer | T1 | MiMo Free → DS Free → Big Pickle | Gemini 3.7 |
| release-verifier | T0 | deterministic gates → DS Free/MiMo | Luna for anomalies; T5 for critical release |
| issue-author | T1 | MiMo Free → DS Free → Big Pickle | normally no escalation |
| issue-triager | T1 | DS Free → MiMo Free → Big Pickle | normally no escalation |

## Cost controls

1. **Pointer over payload:** provide document/file references and focused excerpts instead of broad context.
2. **Reuse evidence:** do not ask multiple models to rediscover the same compiler/test output.
3. **One serious attempt per model by default:** retries need new evidence or a narrow repair target.
4. **Stop on pass:** successful tests/gates close low/medium-risk tasks without an expensive ceremonial review.
5. **Specialize quotas:** reserve Gemini for tasks where multimodality/architecture/review adds value instead of using it for every mechanical edit.
6. **No unbounded recursion:** fallback depth remains bounded by `fallbacks.json` and the active context budget.
7. **Track observed usage:** Project Intelligence should record input/output tokens, provider/model, tier, retries, evidence outcome and observed/estimated monetary cost.

## Provider-failure behavior

- Model unavailable/rate-limited: move laterally to the next approved model in the same tier when possible.
- Entire provider unavailable: use an approved alternate provider at the same tier before escalating quality.
- Free pool unavailable: enter T2 only if the task still needs an LLM.
- Sensitive task: skip free OpenCode routes.
- T5 provider unavailable: do not silently replace cross-provider review with two models from one provider for security-critical work; require human waiver/review if independence cannot be achieved.

## Updating the policy

Model catalogs are volatile. Adding/removing a model or changing its effort/tier requires explicit edits to the orchestration manifests and a documentation delta. Live availability may be detected at runtime, but runtime discovery does not grant a newly discovered model permission to execute.

The Project Atlas workforce (agents/skills/recipes) remains version-pinned independently of this model policy. **Agent identity and model identity are separate concerns.**
