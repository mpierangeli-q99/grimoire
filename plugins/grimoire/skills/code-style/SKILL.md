---
name: code-style
description: Use when writing, cleaning or reviewing backend/application code — comments, helpers, libraries, errors, data integrity. Check every diff against it before reporting.
---

# Code style (the cleanup guide)

## Comments
- **Default to no comment.** Docstrings 1–3 lines: what it does plus any rule the code can't show. No Args/Returns blocks, no section banners, no comments restating the next line, no commented-out code.
- **No history or rationale in code**: no "used to", "legacy", "v1", "moved from", "instead of", incident stories or plan step numbers. Reasons go in the commit.
- Never touch a file only to update a comment; revert comment-only edits so the file leaves the diff.
- Keep: one-line security notes and non-obvious constraints, pydantic/enum docstrings, route docstrings, Field descriptions, prompts, tool declarations.

## Shape
- **No single-use helpers.** One caller → write it in place. Two or more → one shared copy.
- **Plain readable loops** over dense comprehensions with walrus. "More human" beats fewer lines.
- **Controllers hold only endpoints**, one function per endpoint; shared helpers go in services.
- **Dead code out** (unused functions, unreachable branches, duplicate imports). Exception: features that were designed and built but not used yet stay; ask before deleting them.
- **No OS tricks** (`/proc` writes, fd juggling). Use the plain stdlib/library idiom; measure the plain version first.
- **No special cases for one bad row** of one client. One general rule; let the data show as it is.
- Statuses as enums; HTTP codes as `status.HTTP_...` constants, never bare numbers (a variable named `status` shadows the import: use `code`).

## Libraries
- **Import X and use X.** Before writing a parser/splitter/heuristic, look for a maintained library; keep only the glue it can't do. Compare before/after on real data (identical, or each difference explained).
- Queries through the ORM / query builder, not hand-written SQL strings.
- **Licences:** permissive (MIT/BSD/Apache) plus LGPL/MPL used unmodified. Never GPL/AGPL. Check the package metadata of the library and its deps before proposing it.

## Errors
- Narrow `try`: only the risky I/O inside.
- Our own failures show the user one generic message (Retry / "Something went wrong" + a reference), never internals. Always log the traceback; a caught exception stored only in a state field counts as hidden.
- Retries with the codebase's library (always with a stop condition).
- Don't hand-parse provider quirks; fix upstream (SDK version, structured output).

## Data
- `null` for unknown, never `0`/`""`. Never truncate at fetch; clip at presentation.
- **The backend owns the numbers.** A metric rule lives in one backend function that the dashboard, agents and reports all call. Don't round in the backend.
- **Requests about one entity query one entity.** Only list endpoints read everything.
- **Limits mean "ask for more".** Every cap returns the first slice, says how much there is, and says how to get the next slice.
- Timeouts: one lowered default; override only where measurements show frequent failure.
- Services don't share data through Redis keys; cross-service signals go over HTTP.
- Never loosen a matcher to recover data. Security restrictions stay; change the caller.
