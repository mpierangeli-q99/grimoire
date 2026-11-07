---
name: security-review
description: Use when touching auth, permissions, agent tools that run model-written code or SQL, file/web fetching, uploads, errors or dependencies — and for a security audit of a service.
---

# Security review

## Principles
- **The executor blocks, not our check.** Anything that runs model-written code or queries must be refused by the engine that runs it (read-only DB transaction, external file access disabled and locked, a restricted parser instead of eval). Our validator is a second layer. Prove it: a test that bypasses our check still gets refused.
- Prefer designs where the model has no language at all (structured operations) when the executor can't enforce limits.
- **Permissions live on the server** (identity service / its signed claims). The UI only shows or hides.
- **Never bypass source permissions.** Synced files keep the access list the source gives them; sharing and reuse apply only to our own content. Deleted at the source = deleted for every live path.
- **Internal errors stay generic** to the user (message + reference); the detail goes to logs only. Never raw exceptions in HTTP responses.
- Agents never download files from the web; files reach them only when the user attaches them.

## Audit method
1. Read-only agents in parallel, one per area (auth, agent SQL/code, fetching/SSRF, uploads/parsers, secrets/config, deps/licences). Code, not conclusions.
2. Confirm every claim yourself: run the exploit locally (as a low-permission test user, see `test-accounts`) in a rolled-back transaction or scratch environment.
3. Ranked list in an artifact: severity, evidence (file:line), concrete exploit, fix options, my recommendation.
4. Fix one item at a time with the human's OK; each fix gets a test.
5. Problems in a service you don't own go to its owner as a written spec (evidence, fix code, tests, rollout), not a silent edit.

## Checklist
- SQL validators: scope-aware CTE names, set operations, locks/INTO, function allowlist; read-only driver mode.
- Redirects: a fetch guard must check every hop (HTTP 302, meta refresh, scripts) against internal addresses.
- Uploads: request size cap on every route; zip/OOXML declared size; type by content (libmagic), not extension.
- Office/PDF converters: block external links; escape all text in generated PDFs.
- Secrets: no hardcoded signing keys, `DEBUG`, `ALLOWED_HOSTS=*`; error tracking must not capture frame locals with passwords.
- Containers run as non-root.
- Rate limits: keep per-IP, add per-user next to it.
- Dependencies: no GPL/AGPL; check advisories (`pip-audit`).
