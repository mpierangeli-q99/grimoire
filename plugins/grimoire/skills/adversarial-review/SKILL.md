---
name: adversarial-review
description: Use before declaring any non-trivial change round done, and when asked to review someone else's PR or branch — independent reviewers, a verifier that tries to refute each finding, a second-model pass, and for a PR the branch run on a copy of real data.
---

# Adversarial review

Findings must survive an attempt to kill them. Run this at the end of every change round, before saying "done".

1. **Scope**: your files in the uncommitted diff (other sessions' hunks are not yours). For someone else's PR, the whole PR: see the last section.
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

## Someone else's PR

Reading the diff is not a review: the bugs that matter show up when the branch runs. Do these three first, then steps 2–6 on the PR's diff.

1. **Run the branch on a copy of real data.** Export it to a scratch directory, never over pending work. Copy the local database into a scratch one, apply the PR's migrations and run its tests there. Then run the same real cases on main and on the branch and diff the results: whatever the PR says it doesn't touch must come out identical.
2. **Every claim is a probe.** Take the PR description sentence by sentence ("approved items can't change", "old records come out the same", "the cost is split by days") and write a test that tries to break each one, with the PR's own fixtures. A claim nobody tried to break is unverified.
3. **Every lock, every door.** For anything the PR freezes or forbids, list every route that can write it (create, update, partial update, delete, soft delete, bulk, copy) and try each one. Then change what it depends on (a price, an order, a parent row): a lock that compares inputs misses what changes the result.

Report findings confirmed by running apart from findings by reading, each with route/input → wrong result.
