# Research context for #62: benchmark comparability and harness effects

Additional research gathered while preparing PR #83 (Pick-Your-AI/pick-your-ai-research, base `research/market-intelligence`). It is background information, not instructions and not evidence for acceptance. The note `research/market/benchmark-validation.md` on PR #83 remains the record. Compiled 2026-10-16 and 2026-10-17. Structured version: `research-data.json` (IDs below match it).

## 1. Repository facts at Harness Bench commit b8d89634c6f445fcc68ceb6744a59bef6cdf4863

All read directly from a read-only clone; nothing was executed. Confidence High for each.

| ID | Fact | Location in the Harness Bench repository | Relevance |
| --- | --- | --- | --- |
| F1 | 79 of 274 general tasks declare `semantic_judge` with `score_role: bounded_case_fallback`, `default_model: agent`, `max_fallback_weight` 0.06 to 0.25 (median 0.10). The code caps the weight at 0.25. | `tasks/*/task.yaml`, `src/harnessbench/grading/semantic_judge.py` | An LLM judge, by default the evaluated model, can set part of a task's score. This conflicts with "No LLM judge in scoring" on the website and the paper's judge-free claim. For one model across harnesses the judge is constant; across models it is not. |
| F2 | Per-task `timeout_sec`: 185 tasks at 7,200 s, 44 at 3,600, 17 at 2,400, 28 at 1,800. A runner `--timeout` replaces all of them. | `tasks/*/task.yaml`, `tools/run_benchmark.py` | The time budget is per task, not one deadline. |
| F3 | Runner defaults: temperature 1.0, `--thinking high`, `--max-tokens 32768`, `--context-window 200000`, `--max-agent-turns 150`, `--workers 4`. | `tools/run_benchmark.py` | Tooling defaults. Paper Appendix B.1 states published-run settings (temperature 1.0, model-specific maximum output, 200K context, 150 turns), which differ on max tokens. |
| F4 | Provenance of the 274 general tasks: 212 public workflow adaptations, 42 handcrafted synthetic, 20 adapted synthetic. Multimodal sources are not in this file. | `catalog/task_catalog.csv` | Most tasks adapt publicly documented workflows; fixtures are described as synthetic. |
| F5 | Every general task package includes `tests/` with `cases.yaml`, `ground_truth/`, `reference/` and `test_outcome.py`. | `tasks/<id>/tests/` | Graders and reference answers are publicly readable, a contamination risk for models trained after publication. The exposure start date is unknown. |
| F6 | Lock `standard274.release-v1`: `task_ids_sha256` 8cb63ccd4df67b2c9c829b6e598296635013f1a31c5c9ff05e768508b9fed4e7; `task_set_manifest_sha256` 3a667df15143fa223659f05c137eff6df0d2e1cbbbf571390d6425eaece2b956. | `catalog/standard274.release-v1.lock.json` | Identifies the task set independently of commit links. |
| F7 | Public history is one commit, dated 2026-10-09. Locks for 237- and 254-task sets exist, but their task contents are not in public history. | `git log`, `catalog/` | The 106-task v1 contents cannot be hashed from public sources, so v1-to-v2 overlap is unknown. |
| F8 | A task keeps its ID when its content is redesigned. | `docs/STANDARD274.md` | Matching task IDs does not prove matching content. |
| F9 | 20,454 runs = 274 × 7 × 10 + 26 × 7 × 7. | Paper §4.1, website | Exactly one run per configuration (k = 1). |
| F10 | Pinned harness images: Claude Code 2.1.220, Codex 0.145.0, Nanobot 0.3.0, OpenCode 1.18.8, Goose 1.44.0, OpenClaw 2026.5.26-slim, Hermes v2026.7.20. | `docker/` | Harness versions are pinnable from the repository; whether these match the published runs is not stated. |
| F11 | `docs/PUBLIC_RELEASE_POLICY.md` describes the full repository as the collaborators' copy of a private benchmark and keeps tests and ground truth out of public result bundles. | `docs/PUBLIC_RELEASE_POLICY.md` | Suggests public access to graders may not have been intended (inference). |
| F12 | Scoring: each task's cases sum to 1.00, pass threshold 0.80, core outcomes 0.80 with every core case required; timeouts stay in the denominator. | `docs/EVALUATION_AND_SCORING.md` | Partial credit can raise the mean without a task being fully delivered. |

## 2. What the general literature says about validating such comparisons

