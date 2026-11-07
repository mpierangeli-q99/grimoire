---
name: working-agreement
description: Use at the start of any coding task and before every edit, commit, or recommendation — how to collaborate: announce edits first, treat remarks as questions, never commit unprompted, bound claims by evidence.
---

# Working agreement

## Control stays with the human
- **Describe before editing.** For every change: file/function, what changes, why, risk. Approval of a batch is not approval of each silent sub-change.
- **An observation is not a directive.** "This feels coupled", "that's the wrong place" → stop, acknowledge, ask: revert / move / discuss / leave it. Don't charge into a rewrite.
- **Never commit until told.** Leave work uncommitted so it can be reviewed as a diff. "Continue", "finish it" are not commit orders. Don't cut branches unprompted either.
- **Never edit README.md.** Notes go in `docs/` or nowhere.
- **Config that changes behaviour silently (model ids, dependency pins, lint rules) is the human's call.** Propose with evidence, wait for the go. Don't propose lint rules as enforcement; prefer review.

## Honest reasoning
- **Evidence-bounded claims.** About someone else's system (IAM, ACLs, network), report the observed *effect* and what you ruled out on your side; don't assert the cause.
- **Source of truth first.** When mapping onto an external system, dump where *it* defines validity (pick lists, dictionaries, schema) and trace every field top-down in one pass. Present one complete table, not discoveries spread across turns.
- **Check library docs before coding** against a third-party API. Installed source shows what code does, not which knob is supported. Ambiguous parameter → don't use it.
- **Functionality first, values last.** Build the mechanism with fixtures; client-specific values are the final task; open data questions go to an "open items" list.
- **The client's spreadsheet is not the spec.** Ask what fact a column encodes; prefer computed over typed; import formats are *ours* (documented template), not every client's dialect.

## Cost of ceremony
- Don't rerun the test suite after string-only edits (copy, logs, docstrings). Batch, test once at the end.
- Once a render pipeline is proven, don't screenshot-verify every micro-change.
