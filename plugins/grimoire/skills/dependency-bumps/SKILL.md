---
name: dependency-bumps
description: Use when asked what can be bumped, when upgrading a library, during the periodic dependency/security sweep, or when a supply-chain advisory lands.
---

# Dependency bumps

A bump is a behaviour change until proven otherwise. Read what the version brings, try it in isolation, compare on real data, then apply.

## Scope
- "What can we bump?" in a thread about one package means that package. Offer a wider survey in one line; if you run it, say "dry run, nothing applied".

## Before choosing a version
1. **Read what's new**: changelog and release notes of every version between the pin and the target. List breaking changes, new defaults, removed params, new required deps.
2. **Supply chain**: keep the lock quarantine (7 days). Exemptions only per package and date-capped, removed once the window passes. Check advisories and incidents for the target and its upload date ("if not sure, the older version"). For critical packages, compare the wheel with the git tag and look for startup hooks (`.pth`).
3. **Licence** of the new version and any new transitive dep (no GPL/AGPL).
4. **Catalogs that change behaviour**: some libraries pull live data (e.g. a live model catalog). Check routes against the catalog *bundled* with the target version.

## Trial, in isolation
- Trial lock on a scratch copy; the diff should change only what you meant. Don't let a newer tool silently rewrite the lock format.
- Install into a target dir / scratch venv and run the tests there (`PYTHONPATH=<dir>`), never on the shared environment first.
- **Compare against the current version on real data**: identical outputs, or every difference explained (e.g. converting the same real files, capturing every outgoing LLM request byte-for-byte, rendering the same PDFs).
- **Every provider**: for LLM-layer libraries, smoke every provider we support, direct and through the proxy: streamed tool call, plain answer, non-stream tool call, structured output. A lock that resolves can still fail on the first request with tools.
- Full suite once; the app boots without network access.

## Apply
- Only with the human's go. Pin + lock in the same change; install in the running containers and restart; rebuild images.
- If the bump lets you delete a workaround, delete it and keep a test that fails on the old version (so nobody downgrades by accident).
- Report: what the version brings, what you checked (with numbers), advisories before → after (`uv audit` / `pip-audit`), what you left and why.

## Periodic sweep
- Run the audit on every service lock, dedupe aliases, triage by package: what's fixable now, what's blocked by another pin (say which), what belongs to someone else (base images → devops).
- Process one package at a time, one commit per fix.
