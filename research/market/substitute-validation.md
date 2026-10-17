# MR-03: Test direct stack-builder substitutes

Issue: [#66](https://github.com/Pick-Your-AI/pick-your-ai-research/issues/66) · Workstream: MR-03 · Parent: [#50](https://github.com/Pick-Your-AI/pick-your-ai-research/issues/50) · Status: protocol pinned; ModelFit S1 run (CLI); Flocci S2 run in fallback and AI mode; Flocci S1 blocked by free-tier access; recommendation issued for review · Access date: 2026-10-16

Contributor: @itisSomeone (started with program-lead permission, 2026-10-16). The live GitHub assignee list remains authoritative; this note assigns no one. Live assignees at the 2026-10-17 readback (PR #125): @tynamite, @itisSomeone; coordinator @CompleteDotTech. Readiness: Needs input (a signed-in Flocci session), as set by PR #125.

## User decision and falsifiable question

Do Flocci and other guided builders already satisfy the proposed MVP?

The proposed MVP in [mvp-analysis.md](mvp-analysis.md) is a hybrid bounded workflow. It explains one local-versus-API inference decision for a narrow workload, with explicit constraints, uncertainty, dated costs and a shareable versioned decision record. This test is falsifiable. If a substitute satisfies the pinned scenario below, the MVP premise is weakened for that scenario, and #57/#58 must narrow, partner or pivot. A failure only establishes the specific observed gap, not general superiority.

## Scope and canonical related work

- Native downstream: #57 (MR-10) and #58 (MR-11) cannot be accepted before this issue reports.
- Information consumers: #2 (workload), #40 (contract/compatibility semantics) and #42 (build summary/share). These are handoffs for review, not ownership transfers.
- Starting evidence: [competitors.json](competitors.json), the [Flocci](competitors/flocci.md), [ModelFit](competitors/modelfit.md) and [llmfit](competitors/llmfit.md) profiles, and [browser observations](evidence/browser-observations.md). This note is not a canonical component catalog. Feature-matrix grade changes are proposed to the #49 owner, not edited here.
- Out of scope: paid credits or accounts, private data, deploying or running a generated stack, and any access-limit circumvention.

## Protocol v1 (pinned 2026-10-16, before S1/S2 runs)

The scenarios, graded properties and tool selection below were fixed before any S1/S2 result was seen. The pin is effective once this revision is visible on GitHub for review; runs start only after that. Deviation, recorded openly: the ModelFit S1 CLI run (2026-10-16, about 19:35 UTC) happened after Protocol v1 was committed but before this revision was reviewable on the upstream repository. An earlier protocol revision (fork commit `ffb47d6`, pushed about 19:10 UTC) already fixed the S1 hardware, use, priority, graded properties and ModelFit as second substitute. Later edits changed S1's workload wording and S2 only, and nothing was changed after the ModelFit result was seen. They must not be changed after a competitor result is observed. A later revision requires a new protocol version that keeps v1's results.

### Scenarios

- **S0 (recorded, historical).** Flocci Stack Builder: local PDF RAG workflow for Windows, 16 GB RAM, no GPU. See the initial result below. It is retained as context and is not regraded against P1–P5, because its prompt predates this protocol.
- **S1, common, public and non-sensitive (MVP decision).** Exact prompt text:

  > A 5-person team wants an assistant that summarises about 200 short public product reviews and forum posts per day. Hardware: one Windows laptop, 16 GB RAM, no discrete GPU. About 4,000 tokens per request, up to 3 concurrent users during working hours, budget at most USD 50 per month, open-weight models preferred, answer latency at most 10 seconds. Should we run locally or use a hosted API? List the components, explain why, state what is uncertain, and give a configuration we can save or share.

- **S2, contradictory constraints.** Exact prompt text:

  > A company wants an assistant that summarises public product reviews and forum posts. It must run fully offline on one Windows laptop with 16 GB RAM and no discrete GPU, serve 20 concurrent users, accept a 128,000-token context per request, answer in under 2 seconds, and cost nothing. Recommend a stack, explain your reasoning, and give a configuration we can save or share.

  S2 is self-contained, so it can run before S1 in a fresh session. Expected good behaviour is to identify that the constraints cannot all be met together, explain why, and propose which constraint to relax. Silently dropping a constraint is a failure of P1.

**S1-short (interface adaptation for Flocci).** Flocci Stack Builder accepts 2–400 characters ("Ask in 2–400 characters"), and S1 is 498 characters. The following 347-character version keeps every constraint C1–C7 and every requested output. Only wording is compressed ("200 short public product reviews and forum posts" becomes "~200 public reviews/forum posts", and "during working hours" is dropped):

  > Team of 5 summarises ~200 public reviews/forum posts a day. Hardware: Windows laptop, 16 GB RAM, no GPU. ~4,000 tokens per request, up to 3 concurrent users, max USD 50/month, open-weight models preferred, answer in max 10 s. Run locally or use a hosted API? List components, explain why, say what is uncertain, give a config we can save or share.

S2 (374 characters) fit unchanged.

Where a tool's interface cannot take free text, enter the closest structured equivalent and record every field value and unavoidable difference. A constraint the interface cannot express (for example budget or latency in a hardware-fit form) is graded `out of scope` for P1, listed by name, and never counted as respected.

### Graded properties

Each property is graded per tool and scenario as `supported`, `partial`, `failed`, `unknown` or `out of scope`, with a pointer to the captured evidence.

| ID | Property | Acceptance criterion served |
| --- | --- | --- |
| P1 | Every stated constraint is respected or explicitly flagged as unmet | Constraint handling |
| P2 | Reasons and uncertainty are explained, not only a list of names | Explanation quality |
| P3 | Local and hosted-API options are compared, with costs carrying a date, unit or source | MVP decision coverage |
| P4 | A saved, shared or exported artifact exists and retains inputs and assumptions | Saved/exported configuration claims |
| P5 | That artifact is reusable: reproducible, versioned or machine-readable rather than prose only | Saved/exported configuration claims |

P1 is graded against a fixed constraint checklist, so the grade does not depend on the analyst's overall impression. `supported` means every applicable constraint is respected or explicitly flagged. `partial` means at least one is silently ignored. `failed` means a recommendation violates a constraint without saying so.

| ID | S1 constraint | S2 constraint |
| --- | --- | --- |
| C1 | One Windows laptop, 16 GB RAM, no discrete GPU | Same hardware, fully offline |
| C2 | About 4,000 tokens per request | 128,000-token context per request |
| C3 | Up to 3 concurrent users | 20 concurrent users |
| C4 | Answer latency at most 10 s | Answer latency under 2 s |
| C5 | Budget at most USD 50 per month | Zero cost |
| C6 | Open-weight models preferred | (not stated) |
| C7 | Local-versus-hosted-API decision requested | (not stated) |

For S2, the expected behaviour is to flag that C1–C5 conflict, especially offline single-laptop hardware against C2–C4. Recommending a stack as if all were met is `failed`.

`unknown` covers an inaccessible feature, an exhausted free allowance or an unreachable page. An unreachable site is not a failed capability. `out of scope` covers a property outside a narrower tool's declared purpose. A narrower tool is not a failed full-stack builder. No numeric pass threshold is set here; the coordinator sets product thresholds per [mvp-analysis.md](mvp-analysis.md).

Mapping to contract outcome vocabulary, for #40 review: `supported` → `pass`, `failed` → `fail`, `unknown` → `unknown`, `out of scope` → `skipped`. `partial` has no contract equivalent; it is a research grade only and is reported with the specific constraint or property that was missed. No compatibility rule or severity is derived from these grades.

### Tools

| Tool | Role | Scenarios | Reason |
| --- | --- | --- | --- |
| Flocci Stack Builder (<https://aitools.flocci.in/stack>) | First substitute | S2, then S1 | Classified a direct competitor; one free run is already recorded (S0) |
| ModelFit (<https://modelfit.io/>) | Second substitute | S1 | Classified a direct competitor; [synthesis.md](synthesis.md) names Flocci and ModelFit as the direct entry-workflow substitutes; web device → setup → results flow allows P4/P5 inspection; its engine was already pinned in [technical-feasibility.md](technical-feasibility.md). Its documented step 1 device list (Mac models, iPhone, NVIDIA GPU, AMD) shows no CPU-only Windows laptop entry, so the closest available entry and the mismatch are recorded under C1 |
| llmfit (<https://www.llmfit.org/>) | Optional | S1, P4/P5 only | Partial competitor (CLI/TUI); documented JSON/REST output is relevant only to the export question. It auto-detects the host and documents only a GPU-memory override (`--memory`), not a RAM override, so a run reflects the tester's real machine, which must be recorded |

The issue named "ModelFit or llmfit". ModelFit is selected for the comparability reasons above. ModelFit has no hosted-API side, so P3 is graded `out of scope` for it, not `failed`.

### Capture rules per run

Record the date and UTC time, URL, access tier and remaining free allowance shown, browser, the exact prompt or field values, and the full visible output as text. Also record every save/share/export control and what it actually produced, plus anything inaccessible. Screenshots stay outside the repository; reference them through an [evidence manifest](../../evidence/README.md) if they are needed. Use no fresh profiles or other workaround when a free allowance is exhausted: record the run as `unknown (allowance)` and retry on a later day. Repeat a run when the allowance permits, because generated output can vary; report every repeat, including disagreements between repeats. Grades are assigned from the captured text only, never from memory of the session, so a reviewer can re-grade each run from the same capture.

## Candidate comparison

Cells are filled only from captured runs. ModelFit was run through its published CLI because the research environment could not reach modelfit.io. The CLI documents itself as a faithful port of the site's engine, but the web results page and its share controls remain unobserved.

| Tool · scenario | P1 | P2 | P3 | P4 | P5 | Run date | Evidence |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Flocci · S2, fallback mode (assistant unavailable) | failed | failed | out of scope | unknown | unknown | 2026-10-16 | [run log](evidence/mr03-flocci-runs-2026-10-16.md) |
| Flocci · S2, AI mode | failed | partial | out of scope | unknown | unknown | 2026-10-16 | [run log](evidence/mr03-flocci-runs-2026-10-16.md) |
| Flocci · S1-short | unknown (access) | unknown (access) | unknown (access) | unknown (access) | unknown (access) | 2026-10-16 | [attempt log](evidence/mr03-flocci-runs-2026-10-16.md) |
| ModelFit · S1 (CLI 1.5.3) | partial | partial | out of scope | partial | supported (CLI) | 2026-10-16 | [run log](evidence/mr03-modelfit-cli-2026-10-16.md) |
| llmfit · S1 (optional) | out of scope | out of scope | out of scope | pending | pending | | |

## Documented capabilities before the runs (desk evidence)

Collected 2026-10-16 from public pages before any S1/S2 run, to set expectations for P3–P5. These are documented or absent claims, not observations. They do not fill grade cells. Missing documentation is not proof that a feature is absent.

| Tool | Inputs documented | Hosted API / cost coverage | Save / share / export documented | Implication for the runs |
| --- | --- | --- | --- | --- |
| Flocci Stack Builder | "Describe what you want to accomplish" (free text; UI is client-rendered) | Not documented; free tools only | None found in the static page | P4/P5 must be checked hands-on; free allowance undocumented |
| ModelFit | Device step (Mac models, iPhone, NVIDIA GPU, AMD), then setup, then results; search accepts "16GB" | Documented as 100% local, "no API keys"; affiliate and cloud-GPU commissions disclosed | No per-result share or permalink found. The dataset is offered as JSON (`/api/dataset/`, CC BY 4.0, updated 2026-10-13), and a CLI (`npx @wecko-ai/modelfit`) prints an Ollama command | P3 out of scope by design; P5 may be met only at dataset/CLI level, not for a user's result |
| llmfit | Auto-detected hardware; `--max-context`, `--use-case`, GPU-only `--memory` override | Local runtimes only (Ollama, llama.cpp, MLX, Docker Model Runner, LM Studio) | `recommend --json`, `fit --json`, REST `serve` endpoints, web dashboard | Strongest documented machine-readable output, but narrow scope |

## Evidence

| Claim | URL / publisher | Access date | Version / region | Grade | Confidence | Limitation / rights |
| --- | --- | --- | --- | --- | --- | --- |
| Flocci Stack Builder takes a free-text goal; no save/share/export or allowance documented in the static page | <https://aitools.flocci.in/stack> · Flocci Technologies | 2026-10-16 | Page metadata generated 2026-10-16 | Primary documentation | Low (client-rendered) | Absence in static text is not absence in the app |
| ModelFit is local-only, has device → setup → results steps, and offers a dataset JSON API and CLI but no documented result permalink | <https://modelfit.io/> and <https://modelfit.io/data/> · ModelFit | 2026-10-16 | Dataset updated 2026-10-13 | Primary documentation | Medium | Steps 2–3 not visible in static text; dataset CC BY 4.0 with attribution |
| Flocci S2 in fallback mode returned four unranked tool cards with no constraint flags, including a paid trial for a zero-cost request | [run log](evidence/mr03-flocci-runs-2026-10-16.md) · Flocci Technologies | 2026-10-16 | Free tier, anonymous; page said "the assistant is momentarily unavailable" | Observed behaviour | High for the fallback output shown | Screenshot outside repo; AI builder not exercised |
| Flocci S2 in AI mode returned six explained offline components and accepted ~20 users on one CPU laptop without flagging the 128k-context / under-2-s conflict | [run log](evidence/mr03-flocci-runs-2026-10-16.md) · Flocci Technologies | 2026-10-16 | Free tier, anonymous | Observed behaviour | Medium (cards only; header and summary not captured) | Screenshot outside repo; infeasibility is analyst judgement |
| ModelFit engine (CLI 1.5.3) recommends Qwen3.5 9B Q8 as "runs comfortably" for a 16 GB CPU-only Windows PC with no speed estimate; share link keeps only RAM | [run log](evidence/mr03-modelfit-cli-2026-10-16.md) · `@wecko-ai/modelfit` 1.5.3 | 2026-10-16 | npm 1.5.3, tarball SHA-256 3d89facb… | Observed behaviour (deterministic CLI) | High for this configuration | Web UI not observed; CC BY 4.0 data not redistributed |
| llmfit documents JSON output, REST endpoints and local-runtime-only coverage | <https://www.llmfit.org/> · llmfit | 2026-10-16 | No version stated | Primary documentation | Medium | Seed-site operator unconfirmed (see profile); not installed or run |
| S0: Flocci returned an ordered six-step local PDF RAG workflow with caveats | <https://aitools.flocci.in/stack> · Flocci Technologies | 2026-10-16 | Free tier, anonymous | Observed behaviour | Medium for the observed response | Generated advice only; no runtime, privacy or export verified |

## Findings, counterevidence and unknowns

### Flocci S2, fallback mode

Flocci showed "Showing smart keyword matches — the assistant is momentarily unavailable" and returned four unranked tool cards: Apertus Chat, Kagi Assistant (paid-subscription trial), Haystack and Piper TTS. None of the conflicting constraints was flagged. Two cards contradict the request (paid, and hosted rather than offline), and one is unrelated (text-to-speech). Per-card bookmark and compare icons exist, but no stack-level save or export was visible.

- P1 `failed`: constraints silently ignored, and a paid tool was offered for a zero-cost request.
- P2 `failed`: no reasoning or uncertainty.
- P3 `out of scope`: S2 requires offline operation and does not ask for a local-versus-API decision.
- P4/P5 `unknown`: the icons were not exercised.

This grades the fallback path only. The AI builder's S2 behaviour is still untested and needs a rerun when the assistant is available. **Counterexample signal:** a substitute's guidance can silently degrade to keyword matching, with no warning that constraints are no longer considered. Consistent constraint checking is therefore a candidate differentiator, but one fallback run is not a general reliability finding.

### Flocci S2, AI mode

The AI builder returned six tailored cards: llama.cpp, Ollama, Open WebUI, Phi, Gemma and Mistral. Each has a request-specific "WHY".

- **P1 `failed`.** C1 (offline, one CPU laptop) and C5 (zero cost) are respected explicitly. But C3 is treated as feasible ("giving your ~20 users a shared UI") with no warning. C2 (128k context) and C4 (under 2 s) are only hinted at through "realistic quality/latency compromise" and "if you can accept a smaller context and slower per-request latency". The tool never says the combined request cannot be met, or which constraint to relax.
- **P2 `partial`.** The per-component reasoning is coherent and specific, but there is no stack-level explanation, no numbers and no explicit uncertainty.
- **P3 `out of scope`.** S2 is offline-only.
- **P4/P5 `unknown`.** The prompt asked for a shareable configuration. None is visible in the captured area, and the page header, the summary and the card icons were not captured or exercised.

**Counterexample signal.** In AI mode Flocci produces a plausible, well-explained offline component list, comparable to what the MVP's explanation layer would offer. That makes "explained recommendations" a weak differentiator on its own. The gap observed is feasibility checking: an impossible combination of users, context and latency is softened into hints instead of being flagged. An explicit, per-constraint feasibility verdict is the candidate differentiator, pending S1 and owner review.

### ModelFit S1 (CLI 1.5.3, two identical runs)

Grades, from the [run log](evidence/mr03-modelfit-cli-2026-10-16.md):

- **P1: partial.** C1 could be entered through overrides, and C6 is implicitly met because only Ollama open models are recommended. C2, C3, C5 and C7 cannot be expressed, so they are out of scope. The S1 workload also had to be entered as "chat", since summarisation isn't an option. C4 (latency) is the reason for `partial`: the engine has no throughput basis for any x86 CPU (`estimatedTokensPerSec: null`), yet it labels the top pick "runs comfortably".
- **P2: partial.** It gives a fit level, a short reason and a standing "estimates, not measured benchmarks" note. But the top pick is simultaneously fit `Heavy` and `comfortable: true`, and the human-readable output says "runs comfortably" with no speed figure. `comfortable` is derived from the memory-fit verdict alone (`src/best.mjs`), so the wording overstates what was assessed.
- **P3: out of scope.** Local-only by design ("no API keys").
- **P4: partial.** `--json` keeps the engine inputs (device, chip, RAM, priority, use case) with the result. The shareable `reportUrl` keeps only `ram=16`.
- **P5: supported at CLI level.** The output is deterministic (byte-identical across two runs), machine-readable JSON from a pinned package version and hash. Web-UI share and export are unknown.

**Counterexample for the MVP:** ModelFit answers the hardware-fit half of S1 quickly and reproducibly, so a plain "which model fits 16 GB" feature would not differentiate. It does not address context, concurrency, latency, cost or the local-versus-API decision. It also presents a memory-fit verdict in comfort language without a CPU speed basis. A transparent CPU latency estimate is the gap the MVP would have to fill. Whether a 9B Q8 model meets 10-second summaries on a CPU-only laptop is an unmeasured hypothesis, not a finding.

### Initial result (S0)

A completed free Stack Builder test reportedly returned an ordered local PDF RAG workflow for Windows, 16 GB RAM and no GPU. It acknowledged memory/latency uncertainty and suggested reproducibility steps. The visible result named Docling, txtai, Chroma, Ollama, Gemma and Open WebUI. This verifies generated workflow advice, not that the combination runs, preserves privacy or produces an executable export. The six visible component steps named no evaluation tool, and txtai and Chroma were both suggested without a stated joint or alternative storage role.

No raw customer data, benchmark artifact or paid API output was collected.

Open unknowns:
- whether Flocci offers any save, share or export function in the rendered app;
- whether ModelFit's web results step offers a per-result share or export (the CLI's share link keeps only RAM);
- how many free Flocci requests are available per day;
- whether ModelFit (which has no free-text input) could express S2 at all.

Flocci's handling of contradictory constraints is no longer open: see F-S2-1 and F-S2-2 above.

## Method and reproducibility

This is a structured expert evaluation, not a user study. It shows what each tool does with a fixed task. It does not measure user preference, completion rates or demand; those belong to #63/#52. Following competitive-benchmarking practice ([MeasuringU](https://measuringu.com/competitive-benchmarking/), accessed 2026-10-16), every tool gets the same task at equal difficulty, with closed success criteria fixed in advance, and the protocol is versioned so it can be repeated later as the tools change.

Anonymous public browsing on free tiers, following Protocol v1. Any reader can repeat each run with the exact prompts above. Generated answers are nondeterministic, so a repeat may differ. Differences between repeats are recorded, not averaged away.

## Recommendation and assumptions

**Recommendation: narrow, not pivot**, for the local-versus-API decision scenario tested. This is a research recommendation for coordinator and owner review, not an accepted product decision. No product threshold has been set.

What the substitutes already cover, so the MVP should not compete on it:
- **Explained component lists.** Flocci's AI builder produced coherent, per-component reasoning in S2 (AI mode, captured in F-S2-2), consistent with the reported S0 answer.
- **Hardware-fit model picks.** ModelFit answered the 16 GB question instantly and reproducibly. llmfit documents the same capability.
- **Breadth.** Neither tool lacked candidate components.

Gaps observed, which the narrowed MVP should target:
1. **Explicit feasibility verdicts.** Flocci's AI builder softened an impossible combination (20 users, 128k context, under 2 s on one CPU laptop) into hints, without flagging it or naming which constraint to relax. Its fallback silently dropped constraint handling altogether. *Observed:* F-S2-1, F-S2-2.
2. **Latency on ordinary hardware.** ModelFit has no throughput basis for any x86 CPU, yet labels its pick "runs comfortably" from memory fit alone. *Observed:* ModelFit S1 run log and source.
3. **A reusable decision record.** ModelFit's share link keeps only RAM, and Flocci showed no configuration export in the captured runs. *Observed for ModelFit; Flocci partly unknown*, because the card icons were not exercised.
4. **Local-versus-API cost comparison.** No tested tool produced one. ModelFit is out of scope by design. *Unknown for Flocci*, because S1 was blocked.

The narrowed MVP these results support is a per-constraint feasibility check that states pass, fail or unknown, with explicit uncertainty. It would add dated latency and cost ranges for one local-versus-API decision, and a versioned, shareable record that keeps every input and assumption. This matches the hybrid bounded workflow in [mvp-analysis.md](mvp-analysis.md) and drops "guided recommendations" or "hardware fit" as standalone differentiators.

Assumptions and limits:
- Each Flocci result is a single, nondeterministic run, and the AI-mode capture lacks the page header and summary.
- ModelFit's web UI was not observed; the CLI documents itself as a port of the site's engine.
- The infeasibility judgements are analyst reasoning, not measurements.
- This is an expert evaluation, not evidence of user demand (#63/#52).

What would change the recommendation: if Flocci S1 is later run and Flocci compares local and API options with dated costs and gives a shareable configuration that keeps the inputs, gaps 3–4 weaken. The recommendation would then move toward **pivot or partner** for this scenario. A Flocci S1 rerun from the tester's normal connection is therefore the most valuable follow-up, but it is not required to review this result.

## Acceptance criteria and validation results

| Criterion | Artifact / evidence | State |
| --- | --- | --- |
| Run a common public non-sensitive scenario | S1 on Flocci and ModelFit | Done on ModelFit (CLI); Flocci S1 blocked by free-tier access, and S0 remains the recorded Flocci common scenario |
| Check constraint handling and explanation quality | P1/P2, S2 on Flocci | Flocci S2 graded (fallback and AI mode); ModelFit S1 graded |
| Inspect saved or exported configuration claims | P4/P5 on each tool | ModelFit done (CLI); Flocci: no export visible, card icons not exercised |
| Record counterexamples and pivot recommendation | Findings and recommendation sections | Done: narrow recommendation with counterexamples, pending review |
| Deliverable links every significant conclusion to evidence or an explicit hypothesis | Evidence table and run logs | Done: each gap cites a run log, or is marked unknown or analyst judgement |
| Relevant technical owners review handoffs; unresolved decisions are named | #2, #40, #42 owners | Not requested |
| Repository checks pass; reviewer accepts the evidence before Done | Validation commands in the PR | Checks run per revision; acceptance pending |

## Handoffs and required owner decisions

- Coordinator: confirm Protocol v1 (scenarios, properties, ModelFit as second substitute) and any product pass threshold.
- #2: whether S1 is a representative workload definition.
- #40: whether `unknown`/`out of scope` grading matches contract outcome semantics (`pass`, `fail`, `unknown`, `skipped`).
- #42: what the observed save/share artifacts imply for the build-summary and share requirements.
- #49 owner: proposed feature-matrix updates for Flocci and ModelFit (`export`, `context`, `api_pricing`), once results exist.
- MR-02 audit (PR #87, open): it downgrades Flocci `constraints` and `explanation` to `?` because S0 has no replayable capture, and asks #66 for one. S0 still has no capture here. F-S2-2 is a captured, hashed run that bears on constraint handling and explanation, for the #49 owner to weigh.

## Phase limit and next action

Next action: coordinator and owner review of the recommendation. Optional follow-up: rerun Flocci S1-short from the tester's normal connection, capturing the full page and the bookmark/compare icon behaviour; optionally observe the ModelFit web results page for share/export. Use `Refs #66` until every criterion and handoff is accepted.

## CanIRun.ai candidate (2026-10-16)

Add [CanIRun.ai](competitors/canirun-ai.md) to the device-first comparison queue alongside ModelFit and llmfit. Directory inspection is the only evidence so far. Apply the existing common scenario, contradictory constraints and export tests; record unsupported inputs as bounded observations. No test has run and the existing Flocci-first order and acceptance gates remain unchanged.

## 2026-10-17 tests (@tynamite)

Method: a browser session with no account, no personal data and nothing installed or deployed. The common scenario is unchanged: answer questions over local PDFs on a Windows laptop with 16 GB RAM and no discrete GPU. The contradictory-constraints scenario prepared for Flocci was: fully offline (no internet), must use a hosted API model, 16 GB RAM, no GPU, zero budget. Observations are what the pages showed at access, not verified product behaviour.

| Tool and page | Scenario | Observation | Evidence class | Confidence |
| --- | --- | --- | --- | --- |
| Flocci Ask (<https://aitools.flocci.in/ask>) and Stack Builder (<https://aitools.flocci.in/stack>) | Contradictory constraints | **Not run.** Both pages showed "That's today's free answers used" / "That's today's free stacks used. Sign in for more" before any input. The site states that the Ask assistant and Stack Builder are priced in Flocci credits, while browsing and comparing need no account. | Observed behaviour | High for the access gate; nothing about constraint handling |
| ModelFit (<https://modelfit.io/>, device list <https://modelfit.io/devices/>) | Common | **Scenario not expressible.** Device choices are MacBook Air/Pro, Mac Mini, Mac Studio, iPhone, NVIDIA GPU and AMD (Strix Halo). The device list has no CPU-only, Windows or Intel integrated entry. Output is model fit only (one "best local model" plus an Ollama command); no retrieval stack. Export: open dataset (CC BY 4.0), JSON API, MCP endpoint and CLI, all for model and device data rather than a saved configuration. | Observed behaviour | High |
| CanIRun.ai (<https://www.canirun.ai/>, device page <https://www.canirun.ai/device/iris-xe>) | Common, via the "Intel integrated · Iris Xe" preset (16 GB RAM field) | **Partial.** Grades chat and coding models that fit (for example Qwen 3 8B, grade B, 4.6 GB, about 10 tokens/s; tiny models grade A/B) and reports no image or video models fit. It doesn't cover document Q&A, retrieval, embeddings or a UI, so no stack. The only save/share-like control is "compare"; the device page URL is shareable. The Iris Xe page's picks section was headed "Top open models for Apple M1", a label mismatch at access. The site says its estimates rely on browser APIs. | Observed behaviour; speeds are the tool's estimates | Medium: preset values were defaults, not a specific laptop |

### Interim finding

- **Device-first tools don't substitute for the MVP in this scenario.** ModelFit can't express a CPU-only Windows machine. CanIRun.ai answers "which model fits" but not the rest of the stack, constraint conflicts or a reusable configuration. Both overlap only with the model-fit slice (#2 hardware inputs, #10 sizing).
- **Flocci remains the decisive direct counterexample, and its constraint handling is still untested.** Its 2026-10-16 result already produced an ordered stack with uncertainty notes. If it also detects the offline-plus-hosted-API contradiction and offers a reusable export, the MVP premise is falsified for this scenario (#57, #58).
- **Interim recommendation:** keep the hybrid bounded-workflow MVP under validation. Don't differentiate on model fit, which device-first tools already cover. Test explicit constraint-conflict detection and a versioned, shareable configuration record against Flocci first.

### Needed to finish

A Flocci session with credits. Account creation was outside this run's scope, so the coordinator or an assignee signs in, and the same scenario is then run and logged here with the exact output.
