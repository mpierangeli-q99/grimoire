---
name: llm-agents
description: Use when designing or changing LLM agents, tools, prompts, schemas, model routing or agent output.
---

# LLM agents & prompts

## Layering
- **The agent loop stays agnostic.** No feature code in the loop; a tool that needs something carried adds a generic field to its result.
- **Agent memory travels in tool results**, visible in history. Never put one agent's private state in shared structures (session, conversation).
- Tool output is observation, not a control channel. Step labels come from the model's own reason or the real argument.
- **Tool names by scope**: `<feature>_<object>`; labels are the user's words for the action, never a backend name.

## Let the model judge
- **No code gates on judgment.** Don't decide with counters ("≥ 3 tool calls"); put the criteria in the prompt. Code enforces only facts.
- **Agents see real names.** Give the full list of real names (tables, documents, tags) with descriptions plus a tool that opens one by exact name. No word-splitting regex search in between.
- **Thresholds are tool parameters.** The user's value wins; if it changes the answer a lot, the agent asks; otherwise it picks from the data and says which. Statistical cuts validated on real data stay in the tool; the agent never lowers one to find something.
- **Agents doubt before explaining.** Tools return data-specific doubts (what could make the result wrong, which record would confirm it). The agent confirms with a second source and grades findings. Re-running the same call is not verification.

## Prompts
- Write instructions in positive form.
- Language: models follow the user's language already. It breaks when the engine sends its own English text as `role=user`; fix it there. Engine notices are English; generated content follows the configured language.
- Plain words by default; real table/column names and SQL only when the user asks technically. Internal tool names never.
- Closed schemas with named required-nullable fields; optional `x?` fields loop structured output.

## Sources and output
- Sources come from what tools returned, not from what the model cites. Citations resolve only against that list.
- Web answers without source links fall back to a search that has them.
- Exports draw charts or show tables; never raw JSON.
- Integrate declared values verbatim; show discrepancies as plain prose.

## Models and providers
- Exact model pins. Swaps are the human's call, with evidence.
- Direct providers (Anthropic, OpenAI, Gemini, Azure) must work fully; a proxy that can't carry a feature skips it.
