# AGENTS.md

This repo is the agent-legibility design spec for Customer.io product UI. AI agents (Claude Design, Cursor, Stitch, Copilot) reading this repo should follow these rules before generating any UI.

The human prompting you will usually **not** name the spec sections. Your job is to figure out which files apply, read them, and then generate. Do not ask the human to direct you through the spec.

## Routing — figure out which files to read

For every UI prompt, run this routing logic before generating:

1. **Always read** [DESIGN.md](./DESIGN.md). It carries the cross-cutting rules: voice, components, states, density, color tokens, refinement levers, anti-patterns, escape hatches. No exceptions.
2. **Always reach for** [tokens.md](./tokens.md) when you need a specific color, spacing, border, elevation, or typography value. Never inline a hex code or a pixel value.
3. **Match the surface in the prompt to a per-component file**, and read it:
   - Prompt mentions modal, dialog, confirmation, feature moment, lightbox → [modal.ai-guidelines.md](./modal.ai-guidelines.md)
   - Prompt mentions banner, alert, notice, announcement, warning bar → [banner.ai-guidelines.md](./banner.ai-guidelines.md)
   - Prompt mentions card, panel, tile, container, settings group → [surfaces.ai-guidelines.md](./surfaces.ai-guidelines.md)
   - Prompt mentions table, list, grid, data display → [data-table.ai-guidelines.md](./data-table.ai-guidelines.md)
   - Prompt mentions button, action, CTA → [button.ai-guidelines.md](./button.ai-guidelines.md)
   - Prompt mentions label, status pill, badge for category → [label.ai-guidelines.md](./label.ai-guidelines.md)
   - Prompt mentions breadcrumb, path, nav hierarchy → [breadcrumbs.ai-guidelines.md](./breadcrumbs.ai-guidelines.md)
4. **If the prompt spans multiple surfaces** (e.g., "build a settings page with a banner at the top and a data table below"), read all the matching per-component files. The cross-cutting rules in DESIGN.md still apply.
5. **If the prompt is ambiguous about the surface** (e.g., "design something that shows campaign performance"), make a best-guess routing call, name the call in your response ("I'm treating this as a card-based dashboard, reading surfaces.ai-guidelines.md"), and proceed. If you guessed wrong, the human will redirect.
6. **If no per-component file matches** (e.g., the user wants a navigation, a tooltip, a tab strip), fall back to the cross-cutting rules in DESIGN.md and the relevant `## Other Pluma components` section. Self-flag that the per-component file does not exist yet — that is a v0.2 backlog item.

## Six-check pre-flight

After routing and reading, run this before delivering any UI work:

1. **Voice** — sentence case, no exclamation marks, no "please" / "sorry" / "oops" / "easy" / "simply" / "just" / "users". Tabular numerals on numbers. Single-verb button labels. Action-oriented titles.
2. **Components** — only Pluma primitives. No invented variants.
3. **Tokens** — every color a token name from [tokens.md](./tokens.md), every padding on the spacing scale (`space-100`–`space-400`). No inline hex, no pixel padding, no `flex` overrides on split panels.
4. **States** — loading, empty, error, success, permission-denied all named.
5. **Refinement levers** — for every layout call, name the lever (padding scale, media placement, panel proportion, density, edge treatment), the recipe, and whether the choice ships solo or requires design review per the bounds tables.
6. **Anti-patterns** — final scan against [DESIGN.md anti-patterns](./DESIGN.md#anti-patterns--never-generate).

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

## Prompt cookbook

Humans asking you for UI may be starting from [prompts.md](./prompts.md), a cookbook of task-shaped recipes. If a prompt looks like it came from a recipe, treat the recipe as the human's intent and the spec as the authority — they should agree. If they disagree, the spec wins, and you flag the cookbook entry as needing an update.
