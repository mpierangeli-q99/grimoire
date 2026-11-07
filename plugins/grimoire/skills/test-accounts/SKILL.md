---
name: test-accounts
description: Use when testing permissions, access control, multi-user behaviour or load locally — create a matrix of test users and roles, test data, and realistic load with several users.
---

# Test accounts

Test with users that have exactly the permissions under test, never only with your own superuser.

## Permission matrix
Create local users, each with its own clearly-named test role, one permission step apart: the most basic user, then one more permission at a time, plus a staff user without the admin role and a superuser without roles.

- Create test data clearly marked (e.g. "TEST permissions – report", forced to a state where it never auto-runs anything paid).
- For each user: what the UI offers, what the **API** answers (the server must refuse, not the UI hide), and what the agent can reach through chat (documents, SQL, tools).
- Write the result as an access matrix (user × action → allowed/refused, expected vs actual).
- Synced content keeps the source's permissions: test that sharing/reuse never widens them.
- Messaging tests only target your own DM or the team's testing channel.

## Load
- One user in parallel measures that user's rate limit, not the system. Use **several users**.
- Per-IP limits are intentional (anti-flood). Send a distinct `X-Forwarded-For` per simulated user, with the server started with `--proxy-headers`, or you measure the limiter.
- Size the test to production (users × conversations, container RAM/CPU). Run with Docker limited to prod's CPU/RAM.
- Measure memory per path (process RSS before/after, per request type), list the top consumers with numbers, and rank critical/high/medium. Fixes go one at a time with an example and metrics.
- Cheap cases first; anything that spends real money (long agent runs, client warehouse queries) needs the human's OK.
