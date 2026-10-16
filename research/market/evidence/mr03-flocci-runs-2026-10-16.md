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
