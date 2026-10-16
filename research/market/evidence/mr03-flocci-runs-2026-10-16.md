# MR-03 run log: Flocci Stack Builder — 2026-10-16

Issue: [#66](https://github.com/Pick-Your-AI/pick-your-ai-research/issues/66) · Protocol v1 in [substitute-validation.md](../substitute-validation.md).

## Run F-S2-1: S2 (contradictory), degraded mode

- **When and how:** 2026-10-16, captured by about 20:05 UTC. Tester's own browser, anonymous free tier. Executed by @itisSomeone and transcribed from a screenshot. The screenshot is kept outside the repository, SHA-256 `fb2c7a9c0a64655352ddab4d36dc76b50867fbcf58b4559ef54cdfef60b8aafa`.
- **Input:** the exact S2 prompt from Protocol v1. The page heading echoes it as "An AI stack for: A company wants an assistant that summarises public product reviews and forum posts. It must run fully offline … and cost nothing. Recommend a sta". The heading is truncated in the page title only.
- **Mode banner, verbatim:** "Showing smart keyword matches — the assistant is momentarily unavailable." The subtitle reads "The closest tools across categories for your goal."
- **Visible output:** four numbered cards. More may have been below the captured area.

| # | Category label | Tool | Card text (verbatim) | Badge |
| --- | --- | --- | --- | --- |
| 1 | AI Chat & Assistants | Apertus Chat (Public AI) | "Chat directly on Apertus 1.5, the fully open Swiss model from ETH, EPFL and CSCS (released 2026-07-24)" | FREE |
| 2 | Research & Learning | Kagi Assistant | "Bundled with Kagi's paid ad-free search subscription" | TRIAL |
| 3 | AI Agents & Automation | Haystack | "Apache-2.0 framework for context-engineered, production RAG and agent pipelines" | FREE |
| 4 | Content & Media Generation | Piper TTS | "Fast, fully offline neural TTS for many languages and voices" | FREE |

- **Controls per card:** "Visit ↗", a bookmark icon and a compare (two-column) icon. The tester followed Haystack's "Visit ↗", which opened the vendor homepage <https://haystack.deepset.ai/> and carried no request or configuration. The bookmark and compare icons were not exercised. No stack-level save, share or export control was visible.

### Observations

- In this mode the builder did not flag any of the conflicting constraints: offline single laptop versus 20 users, 128k context and under 2 s, and zero cost.
- Two results conflict with the request itself. Kagi Assistant is tied to a paid subscription, against the zero-cost constraint. Apertus Chat is presented as a hosted chat, against the offline constraint. Piper TTS is a text-to-speech tool, unrelated to summarisation.
- No ordering rationale, reasoning or uncertainty was shown. This differs sharply from the AI-generated six-step workflow observed in S0.

### Validity

This run exercised Flocci's **fallback keyword matcher**, not its AI Stack Builder, by the page's own statement. It is valid evidence of what a user receives when the assistant is unavailable. It is **not** a test of the AI builder's constraint handling. S2 must be rerun when the assistant is available before the AI builder's P1/P2 are graded. The run also preceded public visibility of Protocol v1. The S2 text used was the locally pinned version, and nothing was changed after the result was seen.

## Run F-S2-2: S2 (contradictory), AI mode

- **When and how:** 2026-10-16, captured by about 20:09 UTC. Same tester, browser and free tier as F-S2-1. The tester confirmed the exact S2 prompt by pasting it with the result. Transcribed from a screenshot kept outside the repository, SHA-256 `f89510e20c5a02fff654ad152b93ad9b535fb4507002e4b42d97b8a9471bc3f6`.
- **Capture limits:** the screenshot shows six result cards only. The page header (and so whether the "assistant unavailable" banner was absent), any stack-level summary and anything below the cards are not in the capture. The tailored "WHY" text on every card indicates the AI path, not the keyword fallback; this is an inference.
- **Visible output:** six cards, each with a category, tags, a request-specific "WHY" and generic feature bullets. All are badged FREE.

| Tool | Category | "WHY" text (verbatim) |
| --- | --- | --- |
| llama.cpp | Local LLM Runners | "CPU-only inference engine with GGUF quantisation and an OpenAI-compatible llama-server, so the whole stack can run offline on one Windows laptop." |
| Ollama | Local LLM Runners | "Simplest way to pull and run a small quantised model locally on CPU; friendlier than raw llama.cpp for a non-specialist team." |
| Open WebUI | Local LLM Runners | "Self-hosted ChatGPT-style interface that talks to Ollama or any OpenAI-compatible endpoint, giving your ~20 users a shared UI with accounts and chat history." |
| Phi (Microsoft) | Notable Open Models | "MIT-licensed small model (Phi-4-mini class) that runs acceptably on 16 GB CPU RAM and is the realistic quality/latency compromise for offline summarisation." |
| Gemma (Google) | Notable Open Models | "Google's lightweight open Gemma models fit in modest RAM and are a good alternative if Phi's output style suits your reviews less well." |
| Mistral Open Models (Magistral, Devstral, Voxtral) | Notable Open Models | "Mistral Small open weights are a step-up quality option if you can accept a smaller context and slower per-request latency." |

- **Controls per card:** "Visit ↗", bookmark and compare icons, not exercised in this capture. No configuration file, export or share control is visible in the captured area, although the prompt asked for "a configuration we can save or share".

### Observations against the S2 checklist

- C1 (offline, one 16 GB CPU laptop) is respected and stated explicitly ("the whole stack can run offline on one Windows laptop").
- C5 (zero cost) is respected: all picks are free and open.
- C3 (20 concurrent users) is accepted as feasible ("giving your ~20 users a shared UI"), with no warning that one CPU-only laptop cannot serve 20 concurrent requests at the stated context and latency.
- C2 (128k context) and C4 (under 2 s) are acknowledged only indirectly. "Realistic quality/latency compromise" (Phi) and "if you can accept a smaller context and slower per-request latency" (Mistral) imply relaxation, but neither names the 128k or 2 s targets as unachievable, nor says which constraint must change.
- The infeasibility of meeting C2–C4 together on C1 hardware is analyst judgement from general CPU-inference constraints, not a measurement in this study.

## Attempt F-S1-1: S1-short, blocked

- **When:** 2026-10-16, about 20:21 UTC, same tester and browser, after F-S2-1 and F-S2-2.
- **First try:** the full S1 text was rejected with "Ask in 2–400 characters", so the S1-short adaptation was prepared. No answer was produced.
- **Retry:** with S1-short, the tester reported "I can't access it anymore". The exact on-screen message was not captured. The tester had been browsing through a VPN. Believing the failure was a connection problem, they switched to a different VPN endpoint to troubleshoot, not to obtain extra allowance. It was still blocked and produced no output. Recorded for transparency. The F-S2 runs may also have been made through a VPN, so the apparent access region is not known. The run is recorded as `unknown (access)`, to retry on a later day from the tester's normal connection.
- **Observation:** on the anonymous free tier, the tester obtained two answers (one fallback, one AI) before losing access in the same session. This is a single observation, not a documented allowance.
