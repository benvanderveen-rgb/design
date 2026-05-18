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
