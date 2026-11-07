---
name: capture-feedback
description: Use when the user corrects how you work, rejects an approach, or confirms a non-obvious one — turn it into a durable rule with its reason.
---

# Capture feedback as rules

Every rule in this plugin started as a correction. Saving it with its reason means it is never needed twice.

When the user corrects you ("don't do X", "why not Y?", "never…") or approves a non-obvious choice, save a memory:

```
---
name: <short-kebab-slug>
description: <one line, used to decide relevance>
metadata:
  type: feedback
---
<the rule, one or two sentences, with the user's words if short>

**Why:** what happened. The reason lets the rule be applied with judgement.
**How to apply:** concrete triggers and the exact alternative.
```

- One fact per file; a one-line pointer in `MEMORY.md`. Update an existing rule rather than duplicating; delete rules that turn out wrong.
- Date relative words ("today" → the date).
- Generic rules (style, process, testing) belong in this plugin; project facts (ports, containers, tables) in the repo's `CLAUDE.md` or project memory.
- Promote stable generic memories into this plugin by PR.
