# ListBoxComponent API Reference

This file is a focused extraction of the official Syncfusion ASP.NET Core ListBox API. It lists all public properties, methods, and events.  
Reference: https://help.syncfusion.com/cr/aspnetcore-js2/syncfusion.ej2.dropdowns.listbox.html

---

## Properties

- `actionFailureTemplate` (string)
  - Template to display when a remote data request fails. Default: `'Request failed'`.

- `allowDragAndDrop` (boolean)
  - Enables drag and drop functionality between ListBox components. Default: `false`.

- `allowFiltering` (boolean)
  - When set to `true`, displays a search/filter box. Default: `false`.

- `cssClass` (string)
  - Adds custom CSS classes to the root element. Default: `null`.

- `dataSource` (array or DataManager)
  - Accepts list items from local data or remote service. Default: `[]`.

- `disabled` (boolean)
  - Disables the entire ListBox component. Default: `false`.

- `disabledFields` (FieldSettingsModel)
  - Fields configuration for mapping disabled items. Default: `null`.

- `enableRtl` (boolean)
  - Enables right-to-left rendering. Default: `false`.

- `enabled` (boolean)
  - Specifies whether the component is enabled. Default: `true`.

- `enableVirtualization` (boolean)
  - Enables virtual scrolling for large datasets. Default: `false`.

- `fields` (FieldSettingsModel)
  - Maps data columns to component. Sub-properties:
    - `text` — Display field
    - `value` — Value field
    - `iconCss` — Icon class field
    - `groupBy` — Group field
    - `disabled` — Disabled status field
  - Default: `{ text: null, value: null }`

- `filterBarPlaceholder` (string)
  - Placeholder text for filter bar. Default: `'Search'`.

- `floatLabelType` (FloatLabelType)
  - Float label behavior: `'Never'`, `'Always'`, `'Auto'`. Default: `'Never'`.

- `headerTemplate` (string)
  - Template for component header. Default: `null`.

- `height` (string or number)
  - Height of the ListBox. Default: `'300px'`.

- `htmlAttributes` (object)
  - Additional HTML attributes as key-value pairs. Default: `{}`.

- `itemTemplate` (string)
  - Template for each list item. Default: `null`.

- `locale` (string)
  - Localization culture. Default: `'en-US'`.

- `noRecordsTemplate` (string)
  - Template when no data is available. Default: `'No records found'`.

- `query` (Query)
  - External query to execute with data processing. Default: `null`.

- `scope` (string)
  - CSS class for scope of ListBox interaction. Default: `null`.

- `selectionSettings` (SelectionSettingsModel)
  - Selection mode configuration:
    - `mode` — `'Single'` or `'Multiple'`
    - `type` — `'Checkbox'` or `'Default'`
  - Default: `{ mode: 'Single' }`

- `sortOrder` (SortOrder)
  - Sort order: `'None'`, `'Ascending'`, `'Descending'`. Default: `'None'`.

- `value` (array or string)
  - Value(s) of selected item(s). Default: `null`.

- `width` (string or number)
  - Width of component. Default: `'100%'`.

---

## Methods

- `addItems(items, itemIndex?)`
  - Adds item(s) to the ListBox.

- `clear()`
  - Clears all selections.

- `destroy()`
  - Removes the component and detaches event handlers.

- `disableItem(item)`
  - Disables a specific item.

- `enableItem(item)`
  - Enables a specific item.

- `focusIn()`
  - Sets focus to the component.

- `focusOut()`
  - Removes focus from the component.

- `getItems()`
  - Returns all list items.

- `getSelectedItems()`
  - Returns selected item(s) with their data.

- `moveAllTo(targetListBox)`
  - Moves all items to another ListBox (for dual list setup).

- `moveDown()`
  - Moves selected item down in the list.

- `moveUp()`
  - Moves selected item up in the list.

- `moveTo(targetListBox)`
  - Moves selected item(s) to another ListBox.

- `removeItem(item)`
  - Removes a specific item from the list.

- `selectAll()`
  - Selects all items in the ListBox.

---

## Events

- `actionBegin` — Triggered before fetch request.
- `actionComplete` — Triggered after data fetch completes.
- `actionFailure` — Triggered when data fetch fails.
- `blur` — Triggered when component loses focus.
- `change` — Triggered when selection changes.
- `created` — Triggered after component creation.
- `destroyed` — Triggered after component destruction.
- `dblClick` — Triggered on double-click of an item.
- `drag` — Triggered during item drag.
- `dragStart` — Triggered when drag starts.
- `dragStop` — Triggered when drag stops.
- `drop` — Triggered on item drop.
- `focus` — Triggered when component receives focus.
- `remove` — Triggered when item is removed.
- `select` — Triggered when item selected.
