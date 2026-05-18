# DESIGN.md — Customer.io Product Design System

Single source of truth for AI agents and humans building Customer.io product UI. Read this before generating any screen, component, or copy.

**For AI agents (Cursor, Claude, Stitch):** the rules sections below define tone, voice, components, states, density, and anti-patterns.
**For engineers:** the component documentation index, Pluma MCP setup, and doc template at the bottom.
**For designers and PMs:** the canonical reference for how Customer.io UI looks, behaves, and sounds.

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
- Strip features when not needed — don't add search, filters, or row selection unless users require them
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

The files below document individual Pluma components in depth. The rules above are cross-cutting guidance; per-component API details, accessibility, and code examples live in these files. Reach for the relevant doc when this file's reach-for rules aren't specific enough.

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

The Pluma MCP server (`@customerio/pluma-mcp`) exposes Pluma's design tokens, component specs, and usage guidelines directly to Claude. Once connected, you can ask questions and get accurate, up-to-date design system context without leaving your workflow.

### Prerequisites

- Node.js 18+ and `npx` available on your PATH
- A GitHub token with access to the `customerio` org (for package resolution)
- An `.npmrc` configured to authenticate against the Customer.io private npm registry

### Configuration

Add the following to your MCP host config file. For **Claude Desktop**, this is `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows). For **Claude Code**, add it to `.claude/settings.json` under `mcpServers`.

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

- [Pluma on Figma](#) — internal link, update with the Figma file URL
- [Pluma Storybook](#) — internal link, update with the Storybook URL
- [Pluma GitHub repo](https://github.com/customerio/pluma) — internal
- [Customer.io Design blog](https://medium.com/customer-io-design)
- DESIGN.md mission and project plan: see `Design.md project/DESIGN-md-mission.md` and the Linear project at https://linear.app/customerio/project/designmd-3d70a4786aa1
