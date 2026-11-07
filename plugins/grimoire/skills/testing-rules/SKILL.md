---
name: testing-rules
description: Use when writing, running, reviewing or deleting tests — what to assert, what not to test, when to run the suite.
---

# Testing rules

## What to assert
- **Contracts, not copy.** Never assert error messages, labels or prompt phrasing. Assert status codes, error codes, field names, computed values. Need something to assert on? Add a structured field.
- **No model names in tests** (`gpt-…`, `claude-…`). Read them from the route tables or use a placeholder; patch catalog lookups.
- No mock-wiring tests, no registry/config-mirroring tests, no real client config (build a synthetic one).
- **No tests for switches.** Small on/off changes get none; if one breaks an existing test, adjust it and stop. Real logic still gets tests.
- Single-use helpers are inlined, so tests drive the real entry point (the tool's `execute`, the endpoint).
- Security fixes get a test that bypasses our own check and still gets refused by the layer below.

## When to run
- While iterating: only the touched area's tests after each edit.
- Full suite once per round (before reporting done or committing).
- Not after string-only edits.
- Run tests where the app runs (inside the container), all external services mocked.
- Batch several changes into one test round; don't do a before/after live run per change.
