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
