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
