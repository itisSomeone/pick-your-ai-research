# MR-09 research package: benchmark comparability and harness effects

Issue: Pick-Your-AI/pick-your-ai-research #62 (parent #56). Pull request: #83, base `research/market-intelligence`. Prepared 2026-10-17.

This package is background research for the #62 owners. It is not evidence for acceptance. The authoritative record is `research/market/benchmark-validation.md` on PR #83.

## Contents

| File | Audience | What it holds |
| --- | --- | --- |
| `README.md` | People and agents | Summary, conclusions, how to use the package |
| `research-data.json` | Agents and scripts | Sources, approaches, repository facts, claim verdicts, gates and open questions, with IDs and cross-references |
| PR #83 note | Reviewers | The record itself: `research/market/benchmark-validation.md` on [PR #83](https://github.com/Pick-Your-AI/pick-your-ai-research/pull/83) (snapshot not duplicated here) |
| `approaches.md` | People | Full write-up of twelve general validation approaches with evidence |
| `RESEARCH_CONTEXT.md` | People and AI agents | Additional research findings: repository facts, literature, claim standing, missing information; suitable as a PR comment |

## Conclusions

1. **No published harness-effect claim survives yet.** Every claim fails G1, because no source links the published tables to model snapshots, harness pins and grader code. G3 is unknown for all of them, because per-task results are not public.
2. **The grader is not judge-free.** At commit `b8d8963`, 79 of 274 general tasks let an LLM judge (by default the evaluated model) set up to 25% of a task's score. This contradicts the paper and the website.
3. **Each configuration ran once.** 20,454 = 274 × 7 × 10 + 26 × 7 × 7, so run-to-run variance cannot be separated from task sampling.
4. **Contamination risk is real but unmeasured.** Every task package publishes its tests and ground truth. Training cutoffs and the v1-to-v2 task overlap are unknown.
5. **Mean score and full credit can rank harnesses differently.** The pooled published profile shows this, but the reversals are small and untested.

## Replication design considered

The smallest informative replication found uses one model and two harnesses, with k ≥ 3 runs per task and the judge fixed, reporting mean score, full credit, pass^k and cost per full-credit task. It would need #36 approval, #39 pricing and owner-set margins δ and δm. A full model × harness grid is mainly informative if harness rankings are wanted.

## Using the JSON

- `sources[].id` (S1 to S13) is cited by `approaches[].sources` and `harness_bench_facts[].source`.
- `harness_bench_facts[].confidence` is High only for facts read directly from the pinned repository.
- `published_claims[].grade` is "Vendor claim" for every performance figure. Do not treat these as measurements.
- Evidence grades follow the project's four grades: Primary documentation, Vendor claim, Independent measurement and Estimate.

## Limits

Nothing was run, and no paid service or bulk download was used. Website figures are dated transcriptions. The arXiv v2 full text was re-read by the maintainer on PR #83, not by this package's author.
