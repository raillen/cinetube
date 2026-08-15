# LLM Routing & Fallback Policy — CineTube

> Canonical operational explanation for `.ai/orchestration/model-policy.json`, `model-catalog.json`, `model-routing.json` and `fallbacks.json`.

## Objective

Minimize token consumption and marginal monetary cost without weakening correctness, security or release evidence. Model strength is not used as a status symbol: the orchestrator starts with deterministic evidence or the cheapest capable worker and escalates only when measurable evidence or task risk justifies it.

Model routing is operational infrastructure. It must not leak provider-specific assumptions into the CineTube domain architecture.

## Fixed effort rules

These rules are project-owner decisions and are not dynamically reduced to save money:

- **GPT-5.6 Luna:** always `extra-high` reasoning effort.
- **All other paid models, including Muse Spark 1.2 Contributor:** always `high` reasoning effort when the provider exposes an effort control.
- **OpenCode free models:** always the maximum reasoning effort supported by that model/provider.
- The orchestrator may choose a cheaper model or a lower tier, but it must not lower the configured effort of a selected model.

Economy comes primarily from avoiding unnecessary calls, restricting context, choosing cheaper tiers and stopping when evidence passes.

## Approved model pool

### Google

- `Gemini 3.7 Flash` — efficient agentic/coding and multimodal work; default Google workhorse.
- `Gemini 3.1 Pro` — deep/critical reasoning and cross-provider verification.

The orchestrator must verify the live provider identifier before invocation because provider catalogs can update faster than checked-in documentation.

### OpenCode — free pool

- `DeepSeek V4 Flash Free`
- `MiMo-V2.5 Free`
- `Big Pickle`

These are T1 workers only. Availability and free status are runtime concerns and may change.

**Security restriction:** while the free-period data policy permits use of activity for model improvement, these models may receive only public or non-sensitive project context. Never send credentials, secrets, private user data, production dumps, private backups, payment data or similarly sensitive context to this pool.

`Big Pickle` is a stealth model. It can provide a cheap alternative attempt, but it must never be the final authority for security, cryptography, architecture locks, privacy, billing or irreversible migrations.

### Meta — Muse Code Contributor

- `Muse Spark 1.2 Contributor` — Muse Spark 1.2 accessed through the discounted Contributor tier of Muse Code.

`Contributor` is an **access/data-policy tier**, not a different base model. It is deliberately positioned early in T2 and, for difficult non-sensitive coding, early in T3 because Muse Spark 1.2 is designed for agentic software engineering, long-horizon implementation, debugging, tool use and test/verification loops.

The discount has a material privacy trade-off: Contributor activity may be used to improve Meta products. Therefore this route is treated as **restricted-data**, just like the OpenCode free pool from a context-admission perspective.

Allowed examples:

- public/non-sensitive source code;
- unit/integration tests and fixtures without private data;
- ordinary implementation and refactoring;
- reproducible bugs stripped of sensitive material;
- public documentation and repository context.

Forbidden examples:

- credentials, tokens and secrets;
- private user data;
- payment or Mercado Pago data;
- real backup contents;
- production dumps/logs containing private information;
- proprietary sensitive material;
- security-critical context where disclosure would increase risk.

Muse Spark 1.2 Contributor is **not admitted to T4 or T5** and cannot be final authority for security, crypto, auth, billing, privacy or irreversible-data decisions. For sensitive work the orchestrator removes it from the route and continues with the lowest valid unrestricted model.

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

Examples include `go test`, builds/static checks, Svelte/TypeScript checks, migration verification, benchmark comparison, repository search, JSON/schema validation and provider contract fixtures.

A failing deterministic check should be supplied to the next tier as focused evidence rather than sending a broad repository dump.

### T1 — free worker

Default route:

1. OpenCode DeepSeek V4 Flash Free
2. OpenCode MiMo-V2.5 Free
3. OpenCode Big Pickle

Use for repository exploration, summaries, documentation maintenance, issue work, low-risk implementation, test generation and mechanical refactors where automated evidence can verify the result.

Skip T1 entirely for sensitive context.

### T2 — economic paid worker

Default general route for **non-sensitive coding**:

1. Meta Muse Spark 1.2 Contributor (`high`)
2. Command Code DeepSeek V4 Flash (`high`)
3. Command Code MiniMax M3 (`high`)
4. Google Gemini 3.7 Flash (`high`)

Muse Contributor comes first because the objective is marginal-cost efficiency and it is purpose-built for agentic coding. This ordering is conditional on the context passing the restricted-data check.

For sensitive context, remove Muse Contributor and use:

1. Command Code DeepSeek V4 Flash
2. Command Code MiniMax M3
3. Google Gemini 3.7 Flash

Agent specialization can reorder T2. Gemini 3.7 Flash remains preferred earlier for visual/multimodal UI, UX and architecture work.

### T3 — senior worker

Default non-sensitive coding route:

1. Meta Muse Spark 1.2 Contributor (`high`)
2. Command Code GPT-5.6 Luna (`extra-high`)
3. Command Code GLM 5.2 (`high`)
4. Google Gemini 3.7 Flash (`high`)

The reason Muse remains first is economic: a difficult long-horizon coding task should get one serious Contributor attempt before consuming Luna/GLM credits when the context is safe to share and deterministic gates can judge the result.

For sensitive T3 work, Muse is removed and Luna becomes the first senior coding worker.

T3 must remain uncommon. Passing evidence ends the task; do not send a successful T2 patch to Luna merely for reassurance.

### T4 — critical reasoning

