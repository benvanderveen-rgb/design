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
