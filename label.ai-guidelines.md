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
