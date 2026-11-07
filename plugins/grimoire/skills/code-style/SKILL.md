---
name: code-style
description: Use when writing or reviewing any backend/application code — comments, abstractions, error handling, data integrity and API payload rules.
---

# Code style

## Shape
- **Minimal comments/docstrings.** One line stating the contract, or nothing if self-evident. No "why I wrote it this way" inside bodies.
- **No history comments.** Nothing that only makes sense as a diff ("previously…", "fixes the bug where…", "regression guard"). That belongs in the commit.
- **Don't over-engineer.** Logic used by one thing lives inside that thing. No module-level "library" APIs just in case; collapse overlapping constants.
- **No single-caller wrappers in shared modules.** Call the underlying function or declare it locally. Extract when a second caller appears.
- **Codebase convention over DRY.** Before adding a helper, grep for precedent. No precedent → inline repetitions beat a one-off abstraction.
- **No domain specifics in code.** Entity names (customers, sites, documents) live only in params objects or user prompts — never baked into schemas, prompts, or tests. Verify with fictional params.

## Errors
- **Narrow try scope.** Only the risky I/O inside `try`; construction and assignment above it.
- **Never put the raw exception in an HTTP response `detail`.** Static safe message + `raise ... from exc`. No extra warning log just to keep it.
- **Named status constants** (`status.HTTP_403_FORBIDDEN`), never bare numbers — when raising, comparing, or in sets.
- **Retries use the codebase's retry library** (e.g. tenacity `AsyncRetrying` + `TryAgain`, always a `stop`). Hand-roll only for side-effect-gated single retries. Gotcha: inject the sleep via the constructor in tests.
- **Don't hand-parse provider/SDK quirks** (regex over model output, AST walks). Fix upstream: SDK version, model, prompt, structured output, or accept the retry.

## Data integrity
- **null for unknown.** Uncomputable → `null`, never `0`, `""` or a plausible placeholder. Align null semantics across queries computing the same metric.
- **Never truncate text** at fetch/transform (`CAST(... VARCHAR(500))`, `text[:N]`). Clip at the presentation layer.
- **Filter forgivable input, reject only if nothing usable.** Strip, drop empties, dedupe order-preserving; error only when the cleaned result is empty.
- **Internal wiring ids stay out of payloads** (credential ids, external ERP codes) unless something actually reads them.
- **Archived ≠ gone.** Readers resolve archived entries; pickers/offers show active only.
- **Never loosen a matcher to recover data.** A wrong match is silent and poisons downstream; a miss is visible. Report misses.
- **Security-motivated restrictions stay** (e.g. a SQL validator blocking UNION). Fix upstream by changing the caller.
