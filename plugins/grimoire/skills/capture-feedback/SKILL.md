---
name: capture-feedback
description: Use when the user corrects how you work, rejects an approach, or confirms a non-obvious one — turn it into a durable rule with its reason.
---

# Capture feedback as rules

This is how the rules in this plugin were made. Every correction becomes a rule that is applied automatically after that.

When the user corrects you ("don't do X", "why not Y?", "never…") or approves a non-obvious choice:

1. Save a memory (or propose a rule for this plugin) in this shape:

   ```
   <the rule, one or two sentences>

   **Why:** what happened, and the user's words if short. The reason lets the rule be applied with judgement, not mechanically.
   **How to apply:** concrete triggers and the exact alternative to use.
   ```
2. One fact per rule. Update an existing rule rather than duplicating it; delete rules that turn out wrong.
3. Separate **generic** rules (style, testing, process: they belong in this plugin) from **project facts** (they belong in the repo's CLAUDE.md).
4. Periodically promote stable generic memories into this plugin via PR.
