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
