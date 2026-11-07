---
name: measure-before-claiming
description: Use before stating any accuracy, performance, cost or "it's the same" claim — measure on all the data, outside the client, before/after, with few repeats and many cases.
---

# Measure before claiming

- **Research runs on real data.** When investigating a library, method or capability, prototype it against the local test database with real (anonymized) data and show the actual results, never invented numbers.
- **All the data, not a hand-picked sample.** Run methods and defaults on every entity (every well, every file) before writing a number. Small samples overturned our own claims more than once.
- **Outside the client.** Parameters tuned on one client's data are in-sample. Freeze them and rerun on public data from other companies; report wins, inconclusive results and losses separately.
- **Free data first.** Measure on local/free databases; client warehouses only for single smoke runs of warehouse-specific changes.
- **1–2 runs per case.** More signal = more cases, not more repeats. Expensive runs need the human's OK; say the cost in one line.
- **Before/after on real data** for every library swap, refactor of a read path or "byte-identical" claim: dump the outputs of HEAD and of the change and diff them.
- **Before/after tables** for storage/API changes: bytes, queries, rows, latency; pros, cons, who could break.
- Pick defaults (timeouts, limits) from traces/stats, not from guesses.
- Know your local traps (dev-server reload delays, dev builds, keep-alive stalls) before drawing timing conclusions.
- Grade findings: backed by a record / beyond noise / unexplained / possibly chance.
