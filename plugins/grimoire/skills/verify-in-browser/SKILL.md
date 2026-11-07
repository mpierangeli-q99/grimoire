---
name: verify-in-browser
description: Use when a UI change has a visual constraint to confirm (scrollbars, sizes, dark mode, phone width), or a logged-in screen must be checked — Playwright against the real local frontend.
---

# Verify in a real browser

A stated visual constraint ("the scrollbar must stop there", "the diagram must not grow") is checked in a real render before it is called done. If you can't check it, say so.

## Setup
- One frontend: check on the same local frontend + engine the human uses, so everyone looks at the same build. Remove side dev servers once their work is merged in.
- Playwright from the frontend's own install with `chromium.launch({ channel: "chrome" })`.
- `waitUntil: "domcontentloaded"` + wait for a selector (SSE streams mean `networkidle` never comes).
- `ignoreDefaultArgs: ["--hide-scrollbars"]` to see scrollbars. Paint the part under test (e.g. a red thumb).

## Logged in
- Mint a real access token from the user-manager inside its container (the same function login uses, so the claims are valid), pass it straight into the script as the `access_token` cookie. Never write it to a file.
- Use a test account with the permissions the screen needs (see `test-accounts`).
- Fake states only in the browser: `page.route` the endpoint, fetch the real response, edit the JSON, `route.fulfill`. Register the catch-all first, specific routes after.
- Never confirm actions that run or spend anything.

## Without login
- Theme/CSS checks: a public page (e.g. forgot-password) loads the same root layout and CSS; preset `localStorage.theme`.
- Component checks: a temporary harness page outside the auth middleware, faked APIs. Delete it, restart the dev container, confirm 404, then revert temporary exports.

## Check
- Light, dark and phone width. Measure overflow (`scrollWidth` vs `clientWidth`).
- Data answers too: look at the chart before writing a narrative from summary statistics.
- Performance numbers only from a production build (`next build && next start`), never the dev server.
