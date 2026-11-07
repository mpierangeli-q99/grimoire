# grimoire — how we work with Claude Code at Q99

Pier's way of working with Claude Code, packed as a plugin so every Q99 developer gets the same rules. Each rule started as a correction he gave Claude and kept with its reason, so it is never needed twice.

## Install

```
/plugin marketplace add mpierangeli-q99/grimoire
/plugin install grimoire@grimoire
```

The skills load by themselves when the task matches.

A short `SessionStart` hook adds the few rules that must always apply (one item at a time, name your picks, commit only when asked; see `plugins/grimoire/hooks/session-start.md`) to every session. To turn it off, disable the plugin with `/plugin`.

## How Pier works

Read this before the skills. The skills hold the rules; this section explains the stance behind them, so you can apply them with judgement instead of mechanically.

### The stance

Claude works like a senior engineer on the team. It reads widely, proposes, measures and does the work. **The developer keeps the decisions.** Claude never takes a product, UX or architecture call silently, and whatever it did decide on its own, it says so. The developer reads every diff before it becomes a commit.

Speed comes from not redoing work. Doing less at a time is how you get there: one item, checked, then the next.

### A normal round

1. **Ask, and let Claude read first.** Claude reads the docs and the changelogs of what it will touch, plus the existing code. It does that before proposing anything.
2. **Get a plan as a list.** Big work becomes a numbered checklist, often on an artifact page with real examples and, for anything visual, 2–4 versions side by side. You pick.
3. **One item at a time.** Claude does one item, you check it, and the list gets ticked. "continue" means the last one was fine, do the next.
4. **Try it apart, then merge.** New ideas are prototyped in a scratchpad, a scratch environment or a side container, on real data. Only when they work do they reach the repo.
5. **Verify for real.** Run the touched tests and render the screen in a real browser. Test with users that have the right permissions, and measure on all the data. Claude never calls "works" what it didn't run or render.
6. **Adversarial review.** At the end of the round, independent reviewers look for bugs and a verifier tries to refute each finding. A model from another vendor then reviews the diff. Real findings get fixed with tests.
7. **Report.** Claude reports what changed, what was checked with results, what it did not check, and a "decisions I made" line for every number or variant you didn't choose.
8. **Commit when you say so.** Claude commits grouped by area, on the repo's convention. You push.

### Why the rules exist

Each rule came from a real correction. A few examples, so the reasons are clear:

- **"Change only what was flagged."** The feedback was "I don't like the texts" on a landing page. Claude rebuilt the visuals too, and the visuals were the part that was loved. Read complaints literally, and keep the version that got the reaction.
- **"Name your own picks."** "Who said a limit of 40 per user? I said queue the runs." Claude had swapped the idea for its own variant and reported it as approved.
- **"UX trade-offs are the developer's call."** To fix a nav overflow, Claude made the rail icon-only. The discussion that followed produced a better design than either version had.
- **"Measure on all the data."** A method that was "within 10%" on 4 hand-picked examples held on fewer than a quarter of the full set.
- **"Cut volume, not retries."** Rate-limit errors got a retry with backoff, and it was rejected at once: "why are we hitting the limit?" One read-merge-write per item was the cause.
- **"The executor blocks, not our check."** Our own SQL validator was the only thing stopping a model-written query from reading server files. "Impossible" has to mean the engine itself refuses.
- **"Respect what exists."** A tile on one dashboard looked different from the same tile on another. Users read drift as unfinished work.
- **"Smallest version first."** A 90-line custom retry transport was replaced by 20 lines around the library: "is all that machinery necessary?"
- **"No history in comments."** Rationale paragraphs in docstrings made the code harder to read. Reasons belong in the commit.

### What the plugin doesn't carry

- **Project facts** (ports, containers, how to mint a test token, which service to restart) go in each repo's `CLAUDE.md`.
- **Your own decisions** ("don't re-propose X", "I test the chat myself") build up in your Claude memory through `capture-feedback`.

The plugin is the starting point. Your corrections make it yours.

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
