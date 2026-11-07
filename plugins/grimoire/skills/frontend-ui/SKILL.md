---
name: frontend-ui
description: Use when building or reviewing frontend UI — sizing, shared components, design-system calls, empty values, permissions in the UI, help docs.
---

# Frontend & UI

## Sizing
- **No hardcoded sizes.** No `width: 92`, `text-[12.5px]`, `size={72}`. Use rem/em tokens, %, viewport/container units, auto/`fr` grid tracks, container queries, and one elastic element that absorbs leftover space.
- Must work on laptop screens (1366×768, 1536×864), not only large monitors.
- **Screens fit the window.** Admin/review/form screens never scroll as a page: root at window height, panels `flex: 1; min-height: 0` with their own `overflow: auto`, sticky table headers, action bar pinned. Scrolling pages are for dashboards only.
- A minimum size never crops a title.

## One look
- **Before building anything, look for what already exists and respect it**: filters, navbars, rails, headers, tab bars, dashboards, tiles, cards, tables, dialogs, buttons, tooltips, fonts, spacing. If it exists, reuse it as is. If it doesn't, build it once as a shared component in the app's existing style, so the next screen reuses it.
- Keep the UI theme everywhere: same fonts, same field look, same header and tile anatomy. A screen that looks different reads as unfinished.
- **Equivalent things look the same.** Same tile on two dashboards = the same shared component. Filters use the standard input/select. When two looks differ, ask which wins.
- Design-system choices (tokens, scales, naming) are an expert call: decide, implement, verify with computed styles or screenshots. Product UX changes still go to the human with options.
- Toggle buttons are the standard icon chip, never a classic outlined text button; add a short label when the icon alone is unclear.
- Tooltips follow the theme (paper background, divider border, theme text). Check dark mode.
- Never place tiles/panels automatically; the layout is the user's.
- No coloured left-edge accent bars. Dialogs: title + context, labelled groups, footer saying why the primary is disabled.

## Data in the UI
- **The frontend only formats** what the backend sends. No recomputing or filtering.
- **Show "Unknown"** (secondary colour) for a field that applies but has no value; hiding it reads as a bug.
- **Permissions are the server's.** The UI may read flags only to show/hide. A hidden button is never protection; when the server refuses, show the refusal. Flag any UI-only gate in review.
- Usage/cost data only for staff/admins.
- UI chrome in English, full words. Client data stays as written.

## Every visible change updates the help docs
- New button, removed action, renamed label, changed behaviour or who can do what → edit the in-app help guide in the same change, quoting UI strings exactly. Not for refactors or cosmetic detail.

## Verification
- A `200` doesn't prove it renders. Use `verify-in-browser` for any visual constraint the human stated.
- When the human will try the UI themselves: run typecheck + lint, hand it over, and say what you did not render.
