---
name: frontend-ui
description: Use when building or reviewing frontend UI — tables, dialogs, layout, copy, data flow between backend and frontend.
---

# Frontend & UI

## Data
- **The frontend never transforms data.** No filtering, recomputing, re-basing. Every derived number comes from the backend payload; existing FE derivations are debt to move server-side.
- **Explicit save, never autosave.** Edits stage on the client; one Save sends everything and reloads once. Exception: creates that need a server-minted id.

## Layout
- **Responsive everywhere.** Must work at 1366×768 and 1536×864 (laptop DPI scaling), not only 2K. Viewport-relative sizes, grids that collapse, wide content gets its own `overflow-x: auto`. Measure with Playwright (`scrollWidth` vs `clientWidth`) before calling it done.
- **Stack vertically, not horizontally.** A table that doesn't fit: fold related column pairs into one stacked cell. A cell shows the same kind of thing on every row.
- **Reuse the app's table idiom** (filter panel, sortable/resizable table, pagination only for unbounded lists). Never a hand-written `<table>`. Selects in cells fill their width.
- **Group rows must read as headings:** tinted band, bold uppercase label, count, labelled subtotal.
- **Dialogs need structure:** title + context line, each group a labelled card, segmented controls for switches, footer with divider and the reason the primary is disabled. Every button says what it does.
- **No coloured left-edge accent bars.** Selected state = tint + weight, or a uniform border.
- **Persistent secondary nav is a column**, not a floating popover or header tabs.

## Copy
- UI chrome in **one language (English)**, full words, no crops ("Description", not "Descrip."). Client data stays as the client wrote it.
- Terse: titles and tags carry the rules; explain only what the user can't see.

## Verification
A `200` from the route doesn't prove it renders (client-side auth ships a shell). Use a real browser.
