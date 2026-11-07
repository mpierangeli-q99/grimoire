---
name: release-flow
description: Use when committing, pushing or releasing across Q99 repos — order of pushes (shared library first, then pins), the lock quarantine, client plugins on branches, which repos go straight to main.
---

# Release flow across repos

## Order
1. **Shared library first** (q99-utils): commit, push, get its hash/tag.
2. **Then bump the pins** in every service that uses it (engine, user-manager, …): hash-only repin in `pyproject.toml` + `uv.lock`, verify with `uv lock --check` and the service's tests. A service never ships pointing at an unpushed library commit.
3. Then the services, in the order their changes depend on each other (backend before the frontend that calls it).

## Quarantine
- The locks keep `exclude-newer = "7 days"`: a version younger than 7 days is not installable. Don't pin it.
- Exemptions only per package, date-capped, for a real advisory, and removed once the window passes.
- Some versions are banned (known incidents or breakage); check the repo's notes and advisories before choosing one.

## Branches
- **Client plugins always go through a branch + PR**: `main` is protected, CI is required. A release bumps `clients/<slug>/pyproject.toml` and runs `uv lock` in the same commit; the merge's CI cuts the `<slug>/vX.Y.Z` tag, and the engine image installs the plugin by that tag. Branch from `origin/main` and unset the upstream so a bare `git push` can't aim at main. Check the change is safe with every engine version it may ship with.
- **Everything else can go straight to `main`**, if that is the team/developer's convention for the repo. Ask when unsure.
- `main` doesn't deploy; a release does.

## Commits
- Commit only when asked; the developer pushes.
- Group by area, each commit builds alone; one fix per commit when asked for a reviewable history.
- Don't touch a shared git index; never push for someone else.
