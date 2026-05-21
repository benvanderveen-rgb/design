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
