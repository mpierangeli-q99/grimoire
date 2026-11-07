---
name: testing-rules
description: Use when writing, reviewing or deleting tests — what is worth asserting and what isn't.
---

# Testing rules

Tests describe **contracts**, not copy or config.

- **Never assert product copy.** Error messages, labels, placeholder text, prompt phrasing. Assert status codes, `code` discriminators, `errors[].loc/type`, field names, block types, computed values. Importing the code's own constant is fine; a hard-coded literal is not. Missing a structured field to assert on? Add one.
- **No mock-wiring tests.** If a unit test proves a helper's contract, don't add a mock-heavy test proving it was called. Exception: something invisible in the diff (a kwarg actually reaching the SDK).
- **No registry-mirroring tests** (asserting exact model ids or config table contents). Test behaviour: every tier resolves, tiers stay distinct, validators reject bad combos.
- **No real external/client config in tests.** Build a synthetic definition and patch the loader, so the suite doesn't couple to another repo's release.
- **Don't rerun after string-only edits.** Run after logic, imports, signatures — once per batch.
- All external services mocked; run tests where the app actually runs (e.g. inside the container, never a stale host venv).
