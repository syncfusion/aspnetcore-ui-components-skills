# ComboBoxComponent API Reference

This file is a focused extraction of the official Syncfusion ASP.NET Core ComboBox API. It lists all public properties, methods, and events.  
Reference: https://help.syncfusion.com/cr/aspnetcore-js2/syncfusion.ej2.dropdowns.combobox.html

---

## Properties

- `actionFailureTemplate` (string)
  - Template to display when a remote data request fails. Default: `'Request failed'`.

- `allowCustom` (boolean)
  - Enables users to enter custom values not in the data source. Default: `true`.

- `allowFiltering` (boolean)
  - When set to `true`, displays a search box for filtering items. Default: `false`.

- `allowObjectBinding` (boolean)
  - Enables binding object values to the component. Default: `false`.

- `allowResize` (boolean)
  - Gets or sets whether the ComboBox popup can be resized by the user. Default: `false`.

- `autofill` (boolean)
  - Suggests the first matched item when searching. Default: `false`.

- `cssClass` (string)
  - Adds custom CSS classes to the root element. Default: `null`.

- `dataSource` (array or DataManager)
  - Accepts list items from local data or remote service. Can be array of strings/objects or DataManager. Default: `[]`.

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
    - `iconCss` — Icon class field
    - `groupBy` — Group field
    - `disabled` — Disabled status field
  - Default: `{ text: null, value: null }`

- `filterType` (FilterType)
  - Determines filter behavior: `'StartsWith'`, `'EndsWith'`, `'Contains'`. Default: `'StartsWith'`.

- `floatLabelType` (FloatLabelType)
  - Float label behavior: `'Never'`, `'Always'`, `'Auto'`. Default: `'Never'`.

- `footerTemplate` (string)
  - Template for dropdown footer. Default: `null`.

- `groupTemplate` (string)
  - Template for group headers. Default: `null`.

- `headerTemplate` (string)
  - Template for dropdown header. Default: `null`.

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

- `itemTemplate` (string)
  - Template for each list item. Default: `null`.

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

- `readonly` (boolean)
  - Disables user interactions when `true`. Default: `false`.

- `showClearButton` (boolean)
  - Shows/hides clear button. Default: `true`.

- `sortOrder` (SortOrder)
  - Sort order: `'None'`, `'Ascending'`, `'Descending'`. Default: `null`.

- `text` (string)
  - Display text of selected item. Default: `null`.

- `value` (number or string or boolean or object)
  - Value of selected item. Default: `null`.

- `width` (string or number)
  - Width of component. Default: `'100%'`.

- `zIndex` (number)
  - z-index of popup element. Default: `1000`.

---

## Methods

- `addItem(items, itemIndex?)`
  - Adds item(s) to the component.

- `clear()`
  - Clears the selected value.

- `destroy()`
  - Removes the component and detaches event handlers.

- `disableItem(item)`
  - Disables a specific item.

- `filter(dataSource, query?, fields?)`
  - Filters data using a query.

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

- `hideSpinner()`
  - Hides the loading spinner.

- `open()`
  - Opens the popup.

- `showPopup()`
  - Shows the popup.

- `showSpinner()`
  - Shows the loading spinner.

---

## Events

- `actionBegin` — Triggered before fetch request.
- `actionComplete` — Triggered after data fetch completes.
- `actionFailure` — Triggered when data fetch fails.
- `blur` — Triggered when component loses focus.
- `change` — Triggered when value changes.
- `created` — Triggered after component creation.
- `customValueSpecifier` — Triggered to confirm custom value.
- `destroyed` — Triggered after component destruction.
- `filtering` — Triggered during filtering.
- `focus` — Triggered when component receives focus.
- `open` — Triggered when popup opens.
- `select` — Triggered when item selected.
