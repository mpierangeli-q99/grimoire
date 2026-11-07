---
name: llm-agents
description: Use when designing or changing LLM agents, tools, prompts, extraction schemas, model routing or agent-generated documents.
---

# LLM agents & prompts

## Layering
- **Agents sit below flows.** An agent emits a flat, honest event contract (tool_start/tool_result/token/done). Presentation (nesting, grouping, citation format) is the flow/streaming layer's job.
- **An agent interprets only concepts it is a stakeholder in** (e.g. sources it accumulates). Everything else in tool metadata is forwarded unread.
- **Neutral vocabulary.** Name agent params for what they *do* (`inline_citations: bool`), never what they're *for* (`mode="report"`).
- **Tool output is observation, not a control channel.** No hand-holding text appended to results.
- **Step labels come from the model's own one-line reason** or the actual argument (the query), verbatim, never regex-chopped.

## Schemas & prompts
- **Closed schemas over dynamic lists.** Ask for specific named fields (required-nullable). No attribute/value indirection or generated models. Too big for the grammar → split into more agents.
- **Briefs never steer to specific documents.** State *what* to extract; search finds *where*.
- **Language lives in one place** (config), never repeated in briefs.
- **Structured emission without thinking.**

## Output honesty
- **Integrate declared values verbatim** ("near 450 m", not "the target zone").
- **Show discrepancies in place, as plain prose:** "5 blades, another source says 6". No glyphs, filenames or JSON.
- **Render whatever exists.** Each sub-block renders on its own; the empty message only if nothing rendered. "Searched, found nothing" ≠ "pending".

## Models
- **Exact pins, never `*-latest` aliases.** They re-point with no diff.
- **Model swaps are the human's call.** They carry behaviour changes (structured-output gates, envelopes), not just cost and quality. Bring evidence, recommend, wait.
- Spreadsheets in retrieval: prefer grep over vector search.
