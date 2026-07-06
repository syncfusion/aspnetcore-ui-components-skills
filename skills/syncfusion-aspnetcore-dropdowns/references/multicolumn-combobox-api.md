# MultiColumn ComboBox API Reference

This file is a focused extraction of the official Syncfusion ASP.NET Core MultiColumn ComboBox API.  
Reference: https://help.syncfusion.com/cr/aspnetcore-js2/syncfusion.ej2.multicolumncombobox.html

---

## Properties

- `actionFailureTemplate` (string)
  - Template to display when a remote data request fails. Default: `'Request failed'`.

- `allowFiltering` (boolean)
  - When set to `true`, displays a search/filter box. Default: `false`.

- `allowObjectBinding` (boolean)
  - Enables binding object values to the component. Default: `false`.

- `autofill` (boolean)
  - Suggests the first matched item when searching. Default: `false`.

- `cssClass` (string)
  - Adds custom CSS classes to the root element. Default: `null`.

- `dataSource` (array or DataManager)
  - Accepts list items from local data or remote service. Default: `[]`.

- `debounceDelay` (number)
  - Delay in milliseconds for filtering operations. Default: `300`.

- `enablePersistence` (boolean)
  - Persists component state between page reloads. Default: `false`.

- `enableRtl` (boolean)
  - Enables right-to-left rendering. Default: `false`.

- `enableVirtualization` (boolean)
  - Enables virtual scrolling for large datasets. Default: `false`.

- `enabled` (boolean)
  - Specifies whether the component is enabled. Default: `true`.

- `fields` (FieldSettingsModel)
  - Maps data columns to component. Sub-properties:
    - `text` — Display field
    - `value` — Value field
  - Default: `{ text: null, value: null }`

- `filterBarPlaceholder` (string)
  - Placeholder text for filter bar. Default: `'Search'`.

- `filterType` (FilterType)
  - Filter behavior: `'StartsWith'`, `'EndsWith'`, `'Contains'`. Default: `'StartsWith'`.

- `floatLabelType` (FloatLabelType)
  - Float label behavior: `'Never'`, `'Always'`, `'Auto'`. Default: `'Never'`.

- `footerTemplate` (string)
  - Template for dropdown footer. Default: `null`.

- `groupTemplate` (string)
  - Template for group headers. Default: `null`.

- `headerTemplate` (string)
  - Template for dropdown header. Default: `null`.

- `height` (string or number)
  - Height of popup grid. Default: `'300px'`.

- `htmlAttributes` (object)
  - Additional HTML attributes as key-value pairs. Default: `{}`.

- `ignoreAccent` (boolean)
  - Ignores diacritical characters during filtering. Default: `false`.

- `ignoreCase` (boolean)
  - Case-insensitive filtering. Default: `true`.

- `index` (number)
  - Index of selected item. Default: `null`.

- `isDeviceFullScreen` (boolean)
  - Enables fullscreen popup on mobile when filtering. Default: `true`.

- `locale` (string)
  - Localization culture. Default: `'en-US'`.

- `noRecordsTemplate` (string)
  - Template when no data is available. Default: `'No records found'`.

- `placeholder` (string)
  - Input placeholder text. Default: `null`.

- `popupHeight` (string or number)
  - Height of popup list. Default: `'300px'`.

- `popupWidth` (string or number)
  - Width of popup list. Default: `'100%'`.

- `query` (Query)
  - External query to execute with data processing. Default: `null`.

- `readonly` (boolean)
  - Disables user interactions when `true`. Default: `false`.

- `showClearButton` (boolean)
  - Shows/hides clear button. Default: `true`.

- `sortOrder` (SortOrder)
  - Sort order: `'None'`, `'Ascending'`, `'Descending'`. Default: `null`.

- `text` (string)
  - Display text of selected item. Default: `null`.

- `value` (number or string)
  - Value of selected item. Default: `null`.

- `width` (string or number)
  - Width of component. Default: `'100%'`.

- `zIndex` (number)
  - z-index of popup element. Default: `1000`.

---

## Methods

- `actionComplete(args)`
  - Method to handle action complete event.

- `addItem(items, itemIndex?)`
  - Adds item(s) to the component.

- `clear()`
  - Clears the selected value.

- `destroy()`
  - Removes the component and detaches event handlers.

- `disableItem(item)`
  - Disables a specific item.

- `focusIn()`
  - Sets focus to the component.

- `focusOut()`
  - Removes focus from the component.

- `getDataByValue(value)`
  - Gets the data object matching the given value.

- `getItems()`
  - Returns all list items.

- `hidePopup()`
  - Hides the popup.

- `open()`
  - Opens the popup.

- `showPopup()`
  - Shows the popup.

---

## Events

- `actionBegin` — Triggered before fetch request.
- `actionComplete` — Triggered after data fetch completes.
- `actionFailure` — Triggered when data fetch fails.
- `blur` — Triggered when component loses focus.
- `change` — Triggered when value changes.
- `created` — Triggered after component creation.
- `destroyed` — Triggered after component destruction.
- `filtering` — Triggered during filtering.
- `focus` — Triggered when component receives focus.
- `open` — Triggered when popup opens.
- `select` — Triggered when item selected.

---

## Column Properties

Each `<e-column>` supports:

- `field` (string) — Maps to data property
- `header` (string) — Column header text
- `width` (number or string) — Column width
- `textAlign` (string) — Text alignment: `'Left'`, `'Right'`, `'Center'`
- `format` (string) — Format string for display
- `visible` (boolean) — Column visibility
- `allowSorting` (boolean) — Enable sorting on column
- `allowSearching` (boolean) — Include in search
