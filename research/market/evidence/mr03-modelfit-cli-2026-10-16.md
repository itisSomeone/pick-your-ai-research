# MR-03 run log: ModelFit S1 through the published CLI — 2026-10-16

Issue: [#66](https://github.com/Pick-Your-AI/pick-your-ai-research/issues/66) · Protocol v1 in [substitute-validation.md](../substitute-validation.md).

## Conditions

- **Why the CLI:** the research environment's network policy blocked `modelfit.io`, while the npm registry was reachable. The package README describes the CLI's ranking as "a faithful port of ModelFit's recommendation engine" and says the bundled data and engine "are kept in sync with modelfit.io — the site stays the source of truth". The web UI was **not** observed. The CLI result stands for the engine, not for the web results page or its share controls.
- **Package:** `@wecko-ai/modelfit` 1.5.3, MIT, npm `time.modified` 2026-10-14T03:40:12Z. Tarball SHA-256 `3d89facb7701d217ddffbc93fc60226415c69644ab912815625264f69d678be3` (npm shasum `73ea6a2666eaca102c3b6890aecbf57b31974475`). Bundled model data is ModelFit, CC BY 4.0. The tarball and data are not redistributed here.
- **Runtime:** Node.js v22.22.0 on Linux, CPU only, 16 GB RAM. The tester host is irrelevant to the result because all engine inputs were overridden (see `engineInput` below).
- **Network:** the default command path makes no network call. Only `modelfit bench --submit` posts data, and it was not run. The source was checked for `fetch`, HTTP and child-process calls before running.
- **Access date:** 2026-10-16, about 19:35 UTC.

## Mapping S1 to ModelFit inputs

| S1 constraint | ModelFit input | Note |
| --- | --- | --- |
| C1 Windows laptop, 16 GB, no GPU | `--device "Windows PC" --chip "Intel Core i7" --ram 16` | No laptop device type; "Intel Core i7" is a representative CPU name, not in the engine's chip table |
| C2 About 4,000 tokens per request | none | No context input |
| C3 Up to 3 concurrent users | none | No concurrency input |
| C4 Latency at most 10 s | none | No latency input |
| C5 At most USD 50/month | none | No budget input; local-only tool |
| C6 Open-weight preferred | implicit | Only Ollama-runnable models are recommended |
| C7 Local versus hosted API | none | Local-only by design |
| Workload: summarisation | `--use-case chat` | Options are coding, chat, translation and mixed; summarisation is absent, so chat is the closest |
| Priority | `--priority balanced` | Default |

## Commands and output

Command (run twice):

```bash
modelfit --json --no-color --ram 16 --device "Windows PC" --chip "Intel Core i7" --use-case chat --priority balanced
```

Both runs produced byte-identical JSON (SHA-256 `0c7f1d9dbcbe4c68656f8541c203710719bee4f685223608b2de6d0ce8b24ed7`). Extract, copied from the output:

```text
best: Qwen3.5 9B Instruct (Q8) · quantization Q8_0 · fit "Heavy" · localVerdict "local_feasible"
      comfortable: true · estimatedTokensPerSec: null
      why: "This model may feel memory-heavy on 16 GB RAM, but it is still listed for balanced speed and quality."
      ollamaCommand: "ollama run qwen3.5:9b-q8_0"
alternatives: Qwen3.5 9B Instruct (Q4_K_M, OK) · Qwen3 8B (Q4_K_M, OK) · Gemma 4 12B (Q4_K_M, OK)
hardware.engineInput: {deviceType "Windows PC", chip "Intel Core i7", ramGb 16, priority "Balanced", useCase "Chat"}
hardware.budgetSource: "system RAM (CPU inference)"
reportUrl: "https://modelfit.io/search/?ram=16"
note: "tokens/sec are estimates, not measured benchmarks."
```

Human-readable output for the same inputs, copied verbatim:

```text
Target    Windows PC · Intel Core i7 · 16 GB (override)
  ▶ Qwen3.5 9B Instruct (Q8)   runs comfortably
    $ ollama run qwen3.5:9b-q8_0
    The most capable model your machine can run. → https://modelfit.io/models/qwen/
Also fits  Qwen3.5 9B Instruct · Qwen3 8B   (modelfit --all for the full list)
Estimates, not measured benchmarks · Data: ModelFit (CC BY 4.0)
```

## Source observations

- `src/engine.mjs` keeps throughput baselines only for Apple M- and A-series chips (`CHIP_BASE_TPS`). An unknown chip returns `null` "not a made-up number". So no x86 CPU, including a CPU-only Windows laptop, gets a speed estimate.
- `src/best.mjs` sets `comfortable` from `localVerdict === 'local_feasible'` alone. "Runs comfortably" therefore reflects a memory-fit verdict, not throughput or latency.
- `reportUrl` encodes only `ram=16`. Device, chip, use case and priority are not in the shareable link.

## Limits

This is one engine configuration through the CLI, run deterministically. It is not a web-UI observation, a runtime measurement or a test of whether the model meets S1's latency. Any statement about actual CPU speed is a hypothesis unless measured.
