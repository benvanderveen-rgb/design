# DESIGN.md bundle — Customer.io

**Refreshed 2026-06-01.** A paste-friendly snapshot of Customer.io's design spec — design principles, voice rules, refinement levers, anti-patterns, governance, and per-component guidelines — consolidated into one file for tools that can't read source files directly.

The spec is in transition. Pluma's public site at [pluma.customer.io](https://pluma.customer.io/) is now shareable, with the Polaris-shaped IA (Foundations / Components / Patterns / Advanced) already in place. This bundle is the point-in-time snapshot to use until DESIGN.md's payload — voice, refinement levers, states, density, anti-patterns — is ported into Pluma's Patterns section. When you can connect to Pluma directly, prefer that over this file.

## How to use this bundle when you read it

You will usually be asked to generate, critique, or refactor Customer.io UI by a human who does **not** name the spec section to use. Your job is to figure out which part of this bundle applies, read it, and then generate.

### Routing — figure out which section to read

For every UI prompt:

1. **Always read** the DESIGN.md section below. It carries design principles, voice, components, states, density, refinement levers, anti-patterns, escape hatches.
2. **Reach for the tokens.md section** for any color, spacing, border, elevation, or typography value. Never inline a hex code or pixel value.
3. **Match the surface in the prompt to a per-component section below:**
   - Modal, dialog, confirmation, feature moment → `Modal`
   - Banner, alert, notice, announcement → `Banner`
   - Card, panel, tile, container, settings group → `Surfaces`
   - Table, list, grid, data display → `DataTable`
   - Button, action, CTA → `Button`
   - Status pill, badge for category → `Label`
   - Breadcrumb, nav hierarchy → `Breadcrumbs`
4. If the prompt spans multiple surfaces, read all matching sections. Cross-cutting rules in DESIGN.md still apply.

### Six-check pre-flight before delivering UI work

1. **Voice** — sentence case, no exclamation marks, no "please" / "sorry" / "oops" / "easy" / "simply" / "just" / "users". Tabular numerals on numbers. Single-verb button labels. Action-oriented titles.
2. **Components** — only Pluma primitives (`PlumaButton`, `PlumaBanner`, `PlumaBox`, etc.). No native `<button>`, `<select>`, or custom-div modals.
3. **Tokens** — every color is a token name, every padding is on the spacing scale (`space-100`–`space-400`). No inline hex, no pixel padding, no `flex` overrides on split panels.
4. **States** — loading, empty, error, success, permission-denied all named.
5. **Refinement levers** — for every layout call, name the lever (padding scale, media placement, panel proportion, density, edge treatment), the recipe, and whether the choice ships solo or needs design review.
6. **Anti-patterns** — final scan against the anti-pattern list in the DESIGN.md section.

### Self-flag at the governance bar

If a refinement crosses into "Requires design review" in any bounds table — custom split ratios, mixed density on one surface, full bleed combined with elevation, a new media slot — stop. Name what is being reached for. Do not ship past the bar silently.

### Names

- File: `DESIGN.md`
- System: `Pluma`
- Package: `@customerio/ui`

---

## Bundle contents

The sections below appear in this order:

