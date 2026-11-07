# grimoire — pier's way of working with Claude Code

Rules and workflows I use daily with Claude Code. Each one came from correcting Claude and keeping the reason.

## Install

```
/plugin marketplace add <github-user>/grimoire
/plugin install grimoire@grimoire
```

## What's inside

| Skill / agent | When it kicks in |
|---|---|
| `working-agreement` | Every task: announce edits first, remarks ≠ orders, never commit unprompted, evidence-bounded claims |
| `code-style` | Writing/reviewing code: minimal comments, no over-engineering, narrow try, null for unknown, no truncation |
| `testing-rules` | Tests: assert contracts, never copy; no mock-wiring or config-mirroring tests |
| `frontend-ui` | UI: responsive at laptop sizes, explicit save, FE never transforms data, tables/dialogs idioms |
| `llm-agents` | Agents/prompts: layering, closed schemas, honest output, exact model pins |
| `adversarial-review` + `adversarial-verifier` agent | Before "done": parallel reviewers, then verifiers that try to refute each finding |
| `capture-feedback` | Turns your own corrections into rules (how this plugin grows) |

## Companion plugins I use

From the official marketplace: `superpowers` (brainstorming → plans → TDD → verification), `code-review`, `feature-dev`, `frontend-design`, `playwright`, `chrome-devtools-mcp`, `context7` (library docs).
Built-ins: `/code-review`, `/simplify`, `/security-review`, Artifacts for shareable reports.

## Project-specific rules

Rules tied to one codebase (auth headers, container commands, DB timeouts) belong in that repo's `CLAUDE.md`, not here.