Pool:

- Google Gemini 3.1 Pro (`high`)
- Command Code GPT-5.6 Luna (`extra-high`)
- Command Code GLM 5.2 (`high`)

Use for critical architecture, security design, cryptography boundaries, backup integrity, authentication, billing, privacy, irreversible migrations and other tasks where a wrong decision has high recovery cost.

Muse Contributor and OpenCode free models are not T4 authorities.

### T5 — independent cross-provider verification

T5 is not simply “a stronger model.” It requires two independent model passes from different unrestricted providers when available.

Preferred pairs:

1. Google Gemini 3.1 Pro + Command Code GPT-5.6 Luna
2. Google Gemini 3.1 Pro + Command Code GLM 5.2

Typical workflow:

1. primary author produces design/patch with evidence;
2. independent reviewer receives the smallest sufficient context plus acceptance criteria/evidence;
3. deterministic gates run;
4. material disagreement escalates to human review instead of endless model voting.

Muse Contributor and the OpenCode free pool are excluded from T5 because independent critical verification must not depend on restricted-data routes.

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
| explorer | T1 | DS Free → MiMo Free → Big Pickle → Muse Contributor | DS paid / Gemini 3.7 for complex multimodal exploration |
| architect | T2 | Gemini 3.7 → Muse Contributor → GLM 5.2 → Luna | Gemini 3.1 Pro + Luna (T5) |
| implementer | T1 | DS Free → MiMo Free → Muse Contributor → DS paid → MiniMax | Luna → GLM |
| frontend-engineer | T1 | MiMo Free → DS Free → Muse Contributor → MiniMax → DS paid | Gemini 3.7 visual; Luna hard implementation |
| backend-engineer | T1 | DS Free → MiMo Free → Muse Contributor → DS paid → MiniMax | Luna → GLM |
| database-engineer | T1 | DS Free → Muse Contributor → DS paid | Luna/GLM; T5 critical migration |
| networking-engineer | T2 | Muse Contributor → DS paid → MiniMax → Gemini 3.7 | Luna |
| ux-architect | T2 | Gemini 3.7 → MiMo Free (text-only) → MiniMax | Gemini 3.1 Pro |
| design-system-engineer | T2 | Gemini 3.7 → Muse Contributor → MiMo Free → MiniMax | Luna |
| accessibility-reviewer | T1 | DS Free source checks | Gemini 3.7 visual audit; DS paid fallback |
| security-architect | T4 | Gemini 3.1 Pro → GLM → Luna | independent Luna/Google T5 review |
| security-reviewer | T4 | Luna → Gemini 3.1 Pro → GLM | T5; preserve independence |
| tester | T0 | tools → DS Free → MiMo Free → Big Pickle → Muse Contributor | DS paid; Luna hard investigation |
| quality-reviewer | T1 | DS Free → MiMo Free → Muse Contributor → Gemini 3.7 | Luna |
| reviewer | T1 | DS Free → Muse Contributor → Gemini 3.7 → DS paid | Luna/GLM; T5 if critical |
| debugger | T2 | Muse Contributor → DS paid → MiniMax | Luna → GLM → Gemini 3.7 |
| documentation-maintainer | T1 | MiMo Free → DS Free → Big Pickle | Gemini 3.7 |
| release-verifier | T0 | deterministic gates → DS Free/MiMo → Muse Contributor | Luna anomalies; T5 critical release |
| issue-author | T1 | MiMo Free → DS Free → Big Pickle | normally no escalation |
| issue-triager | T1 | DS Free → MiMo Free → Big Pickle | normally no escalation |

All routes containing Muse Contributor are automatically filtered out for sensitive context.

## Cost controls

1. **Pointer over payload:** provide document/file references and focused excerpts instead of broad context.
2. **Reuse evidence:** do not ask multiple models to rediscover the same compiler/test output.
3. **One serious attempt per model by default:** retries need new evidence or a narrow repair target.
4. **Stop on pass:** successful tests/gates close low/medium-risk tasks without an expensive ceremonial review.
5. **Exploit safe cheap routes:** use Muse Contributor early when context is non-sensitive and automated evidence can verify the patch.
6. **Specialize quotas:** reserve Gemini for multimodality/architecture/review and Luna/GLM for tasks that survive cheaper valid attempts or are inherently high risk.
7. **No unbounded recursion:** fallback depth remains bounded by `fallbacks.json` and the active context budget.
8. **Track observed usage:** Project Intelligence records tokens, cost, data-policy class, tier, retries, evidence and final outcome.

## Provider-failure behavior

- Model unavailable/rate-limited: move laterally to the next approved model in the same tier when possible.
- Entire provider unavailable: use an approved alternate provider at the same tier before escalating quality.
- Free pool unavailable: enter T2 only if the task still needs an LLM.
- Sensitive task: remove all restricted-data routes — OpenCode free and Muse Contributor — before selection.
- T5 provider unavailable: do not silently replace cross-provider review with two models from one provider for security-critical work; require human waiver/review if independence cannot be achieved.

## Updating the policy

Model catalogs and pricing are volatile. Adding/removing a model, changing its effort/tier or changing a provider data-policy classification requires explicit edits to the orchestration manifests and a documentation delta. Runtime discovery does not grant a newly discovered model permission to execute.

Do not freeze provider prices in architecture. Cost telemetry should use observed/current provider rates so routing can later be tuned from real CineTube data.

The Project Atlas workforce (agents/skills/recipes) remains version-pinned independently of this model policy. **Agent identity and model identity are separate concerns.**