1. [DESIGN.md](#designmd--customerio-product-design-system) — the spec
2. [tokens.md](#tokensmd) — color and typography token reference
3. [Banner](#banner) — Banner / Alert per-component guidelines
4. [Breadcrumbs](#breadcrumbs) — Breadcrumbs per-component guidelines
5. [Button](#button) — Button per-component guidelines
6. [DataTable](#datatable) — DataTable per-component guidelines
7. [Label](#label) — Label per-component guidelines
8. [Modal](#modal) — Modal per-component guidelines
9. [Surfaces](#surfaces) — Card / Box / Panel per-component guidelines (new 2026-05-18)

---

# DESIGN.md — Customer.io Product Design System

The rules layer for AI agents and humans building Customer.io product UI. Read this before generating any screen, component, or copy.

This file sits on top of **Pluma** — Customer.io's design system at https://pluma.customer.io/ — and defines the cross-cutting tone, voice, states, density, refinement levers, and anti-patterns that Pluma's component and pattern pages do not yet name. Component APIs, foundations, and the canonical MCP setup live in Pluma; this file is the opinionated layer that constrains how the pieces compose.

**For AI agents (Cursor, Claude, Stitch):** the rules sections below define tone, voice, components, states, density, and anti-patterns. For component APIs, query the Pluma MCP or visit the Pluma site.
**For engineers:** the rules below plus the Pluma MCP setup at https://pluma.customer.io/overview/mcp-server.
**For designers and PMs:** the reference for how Customer.io UI looks, behaves, and sounds, paired with Pluma's component and pattern pages.

---

## Design principles

These three principles are the layer everything below expresses. The rules — tone, components, states, density — are *how* we build; the principles are *why* we resolve a decision one way and not another.

**How to use them (this is the point).** A generic prompt produces defensible, average-case UI because it has no reason to prefer one good option over another, so it hedges toward convention. Principles give the agent a reason. They are tiebreakers: when two or three layouts, flows, or copy choices are all reasonable, the principle names the one that is *ours*. That is where distinctiveness comes from — not more rules, but stated preferences that resolve the ties a generic model would resolve blandly.

Each principle has two layers. The **narrative** is the intent, for humans and for context. The **Apply** block is the operational contract: the testable directives an agent keys off. When generating UI, treat the Apply blocks as binding — if a choice contradicts one, it is wrong, the same as violating a component rule below. When the narrative and the Apply block seem to diverge, the Apply block wins for the specific decision in front of you.

**Resolving the built-in tensions.** The principles intentionally pull against each other — flexibility vs. familiarity, speed vs. friction. That nuance is for humans; an agent needs the resolution stated. The defaults are: **paved over dirt, familiar over novel, fast over slow.** Each has named exceptions below. Absent an exception, take the default.

### One paved road, many dirt roads

Our champions love us for knowing anything is possible. Power and flexibility are at our core. We will continue to build dirt roads to allow customers to realize this power. Not everyone wants to go offroading. There must be a paved road. We are opinionated about the primary path our customer should take. That provides the focus needed to deliver a quality experience. It is hard to create a quality experience for everyone or every path.

We acknowledge teams use Customer.io. A paved road looks different for a growth practitioner than a data partner. We always offer guidance and direction to help customers make progress toward their goal. We aim to help customers level up their skills, instead of requiring them.

A paved road...

- is a smooth ride, free from unwanted bumps. There cannot be an unresolved edge case, inconsistent label, or a missing empty state.
- is built by sequencing capabilities, not removing them. Functionality is contextually relevant instead of always visible.
- leads to a destination. That's your north star. This is how power can feel personal.

**Apply:**

- Design the primary path first and make it the visual and interactive default. Power-user and edge-case affordances remain reachable but never compete with the primary path for attention.
- Reveal capability by sequence and context, not all at once. Surface a control when its moment arrives; do not show every option upfront. (See [Density](#density) and progressive disclosure in component rules.)
- Every screen on the paved road resolves all five [states](#states--every-screen-must-define-all-five). A missing empty state, an inconsistent label, or an unhandled edge case means the road is not paved — do not ship it.
- Every primary flow has a clear destination and a next step toward it. No screen is a dead end.
- **Exception (dirt roads):** advanced or escape-hatch paths may expose more and guide less — but only when the primary path is fully built first, and the dirt road must be a deliberate detour, not the default.

### Do the least surprising thing

Our customers trust us to orchestrate their messaging strategy. Sending real messages to real people is high stakes. Trust is non-negotiable. We build trust through predictable and accepted patterns. We exhaust the system first before innovating. We invent to move the system forward, not to move faster in the moment.

Our innovation is what the product does for customers: the insight, the automation, the capability. Our customer's messaging strategy should be novel. Our interface should feel familiar. Predictable patterns help us deliver clarity, usability, and accessibility.

**Apply:**

- When a choice has a conventional pattern and a novel one and both work, take the conventional one. "Least surprising" is the tiebreaker.
- Exhaust Pluma before inventing. Reuse an existing component, token, or pattern rather than composing a new one. A new primitive is a last resort that goes through `#design-systems`, never a shortcut to ship faster. (Reinforces [Components — use Pluma only](#components--use-pluma-only-never-invent-primitives) and [Escape hatches](#escape-hatches).)
- Locate novelty in what the product *does* — the insight, automation, capability — not in how the interface looks or behaves. A novel layout for a familiar task is a defect, not a feature.
- **Relationship to dirt roads:** flexibility (Principle 1) is about how much power a path exposes; familiarity (this principle) is about the patterns that path is built from. A dirt road can be powerful and still use familiar, predictable patterns — and it should.

### Every action should build confidence

Our customers aren't trying to start a campaign, they're trying to grow their business. We design with their ambition in mind, not just their workflow. Customers should feel certain they completed the task, did it well, and know what to do next. They should feel more capable than when they started. That confidence will compound and build momentum over time. The moment after the click is as important as the click itself.

Speed is a virtue in most of our product. AI can compress the work of creating segments or building campaigns. We deliberately introduce friction at the seam. A small change to a segment can change behavior in a campaign, and ultimately send the wrong messages to the wrong audience. Our Agent knows more than our customer when it comes to their event data, audience, campaigns, etc. The product can be a thought partner, but our customers always remain in control. They are the expert.

**Apply:**

- Every action resolves to an unambiguous outcome: a success state confirming it completed (see [States — Success](#states--every-screen-must-define-all-five)) and, where relevant, the next step toward the customer's goal. The moment after the click is part of the design, not an afterthought.
- Default to speed. Do not add confirmation steps, interstitials, or "Are you sure?" friction to routine, low-stakes, or reversible actions. (Reinforces the [anti-pattern](#anti-patterns--never-generate) against unnecessary confirmation modals.)
- **Exception (friction at the seam):** insert a deliberate confirmation step only where a change cascades into live messaging — editing a segment that feeds a running campaign, changing an audience, altering a trigger. Name the downstream effect in the confirmation ("This segment feeds 3 live campaigns"), not a generic warning.
- AI proposes, the customer decides. The Agent may suggest, draft, or pre-fill using what it knows about the customer's data — but high-stakes changes are surfaced for explicit confirmation, never auto-executed. Keep the customer in control; they are the expert on their intent.

---

## Tone

- Dense, calm, tool-not-brochure
- Craft visible in the details, never decorative
- Design serves the data, not the other way around

## Voice (apply everywhere)

- Sentence case throughout — titles, labels, headers, columns, button labels
- Imperative for buttons: "Save segment", not "Click here to save"
- Single action verb per button label, prefer one verb over a phrase
- Add an ellipsis when the action requires an additional step ("Export...")
- No exclamation marks anywhere in product UI
- Say "people" not "users"
- Numbers as plain numbers (12,403 not "12.4K") in body; abbreviate only in badges above 1k (1.2k, 42.7k, 1.4M)
- No "please", "sorry", "oops", "easy", "simply", or "just"
- Match tone to severity: direct and factual for errors and warnings, conversational for features and info

## Components — Use Pluma only, never invent primitives

When in doubt about a component's API, query the Pluma MCP (setup at bottom of this file) or check the per-component docs in the index. The rules below are the cross-cutting reach-for guidance.

### Button (`PlumaButton`)

- `variant="primary"` — limit one per page or workflow
- `variant="secondary"` — multiple allowed
- `variant="subtle"` — least important action (called "Tertiary" in design language; the prop value is `subtle`)
- Use `isDanger` for destructive actions, works with all variants
- Use `href` to render as a link for navigation or external URLs, never for in-page actions
- Use `isIconOnly` only in condensed layouts or menus
- Order actions right-to-left from most important to least important
- Loading: rely on `autoLoading` (default true) when `onClick` returns a promise, or set `isLoading` explicitly
- Renders as `<button type="button">` by default; renders as anchor when `href` is provided

### Banner (`PlumaBanner`)

Use for non-modal, page-level information: status changes, warnings, feature announcements, system notices.

- Do not use Banner for field validation — use inline form errors
- Do not use Banner for action confirmations — use Snackbar (auto-dismissing)
- Match variant to semantic meaning, never pick variant for color alone:
  - `default` — neutral tips and guidance
  - `information` — helpful context
  - `success` — confirms positive outcome
  - `caution` — issue may require attention
  - `error` — critical issue, must address immediately
  - `feature` — newly available feature or system update
- Types: `inline` (default, in page flow), `alert` (page-level, elevated shadow), `announcement` (app-wide, full-width)
- Limit to one primary action per banner; add a dismiss action only alongside it
- Do not put all info in the title alone — use description (children) for detail
- Title: sentence case, few strong summary words

For icon swaps, outlined vs. filled treatment, action layout, and edge treatment, see [Refinement levers](#refinement-levers) and the [Banner refinement recipes](./banner.ai-guidelines.md#refinement).

### Label (`PlumaLabel`)

Use for status or category indicators with short descriptive text ("Active", "Draft"). Limit text to one or two words.

- Use `showStatusIndicator` for lifecycle status (running/stopped, online/offline)
- Do not use Label for numeric quantities — use Badge instead
- Do not use Label for user-generated interactive tags — use Tag instead
- Semantic colors (use consistently across app, never decoratively):
  - red — critical or destructive
  - yellow — warning
  - green — success or functional
  - blue — info
  - grey — neutral
- Status indicator restricts color to: red, yellow, green, blue, purple, grey

### Breadcrumbs (`PlumaBreadcrumbs`)

- Add only on pages two or more levels deep
- Place on the left side of the top nav bar
- Represent actual page hierarchy, not session history or navigation path
- Sentence case, use full page titles
- Last item renders as plain text (current page) by default

### DataTable (`PlumaDataTable`)

- Use for tabular data with multiple fields
- Never use DataTable for page layout — use Box or Grid
- Strip features when not needed — don't add search, filters, or row selection unless the screen requires them
- Provide a `caption` (required, hidden visually, for assistive tech)
- Column headers: sentence case, full words, no abbreviations
- Show a meaningful empty state with title, description, icon, and action — never leave blank
- Keep minimum columns visible by default; allow show/hide via `withColumnsSettings`
- `layout="fixed"` (default) for equal columns; `layout="auto"` only when readability requires horizontal scroll
- Use `isStriped` for large datasets, especially with infinite scrolling
- Loading state: `isLoading` shows skeleton rows; configure `loadingState.rowCount`
- Empty state: configure `emptyState` with title, description, icon, action — or use `emptyStateComponent`
- Sorting: single-column only; use `withSorting`
- Pagination: use `withPagination`; for very large datasets use `withVirtualizer` instead

### Modal (`PlumaModal`)

Use for tasks requiring the user's immediate attention: action confirmations, short forms, feature moments, supplementary workflows.

- Never nest Modals — rework the flow, move content inline, or use a Popover
- Modal for confirmations or input; Drawer for less contextual content; Popover for small inline supplementary content
- Keep Modal focused on a single task — move multi-step or lengthy content to a dedicated page
- Titles in sentence case, action-oriented ("Delete campaign"), never vague ("Are you sure?")
- Footer action labels: specific verbs that match the Modal's purpose — never "OK" or "Submit" when a specific verb works
- Compose with subcomponents: `ModalBody`, `ModalFooter`, `ModalSplit`, `ModalInset`

Common patterns:

- **Confirmation** — hide close button (`shouldShowCloseButton={false}`), primary/secondary footer actions
- **Destructive confirmation** — hide close button, require explicit input (e.g., text field), disable primary until met, mark primary `isDanger`
- **Form** — disable overlay click dismissal (`shouldCloseOnOverlayClick={false}`) to prevent accidental data loss
- **Save/Discard** — hide close button, three footer actions: go back, discard (`isDanger`), save (primary)

For padding, media placement, panel proportion, density, and edge treatment, see [Refinement levers](#refinement-levers) and the [Modal refinement recipes](./modal.ai-guidelines.md#refinement).

### Surfaces — Card, Box, Panel (`PlumaBox`)

Pluma does not ship a single `Card` primitive. Surfaces are composed from `Box` with sanctioned tokens for padding, surface, border, elevation, and media placement.

- Compose from `Box` and `Stack` — never a `<div>` with inline styles
- Use a surface to group related content into a single visual unit; use a section divider with no background when content belongs to the page flow
- Never nest filled surfaces more than two levels deep
- Elevation `prominent` is reserved for overlays (popovers, dropdowns) — not for in-flow cards
- For padding scale, media placement, density, and edge treatment, see [Refinement levers](#refinement-levers) and the [Surface refinement recipes](./surfaces.ai-guidelines.md#refinement)

### Cross-cutting patterns across Pluma components

These apply broadly — default to them unless a specific component's rules override:

- **Polymorphic via `as` prop.** Most components accept `as` to change the rendered element (defaults vary). Use it rather than wrapping in another tag.
- **React content via `children`, Ember via `{{yield}}`.** Component text content uses React's `children` or Ember's default block.
- **Ember invocation uses `@` prefix on props.** Example: `<PlumaBanner @variant="information" @onClose={{this.close}}>Text</PlumaBanner>`.
- **`autoLoading` defaults to true.** On Button and Banner actions — when `onClick` returns a promise, a loading state shows automatically. Set `isLoading` to override.
- **`isDisabled` lightens colors and blocks interaction.** Standard across all interactive primitives.
- **Sprinkle props for spacing and layout.** Components extending Box accept spacing and layout utilities via consistent prop names.
- **Deprecated props throw runtime errors in dev** when used in invalid combinations — catch them before production.

### Other Pluma components

For components not detailed above (Badge, Avatar, Input, Checkbox, Radio, Toggle, Tag, Tooltip, Spinner, Dropdown, Tabs, Empty state, Form, Navigation), see the component documentation index below. Default to the Pluma primitive in every case; never invent a custom version.

## States — every screen must define all five

- **Loading:** skeleton with a 200ms delay; never block the screen with a centered spinner
- **Empty:** icon + one-line "what this is" + single primary CTA; never leave blank or say "Nothing to show"
- **Error:** what went wrong + why (if knowable) + the action that moves the user forward. Never raw API errors or stack traces.
  - Primary action is a recovery path ("Re-upload corrected file", "Reconnect account"), not "Dismiss" or "OK"
  - For partial results, lead with what succeeded ("8,556 of 12,403 rows imported"), then name the gap, then the action — "failure" framing overstates the damage when most of the operation worked
  - Non-trivial recoveries get a three-action footer: dismiss, detail ("View error log"), fix — right-aligned, in that order
- **Success:** Snackbar toast (3s auto-dismiss) for ephemeral confirmations, inline checkmark for in-place edits
- **Permission-denied:** show feature disabled with a tooltip explaining the role required; do not hide the feature

## Density

**Bias generous over tight.** When two sanctioned values would both work — padding, gap, row height, field spacing, surface margin — choose the more generous one. The point is *calm density*: close enough that grouped elements relate, far enough that surfaces breathe. Spec-attached output should read as deliberately spaced compared to tight-by-default agent output. If a surface looks correct but cramped, it's wrong. A few pixels of extra room across a screen is the difference between "the agent shipped it" and "a designer touched it."

Where this bias lands in practice:

- **Content surfaces** (cards holding body copy, settings panels, feature moments) — default to `space-300` (24px) padding, not `space-200`. The latter is the compact floor; reach for it only when the surface lives in a dense grid.
- **Form field gaps** — 24px is the baseline named below; lean to 28–32px when fields carry helper text or when the form is part of an onboarding flow.
- **Field-group gaps** — 32px baseline; reach for 40px when groups represent distinct stages of a flow.
- **Table rows** — 36px is the default named below; consider 40px when row content includes multi-line cells or paired text-and-meta.
- **Stack gaps inside surfaces** — `space-200` is the baseline; `space-300` when children are reading-weight (headings, body paragraphs) rather than scanning-weight (rows, chips, metric tiles).

The bias is one step, not two. Don't jump to `space-400` because more is more — calm density, not roomy density.

### Concrete defaults

- **Body text:** `text-product-p` (sans, 14px, regular weight, 20px line-height)
- **Small text:** `text-product-ps` for metadata and helper text, `text-product-pxs` for timestamps and captions
- **Headings:** `text-product-h1` through `text-product-h4`
- Full typography scale and reach-for rules live in [tokens.md](./tokens.md#typography-tokens)
- Table row height: 36px default, 32px compact (toggle when >50 rows), never below 32px
- Tabular numerals on all numbers (`font-variant-numeric: tabular-nums`)
- Right-align numbers, left-align text, center-align icon-only columns
- Truncate with tooltip over wrap in tables (except where reading the full value matters, like subject lines)
- Form density: 36px inputs, 8px label-to-input gap, 24px between fields, 32px between field groups

## Color, tokens, and charting

The full token reference lives in [tokens.md](./tokens.md). Read it before generating any color-related code.

### Token vocabulary at a glance

Token names follow the pattern `color-{family}-{intent}-{weight}-{state}`.

- **Families:** `surface` (backgrounds), `border`, `text`, `icon`
- **Intents:** `base` (neutral), `accent` (brand teal), `caution` (warning yellow), `critical` (error/destructive red), `information` (info blue), `success` (green), `feature` (purple)

The Customer.io brand accent is `teal_spruce` — a dark, spruce-leaning teal, not a blue. Generic "primary blue" is never the right choice for a primary action. Blue tokens (`blue_wave`, `blue`) are reserved for the `information` intent, focus rings, and the Label `blue` semantic.
- **Weights:** default, `bold` (stronger), `subtle` (lighter), `minimal` (lightest tint)
- **States:** default, `hover`, `active`, `disabled`, `focus`

Examples: `color-surface-critical-subtle` is a light red error background, `color-text-success-bold` is strong green confirmation text, `color-border-focus` is the focus ring.

The CSS-style token name maps to JS as `themeVars.color['{name-without-color-prefix}']`. For example, `color-surface-active` becomes `themeVars.color['surface-active']`.

### Reach-for rules

- **Every color value must come from a token.** Never inline a hex code.
- **Primary CTA:** `color-surface-accent` background, `color-text-white` text, `color-surface-accent-hover` on hover
- **Destructive CTA:** use the Button's `isDanger` prop, or `color-surface-critical` background with `color-text-white` text
- **Body text:** `color-text-base` (default), `color-text-subtle` (secondary), `color-text-minimal` (tertiary)
- **Card surfaces:** `color-surface-base` (default card), `color-surface-subtle` (panels nested inside cards), `color-background-default` (page background)
- **Focus ring:** `color-border-focus` (always `palette-blue_wave-400`)
- **Color is never the only signal of state.** Always pair with an icon or text label.

### Status colors (semantic mapping)

| Intent | Surface (subtle) | Text (bold) | When to use |
|--------|------------------|-------------|-------------|
| Information | `color-surface-information-subtle` | `color-text-information-bold` | Neutral notices, info banners |
| Success | `color-surface-success-subtle` | `color-text-success-bold` | Confirmations, healthy states |
| Caution | `color-surface-caution-subtle` | `color-text-caution-bold` | Warnings, attention needed |
| Critical | `color-surface-critical-subtle` | `color-text-critical-bold` | Errors, destructive actions |
| Feature | `color-banner-feature-background` | (purple_nova text) | New features, promotional |

### Journeys canvas (workflow tokens)

The journey canvas uses a domain-specific categorical palette. Map node type to token:

- **Data nodes** (audience, attribute, segment changes) → `color-surface-workflow-data`
- **Delay / wait nodes** → `color-surface-workflow-delays`
- **Flow-control nodes** (split, exit, branching) → `color-surface-workflow-flowcontrol`
- **Message-sending nodes** (email, push, SMS, in-app) → `color-surface-workflow-messages`

Outside the journey canvas, do not use workflow tokens. Inside it, do not invent new node colors.

### Charting and data-viz rules

- Use a defined chart palette for categorical data (campaigns, channels, segments). Never use the brand accent as a generic chart series — it loses meaning as emphasis elsewhere.
- Sequential palettes for ordered data (heatmaps, density)
- Diverging palettes for delta or A/B comparison
- Sparklines: 1px stroke, no fill, no axes, no grid — the number next to it carries the load
- Charts with more than three data points require hover tooltips
- Prefer one well-labeled chart over a dashboard of six

### Token deprecations

- `color-banner-warning-*` is deprecated — use `color-banner-caution-*`
- Banner `variant="warning"` is deprecated — use `variant="caution"`

## Refinement levers

Pluma deliberately exposes a narrow set of props per component. Most of the layout nuance designers reach for — padding shifts, image placement, panel weighting, edge treatments — is not a prop on the component. It is a **composition recipe** built from primitives Pluma already ships: `Box`, `ModalSplit`, `ModalInset`, `Stack`, `Grid`, sprinkle spacing props, and the spacing-token scale.

The rules in this section define the bounded vocabulary AI agents and engineers can reach for when a screen needs refinement that the base component does not name. Treat it as the only sanctioned vocabulary — outside this list, refinement falls back to the [Escape hatches](#escape-hatches) bar.

### Five cross-cutting levers

Every refinement is one of these five. Name the lever you are reaching for before you compose.

- **Padding scale** — adjust the inner spacing of a surface. Three steps only: `compact`, `default`, `relaxed`. Compose with sprinkle `padding` props or by wrapping content in `Box` with a spacing token. Never apply arbitrary pixel values.
- **Media placement** — where an image, illustration, or visual sits relative to the body. Four slots only: `top`, `leading`, `trailing`, `bleed` (edge-to-edge). Compose with `ModalSplit`, `ModalInset`, or a `Box` with the `inheritPadding` pattern.
- **Panel proportion** — for split layouts, the relative weight of the two columns. Three ratios only: `50/50` (balanced, default), `40/60` (narrative leads), `60/40` (form leads). Express via `ModalSplit` width tokens, never as a `flex` override.
- **Density** — vertical and horizontal rhythm. Three steps: `compact`, `default`, `relaxed`. Maps to the spacing-token scale; never to inline pixel values. Density should be set at the surface level and inherit downward.
- **Edge treatment** — how content meets the surface boundary. Three modes: `inset` (full padding, default), `flush` (zero padding on one or more edges), `bleed` (content extends to the full surface bounds, including media). Compose with `ModalInset`, `Box` with negative-margin tokens, or the surface's `inheritPadding={false}` pattern.

### How to compose

Start from the base component, then layer one or two levers at most. Composing more than two levers on a single surface is a flag that the layout has outgrown the component.

- **Adjusting padding** — wrap or replace the default body with `<Box padding={spacingToken}>` using `space-100` (compact), `space-200` (default), `space-300` (relaxed). The token names map to the spacing scale in [tokens.md](./tokens.md#spacing-tokens).
- **Adding a top image** — place the media inside `<ModalInset inheritPadding={false}>` above `ModalBody`. The image bleeds to the surface edges; body padding resumes below.
- **Adding a side image** — use `<ModalSplit>` before `<ModalBody>` for a leading panel, after for a trailing panel. Side is set by DOM order, not a prop.
- **Mixing inset and standard content** — `ModalInset` accepts content alongside padded body content within the same `ModalBody`. Use this for full-width banners or dividers inside an otherwise-padded modal.
- **Tightening density inside a surface** — apply density at the surface root, then let descendants inherit. Do not set density on individual rows or fields.

### Governance — ship solo vs. ask design

Each lever has a bounded range that ships solo and a threshold that requires design review. The bound is the line where the refinement stops being a recipe and starts being a new pattern.

| Lever | Ships solo (no review) | Requires design review |
|---|---|---|
| Padding scale | One step from default (`compact` or `relaxed`) using sanctioned spacing tokens | Custom padding values, or stepping more than one level (e.g., compact → relaxed in the same surface) |
| Media placement | Any of the four sanctioned slots, single image per surface | Multiple media slots on one surface, or media that overlaps non-media content |
| Panel proportion | Any of the three sanctioned ratios using `ModalSplit` | Custom ratios, three-column layouts, or proportion changes mid-flow |
| Density | One step from default | Mixed density within a single surface, or density applied per-field rather than per-surface |
| Edge treatment | `inset` (default) or single-side `flush` | Full-`bleed` content that crosses navigation chrome, or `bleed` combined with shadows |

When a refinement crosses into the right column, do not ship it. Open the conversation with `#design-systems` or attach a design review to the PR. The bound exists because the right column is where new patterns are born — and new patterns belong in Pluma, not in a one-off composition.

### Per-component refinement recipes

The cross-cutting levers above describe the vocabulary. Concrete recipes — which props, which subcomponents, which tokens — live in the per-component AI guideline files:

- Modal: see [modal.ai-guidelines.md](./modal.ai-guidelines.md#refinement)
- Banner: see [banner.ai-guidelines.md](./banner.ai-guidelines.md#refinement)
- Cards and surfaces: see [surfaces.ai-guidelines.md](./surfaces.ai-guidelines.md#refinement)

When a per-component file does not have a Refinement section yet, the levers above are the only available source. Composing outside that vocabulary is an [escape hatch](#escape-hatches) by definition.

## Anti-patterns — never generate

### Deprecations to avoid in any generated code

- `variant="warning"` on Banner — deprecated, use `variant="caution"`
- `isError` on Button — deprecated, use `isDanger`
- `unsafe_isActive` — deprecated, use `isActive`
- `onSortingChange` top-level on DataTable — deprecated, use `sorting.onChange`

### Component misuse

- Native HTML `<button>`, `<select>`, or `<input>` when Pluma equivalents exist
- Custom modals or dropdowns built from divs
- DataTable used for page layout
- Label used for numbers (use Badge) or interactive user-generated tags (use Tag)
- Banner used for field errors (use inline form errors) or action confirmations (use Snackbar)
- Multiple primary buttons on one screen
- Multiple primary actions in a single Banner
- Native `<select>` when there are more than 10 options (use Combobox)

### Visual and structural

- Inline hex values — always use Pluma tokens
- Blue primary buttons — the brand primary is `color-surface-accent` (`teal_spruce`, a dark spruce-leaning teal). Blue tokens are reserved for `information`, focus rings, and the Label `blue` semantic
- Arbitrary pixel padding values — use the sanctioned spacing scale (`space-100`, `space-200`, `space-300`) per the [Refinement levers](#refinement-levers) padding range
- `flex` or inline-style overrides on split panels — use the three sanctioned `ModalSplit` ratios; custom ratios go through design review
- Custom media slots beyond `top`, `leading`, `trailing`, `bleed` — additional slots are a pattern, not a refinement
- Density set per-field or per-row instead of per-surface
- "Are you sure?" modals for non-destructive actions
- Decorative shadows (every shadow must communicate elevation)
- Color as the only signal of state (always pair with icon or text)
- Title-case headers mixed with sentence-case in the same surface

### Voice violations

- Exclamation marks anywhere in product UI
- "Click here" or pronoun-led button labels
- Marketing-tone copy ("Awesome", "Amazing", "Just", "Simply")
- "Sorry" or "Oops" in error messages (apologize by fixing the underlying issue)
- "Users" — say "people"

## Escape hatches

When a rule must be broken:

- Prefer a documented exception in this file over a one-off
- Prefer a new token over an inline value
- Prefer a composition of existing primitives over a new primitive
- If you're reaching for any anti-pattern above, stop and ping `#design-systems`

---

## Component documentation index

Per-component API details, accessibility, and code examples live on the public Pluma site at https://pluma.customer.io/components and https://pluma.customer.io/foundations. The table below maps each topic to the canonical Pluma page; the repo-level paths in this section refer to legacy or in-progress local files and are being phased out as Pluma's site becomes the single reference.

The rules above are cross-cutting guidance; reach for the Pluma page when this file's reach-for rules aren't specific enough. Each Pluma page includes a "Copy for LLMs" affordance for pulling agent-ready context.

### Foundations

| Topic | File |
|---|---|
| Color tokens | [foundations/color.md](foundations/color.md) |
| Typography | [foundations/typography.md](foundations/typography.md) |
| Spacing & layout | [foundations/spacing.md](foundations/spacing.md) |
| Iconography | [foundations/iconography.md](foundations/iconography.md) |
| Motion & animation | [foundations/motion.md](foundations/motion.md) |

### Atoms

| Component | File |
|---|---|
| Button | [components/atoms/button.md](components/atoms/button.md) |
| Badge | [components/atoms/badge.md](components/atoms/badge.md) |
| Avatar | [components/atoms/avatar.md](components/atoms/avatar.md) |
| Input | [components/atoms/input.md](components/atoms/input.md) |
| Checkbox | [components/atoms/checkbox.md](components/atoms/checkbox.md) |
| Radio | [components/atoms/radio.md](components/atoms/radio.md) |
| Toggle | [components/atoms/toggle.md](components/atoms/toggle.md) |
| Tag / Chip | [components/atoms/tag.md](components/atoms/tag.md) |
| Tooltip | [components/atoms/tooltip.md](components/atoms/tooltip.md) |
| Spinner | [components/atoms/spinner.md](components/atoms/spinner.md) |
| Label | [components/atoms/label.md](components/atoms/label.md) |

### Molecules

| Component | File |
|---|---|
| Alert / Banner | [components/molecules/alert.md](components/molecules/alert.md) |
| Modal | [components/molecules/modal.md](components/molecules/modal.md) |
| Dropdown | [components/molecules/dropdown.md](components/molecules/dropdown.md) |
| Tabs | [components/molecules/tabs.md](components/molecules/tabs.md) |
| Table | [components/molecules/table.md](components/molecules/table.md) |
| Empty state | [components/molecules/empty-state.md](components/molecules/empty-state.md) |
| Form | [components/molecules/form.md](components/molecules/form.md) |
| Navigation | [components/molecules/navigation.md](components/molecules/navigation.md) |
| Breadcrumbs | [components/molecules/breadcrumbs.md](components/molecules/breadcrumbs.md) |

**Adding a new component doc:** create a new `.md` file in the appropriate folder (`foundations/`, `components/atoms/`, or `components/molecules/`) using the template at the bottom of this file, then add a row to the relevant table above.

---

## Reference files in this folder

These files live alongside this `DESIGN.md` in the project folder and provide depth that the rules above only summarize. Treat them as authoritative when in doubt.

### Foundation references

- [tokens.md](./tokens.md) — Color and typography token reference: semantic color vocabulary, every surface/border/text/icon token with its underlying palette, component-specific aliases (Banner, Label, Navigation, Toggle), Journeys workflow tokens, composite text tokens (headings, body, labels, links, code), and deprecations

### Per-component AI guidelines

- [banner.ai-guidelines.md](./banner.ai-guidelines.md) — Banner / Alert component
- [breadcrumbs.ai-guidelines.md](./breadcrumbs.ai-guidelines.md) — Breadcrumbs component
- [button.ai-guidelines.md](./button.ai-guidelines.md) — Button component
- [data-table.ai-guidelines.md](./data-table.ai-guidelines.md) — DataTable component
- [label.ai-guidelines.md](./label.ai-guidelines.md) — Label component
- [modal.ai-guidelines.md](./modal.ai-guidelines.md) — Modal component
- [surfaces.ai-guidelines.md](./surfaces.ai-guidelines.md) — Card, Box, Panel — composed surface recipes

### What's not yet documented at the AI-guideline level

The following Pluma primitives are referenced in the component index above but do not yet have a per-component AI guidelines file. Until those land, treat the reach-for rules in this `DESIGN.md` as the only available source:

- **Atoms:** Badge, Avatar, Input, Checkbox, Radio, Toggle, Tag, Tooltip, Spinner
- **Molecules:** Dropdown, Tabs, Empty state, Form, Navigation
- **Foundations:** Color tokens, Typography, Spacing, Iconography, Motion (referenced but not yet expanded with concrete values)

Adding per-component AI guidelines for these is the highest-leverage way to expand this file's coverage.

---

## Pluma MCP Server — Setup

> Canonical setup page: https://pluma.customer.io/overview/mcp-server. The summary below is included so this file remains usable as a standalone bundle; treat the Pluma page as the source of truth when the two disagree.

The Pluma MCP server (`@customerio/pluma-mcp`) exposes Pluma's design tokens, component specs, and usage guidelines directly to Claude. Once connected, you can ask questions and get accurate, up-to-date design system context without leaving your workflow.

### Prerequisites

- Node.js 18+ and `npx` available on your PATH
- A GitHub token with access to the `customerio` org (for package resolution)
- An `.npmrc` configured to authenticate against the Customer.io private npm registry

### Configuration

Add the following to your MCP host config file. For **Claude Desktop**, this is `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows). For **Claude Code**, add it to `.claude/settings.json` under `mcpServers`. For **Cursor**, see `~/.cursor/mcp.json`.

```json
{
  "mcpServers": {
    "pluma": {
      "command": "npx",
      "args": [
        "-y",
        "@customerio/pluma-mcp@latest"
      ],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}",
        "NPM_CONFIG_USERCONFIG": "/Users/YOUR_USERNAME/.npmrc"
      }
    }
  }
}
```

**Fill in before saving:**

- `GITHUB_TOKEN` — set this as an environment variable in your shell profile (e.g. `export GITHUB_TOKEN=ghp_...` in `~/.zshrc`), or replace `${GITHUB_TOKEN}` with the token string directly (not recommended for shared configs).
- `NPM_CONFIG_USERCONFIG` — replace `/Users/YOUR_USERNAME/.npmrc` with the absolute path to the `.npmrc` file that holds your private registry credentials.

### Verifying the connection

Once configured, restart Claude Desktop (or reload the Claude Code session). You should see `pluma` listed as a connected MCP server. Confirm it's working by asking Claude:

> "What Pluma components are available?"

### Using the Pluma MCP

Once connected, Claude can answer questions grounded in Pluma's actual component library. Useful prompts:

- *"What are the usage guidelines for the Pluma Button component?"*
- *"Show me the design tokens for spacing in Pluma."*
- *"What's the correct Pluma component to use for an inline notification?"*
- *"Does Pluma have a pattern for empty states?"*

The MCP server gives Claude direct access to Pluma's source of truth, so answers reflect the current state of the design system rather than Claude's general training knowledge.

---

## Component Doc Template

Use this structure when creating a new component file:

```markdown
# [Component Name]

> One-sentence description of what this component is and what it's for.

## Usage

When to use and when not to use this component.

## Variants

List and describe the available variants or sizes.

## Props / Tokens

| Name | Type | Default | Description |
|---|---|---|---|
| `variant` | string | `primary` | Visual style of the component |

## Accessibility

WCAG considerations, keyboard interactions, ARIA roles.

## Examples

Code snippets or links to Storybook.

## Related

Links to related components or Figma frames.
```

---

## Resources

- [Pluma design system](https://pluma.customer.io/) — canonical site (public-shareable). Foundations, Components, Patterns, Advanced, Fly Migration, Changelog.
- [Pluma MCP setup](https://pluma.customer.io/overview/mcp-server) — maintained MCP configuration for Cursor, Claude Code, and other hosts.
- [Pluma About](https://pluma.customer.io/overview/about) — system positioning, lifecycle, accessibility, history.
- [Pluma GitHub repo](https://github.com/customerio/pluma) — internal
- [Customer.io Design blog](https://medium.com/customer-io-design)
- DESIGN.md mission and project plan: see `Design.md project/DESIGN-md-mission.md` and the Linear project at https://linear.app/customerio/project/designmd-3d70a4786aa1


---

# Pluma Design Tokens

Source-of-truth reference for Pluma's semantic design tokens — color and typography. This file is the authoritative list. Use these token names in code, never inline hex values, never raw palette references, never hand-rolled font-size/weight/line-height combinations.

## How to use this file

- Reach for the **semantic token name** (e.g., `color-surface-critical-subtle`) — not the underlying palette
- The CSS-style token name maps to the JS access pattern by stripping the `color-` prefix:
  `color-surface-active` → `themeVars.color['surface-active']`
- Component-specific tokens (Banner, Label, Navigation, Toggle) are aliases pointing back to semantic tokens. They exist for component-internal use; in feature code, prefer the underlying semantic token.

## Semantic vocabulary

The vocabulary is shared across surface, border, text, and icon token families.

### Intent prefixes

| Prefix | Meaning | Underlying palette family |
|--------|---------|---------------------------|
| `base` | Default / neutral | `grey_charcoal` |
| `accent` | Brand accent, primary actions | `teal_spruce` |
| `caution` | Warning, attention needed | `yellow_mustard` |
| `critical` | Error, destructive | `red_flame` |
| `information` | Informational, neutral notice | `blue_wave` |
| `success` | Success, positive confirmation | `green_verdant` |
| `feature` | New feature, promotional | `purple_nova` |
| `invert` | Inverse contrast (dark on light surfaces, etc.) | `teal_spruce` (dark) |

### Weight modifiers

| Modifier | Meaning |
|----------|---------|
| (none) | Default weight |
| `-bold` | Stronger / heavier emphasis |
| `-subtle` | Lighter / quieter |
| `-minimal` | Lightest tint (often used for backgrounds behind text) |

### State modifiers

| Modifier | Meaning |
|----------|---------|
| `-hover` | Hover state |
| `-active` | Pressed / selected state |
| `-disabled` | Disabled state |
| `-focus` | Focus ring |

These compose: `color-surface-critical-subtle-hover` is the hover state of the lightest critical surface.

---

## Surface tokens

Use for backgrounds, fills, and container surfaces.

### Surface — base

| Token | Underlying |
|-------|------------|
| `color-surface-active` | `palette-grey_charcoal-050` |
| `color-surface-active-disabled` | `palette-grey_charcoal-050` |
| `color-surface-active-hover` | `palette-grey_charcoal-100` |
| `color-surface-base` | `palette-grey_charcoal-white` |
| `color-surface-base-disabled` | `palette-grey_charcoal-200` |
| `color-surface-base-hover` | `palette-grey_charcoal-015` |
| `color-surface-bold` | `palette-grey_charcoal-100` |
| `color-surface-bold-hover` | `palette-grey_charcoal-200` |
| `color-surface-invert` | `palette-teal_spruce-700` |
| `color-surface-invert-bold` | `palette-teal_spruce-800` |
| `color-surface-invert-bold-hover` | `palette-teal_spruce-700` |
| `color-surface-invert-hover` | `palette-teal_spruce-600` |
| `color-surface-invert-subtle` | `palette-grey_charcoal-500` |
| `color-surface-subtle` | `palette-grey_charcoal-025` |
| `color-surface-subtle-disabled` | `palette-grey_charcoal-025` |
| `color-surface-subtle-hover` | `palette-grey_charcoal-050` |

### Surface — accent

| Token | Underlying |
|-------|------------|
| `color-surface-accent` | `palette-teal_spruce-400` |
| `color-surface-accent-disabled` | `palette-grey_charcoal-200` |
| `color-surface-accent-hover` | `palette-teal_spruce-500` |
| `color-surface-accent-minimal` | `palette-teal_spruce-100` |
| `color-surface-accent-subtle` | `palette-teal_spruce-200` |

**Brand teal — visual reference.** `palette-teal_spruce-400` is Customer.io's brand accent: a **dark, spruce-leaning teal**, not a blue, not a bright turquoise. It is the color of every primary button in the live product. Approximate hex: `#1F4548` (confirm against Pluma's CSS variable definitions — `themeVars.color['surface-accent']` is the runtime source of truth). If a generated UI shows a sea-blue or sky-blue primary button, the token has been swapped for an inline hex — fix it back to `color-surface-accent`.

### Surface — caution

| Token | Underlying |
|-------|------------|
| `color-surface-caution` | `palette-yellow_mustard-500` |
| `color-surface-caution-bold` | `palette-yellow_mustard-600` |
| `color-surface-caution-disabled` | `palette-yellow_mustard-300` |
| `color-surface-caution-hover` | `palette-yellow_mustard-700` |
| `color-surface-caution-minimal` | `palette-yellow_mustard-100` |
| `color-surface-caution-subtle` | `palette-yellow_mustard-200` |
| `color-surface-caution-subtle-hover` | `palette-yellow_mustard-300` |

### Surface — critical

| Token | Underlying |
|-------|------------|
| `color-surface-critical` | `palette-red_flame-400` |
| `color-surface-critical-bold` | `palette-red_flame-700` |
| `color-surface-critical-disabled` | `palette-red_flame-200` |
| `color-surface-critical-hover` | `palette-red_flame-600` |
| `color-surface-critical-minimal` | `palette-red_flame-050` |
| `color-surface-critical-subtle` | `palette-red_flame-100` |
| `color-surface-critical-subtle-hover` | `palette-red_flame-200` |

### Surface — information

| Token | Underlying |
|-------|------------|
| `color-surface-information` | `palette-blue_wave-500` |
| `color-surface-information-bold` | `palette-blue_wave-700` |
| `color-surface-information-disabled` | `palette-blue_wave-200` |
| `color-surface-information-hover` | `palette-blue_wave-800` |
| `color-surface-information-minimal` | `palette-blue_wave-100` |
| `color-surface-information-subtle` | `palette-blue_wave-200` |
| `color-surface-information-subtle-hover` | `palette-blue_wave-300` |

### Surface — success

| Token | Underlying |
|-------|------------|
| `color-surface-success` | `palette-green_verdant-700` |
| `color-surface-success-bold` | `palette-green_verdant-800` |
| `color-surface-success-disabled` | `palette-green_verdant-100` |
| `color-surface-success-hover` | `palette-green_verdant-900` |
| `color-surface-success-minimal` | `palette-green_verdant-050` |
| `color-surface-success-subtle` | `palette-green-200` |
| `color-surface-success-subtle-hover` | `palette-green_verdant-300` |

---

## Border tokens

Use for component borders, dividers, and focus rings.

### Border — base

| Token | Underlying |
|-------|------------|
| `color-border-active` | `palette-grey_charcoal-400` |
| `color-border-active-hover` | `palette-grey_charcoal-500` |
| `color-border-base` | `palette-grey_charcoal-050` |
| `color-border-base-hover` | `palette-grey_charcoal-100` |
| `color-border-bold` | `palette-grey_charcoal-300` |
| `color-border-bold-hover` | `palette-grey_charcoal-400` |
| `color-border-focus` | `palette-blue_wave-400` |
| `color-border-invert` | `palette-grey_charcoal-white` |
| `color-border-minimal` | `palette-grey_charcoal-050` |
| `color-border-minimal-hover` | `palette-grey_charcoal-100` |

### Border — accent

| Token | Underlying |
|-------|------------|
| `color-border-accent` | `palette-teal_spruce-400` |
| `color-border-accent-disabled` | `palette-grey_charcoal-200` |
| `color-border-accent-hover` | `palette-teal_spruce-500` |
| `color-border-accent-minimal` | `palette-grey_charcoal-050` |
| `color-border-accent-subtle` | `palette-teal_spruce-700` |
| `color-border-accent-subtle-hover` | `palette-teal_spruce-800` |

### Border — caution

| Token | Underlying |
|-------|------------|
| `color-border-caution` | `palette-yellow-400` |
| `color-border-caution-disabled` | `palette-yellow-200` |
| `color-border-caution-hover` | `palette-yellow-500` |
| `color-border-caution-minimal` | `palette-yellow-100` |
| `color-border-caution-subtle` | `palette-yellow-200` |
| `color-border-caution-subtle-hover` | `palette-yellow-300` |

### Border — critical

| Token | Underlying |
|-------|------------|
| `color-border-critical` | `palette-red_flame-400` |
| `color-border-critical-disabled` | `palette-red_flame-200` |
| `color-border-critical-hover` | `palette-red_flame-600` |
| `color-border-critical-minimal` | `palette-red_flame-050` |
| `color-border-critical-subtle` | `palette-red_flame-200` |
| `color-border-critical-subtle-hover` | `palette-red_flame-400` |

### Border — dropdown

| Token | Underlying |
|-------|------------|
| `color-border-dropdown` | `palette-grey_charcoal-050` |

### Border — feature

| Token | Underlying |
|-------|------------|
| `color-border-feature-border-information` | `palette-purple_nova-600` |
| `color-border-feature-border-information-disabled` | `palette-purple_nova-300` |
| `color-border-feature-border-information-hover` | `palette-purple_nova-700` |
| `color-border-feature-border-information-minimal` | `palette-purple_nova-100` |
| `color-border-feature-border-information-subtle` | `palette-purple_nova-300` |
| `color-border-feature-border-information-subtle-hover` | `palette-purple_nova-400` |

### Border — information

| Token | Underlying |
|-------|------------|
| `color-border-information` | `palette-blue_wave-700` |
| `color-border-information-disabled` | `palette-blue_wave-300` |
| `color-border-information-hover` | `palette-blue_wave-800` |
| `color-border-information-minimal` | `palette-blue_wave-100` |
| `color-border-information-subtle` | `palette-blue_wave-200` |
| `color-border-information-subtle-hover` | `palette-blue_wave-300` |

### Border — success

| Token | Underlying |
|-------|------------|
| `color-border-success` | `palette-green_verdant-700` |
| `color-border-success-disabled` | `palette-green_verdant-300` |
| `color-border-success-hover` | `palette-green_verdant-800` |
| `color-border-success-minimal` | `palette-green_verdant-050` |
| `color-border-success-subtle` | `palette-green_verdant-200` |
| `color-border-success-subtle-hover` | `palette-green_verdant-300` |

---

## Text tokens

Use for typography color. Default to `color-text-base` for body copy.

### Text — base

| Token | Underlying |
|-------|------------|
| `color-text-active` | `palette-grey_charcoal-black` |
| `color-text-base` | `palette-grey_charcoal-800` |
| `color-text-base-disabled` | `palette-grey_charcoal-300` |
| `color-text-base-hover` | `palette-grey_charcoal-600` |
| `color-text-bold` | `palette-grey_charcoal-black` |
| `color-text-bold-hover` | `palette-grey_charcoal-800` |
| `color-text-code` | `palette-raspberry-700` |
| `color-text-invert` | `palette-grey_charcoal-white` |
| `color-text-minimal` | `palette-grey_charcoal-200` |
| `color-text-placeholder` | `palette-grey_charcoal-400` |
| `color-text-subtle` | `palette-grey_charcoal-500` |
| `color-text-white` | `palette-grey_charcoal-white` |

### Text — accent

| Token | Underlying |
|-------|------------|
| `color-text-accent` | `palette-teal_spruce-400` |
| `color-text-accent-bold` | `palette-teal_spruce-800` |
| `color-text-accent-disabled` | `palette-blue_wave-300` |
| `color-text-accent-hover` | `palette-teal_spruce-500` |
| `color-text-accent-invert-disabled` | `palette-grey_charcoal-white` |

### Text — caution

| Token | Underlying |
|-------|------------|
| `color-text-caution` | `palette-yellow-400` |
| `color-text-caution-bold` | `palette-yellow-500` |
| `color-text-caution-disabled` | `palette-yellow-100` |
| `color-text-caution-hover` | `palette-yellow-500` |
| `color-text-caution-subtle` | `palette-yellow-200` |

### Text — critical

| Token | Underlying |
|-------|------------|
| `color-text-critical` | `palette-red_flame-400` |
| `color-text-critical-bold` | `palette-red_flame-700` |
| `color-text-critical-disabled` | `palette-red_flame-200` |
| `color-text-critical-hover` | `palette-red_flame-600` |
| `color-text-critical-invert-disabled` | `palette-red_flame-100` |
| `color-text-critical-subtle` | `palette-red_flame-300` |

### Text — information

| Token | Underlying |
|-------|------------|
| `color-text-information` | `palette-blue-500` |
| `color-text-information-bold` | `palette-blue-800` |
| `color-text-information-disabled` | `palette-blue-200` |
| `color-text-information-hover` | `palette-blue-700` |
| `color-text-information-subtle` | `palette-blue-300` |

### Text — success

| Token | Underlying |
|-------|------------|
| `color-text-success` | `palette-green-600` |
| `color-text-success-bold` | `palette-green-800` |
| `color-text-success-disabled` | `palette-green-200` |
| `color-text-success-hover` | `palette-green-800` |
| `color-text-success-subtle` | `palette-green-300` |

---

## Background tokens

App-level page backgrounds.

| Token | Underlying |
|-------|------------|
| `color-background-default` | `palette-grey_charcoal-025` |

---

## Icon tokens

Use for icon fills. Defaults to `color-icon-base` unless a semantic state applies.

| Token | Underlying |
|-------|------------|
| `color-icon-accent` | `palette-teal_spruce-400` |
| `color-icon-accent-disabled` | `palette-grey_charcoal-300` |
| `color-icon-accent-hover` | `palette-teal_spruce-500` |
| `color-icon-accent-invert-disabled` | `palette-pink_blush-050` |
| `color-icon-base` | `palette-grey_charcoal-800` |
| `color-icon-base-disabled` | `palette-grey_charcoal-300` |
| `color-icon-base-hover` | `palette-grey_charcoal-900` |
| `color-icon-bold` | `palette-grey_charcoal-black` |
| `color-icon-button` | `palette-grey_charcoal-800` |
| `color-icon-caution` | `palette-yellow_mustard-600` |
| `color-icon-caution-disabled` | `palette-yellow_mustard-200` |
| `color-icon-critical` | `palette-red_flame-400` |
| `color-icon-critical-disabled` | `palette-red_flame-200` |
| `color-icon-critical-hover` | `palette-red_flame-600` |
| `color-icon-critical-invert-disabled` | `palette-red_flame-050` |
| `color-icon-feature` | `palette-purple_nova-500` |
| `color-icon-feature-disabled` | `palette-purple_nova-300` |
| `color-icon-information` | `palette-blue_wave-600` |
| `color-icon-information-disabled` | `palette-blue_wave-300` |
| `color-icon-invert` | `palette-grey_charcoal-white` |
| `color-icon-minimal` | `palette-grey_charcoal-050` |
| `color-icon-navigation` | `palette-grey_charcoal-600` |
| `color-icon-subtle` | `palette-grey_charcoal-400` |
| `color-icon-success` | `palette-green_verdant-600` |
| `color-icon-success-disabled` | `palette-green_verdant-100` |
| `color-icon-white` | `palette-grey_charcoal-white` |

---

## Component-specific tokens

These are aliases used internally by Pluma components. Prefer the underlying semantic tokens in feature code; these exist so component implementations can be tweaked without touching every consumer.

### Banner

| Token | Underlying |
|-------|------------|
| `color-banner-caution-background` | (alias to `color-banner-warning-background`, see Deprecations) |
| `color-banner-caution-border` | `palette-yellow_mustard-300` |
| `color-banner-default-background` | `palette-grey_charcoal-050` |
| `color-banner-default-border` | `palette-grey_charcoal-200` |
| `color-banner-error-background` | `palette-red_flame-050` |
| `color-banner-error-border` | `palette-red_flame-300` |
| `color-banner-feature-background` | `palette-purple_nova-050` |
| `color-banner-feature-border` | `palette-purple_nova-400` |
| `color-banner-information-background` | `palette-blue_wave-100` |
| `color-banner-information-border` | `palette-blue_wave-200` |
| `color-banner-success-background` | `palette-green_verdant-025` |
| `color-banner-success-border` | `palette-green_verdant-300` |
| `color-banner-warning-background` | `palette-yellow_mustard-100` (deprecated) |

### Label

Each label color comes in three roles: background, icon, text. Pair them consistently.

| Color | Background | Icon | Text |
|-------|-----------|------|------|
| Blue | `palette-blue_wave-100` | `palette-blue_wave-500` | `palette-blue_wave-700` |
| Clementine | `palette-orange_zest-050` | `palette-orange_zest-500` | `palette-orange_zest-700` |
| Green | `palette-green_verdant-025` | `palette-green_verdant-600` | `palette-green_verdant-800` |
| Grey | `palette-grey_charcoal-050` | `palette-grey_charcoal-500` | `palette-grey_charcoal-800` |
| Plum | `palette-plum-100` | `palette-plum-500` | `palette-plum-900` |
| Purple | `palette-purple_nova-100` | `palette-purple_nova-500` | `palette-purple_nova-700` |
| Raspberry | `palette-pink_blush-100` | `palette-pink_blush-500` | `palette-pink_blush-800` |
| Red | `palette-red_flame-050` | `palette-red_flame-500` | `palette-red_flame-700` |
| Teal | `palette-teal_spruce-025` | `palette-teal_spruce-400` | `palette-teal_spruce-600` |
| Yellow | `palette-yellow_mustard-100` | `palette-yellow_mustard-600` | `palette-yellow_mustard-800` |

Token name pattern: `color-label-{color}-{role}` (e.g., `color-label-blue-background`).

### Navigation

Navigation tokens are pure aliases pointing back to base semantic tokens.

| Token | Alias of |
|-------|----------|
| `color-navigation-background` | `color-surface-subtle` |
| `color-navigation-background-hover` | `color-surface-subtle-hover` |
| `color-navigation-background-selected` | `color-surface-base` |
| `color-navigation-icon` | `color-icon-subtle` |
| `color-navigation-icon-collapsed` | `color-icon-navigation` |
| `color-navigation-icon-selected` | `color-icon-accent` |
| `color-navigation-text` | `color-text-base` |
| `color-navigation-text-selected` | `color-text-active` |

### Toggle

| Token | Underlying |
|-------|------------|
| `color-toggle-surface-active` | `palette-teal_spruce-500` |
| `color-toggle-surface-active-disabled` | `palette-grey_charcoal-200` |
| `color-toggle-surface-active-hover` | `palette-teal_spruce-600` |
| `color-toggle-surface-base` | `palette-grey_charcoal-200` |
| `color-toggle-surface-base-disabled` | `palette-grey_charcoal-100` |
| `color-toggle-surface-base-hover` | `palette-grey_charcoal-300` |

---

## Workflow tokens (Journeys canvas)

Domain-specific tokens used for the Journeys canvas node categories. Treat these as a categorical palette — each color encodes node type.

| Token | Underlying | Meaning |
|-------|------------|---------|
| `color-surface-workflow-data` | `palette-pink_blush-400` | Data nodes (audience, attribute changes, segment updates) |
| `color-surface-workflow-delays` | `palette-orange_zest-500` | Delay and wait nodes |
| `color-surface-workflow-flowcontrol` | `palette-teal_spruce-300` | Flow-control nodes (split, exit, branching) |
| `color-surface-workflow-messages` | `palette-purple_nova-300` | Message-sending nodes (email, push, SMS, in-app) |

**Reach-for rule:** when generating Journeys canvas elements, map node type to workflow token. Do not invent new node colors. Outside the journey canvas, do not use workflow tokens.

---

## Typography tokens

Pluma uses **composite text tokens** — each token bundles family, size, weight, line-height, letter-spacing, and decoration into a single named style. Reach for the named style; do not compose properties individually.

The composite tokens reference primitive tokens (`font-size-xl`, `font-size-m`, `font-weight-semibold`, etc.). The primitives themselves are not yet exposed in this file — when in doubt, look at the composite's bundled values below.

### Headings

| Token | Family | Size | Weight | Line height |
|-------|--------|------|--------|-------------|
| `text-product-h1` | sans | `font-size-xl` | semibold | 24px |
| `text-product-h2` | sans | 18px (literal) | bold | 20px |
| `text-product-h3` | sans | `font-size-m` | semibold | 20px |
| `text-product-h4` | sans | `font-size-s` | semibold | 16px |

### Body / paragraph

| Token | Family | Size | Weight | Line height |
|-------|--------|------|--------|-------------|
| `text-product-p` | sans | `font-size-s` | regular | 20px |
| `text-product-p-active` | sans | `font-size-s` | semibold | 20px |
| `text-product-p-secondary` | sans | `font-size-s` | regular | 20px |
| `text-product-ps` | sans | `font-size-xs` | regular | 18px |
| `text-product-ps-active` | sans | `font-size-xs` | semibold | 18px |
| `text-product-pxs` | sans | `font-size-xxs` | regular | 18px |
| `text-product-pxs-active` | sans | `font-size-xxs` | semibold | 18px |
| `text-product-pxs-secondary` | sans | `font-size-xxs` | regular | 18px |

The `-active` suffix swaps weight to semibold for emphasis. The `-secondary` suffix is **identical** to its base in typography — its purpose is to signal pairing with a secondary color token (`color-text-subtle` or similar), not a font change.

### Labels

| Token | Family | Size | Weight | Line height | Letter spacing |
|-------|--------|------|--------|-------------|----------------|
| `text-product-label` | sans | `font-size-xxs` | medium | normal | 1% |
| `text-product-label-secondary` | sans | 12px (literal) | medium | 14px | 0% |

`text-product-label` is the only token with non-zero letter spacing.

### Links

| Token | Family | Size | Weight | Line height | Decoration |
|-------|--------|------|--------|-------------|------------|
| `text-product-link` | sans | `font-size-s` | medium | 20px | underline |
| `text-product-link-secondary` | sans | `font-size-s` | regular | 20px | underline |

Both link tokens use `text-decoration: underline`. The difference is weight (medium vs regular).

### Code

| Token | Family | Size | Weight | Line height |
|-------|--------|------|--------|-------------|
| `text-product-code` | mono | 0.9em (relative) | regular | inherit |

`text-product-code` is the only mono-family text token. Size and line-height are em-relative — they inherit from the surrounding context. Use it for inline code, IDs, attribute keys, and Liquid expressions.

### Typography reach-for rules

- **Page H1 / large heading:** `text-product-h1`
- **Section heading:** `text-product-h2` or `text-product-h3` depending on density
- **Micro heading / subhead:** `text-product-h4`
- **Body paragraph:** `text-product-p`
- **Emphasized body (inline):** `text-product-p-active`
- **Secondary body (muted):** `text-product-p-secondary` paired with `color-text-subtle`
- **Small body** (metadata, helper text): `text-product-ps`
- **Extra-small body** (timestamps, captions): `text-product-pxs`
- **Form labels, table headers:** `text-product-label`
- **Compact labels:** `text-product-label-secondary`
- **Inline links:** `text-product-link`
- **Lower-emphasis links:** `text-product-link-secondary`
- **Inline code, IDs, attribute keys, Liquid:** `text-product-code`

### Typography gotchas

- `text-product-h2` and `text-product-label-secondary` hardcode pixel values (18px and 12px / 14px) instead of using size tokens. Likely because no `font-size-l` exists between `m` and `xl`. Use the tokens as-is; don't try to "fix" them in feature code.
- `-secondary` variants don't change typography, only intent. Pair them with `color-text-subtle` (or another muted color) to make the muted appearance visible.
- Primitive size tokens (`font-size-xl`, `font-size-m`, `font-size-s`, `font-size-xs`, `font-size-xxs`) are referenced but not yet documented in this file. Based on the composite values they appear in, they likely map roughly to `20px / 16px / 14px / 13px / 12px` — confirm against Pluma's foundations before relying on these mappings.
- Primitive weight tokens (`font-weight-regular`, `font-weight-medium`, `font-weight-semibold`, `font-weight-bold`) are referenced but not yet documented in numeric terms.
- No display-size token (for large headline numbers, hero text). Use `text-product-h1` and increase line-height if needed.

---

## Deprecations and gotchas

- **`color-banner-warning-*` is deprecated.** Use `color-banner-caution-*` instead. The `caution` variant is the current semantic for warnings; `warning` exists only for backward compatibility.
- **Information vs blue tokens.** `color-text-information` references `palette-blue-*` while `color-surface-information` references `palette-blue_wave-*`. These are intentionally different shades — don't try to unify them.
- **Yellow vs yellow_mustard.** Same pattern — borders/text reference `palette-yellow-*`, surfaces and icons reference `palette-yellow_mustard-*`. Inconsistency is in the source; preserve it.
- **`color-text-accent-disabled` references `palette-blue_wave-300`**, not a teal shade — appears intentional to maintain disabled-state contrast.

---

## What's missing from this file

- **Dark theme tokens.** This file covers light theme only today. Dark mode is shipping soon — tokens will land directly in Pluma as part of the migration, as a parallel `palette-*` set surfaced through the same MCP and site interfaces. Watch the Pluma repo.
- **Hex values.** Tokens reference palette names (e.g., `palette-grey_charcoal-050`), not raw hex. The palette → hex mapping lives in Pluma's CSS variable definitions (`themeVars.color[...]` resolves at runtime).
- **Primitive typography tokens.** Composite text tokens are documented above; the primitives they reference (`font-size-xl`, `font-weight-semibold`, etc.) are not. Add the primitives when the source data is available so the mapping is concrete.
- **Spacing, radius, shadow tokens.** This file covers color and typography. Other token families are still TODO.
- **Data-viz / chart palettes.** Not yet documented. The workflow tokens above are categorical for Journeys; general chart palettes (categorical, sequential, diverging) are still TODO.

Adding the missing pieces is straightforward once the source data is available.


---

# Banner

## Usage

- Use Banner to communicate important, non-modal information about the current page or workflow.
- Use Banner for status changes, warnings, feature announcements, and system-level notices.
- Use Snackbar instead of Banner for non-disruptive confirmations of user actions that auto-dismiss.
- Use inline form error messages instead of Banner for field-level validation.
- Select the variant that matches the semantic meaning of the message. Never choose a variant for its color alone.
- Allow dismissal for informational, non-critical messages. Make the Banner persistent when the issue must be resolved.
- Limit to one primary action. Add a dismiss action only alongside that primary action.
- Enable only one dismissal method at a time: either a close button (`onClose` without `shouldShowDismissButton`) or a dismiss action button (`shouldShowDismissButton` with `onClose`).

## Types

- `type="inline"` (default) — expands to the full width of its parent container and sits within the page flow. Use for contextual messages tied to surrounding content.
- `type="alert"` — constrained width with an elevated shadow. Use for page-level notices.
- `type="announcement"` — full-width, no border radius, center-aligned. Use for application-wide messages such as maintenance notices or feature launches.

## Variants

- `variant="default"` (default) — neutral, general-purpose tips and guidance.
- `variant="information"` — helpful context and tips the user may be unaware of.
- `variant="success"` — confirms a positive outcome or completed process.
- `variant="caution"` — warns the user of an issue that may require attention.
- `variant="error"` — critical error or issue that must be addressed immediately.
- `variant="feature"` — promotes newly available features or system updates.

## Appearance

- Toggle outlined style (`isOutlined`) for a subtler treatment: white background with a colored border instead of a filled background.
- Override the default icon per variant with `icon` set to any Pluma icon name. Set `icon={null}` to remove the icon entirely.
- Default icons per variant: `default` = `tip`, `information` = `help`, `success` = `check-circle`, `caution` = `warning`, `error` = `error`, `feature` = `feature-launch`.

## Refinement

Banner's prop surface is intentionally narrow. The refinement levers from [DESIGN.md](./DESIGN.md#refinement-levers) map to Banner as recipes built from `type`, `isOutlined`, `icon`, action layout, and the surrounding container. No new props, no custom CSS.

### Padding scale

Banner's internal padding is set by `type`. Choose the type that matches the message weight rather than overriding padding.

- **Compact** — `type="inline"` paired with `isOutlined` and `icon={null}` for a single-line, low-emphasis note inside a card or list. Padding tightens visually because the border replaces the filled background.
- **Default** — `type="inline"` with the variant's default icon. The right answer for nearly every banner.
- **Relaxed** — `type="alert"` for page-level notices that need more presence. The elevated shadow and constrained width create air around the message.

**Ships solo:** matching `type` to message weight using one of the three recipes.
**Requires design review:** wrapping a Banner in a custom `Box` to add padding, or overriding `type` styles with sprinkle margins to fake a different density.

### Media placement (icon as media)

The icon is the only media a Banner carries. Four sanctioned recipes; no inline images.

- **Leading icon** (default) — the variant's default icon, sized 20px, vertically aligned with the title row.
- **Custom icon** — pass `icon="icon-name"` to swap the default. Use only when the default icon misrepresents the message (e.g., a `caution` Banner about a billing issue may swap `warning` for `credit-card`).
- **No icon** — `icon={null}`. Use for dense list contexts where the icon would compete with surrounding affordances, or when the Banner sits adjacent to its own visual anchor (e.g., a status indicator dot).
- **Announcement bleed** — `type="announcement"` renders the icon centered with the message. Reserve for application-wide notices; do not use for contextual messages.

**Ships solo:** any of the four recipes above using a Pluma icon name.
**Requires design review:** inline images inside the Banner, multiple icons in one Banner, or icon positions other than leading.

### Action layout

Banner allows one primary action plus one dismiss. The placement adapts to container width.

- **Inline trailing** (default for ≥576px containers) — action sits to the right of the message text.
- **Wrap below** (automatic for <576px containers) — action wraps onto its own row beneath the message. Controlled by the component, not configurable.
- **Action-only** — pass `action` with `icon={null}` and a single sentence of body copy for a high-emphasis nudge inside a card.

**Ships solo:** the default inline-or-wrap behavior; choosing whether to include an action at all.
**Requires design review:** more than one primary action, action placement above the message, or action wrapped in a custom container.

### Density and visual weight

Banner expresses density through `isOutlined` and `type` rather than spacing tokens.

- **Filled** (default) — variant's semantic surface color. Use when the message is the primary focus of its surface.
- **Outlined** (`isOutlined`) — white background with the variant's color as a border. Use when the Banner sits inside an already-colored surface (a card, a modal body) where a filled background would muddy the hierarchy.
- **Subtle** — `variant="default"` with `isOutlined`. The quietest treatment; reserve for tips and guidance the user can ignore.

**Ships solo:** matching filled vs. outlined to the surrounding surface contrast.
**Requires design review:** combining multiple visual weights in one stack of banners, or applying custom border weights.

### Edge treatment

How the Banner meets the surface around it.

- **Inset** (default for `inline` and `alert`) — the Banner has its own border radius and sits within the page flow.
- **Flush** (`type="announcement"`) — zero border radius, full container width. Use for system-wide notices that span the application chrome.
- **Bleed inside a card** — to place a Banner edge-to-edge inside a `Box` with padding, wrap it in `<Box marginX={negative-spacing-token}>` using the negative-margin tokens documented in tokens.md. Never use inline negative pixels.

**Ships solo:** `inline`, `alert`, or `announcement` per the type's intended use.
**Requires design review:** stacking multiple `announcement` Banners, or any composition that nests a Banner inside another Banner.

## Behaviors

- Providing `onClose` without `shouldShowDismissButton` renders an icon-only close button. Setting `shouldShowDismissButton` with `onClose` renders a text dismiss button instead.
- Disable the close button with `isCloseButtonDisabled`. Disable the dismiss button with `isDismissButtonDisabled`.
- Pass `action` to render a primary action button. Set `action.isLoading` to show a loading indicator. Set `action.autoLoading` (default `true`) to show loading automatically when `onClick` returns a promise.
- Pass `action.href` to render the action as a link; use `action.isExternal` for external URLs.
- Actions wrap below the content when the Banner container is narrower than 576px.

## Content

- Write titles in sentence case. Keep them to a few strong summary words.
- Use the description (children) to support the title with supplementary detail. Descriptions may be used without a title.
- Do not put all information in the title alone.
- Write action button labels as a single verb or verb phrase that clearly describes the action.
- Match tone to variant: direct and factual for error and caution, more conversational for feature and information.

## Implementation Notes

- `variant="warning"` is deprecated. Use `variant="caution"` instead.
- React passes description text as `children`. Ember yields a default block.
- Ember invocation: `<PlumaBanner @variant="information" @onClose={{this.close}}>Description text</PlumaBanner>`.
- The `dismissButtonText` prop overrides the default "Dismiss" label and also implicitly enables the dismiss button even without `shouldShowDismissButton`.
- `withResponsiveContainer` defaults to `true` for inline type when the close button is not shown. Override explicitly when needed.
- The component is polymorphic via `as` (defaults to `div`).
- `action.variant` defaults to `"subtle"`.


---

# Breadcrumbs

## Usage

- Use Breadcrumbs to display the user's current location within a page hierarchy.
- Add breadcrumbs on pages that are two or more levels deep.
- Place breadcrumbs on the left side of the top nav bar on nested pages.
- Represent the actual page hierarchy, not the user's session history or navigation path.

## Behaviors

- Render the last breadcrumb item as plain text (active/current page) by default. Set `shouldRenderLastElementAsLink` to render it as a link instead.
- Collapse middle items into an ellipsis dropdown automatically when items exceed the container width or the `maxItems` threshold (default `4`). The first and last items always remain visible.
- Disable individual breadcrumb items with `isDisabled` on the breadcrumb object.
- Truncate long breadcrumb text with `isTruncated` on the breadcrumb object. Truncation applies a max width of `space-3200`.
- Provide `href` on a breadcrumb object to render it as a link. Provide `onClick` without `href` to render it as a button.

## Content

- Use full page titles for breadcrumb text to clearly indicate location.
- Use sentence case, not title case.

## Implementation Notes

- Pass breadcrumb data as an array of objects to the `breadcrumbs` prop. Each object accepts `text` (required), `href`, `isDisabled`, `isTruncated`, `onClick`, `itemClassName`, `linkClassName`, and `contentClassName`.
- The component renders as a `nav` element by default. Override with `as`.
- The `maxItems` prop controls when collapsing begins. The ellipsis counts as one item toward this limit. The minimum visible count is `3` (first item, ellipsis, last item).
- The component extends Box and supports sprinkle props (spacing, layout utilities).
- Ember invocation: `<PlumaBreadcrumbs @breadcrumbs={{this.items}} @maxItems={{5}} />`. All props use the `@` prefix.


---

# Button

## Usage

- Use Buttons to perform actions on the current page (open a modal, submit a form). Use links/anchors for navigation, downloads, or external URLs.
- Provide `href` to render a Button as a link for CTA use cases — the component renders as an anchor automatically.
- Limit one Primary Button per page or workflow.
- Order actions by importance from right (most important) to left (least important).

## Types

- Regular — includes a label and optionally an icon. The default type.
- Icon-only (`isIconOnly`) — renders only the icon with reduced padding. Use in condensed layouts or for menus and dropdowns. Requires `icon` or `iconSrc`.
- Link (`href`) — renders as an anchor element. Use for calls-to-action that navigate.

## Variants

- `variant="primary"` — the most important action on the page. Limit to one per page.
- `variant="secondary"` — lesser important actions. Multiple allowed per page.
- `variant="subtle"` — the least important action (referred to as "Tertiary" in design language).

## Appearance

- `size="md"` — default size for most use cases.
- `size="sm"` — for smaller screens or less prominent actions.
- `icon` — renders a Pluma icon by name. Leading position by default.
- `iconPosition="trailing"` — places the icon after the label.
- `iconSrc` — renders a custom image icon by URL. Sizing matches Pluma icons.

## Behaviors

- Loading (`isLoading`) — displays a spinner overlay and blocks interaction.
- Auto-loading (`autoLoading`, default `true`) — automatically shows a spinner when `onClick` returns a promise.
- Disabled (`isDisabled`) — prevents interaction and applies lighter colors across all variants.
- Danger (`isDanger`) — applies critical color treatment for destructive actions. Works with all variants.
- Active (`isActive`) — applies the hover/active visual state programmatically.

## Content

- Use sentence case for multi-word labels.
- Start with an action verb. Prefer a single verb over longer phrases.
- Add an ellipsis when the action requires an additional step before execution (e.g., "Export...").
- Use common user questions as labels for AI agent interaction Buttons.

## Implementation Notes

- Tertiary naming mismatch — design docs refer to "Tertiary" but the prop value is `variant="subtle"`. The literal value `"tertiary"` also exists but maps to a different internal style. Use `variant="subtle"` for the Tertiary design intent.
- `unsafe_withSoftDeprecatedSecondaryVariant` — when `true`, the `secondary` variant renders with `tertiary` styling. Used during the layout refresh transition. Can also be set globally via `PlumaProvider` component config.
- `isError` is deprecated — use `isDanger` instead.
- `unsafe_isActive` is deprecated — use `isActive` instead.
- Content — React uses `children` for the label text. Ember uses `{{yield}}` (default block) via `<PlumaButton>Label</PlumaButton>`.
- Button renders as `<button type="button">` by default. When `href` is provided, it renders as a `PlainLink` anchor element instead.


---

# DataTable

## Usage

- Use DataTable to display, organize, compare, and act on tabular data with multiple fields.
- Use DataTable for simpler comparative lists too, but omit interactive features (search, filters, row selection) when they are unnecessary.
- Never use DataTable for page layout. Use Box or Grid instead.
- Add only the features the screen needs. Too many options overwhelm the people scanning the table.

## Types

- `Basic` — Non-interactive table for comparative display. Provide only `data`, `columns`, and `caption`.
- `Interactive` — Enable features like `withRowSelection`, `withSearch`, `withSorting`, `withPagination`, and `bulkActions` for user-driven data workflows.

## Appearance

### Layout

- `layout="fixed"` (default) — Columns divide equally regardless of content. Horizontal scrolling is disabled.
- `layout="auto"` — Columns adjust to content size. Horizontal scrolling is enabled. Use when column content must remain readable.
- Horizontal scrolling is only available with `layout="auto"`.

### Row Styles

- `isStriped` — Alternates row backgrounds to improve readability. Use for large datasets, especially with infinite scrolling.
- `isBorderless` — Removes row borders. Use for a cleaner presentation on smaller datasets.
- `shouldHighlightOnHover` — Highlights rows on hover. Automatically enabled when rows are selectable; set explicitly on non-selectable rows when needed.

### Column Width

- Set column widths via `meta.width` on column definitions. Accepts pixel values (`"200px"`), fractional units (`"2fr"`), and percentages.
- Constrain columns with `meta.minWidth` and `meta.maxWidth`.

## Behaviors

### States

- Loading state (`isLoading`) — Displays skeleton rows. Set `loadingState.rowCount` to control placeholder row count.
- Empty state (`withEmptyState`, default `true`) — Displays when `data` is empty. Configure title, description, icon, and action buttons via `emptyState`. Provide a custom component via `emptyStateComponent`.
- Selected state — Selected rows show a blue background and checked checkbox when `withRowSelection` is enabled.

### Row Selection

- Enable with `withRowSelection`. Accepts `true` for all rows or a function to conditionally enable per row.
- Control externally via `rowSelection` and `onRowSelectionChange`.
- Restrict to single selection with `enableMultiRowSelection={false}`.
- Disable selection for specific rows via `getRowCanSelect`.

### Row Expansion

- Enable with `withRowExpanding`. Adds an expander toggle column.
- Rows with sub-rows (a `children` array by default) expand to show nested rows. Override with `getSubRows`.
- Provide `expandedRowComponent` to render custom content on expansion instead of sub-rows.
- Control which rows can expand via `getRowCanExpand`.
- Pre-expand specific rows via `initiallyExpandedRows`.

### Row Actions

- Pass `rowActions` as an object or a function receiving the row. Renders a dropdown menu in the last cell of each row.
- `dropdownActions` accepts an array of action items or action groups.

### Bulk Actions

- Configure `bulkActions` when `withRowSelection` is enabled. Appears in the header when rows are selected.
- `promotedActions` render as buttons in the header. `dropdownActions` render inside a dropdown.

### Sorting

- Enable with `withSorting`. Sorts by clicking column headers or using the Sort dropdown in the header.
- Control externally via `sorting.value` and `sorting.onChange`. Set `sorting.isManual` for server-side sorting.
- Only single-column sorting is supported.

### Search

- Enable with `withSearch`. Renders a search input in the header. Filters across all columns by default.
- Control externally via `search.value`, `search.onSearch`, and `search.onSearchInput`.
- Set `search.isManual` for server-side search. Provide `search.globalFilterFn` for a custom client-side filter.
- Collapse the search input behind a button with `search.defaultIsExpanded={false}`.

### Filters

- Enable with `withFilters`. Pass a `filters` config object with filter definitions, active values, and change handlers.
- Filters render in the header and can be combined to narrow results.

### Pagination

- Enable with `withPagination`. Renders page controls below the table.
- Configure page size and behavior via `pagination`. Control externally by providing `pagination.page` and `pagination.onPageChange`.
- Set `pagination.isManual` for server-side pagination when `data` is already paginated.

### Infinite Scrolling

- Enable with `withVirtualizer`. Renders only visible rows for large datasets as an alternative to pagination.
- Wrap the table in a container with a fixed height to constrain scrolling.
- Pass `virtualizerOptions` to customize the virtualizer.

### Column Management

- Enable with `withColumnsSettings`. Renders a Columns button in the header for toggling column visibility.
- Set initial hidden columns via `defaultColumnVisibility` (e.g., `{ rainfall: false }`).
- Control column order with `defaultColumnOrder` or `columnOrder`/`onColumnOrderChange`.
- Control visibility externally with `columnVisibility`/`onColumnVisibilityChange`.

### Column Span

- Set `meta.getCellColSpan` on a column definition to span a cell across multiple columns conditionally.

### Saved Views

- Enable with `withSavedViews`. Configure views, creation, editing, and deletion via `savedViews`.

### Load New / Load More

- Use `loadNew` to show a row at the top when new data is available.
- Use `loadMore` to show a row at the bottom for appending more data.

## Content

- Provide a `caption` (required). Use a short, descriptive noun or noun phrase. Captions are hidden visually but required for assistive technology.
- Write column headers in sentence case. Use full words; avoid abbreviations.
- Provide a meaningful empty state with a title, description, and optional action when no data is available. Do not leave the table blank.

## Implementation Notes

- Keep the minimum number of columns visible by default. Allow people to show/hide extras via `withColumnsSettings`.
- `data` and `columns` are required. `columns` follow TanStack Table column definitions — use `createColumnHelper` from `@tanstack/table-core`.
- Row IDs default to the `id` property on each data row, then fall back to row index. Override with `getRowId`.
- Pass `children` to fully replace the default layout (Header + Table + Pagination) with custom composition.
- Ember invocation: `<PlumaDataTable @data={{this.data}} @columns={{this.columns}} @caption="Items" />`. Feature props use `@` prefix (e.g., `@withSearch={{true}}`).
- The component is polymorphic via `as` (defaults to `div`). It extends Box.
- `onSortingChange` at the top level is deprecated. Use `sorting.onChange` instead.


---

# Label

## Usage

- Use Labels to indicate status or categorize items with short descriptive text (e.g., "Active", "Draft").
- Use Badges, not Labels, to display numeric quantities (e.g., notification counts).
- Use Tags, not Labels, for interactive categorization that people can create, edit, or remove. Labels are read-only system indicators.

## Variants

- Base (default) — no status indicator dot. Use for categorical attributes that describe _what_ something is: types, tiers, roles, tags.
- Status (`showStatusIndicator`) — includes a colored dot. Use for operational status that describes _where_ something is in its lifecycle: online/offline, running/stopped, active/draft.

## Appearance

- `color` — available values: `red`, `raspberry`, `clementine`, `yellow`, `green`, `teal`, `blue`, `plum`, `purple`, `grey`, `outline`. Default is `grey`.
- Use color consistently across the application for the same or similar statuses. Never use color purely for decoration.
- Status indicator colors — only `red`, `yellow`, `green`, `blue`, `purple`, and `grey` support the status indicator dot.
- Semantic color mapping: red = critical/destructive, yellow = warning, green = success/functional, blue = info, grey = neutral.
- `bold` — defaults to `true`. Set `bold={false}` for less visually prominent Labels. Light text is not compatible with status indicators.

## Content

- Limit text to one or two words.
- Use sentence case.
- Accurately describe the category or status.
- Never use long phrases or full sentences.

## Implementation Notes

- `showStatusIndicator` restricts `color` to `red`, `yellow`, `green`, `blue`, `purple`, or `grey`. Other colors throw a runtime error.
- `showStatusIndicator` requires `bold` to remain `true` (the default). Setting `bold={false}` with `showStatusIndicator` throws a runtime error.
- Content — React uses `children` for label text. Ember uses `{{yield}}` (default block) via `<PlumaLabel>Text</PlumaLabel>`.
- Label extends `Text` — it renders as a `<span>` by default and can be changed via the `as` prop.


---

# Modal

## Usage

- Use Modal for tasks requiring the user's immediate attention: action confirmations, short forms, feature moments, and supplementary workflows.
- Never nest Modals. Rework the flow, move content inline, or use a Popover instead.
- Use Modal when people must confirm a task or provide input before continuing. Use Drawer for content with less contextual relation to the main page. Use Popover for small, inline supplementary content.
- Keep Modal content focused on a single task. Move multi-step workflows or lengthy content to a dedicated page.

## Common Patterns

- Confirmation -- hide the close button (`shouldShowCloseButton={false}`) and provide primary/secondary footer actions.
- Destructive confirmation -- hide the close button, require additional user input (e.g., a text field), and disable the primary action (`isDisabled`) until input requirements are met. Mark the primary action as `isDanger`.
- Form -- disable overlay click dismissal (`shouldCloseOnOverlayClick={false}`) to prevent accidental data loss. Place submit and cancel buttons in the footer.
- Save/Discard -- hide the close button and provide three footer actions: go back, discard (`isDanger`), and save (primary).
- Informational -- display read-only content with a single primary dismiss button in the footer.
- Feature moment -- use `ModalSplit` for a branded graphic panel alongside the body content. Hide the close button and include a single call-to-action.

## Types

- Standard -- single-column layout with optional header and footer, and scrollable body content. This is the default.
- Split -- two-column layout using `ModalSplit`. Place `ModalSplit` before `ModalBody` for a left panel, or after for a right panel.

## Appearance

- Size (`size`, default `"md"`) -- use `"md"` for most use cases. Use `"sm"` for shorter content and smaller viewports.
- Close button (`shouldShowCloseButton`, default `true`) -- hide it only when the Modal requires an explicit user decision. Always provide a cancel action in the footer when the close button is hidden.
- Inset (`ModalInset`) -- renders content edge-to-edge, ignoring the Modal's default padding. Use for full-width backgrounds, images, or dividers.

## Refinement

Modal exposes more layout nuance through composition than its prop surface suggests. The five refinement levers from the root [DESIGN.md](./DESIGN.md#refinement-levers) map to Modal as follows. Reach for these recipes rather than inventing new props or wrapping the component in custom CSS.

### Padding scale

Adjust the breathing room inside `ModalBody` without changing the Modal frame.

- **Compact** -- wrap body content in `<Box padding="space-100">`. Use for confirmations, single-question forms, and dense data summaries. Pairs with `size="sm"`.
- **Default** -- accept the Modal's built-in body padding. No wrapper needed. This is the right answer for nearly every form and feature moment.
- **Relaxed** -- wrap body content in `<Box padding="space-300">`. Use for feature moments with a single large illustration and minimal text, where the air carries the emphasis.

**Ships solo:** one step from default per surface.
**Requires design review:** mixing compact and relaxed within the same Modal, applying different padding to header vs. body vs. footer, or any pixel value outside the spacing scale.

### Media placement

Where an image, illustration, or icon-art sits relative to the body content. Four sanctioned slots; no others.

- **Top** -- place media inside `<ModalInset inheritPadding={false}>` immediately above `<ModalBody>`. The image bleeds to the Modal edges; the body's padding resumes below it. Use for feature moments and onboarding.
- **Leading** -- use `<ModalSplit>` placed before `<ModalBody>` in DOM order. Renders as a left panel that bleeds to the Modal edges. Use for branded feature moments where the visual carries weight equal to the body.
- **Trailing** -- use `<ModalSplit>` placed after `<ModalBody>` in DOM order. Renders as a right panel. Use sparingly; prefer leading unless the reading order demands the visual on the right.
- **Bleed** -- a top-slot image with no visual frame (no caption, no surrounding chrome) extending to the Modal's full width and the header's top edge. Compose by placing `<ModalInset inheritPadding={false}>` before any header content. Reserve for feature launches and celebratory moments.

**Ships solo:** any single sanctioned slot per Modal, one image per surface.
**Requires design review:** multiple images on one surface, media that overlaps title or footer, or media slotted somewhere other than the four above (mid-body, behind text, etc.).

### Panel proportion (split layouts only)

When `ModalSplit` is used, the relative weight of the panel and the body.

- **50/50** (default) -- balanced visual weight. Use when image and body share equal narrative load.
- **40/60** (narrative leads) -- smaller panel, wider body. Use when the body content (form, list, copy) is the primary focus and the image supports it.
- **60/40** (image leads) -- larger panel, narrower body. Use for hero-style feature moments where the image is the message.

Express the ratio via the `ModalSplit` width tokens documented in tokens.md. Never override with a `flex` value or inline width.

**Ships solo:** any of the three sanctioned ratios.
**Requires design review:** custom ratios, three-column layouts, or proportion changes that shift within the same flow.

### Density

The vertical rhythm inside `ModalBody`.

- **Compact** -- 4px field gap, 16px field-group gap, 32px row height for any inline lists. Use for data-heavy confirmations and review modals.
- **Default** -- 8px field gap, 24px field-group gap. Use for most forms.
- **Relaxed** -- 12px field gap, 32px field-group gap. Use for onboarding moments where the user is reading more than scanning.

Set density once at the Modal root by wrapping body content in a `<Stack gap={token}>` or by applying spacing tokens at the surface level. Do not set per-field gaps.

**Ships solo:** one step from default at the Modal level.
**Requires design review:** mixed density within a single Modal, or density set per-field.

### Edge treatment

How content meets the Modal boundary.

- **Inset** (default) -- standard body padding on all four edges.
- **Flush** -- zero padding on one edge, typically the top or sides, to let a full-width banner, divider, or media element meet the Modal frame. Compose by placing the flush element inside `<ModalInset inheritPadding={false}>` while keeping sibling body content padded.
- **Bleed** -- content extends to all four Modal edges. Reserve for full-image feature moments where the body is overlaid on the image with high-contrast text and a dimming layer (`color-surface-overlay-bold`). Bleed combined with shadows is forbidden -- the shadow no longer communicates elevation when the content extends to the frame.

**Ships solo:** `inset` or single-side `flush`.
**Requires design review:** full `bleed`, or any composition that hides the close button behind media.

### Header and footer refinement

Header and footer follow the same levers, with one constraint: the header's title row and the footer's button row are not customizable layouts. Padding around them is.

- **Title row** -- to add a media element next to the title (e.g., a brand mark or feature icon), pass it as a slot via the `titleAccessory` prop. Do not compose a custom header.
- **Footer** -- `ModalFooter` is fixed-layout: actions right-aligned, spaced. To override alignment, use the `groupVariant` prop on the underlying `ButtonGroup`. Never wrap the footer in a custom row.

**Ships solo:** title accessories, footer `groupVariant` from the sanctioned list.
**Requires design review:** any custom header layout, multi-row footers, or footer content that is not a Button.

## Behaviors

- Dismissal methods -- close button, action button, overlay click, and the Escape key. All are enabled by default.
- Non-dismissable shortcut (`isDismissable={false}`) -- sets `shouldShowCloseButton`, `shouldCloseOnOverlayClick`, and `shouldCloseOnEscapePress` to `false` in one prop. Individual props still override `isDismissable` when explicitly set.
- Overlay click dismissal (`shouldCloseOnOverlayClick`, default `true`) -- disable for form Modals to prevent accidental data loss.
- Escape key dismissal (`shouldCloseOnEscapePress`, default `true`) -- disable alongside the close button for Modals that require an explicit choice.
- Scrolling (`shouldScrollInViewport`, default `false`) -- by default the Modal body scrolls while the header and footer remain fixed. Set to `true` to scroll the entire Modal within the viewport.
- Animation (`animationTransitionDuration`) -- fade and downward slide. The duration is customizable.

## Content

- Write titles in sentence case. Use descriptive, action-oriented titles (e.g., "Delete campaign") -- never vague titles like "Are you sure?"
- Write footer action labels with specific verbs that match the Modal's purpose. Mirror the title's verb when possible.
- Never use generic labels like "OK" or "Submit" when a specific verb is available.
- Keep body content concise and focused on the task at hand.

## Implementation Notes

- Compose Modal with subcomponents: `ModalBody` for scrollable body content, `ModalFooter` for action buttons, `ModalSplit` for split-layout panels, and `ModalInset` for edge-to-edge content.
- `ModalFooter` wraps `ButtonGroup` with `groupVariant="spaced"` -- pass `Button` components directly as children.
- `ModalSplit` panel side is controlled by DOM order relative to `ModalBody`, not a prop.
- `ModalInset` accepts `inheritPadding` (default `true` when used inside `ModalBody`) to control whether it inherits the Modal's padding.
- Control visibility with `isOpen` and `onClose`. `onClose` receives `(event, reason)` where reason is `"close-button-press"`, overlay click, or escape press.
- Lifecycle callbacks: `onOpen` fires when opened, `onCloseComplete` fires after the close animation finishes.
- `initialFocusIndex` overrides which focusable element receives focus when the Modal opens (default is the first).
- `state` prop accepts all Modal props as a single object for use with a ModalsManager pattern.
- `title` prop renders the default header. Omit `title` to render no header.
- Ember invocation: `<PlumaModal @isOpen={{this.isOpen}} @onClose={{this.handleClose}} @title="Modal title">`. Subcomponents: `<PlumaModalBody>`, `<PlumaModalFooter>`, `<PlumaModalSplit>`, `<PlumaModalInset>`.
- Ember default block yields a context value: `<PlumaModal ... as |ctx|>`.
- Global defaults for `animationTransitionDuration` can be set via `PlumaProvider`'s `componentConfig.PlumaModal`.


---

# Surfaces (Card / Box / Panel)

Pluma does not ship a single `Card` primitive. Surfaces are composed from `Box` with a small, sanctioned set of tokens for padding, elevation, border, and media placement. This file documents the recipes that compose those surfaces, the refinement levers available, and the bounds that ship solo versus require design review.

## Usage

- Use a surface to group related content into a single visual unit: a metric card, a settings panel, a campaign summary, a feature tile.
- Compose from `Box` -- never a custom `div` with inline styles.
- Surfaces are containers, not components. The content inside (heading, body, metric, action) is what carries meaning; the surface is its frame.
- Use a surface when a group of content has its own boundary and elevation. Use a section divider with no background when content is part of the page flow.
- Never nest filled surfaces more than two levels deep. A card inside a card inside a card is a flag that the page hierarchy needs rethinking.

## Common patterns

- **Metric card** -- single number with label, optional sparkline. `padding="space-200"`, `surface="base"`, no elevation.
- **Summary card** -- title, body, optional action row. `padding="space-200"` or `space-300`, `surface="base"`, elevation `subtle`.
- **Feature tile** -- icon or illustration, title, body, CTA. Media in the top slot, body padded, action in a footer row.
- **Settings panel** -- form fields grouped under a heading. `padding="space-300"`, `surface="base"`, no elevation; sits inside a page-level frame.
- **Empty-state surface** -- centered icon, title, description, primary CTA. `padding="space-400"`, `surface="subtle"`, no elevation.
- **Callout surface** -- single short message with optional action. Use a Banner instead unless the layout requires a card frame.

## Types

Surfaces vary along three axes: **surface tone**, **elevation**, and **border**. The matrix below names the sanctioned combinations.

- `surface="base"` -- default card background (`color-surface-base`). Pairs with elevation `none` or `subtle`.
- `surface="subtle"` -- nested panel inside a base card (`color-surface-subtle`). Pairs with elevation `none` only.
- `surface="transparent"` -- no fill, border-only. Pairs with `border="default"` and elevation `none`.

Three elevation steps: `none`, `subtle`, `prominent`. Elevation `prominent` is reserved for overlays (popovers, dropdowns) -- not for in-flow cards.

## Variants and tokens

| Token group | Sanctioned values | Use |
|---|---|---|
| `padding` | `space-100`, `space-200`, `space-300`, `space-400` | Inner spacing of the surface |
| `surface` (background) | `base`, `subtle`, `transparent` | Surface tone |
| `border` | `none`, `default`, `subtle` | Surface edge |
| `borderRadius` | `radius-medium`, `radius-large` | Corner softness |
| `elevation` | `none`, `subtle`, `prominent` | Shadow depth |
| `gap` (when surface is a Stack) | `space-100` through `space-400` | Vertical rhythm between children |

Never inline a hex value, a pixel padding, or a `box-shadow` rule. If the value you want is not in the table above, the refinement has outgrown the surface vocabulary -- escalate per the [Refinement](#refinement) bounds.

## Refinement

The five refinement levers from [DESIGN.md](./DESIGN.md#refinement-levers) map to surfaces as composition recipes. Surfaces are the most flexible component family in Pluma; the bounds below keep that flexibility legible.

### Padding scale

The most-used lever. Pick once per surface and let it inherit.

- **Compact** -- `padding="space-100"`. Use for dense list items, metric tiles in a tight grid, and inline cards inside a `DataTable` cell. Best paired with `borderRadius="radius-medium"`.
- **Default** -- `padding="space-200"`. The right answer for most cards.
- **Relaxed** -- `padding="space-300"`. Use for feature tiles, settings panels, and any card where the body is a short paragraph the reader should slow down for.
- **Expansive** -- `padding="space-400"`. Reserve for empty-state surfaces and welcome cards.

Asymmetric padding (different values on different edges) is composed via sprinkle props: `paddingX="space-300" paddingY="space-200"`. Use only when media or a footer needs to bleed past the body padding -- never to fake an alternate scale.

**Ships solo:** one of the four sanctioned values, optionally asymmetric for a documented media or footer reason.
**Requires design review:** padding values outside the sanctioned scale, padding that changes mid-surface, or any inline pixel value.

### Media placement

The slot where an image, illustration, or icon sits inside the surface.

- **Top** -- media as the first child, full surface width, no surface padding above it. Compose by placing the media inside `<Box marginX={negative-padding-token} marginTop={negative-padding-token}>` to cancel the surface's padding on three sides. Body content padded below.
- **Leading** -- two-column layout via `Stack direction="horizontal" gap="space-200"`. Media in the leading column, body in the trailing. Use for list items and summary rows where the media is an icon or thumbnail.
- **Trailing** -- same as leading but with media in the trailing column. Use sparingly; prefer leading unless reading order demands otherwise.
- **Bleed** -- media extends to all four surface edges; body content overlays it with high-contrast text. Reserve for hero tiles and feature launches. Bleed combined with elevation `prominent` is forbidden -- the shadow stops communicating depth when the image reaches the frame.
- **Inset frame** -- media has its own padded frame inside the surface (e.g., an illustration with surface-tinted padding around it). Compose by wrapping the media in `<Box padding="space-100" surface="subtle" borderRadius="radius-medium">`.

**Ships solo:** any single sanctioned slot, one media element per surface, recipe composed from `Stack` and `Box`.
**Requires design review:** multiple media slots in one surface, media that overlaps text without an overlay, or media positioned outside the five slots above.

### Panel proportion (split surfaces)

When a surface uses a horizontal `Stack`, the relative weight of the two columns.

- **50/50** -- both columns weighted equally. Use when neither side dominates.
- **40/60** (leading lighter) -- media or label leading; content trailing. Use for summary rows.
- **60/40** (leading heavier) -- content leading; supporting media or metadata trailing. Use for cards with a CTA on the right.
- **Hug + grow** -- leading column hugs its content (e.g., a 40px icon), trailing column fills the rest. The most common pattern; compose via `Stack direction="horizontal" gap="space-200"` with the leading child sized to its content.

Express proportions via the `Stack` width tokens. Never set widths in pixels or percentages directly.

**Ships solo:** any of the four sanctioned proportions.
**Requires design review:** three-column layouts inside a single surface, or proportions that change responsively within the same component.

### Density

Vertical rhythm between children of a stacked surface.

- **Compact** -- `gap="space-100"` between children. Use for metric tiles and dense lists.
- **Default** -- `gap="space-200"`. The right answer for most cards.
- **Relaxed** -- `gap="space-300"`. Use for feature tiles and onboarding surfaces.

Set density at the surface root via the `Stack` `gap` prop. Do not set per-child margins.

**Ships solo:** one of the three sanctioned gaps per surface.
**Requires design review:** mixed density within a single surface, or per-child margin overrides.

### Edge treatment

How content meets the surface boundary.

- **Inset** (default) -- all children respect the surface padding.
- **Flush** -- one or more children extend to the surface edges. Compose by wrapping the flush child in `<Box marginX={negative-padding-token}>` using the negative-margin tokens. Use for dividers, full-width banners, and edge-to-edge media.
- **Bleed** -- the entire surface content meets the frame on all sides. Reserve for hero tiles with `surface="transparent"` or a full-bleed image.

**Ships solo:** `inset` or `flush` on a single child for a documented reason.
**Requires design review:** flush on multiple children, bleed combined with elevation `prominent`, or any composition that hides the surface border behind content.

### Elevation refinement

Elevation expresses where the surface sits in the page's depth hierarchy. It is not decoration.

- **None** -- surface sits in the page flow. The default for cards inside a section.
- **Subtle** -- surface lifts off the page slightly. Use when the surface is interactive (a clickable card, a hovered row) or when it groups content that needs to be visually separated from a busy background.
- **Prominent** -- reserved for overlay surfaces (popovers, dropdowns, tooltips). Do not use on in-flow cards; if a card needs this much depth, the layout has outgrown the surface vocabulary.

**Ships solo:** `none` or `subtle` based on the role of the surface.
**Requires design review:** elevation `prominent` on an in-flow surface, or multiple elevation levels stacked on top of each other.

## Content

- Titles in sentence case, action-oriented when the surface is interactive.
- Keep body content focused. A surface holding more than three paragraphs of body is usually a section, not a card -- move it inline.
- Pair the title with a short description; do not put all the meaning in the title.
- Action labels follow [Button](./button.ai-guidelines.md) rules: single verb, sentence case.

## Implementation notes

- Compose surfaces from `<Box>` and `<Stack>` -- never `<div>` with inline styles.
- The `as` prop renders the underlying element as `section`, `article`, `aside`, or `li` when the surface has semantic meaning. Default is `div`.
- Surfaces extend Box, so all sprinkle spacing props are available (`padding`, `paddingX`, `paddingY`, `margin`, `gap`, etc.). All values must come from the sanctioned spacing scale.
- For Ember invocation: `<PlumaBox @padding="space-200" @surface="base" @elevation="subtle">` -- subcomponents and prop names mirror React with the `@` prefix.
- Surfaces participate in the same color and elevation token system as the rest of Pluma. See [tokens.md](./tokens.md) for the full surface, border, and elevation token reference.

## Related

- [DESIGN.md - Refinement levers](./DESIGN.md#refinement-levers) -- the cross-cutting vocabulary
- [modal.ai-guidelines.md](./modal.ai-guidelines.md#refinement) -- Modal-specific recipes
- [banner.ai-guidelines.md](./banner.ai-guidelines.md#refinement) -- Banner-specific recipes
- [tokens.md](./tokens.md) -- spacing, surface, border, elevation, and color tokens
