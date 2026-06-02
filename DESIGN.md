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
