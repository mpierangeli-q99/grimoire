---
name: adversarial-review
description: Use before declaring non-trivial work done, or when asked to review a diff/PR — independent reviewers find issues, then adversarial verifiers try to refute each one.
---

# Adversarial review

The goal is findings that survive an attempt to kill them.

1. **Scope**: the uncommitted diff (`git diff`) or the PR. Read the rules in `code-style`, `testing-rules`, `frontend-ui`, `llm-agents` that apply.
2. **Fan out independent reviewers in parallel**, one per dimension (correctness/bugs, house rules, simplification, security if relevant). Give them the **code, not your conclusion**. A reviewer handed your hypothesis returns it confirmed.
3. **Verify each finding with the `adversarial-verifier` agent**, whose job is to *disprove* it with a concrete trace. Keep only CONFIRMED (and clearly-labelled PLAUSIBLE) findings.
4. **Report** findings ranked by severity, each with a concrete failure scenario (input/state → wrong output). No style nits dressed as bugs.
5. **Don't apply fixes** until the human picks which ones (see `working-agreement`).

For UI changes, add a reviewer that drives the real app with Playwright at 1366×768 and 1536×864.

Built-in helpers that pair well: `/code-review <level>`, `/simplify`, `/security-review`.
