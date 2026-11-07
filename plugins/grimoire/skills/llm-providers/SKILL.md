---
name: llm-providers
description: Use when changing model routes, reasoning effort / thinking settings, temperature, model tiers, or anything in the LLM call layer — and when comparing models.
---

# LLM providers, models and effort

## Effort and thinking
- Effort is a per-task choice: pick it per route/task (extraction may need `none`/`low` for speed; hard analysis `high`). Don't add per-model ladders of hardcoded efforts.
- **Check what the provider actually supports**: not every model accepts every value (some reject `none`; adaptive thinking only on some families). Read it from the model catalog, not from a hardcoded list.
- **Inspect the real request**: the client library may translate `reasoning_effort` into something else (e.g. a thinking `display` mode that delays streamed text). Capture the outgoing request before concluding.
- Measure effort changes on real tasks (cost, latency, quality side by side), 1–2 runs per case, more cases. A higher effort is adopted only with the human's OK.
- Temperature: provider default unless measured otherwise.

## Routes
- Exact model pins, never `*-latest`. Swapping a model is the human's call, with evidence (quality side by side on real inputs, price, limits, retirement dates).
- Direct providers work fully first; a proxy that can't carry a feature skips it there.
- Every change keeps all providers working: smoke each one (stream + tool call + plain + structured output). Model names never in tests.
- Rate limits belong to the provider/credential, not to each feature. On 429, cut volume first; a shared pause per credential beats per-feature retries.
- Timeouts: one default bounded by measured traces.

## Comparing models or libraries
- Same real inputs, outputs side by side, differences listed and explained.
- Report cost per case and where the cost comes from (input vs output tokens) before proposing a cheaper model.
- Get a second opinion from another vendor's model for adversarial review (see `adversarial-review`).
