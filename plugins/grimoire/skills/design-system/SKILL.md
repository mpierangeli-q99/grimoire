---
name: design-system
description: Use when standardizing the frontend — colours, type scale, tokens, shared components, charts, themes — or when a style change should live in one place.
---

# Frontend standardization

Goal: any colour/format change happens in **one place**, and every screen follows it.

## How
1. **Find the seams first**: count who reads the theme object, CSS vars, hex literals, chart setups, inline styles. Show the numbers.
2. **One source of truth per concern**:
   - one palette file (brand + surfaces + roles like ink/tint/shadow); the component-library theme and CSS vars are generated from it;
   - one type scale (rem steps), aliasing the framework's named sizes;
   - one shared chart setup (fonts, tooltip, series colours) used by every chart;
   - styles in CSS files on vars, not inline.
3. **Codemod, then lint guard**: move all literals at once (sizes → scale steps, colours → tokens), then a lint rule that errors on new literals outside the theme folder.
4. **Prove nothing moved unintentionally**: computed-style diffs (vars × light/dark), before/after screenshots of every screen (pixel-identical except listed intended changes), Storybook stories for shared pieces.
5. **Production stays clean**: dev-only tools (a proposals switch to try themes) load only in development; check a production build contains none of it.

## Who decides
- Technical standardization (tokens, scales, naming, lint guards) is an expert call: decide, implement, verify, explain in a few lines.
- Product UX (visible text → icons, navigation, removing features) goes to the human with options in an artifact.
- When building from a preview, build exactly the controls picked; nothing it bundled silently (fonts, tints).
- Leave alone what was declared untouchable (e.g. the chat composer).

## Proposals
- Offer visual changes as an artifact with several versions side by side (mockups in the app's style, light and dark), each with a one-line trade-off. The human picks; build that one as drawn.
