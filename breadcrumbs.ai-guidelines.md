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
