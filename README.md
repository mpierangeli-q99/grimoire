# grimoire — how we work with Claude Code at Q99

Martin's way of working with Claude Code, packed as a plugin so every Q99 developer gets the same rules. Each rule started as a correction he gave Claude and kept with its reason, so it is never needed twice.

## Install

```
/plugin marketplace add mpierangeli-q99/grimoire
/plugin install grimoire@grimoire
```

The skills load by themselves when the task matches. Project facts (ports, containers, tables) stay in each repo's `CLAUDE.md`.

## The way of working, in short

1. **One item at a time.** Big work becomes a numbered checklist. Claude proposes the next item, you approve, it does it, you check, it ticks the list. "continue" means go to the next one.
2. **You decide, Claude says what it decided.** UX trade-offs, model swaps, version pins and domain rules are yours. Every number or variant Claude picked on its own is listed as "decisions I made".
3. **Change only what was flagged.** A complaint about the texts changes the texts, not the visuals.
4. **Read the docs, try it apart, then merge.** Docs and changelogs before code; prototype in a scratchpad on real data, then bring it into the repo.
5. **Respect what exists.** Before building a filter, header, tile or nav, find the app's own and reuse it; new pieces become shared components in the same theme.
6. **Smallest version first.** Use the existing seam and maintained libraries ("import X and use X"). No single-use helpers, no history in comments.
7. **Measure before claiming.** Research runs on real data from the local test database, on all entities, outside the client's own data too, with before/after comparisons. 1–2 runs per case; more cases, not more repeats.
8. **Show it on a page.** Audits, plans and proposals become artifacts with real examples, many charts and 2–4 versions side by side.
9. **Verify in a real browser.** Logged in with a minted token, in light, dark and phone width. Nothing unrendered gets called "works".
10. **Test as real users.** A matrix of test accounts, one permission apart. Load tests use several users.
11. **Adversarial review at the end of every round.** Independent reviewers get the code, not your conclusion. A verifier tries to refute each finding, and a second vendor's model reviews the diff.
12. **Commit only when asked.** Changes stay as a diff to read. Never touch a shared git index, never push for someone else. Push q99-utils first, then bump the pins; client plugins go through a branch + PR.

## Skills

| Skill | When it kicks in |
|---|---|
| `working-agreement` | Every task: checklist flow, the human decides, name your picks, subagent briefs, git rules |
| `code-style` | Writing or cleaning code: the cleanup guide, libraries, licences, errors, data rules |
| `testing-rules` | Tests: contracts not copy, no model names, no tests for switches, targeted runs |
| `frontend-ui` | UI: no hardcoded sizes, screens fit the window, one look, server-side permissions, help docs |
| `design-system` | Standardizing the frontend: one palette, type scale, chart setup, codemod + lint guard, pixel diffs |
| `llm-agents` | Agents and prompts: agnostic loop, judgement in prompts, real names, agents that doubt |
| `llm-providers` | Routes, reasoning effort/thinking, model comparisons, all providers working |
| `dependency-bumps` | Bumps and security sweeps: read what's new, quarantine, isolated trial, real-data diff, every provider |
| `measure-before-claiming` | Any accuracy, cost or "it's identical" claim |
| `verify-in-browser` | Visual constraints and logged-in screens, checked with Playwright on the real frontend |
| `test-accounts` | Permission matrix users, test data, realistic load |
| `security-review` | Auth, agent SQL/code, fetching, uploads, secrets: executor-enforced blocks, audit method |
| `adversarial-review` + `adversarial-verifier` agent | Before "done": reviewers, refuters, second-model pass |
| `artifacts` | Shared pages: real examples, many charts, several versions, living fix lists, specs for other owners |
| `release-flow` | Pushing across repos: shared library first, then pins; quarantine; client plugins on a branch + PR |
| `capture-feedback` | Turns your own corrections into memories, and stable ones into this plugin |

## Results

What this way of working has produced so far:

- **Security holes found before clients did.** Permission bypasses in agent SQL, server file reads through model-written queries, and weak spots in auth. Each one was reproduced as a low-permission user, then fixed with a test or handed to its owner as a spec.
- **Bugs tests didn't see.** The second-model review keeps finding races and lost updates in concurrent code.
- **Claims that hold.** Running on all the data instead of a few examples has overturned optimistic numbers before they reached users.
- **Root causes, not patches.** Rate-limit errors were fixed by cutting request volume at the source, not by adding retries.
- **Safer dependencies.** Security advisories dropped sharply with every bump checked on real data and on every provider. The quarantine kept a supply-chain incident away.
- **A consistent frontend.** Sizes and colours live in one place with lint guards, and screens share the same components.
- **Less rework.** Corrections become rules, so the same mistake doesn't come back.

## Growing it

When Claude gets corrected, `capture-feedback` saves the rule with its reason. Rules that hold across projects get promoted here by PR.
