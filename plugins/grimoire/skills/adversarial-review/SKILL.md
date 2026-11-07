---
name: adversarial-review
description: Use before declaring any non-trivial change round done, or when asked to review a diff/PR — independent reviewers, a verifier that tries to refute each finding, and a second-model pass.
---

# Adversarial review

Findings must survive an attempt to kill them. Run this at the end of every change round, before saying "done".

1. **Scope**: your files in the uncommitted diff (other sessions' hunks are not yours).
2. **Self-check against the rules**: `code-style` (the cleanup guide), `testing-rules`, `frontend-ui`, `llm-agents`, `security-review`.
3. **Independent reviewers in parallel**, one per dimension (correctness, house rules, security, simplification). Give them the **code, not your conclusion** — a reviewer handed your hypothesis returns it confirmed. Read-only agents.
4. **Verify each finding** with the `adversarial-verifier` agent: it tries to disprove it with a concrete trace. Keep CONFIRMED; label PLAUSIBLE.
5. **Second model.** Send a bundle to a different-vendor model at high reasoning effort:
   - the diff of your files, the current version of the key files, a short brief of the intent and of what earlier rounds fixed;
   - ask for races, lost updates, permission holes, wrong edge cases — concrete failure scenarios only.
   Check each answer against the code. Fix the real ones with tests. Report which you accepted and which you rejected, and why. Run a second pass when the fixes were non-trivial.
6. **Report** ranked by severity, each with input/state → wrong result. No style nits dressed as bugs.
7. **Don't apply review fixes** the human hasn't picked, unless they are clear bugs in your own round.

For UI changes add a reviewer that drives the real app (see `verify-in-browser`).
Built-ins that pair well: `/code-review`, `/simplify`, `/security-review`.