| Approach | What it can settle | Typical cost | Key finding and source |
| --- | --- | --- | --- |
| Desk comparability audit | Whether two published numbers are comparable at all | No compute | Measures nothing new; filters invalid comparisons |
| Paired statistics, power analysis | Whether a gap exceeds noise | Arithmetic on per-task results | Clustered SEs up to 3.05× naive; about 969 items to detect a 3-point gap at 80% power (Miller 2024, arXiv 2411.00640) |
| Repeated trials, pass^k | Run-to-run stability | 3 to 5 runs per pair | Single SWE-bench Verified runs shift 2.2 to 6.0 points; SD above 1.5 even at temperature 0 (Bjarnason et al. 2026, arXiv 2602.07150) |
| Metric separation, cost frontiers | Whether a mean hides incomplete delivery or cost | Free with per-task logs | $134.50 vs $2.51 for similar HumanEval accuracy (Kapoor et al. 2024, arXiv 2407.01502) |
| Fixed reference scaffold | Fair model ranking; hides harness effects | One scaffold, all models | Scaffold changes break comparability (Epoch AI SWE-bench Verified, 2026) |
| Controlled model × harness grid | Harness effects per model | Thousands of dollars | 6 agents × 16 models, ≥5 runs, 32,155 trials (Terminal-Bench 2.0, arXiv 2601.11868); about $40,000 for 21,730 rollouts (HAL, arXiv 2510.11977) |
| Sensitivity analysis | Fragility of close results | Under $10 per task per model | Formatting alone spread scores up to 76 points (FormatSpread, arXiv 2310.11324) |
| Contamination checks, fresh tasks | Whether results hold on unseen tasks | Low to high | New problems dropped scores up to 13% (GSM1k, arXiv 2405.00332); no method proves absence |
| Benchmark quality audit | Whether tasks and graders are valid | Manual review | 7 of 10 audited agent benchmarks had task-validity flaws (ABC, arXiv 2507.02825) |
| Subsampling | Cheap screening | About 100 items | Under 2% error on means (tinyBenchmarks, arXiv 2402.14992), but about 12% of pairwise rankings can flip (Madaan et al., arXiv 2406.10229) |

Across these sources, desk audits and statistics remove most invalid comparisons cheaply, while only repeated, controlled runs can establish a harness effect. Sources disagree on whether model or harness matters more: Terminal-Bench 2.0 finds the model usually dominates, while Harness Bench reports gaps up to 39.2 points.

## 3. Published claims and their current standing

All figures are Vendor claims (website, accessed 2026-10-16; paper tables per the maintainer's 2026-10-17 recheck).

| Claim | Metric and source table | Standing |
| --- | --- | --- |
| Qwen3.8-27B harness gap of 39.2 points | General-task mean-score range, Table 2 | Fails G1; G3 unknown |
| GLM-5.3 variants, 13.5-point full-credit range | Table 9 | Fails G1; G3 unknown |
| Gaps of 5.0 to 9.5 points within nine models | Table 2 | Fails G1; G3 unknown |
| Per-model harness selection adds 2.53 to 2.73 held-out full-credit points | Table 10 | Fails G1; G3 unknown |
| GPT-5.6 Sol: Nanobot costs 2.97× OpenClaw at 24.82% vs 31.02% full credit | Website | Full-credit gap unestablished; cost ratio is a separate claim |
| Mean-score and full-credit harness orders differ | Pooled website profile | Hypothesis only; reversals are small |

## 4. Information that is still missing

| Gap | Holder of the information |
| --- | --- |
| Model snapshots, providers and run dates behind Tables 2, 9 and 10 | Harness Bench authors |
| Whether the published tables used the `semantic_judge` fallback (F1) | Harness Bench authors |
| Per-task results, needed for paired and clustered errors | Harness Bench authors |
| The v1 106-task list with content hashes (F7, F8) | Harness Bench authors |
| Training cutoffs of the evaluated backends | Model providers' cards |
| Reuse rights for tasks and results (no LICENSE; website reserves all rights) | Harness Bench authors; #41 and #60 |
| Whether a paid replication is wanted, its margins and its placement | #36; pricing #39 |

## 5. Replication design considered (not run)

The smallest informative design found: one model and two harnesses (GPT-5.6 Sol with OpenClaw versus Nanobot, because both rates and the cost ratio are published), at least three runs per task on the 274 general tasks, a fixed judge or deterministic-only scoring, per-task time budgets, and a paired, task-clustered analysis of mean score, full credit, pass^k and cost per fully delivered task. Its cost has not been priced.
