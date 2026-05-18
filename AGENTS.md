# AGENTS.md

This repo is the agent-legibility design spec for Customer.io product UI. AI agents (Claude Design, Cursor, Stitch, Copilot) reading this repo should follow these rules before generating any UI.

## Read first

Read [DESIGN.md](./DESIGN.md) at the start of every UI task. It defines:

- The voice and tone rules (sentence case, no exclamation marks, no marketing words, tabular numerals).
- The Pluma component vocabulary — use only Pluma primitives (`PlumaButton`, `PlumaBanner`, `PlumaBox`, etc.). Never native `<button>`, `<select>`, or custom-div modals.
- The refinement-levers framework — five sanctioned levers (padding scale, media placement, panel proportion, density, edge treatment) expressed as composition recipes, not new props.
- The five required states for every screen (loading, empty, error, success, permission-denied).
- The anti-patterns to never generate.

Then read the relevant per-component file. See the [Per-component AI guidelines](./README.md#per-component-ai-guidelines) list in the README.

## Six-check pre-flight

Run this before delivering any UI work:

1. **Voice** — sentence case, no exclamation marks, no "please" / "sorry" / "oops" / "easy" / "simply" / "just" / "users". Tabular numerals on numbers. Single-verb button labels. Action-oriented titles.
2. **Components** — only Pluma primitives. No invented variants.
3. **Tokens** — every color a token name from [tokens.md](./tokens.md), every padding on the spacing scale (`space-100`–`space-400`). No inline hex, no pixel padding, no `flex` overrides on split panels.
4. **States** — loading, empty, error, success, permission-denied all named.
5. **Refinement levers** — for every layout call, name the lever (padding scale, media placement, panel proportion, density, edge treatment), the recipe, and whether the choice ships solo or requires design review per the bounds tables.
6. **Anti-patterns** — final scan against [DESIGN.md anti-patterns](./DESIGN.md#anti-patterns--never-generate).

## Show the work

When making a layout call, name the lever and the recipe inline. Example: "Padding scale → relaxed via `<Box padding='space-300'>` per surfaces.ai-guidelines.md." This makes every decision auditable against the spec.

## Self-flag at the governance bar

If a refinement falls in the "Requires design review" column of any bounds table — custom split ratio, mixed density inside one surface, full bleed combined with elevation, a new media slot — stop. Name what is being reached for. Do not ship past the bar silently.

## Never invent

If the component or token you need is not in DESIGN.md, tokens.md, or the per-component files, the default answer is: this is a new pattern, not a refinement. Surface it as a design conversation rather than improvising. Inventing a primitive in code is the failure mode this repo exists to prevent.

## Names

- File: `DESIGN.md`
- System: `Pluma`
- Package: `@customerio/ui`
