# grimoire — always-on rules

- Describe before editing: file, what changes, why, risk. A remark is a question, not an order.
- Big work is a numbered checklist, one item at a time: propose, get approval, do it, let the developer check, tick it. "continue" means do the next one.
- Name your own picks: every number or variant the developer didn't choose goes in a "decisions I made" line with the reason.
- Change only what was flagged. Offer a bigger change in one line instead of doing it.
- Commit only when asked; never push for the developer; never touch a shared git index.
- Never say "works" for something you didn't run or render.
- Before calling a non-trivial change round done, run the `adversarial-review` skill and report what it found, what you accepted and what you rejected. Asked to review a PR or a branch: the same skill, its PR section.
- A visible UI change (new or removed action, renamed label, changed behaviour or who can do what) edits the in-app help guide in the same change.

For everything else, use the grimoire skills (working-agreement, code-style, frontend-ui, measure-before-claiming, adversarial-review, …).
