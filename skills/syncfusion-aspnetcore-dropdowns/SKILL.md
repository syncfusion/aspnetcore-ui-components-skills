---
name: syncfusion-aspnetcore-dropdowns
description: Comprehensive guide for implementing Syncfusion aspnetcore dropdown components including DropDownList, MultiSelect. Single and multiple selection control with data binding, filtering, grouping, templates, and accessibility features. Essential for building professional dropdown interfaces with server-side data binding.
metadata:
  author: "Syncfusion Inc"
  category: "Dropdowns"
  version: "33.1.44"
---

# Implementing DropDownList in ASP.NET Core

The DropDownList (`<ejs-dropdownlist>`) is a single-selection control that displays a list of predefined values. It supports local and remote data binding, real-time filtering, grouping, custom templates, virtual scrolling, popup resize, localization, and full WCAG 2.2 AA accessibility.

## Component Overview

**Tag Helper:** `<ejs-dropdownlist>`  
**Namespace:** `Syncfusion.EJ2.DropDowns`  
**NuGet:** `Syncfusion.EJ2.AspNet.Core`

**Key Features:**
- Data binding from local arrays and remote sources (OData V4, Web API, URL Adaptor)
- Value binding for primitive types and complex objects (`allowObjectBinding`)
- Real-time filtering with search box (`allowFiltering`, `filterType`, `debounceDelay`)
- Grouping items using `fields.groupBy`
- Disabled items using `fields.disabled`
- Customizable templates: `itemTemplate`, `valueTemplate`, `headerTemplate`, `footerTemplate`, `groupTemplate`, `noRecordsTemplate`, `actionFailureTemplate`
- Virtual scrolling for large datasets (`enableVirtualization`)
- Popup resize by user (`allowResize`)
- WCAG 2.2 AA compliance, keyboard navigation, ARIA support
- RTL rendering (`enableRtl`)
- Localization (`locale`)
- Float label support (`floatLabelType`)
- State persistence (`enablePersistence`)
- Clear button (`showClearButton`)

## API Reference

📄 **Full API:** [references/api.md](references/dropdownlist-api.md)
- All properties with types, defaults, and descriptions
- All events
- Field settings (`<e-dropdownlist-fields>`)
- Tag helper syntax examples for all features

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/dropdownlist-getting-started.md)
- NuGet package installation
- TagHelper registration (`_ViewImports.cshtml`)
- **⚠️ CDN vs. Local Hosting:** See Security & Trust Boundary below for SRI and local hosting alternatives
- Script Manager registration
- Basic DropDownList tag helper usage
- Popup height/width configuration
- Change event handling

### Data Binding and Value Binding
📄 **Read:** [references/data-binding.md](references/dropdownlist-data-binding.md)
- Binding array of primitive data (strings, numbers)
- Binding complex JSON objects with `<e-dropdownlist-fields>`
- Nested/complex data with dot-notation field paths
- Value binding for primitive types (pre-selecting)
- Object binding with `allowObjectBinding`
- Remote data: OData V4, OData, Web API, URL Adaptor
- Offline mode (`DataManager offline="true"`)
- `actionBegin`, `actionComplete`, `actionFailure` events

### Filtering and Grouping
📄 **Read:** [references/filtering-grouping.md](references/dropdownlist-filtering-grouping.md)
- Enable filtering with `allowFiltering`
- Filter types: `StartsWith`, `Contains`, `EndsWith`
- Minimum character limit before remote request
- Case-sensitive filtering with `ignoreCase`
- Diacritics filtering with `ignoreAccent`
- Debounce delay with `debounceDelay`
- Filter bar placeholder with `filterBarPlaceholder`
- Grouping with `fields.groupBy`
- Custom group header with `groupTemplate`
- Disabled items with `fields.disabled`
- Sort order: `None`, `Ascending`, `Descending`

### Templates and Styling
📄 **Read:** [references/templates-styling.md](references/dropdownlist-templates-styling.md)
- `itemTemplate` — customize popup list items
- `valueTemplate` — customize selected value in input
- `headerTemplate` — static header in popup
- `footerTemplate` — static footer in popup
- `groupTemplate` — custom group headers
- `noRecordsTemplate` — no data state
- `actionFailureTemplate` — remote fetch failure state
- `floatLabelType` — float label behavior
- `cssClass` — custom CSS class
- CSS structures for appearance customization

### Accessibility and Localization
📄 **Read:** [references/accessibility-features.md](references/dropdownlist-accessibility-features.md)
- WCAG 2.2 Level AA compliance
- WAI-ARIA attributes (`aria-haspopup`, `aria-expanded`, `aria-selected`, etc.)
- Full keyboard navigation shortcut reference
- RTL support with `enableRtl`
- Localization with `locale` and `L10n.load()`
- Readonly vs disabled states
- `enablePersistence` for state persistence
- `isDeviceFullScreen` for mobile popup behavior

### Advanced Scenarios
📄 **Read:** [references/advanced-scenarios.md](references/dropdownlist-advanced-scenarios.md)
- Virtual scrolling with `enableVirtualization` (local and remote)
- Virtualization with grouping and filtering
- Customizing item count in virtualization
- Popup resize with `allowResize`, `resizeStart`, `resizeStop`, `resizing` events
- Cascading dropdowns (parent-child dependent selection)
- Remote data with filtering
- Pre-selecting values (by `value`, `text`, or `index`)
- `showClearButton` usage
- `htmlAttributes` for extra HTML attributes
- All events reference with usage examples

## Quick Start Example

**Controller (`HomeController.cs`):**
```csharp
public IActionResult Index()
{
    ViewBag.data = new List<object>
    {
        new { Id = 1, Name = "Electronics" },
        new { Id = 2, Name = "Furniture" },
        new { Id = 3, Name = "Clothing" }
    };
    return View();
}
```

**View (`Index.cshtml`):**
```cshtml
<ejs-dropdownlist id="category"
    dataSource="@ViewBag.data"
    placeholder="Select category"
    change="onCategoryChange">
    <e-dropdownlist-fields text="Name" value="Id"></e-dropdownlist-fields>
</ejs-dropdownlist>

<script>
function onCategoryChange(args) {
    console.log('Selected ID:', args.value);
}
</script>
```

## Common Patterns

### Pattern 1: Basic Selection with Event Handling

```cshtml
<ejs-dropdownlist id="category"
    dataSource="@ViewBag.Categories"
    placeholder="Select category"
    change="onCategoryChange">
    <e-dropdownlist-fields text="Name" value="Id"></e-dropdownlist-fields>
</ejs-dropdownlist>

<script>
function onCategoryChange(args) {
    console.log('Selected ID:', args.value);
}
</script>
```

### Pattern 2: Cascading Dropdowns (Secure Server-Side Proxy)

**⚠️ Security Note:** Avoid client-side direct fetch to untrusted external endpoints. Use a server-side proxy that validates, authenticates, and sanitizes responses.

**Controller (Server-Side Proxy):**
```csharp
public IActionResult Index()
{
    ViewBag.Countries = new List<Country>
    {
        new Country { Id = 1, Name = "USA" },
        new Country { Id = 2, Name = "Canada" }
    };
    return View();
}

// ✅ SECURE: Server-side proxy validates countryId and returns only whitelisted data
[HttpGet("api/cities/{countryId}")]
public IActionResult GetCities(int countryId)
{
    // Validate countryId is within acceptable range
    if (countryId <= 0 || countryId > 100)
        return BadRequest("Invalid country ID");
    
    var cities = new List<City>
    {
        new City { Id = 1, Name = "New York",  CountryId = 1 },
        new City { Id = 2, Name = "Toronto",   CountryId = 2 }
    };
    
    // Return only safe, schema-validated data
    return Json(cities.Where(c => c.CountryId == countryId)
        .Select(c => new { c.Id, c.Name })
        .ToList());
}
```

**View (Secure Client):**
```cshtml
<ejs-dropdownlist id="Country"
    dataSource="@ViewBag.Countries"
    change="onCountryChange"
    placeholder="Select country">
    <e-dropdownlist-fields text="Name" value="Id"></e-dropdownlist-fields>
</ejs-dropdownlist>

<ejs-dropdownlist id="City" placeholder="Select city">
    <e-dropdownlist-fields text="Name" value="Id"></e-dropdownlist-fields>
</ejs-dropdownlist>

<script>
function onCountryChange(args) {
    var cityDDL = document.getElementById('City').ej2_instances[0];
    if (args.value) {
        // ✅ SECURE: Fetch only from same-origin API endpoint (same server)
        fetch("/api/cities/" + encodeURIComponent(args.value), {
            method: 'GET',
            headers: { 'Content-Type': 'application/json' },
            credentials: 'same-origin'  // Include CSRF tokens via cookies
        })
        .then(response => {
            if (!response.ok) throw new Error('Server error: ' + response.status);
            return response.json();
        })
        .then(data => {
            // Validate response is an array before binding
            if (Array.isArray(data)) {
                cityDDL.dataSource = data;
                cityDDL.value = null;
            } else {
                console.error('Invalid data format received');
                cityDDL.dataSource = [];
            }
        })
        .catch(error => {
            console.error('Failed to load cities:', error);
            cityDDL.dataSource = [];
        });
    } else {
        cityDDL.dataSource = [];
    }
}
</script>
```

### Pattern 3: Remote Data with Filtering (Secure Same-Origin Proxy)

**⚠️ Security Note:** Avoid `crossDomain="true"` and direct binding to external OData/WebAPI endpoints. Always proxy through your own server to control validation and access.

```cshtml
<!-- ✅ SECURE: Remove crossDomain, use same-origin server proxy -->
<ejs-dropdownlist id="customers"
    allowFiltering="true"
    filtering="onfiltering"
    query="new ej.data.Query().from('api/customers').select(['ContactName', 'CustomerID']).take(6)"
    placeholder="Select a customer"
    popupHeight="200px">
    <e-dropdownlist-fields text="ContactName" value="CustomerID"></e-dropdownlist-fields>
    <e-data-manager url="/api/customers"
                    adaptor="UrlAdaptor">  <!-- UrlAdaptor for same-origin JSON endpoint -->
    </e-data-manager>
</ejs-dropdownlist>

<script>
function onfiltering(e) {
    var ddl = document.getElementById("customers").ej2_instances[0];
    var query = new ej.data.Query().from('api/customers').select(['ContactName', 'CustomerID']).take(6);
    query = e.text !== '' ? query.where('ContactName', 'startswith', e.text, true) : query;
    e.updateData(ddl.dataSource, query);
}
</script>
```

**Controller (Server-Side Proxy):**
```csharp
// ✅ SECURE: Server validates, filters, and returns safe subset of data
[HttpGet("api/customers")]
public IActionResult GetCustomers([FromQuery] string skip = "0", [FromQuery] string take = "6", [FromQuery] string where = "")
{
    int skipVal = Math.Max(0, int.TryParse(skip, out var s) ? s : 0);
    int takeVal = Math.Min(50, int.TryParse(take, out var t) && t > 0 ? t : 6);
    
    // Example: fetch from secure internal data source
    var customers = _context.Customers
        .AsNoTracking()
        .Select(c => new { c.CustomerID, c.ContactName })
        .Skip(skipVal)
        .Take(takeVal)
        .ToList();
    
    return Ok(new { d = customers, count = customers.Count });
}
```

## Key Properties and When to Use

| Property | Attribute | When to Use |
|----------|-----------|-------------|
| `DataSource` | `dataSource` | Always required to populate items |
| `Fields` | `<e-dropdownlist-fields>` | When binding complex JSON objects |
| `Value` | `value` | Pre-selecting an item |
| `Index` | `index` | Pre-selecting by position |
| `AllowFiltering` | `allowFiltering` | For lists with many items |
| `FilterType` | `filterType` | Customize search match behavior |
| `DebounceDelay` | `debounceDelay` | Reduce filter frequency while typing |
| `EnableVirtualization` | `enableVirtualization` | For very large datasets |
| `AllowResize` | `allowResize` | Allow user to resize popup |
| `ItemTemplate` | `itemTemplate` | Rich item rendering |
| `ValueTemplate` | `valueTemplate` | Custom selected value display |
| `GroupTemplate` | `groupTemplate` | Custom group headers |
| `ShowClearButton` | `showClearButton` | Allow clearing selection |
| `Enabled` | `enabled` | Conditional enable/disable |
| `Readonly` | `readonly` | Display-only mode |
| `EnableRtl` | `enableRtl` | RTL language support |
| `Locale` | `locale` | Localized text |

---

## Next Steps

- **Just starting?** → [Getting Started](references/dropdownlist-getting-started.md)
- **Binding server data?** → [Data Binding](references/dropdownlist-data-binding.md)
- **Need filtering or grouping?** → [Filtering and Grouping](references/dropdownlist-filtering-grouping.md)
- **Customizing appearance?** → [Templates and Styling](references/dropdownlist-templates-styling.md)
- **Accessibility or localization?** → [Accessibility and Localization](references/dropdownlist-accessibility-features.md)
- **Virtual scroll, resize, cascading?** → [Advanced Scenarios](references/dropdownlist-advanced-scenarios.md)
- **Complete API reference?** → [API Reference](references/dropdownlist-api.md)

---

# Implementing MultiSelect Dropdown in ASP.NET Core

The Syncfusion MultiSelect Dropdown is a powerful component for selecting multiple values from a predefined list. It supports checkbox selection, filtering, grouping, templates, virtual scrolling, and rich data binding — all using ASP.NET Core TagHelper syntax.

## Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/multiselect-getting-started.md)
- NuGet package installation and TagHelper setup
- CDN stylesheet and script references
- First MultiSelect with basic configuration
- Popup height/width configuration
- Form integration and MultiSelectFor

### Data Binding & Value Binding
📄 **Read:** [references/data-binding.md](references/multiselect-data-binding.md)
- Binding array of strings, objects, and complex data
- Remote data: OData, WebAPI, URL Adaptor, offline mode
- Fields mapping (text, value, groupBy, iconCss, disabled)
- Primitive vs object value binding (`allowObjectBinding`)
- Preselecting values via `value` property
- Using `isSelected` field to preselect via data source

### Checkbox & Selection Modes
📄 **Read:** [references/checkbox-and-selection.md](references/multiselect-checkbox-and-selection.md)
- Checkbox mode with `CheckBoxSelection` module
- Select All / Unselect All with `showSelectAll`
- Customizing "Select All" and "Unselect All" text
- Maximum selection limit (`maximumSelectionLength`)
- Selection reordering (`enableSelectionOrder`)
- Display modes: Default, Box, Delimiter, CheckBox
- `closePopupOnSelect` behavior
- `hideSelectedItem` to remove selected items from list
- `changeOnBlur` for event timing control

### Filtering
📄 **Read:** [references/filtering.md](references/multiselect-filtering.md)
- Enabling filtering with `allowFiltering`
- Filter types: StartsWith, Contains, EndsWith
- Minimum character threshold before remote fetch
- Case-sensitive filtering
- Diacritics / accent-insensitive filtering (`ignoreAccent`)
- Debounce delay (`debounceDelay`)
- Custom filtering logic via `filtering` event

### Templates & Customization
📄 **Read:** [references/templates-and-customization.md](references/multiselect-templates-and-customization.md)
- Item template for custom list item layout
- Value template for selected chip display
- Group template for category headers
- Header and footer templates
- No records template and action failure template
- Chip customization via `tagging` event and `setClass`
- CSS customization for wrapper, chips, icons, popup
- Icon support via `iconCss` field

### Grouping & Virtualization
📄 **Read:** [references/grouping-and-virtualization.md](references/multiselect-grouping-and-virtualization.md)
- Grouping items using `groupBy` field
- Grouping with checkboxes (`enableGroupCheckBox`)
- Virtual scrolling for large datasets (`enableVirtualization`)
- Virtualization with remote data, grouping, filtering, checkboxes
- Preselecting values with virtualization
- Custom item count in virtualization

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/multiselect-advanced-features.md)
- Custom value entry (`allowCustomValue`)
- Disabled items via `fields.disabled`
- `disableItem` method for dynamic disabling
- Popup resize (`allowResize`)
- Localization (`locale`, L10n translations)
- Accessibility (WCAG 2.2, keyboard navigation, ARIA)
- Sorting (`sortOrder`)
- Right-to-left support (`enableRtl`)
- State persistence (`enablePersistence`)
- `addTagOnBlur` behavior
- `openOnClick` configuration

### API Reference
📄 **Read:** [references/api.md](references/multiselect-api.md)
- Complete properties reference with types and defaults
- All events with descriptions
- FieldSettings sub-properties
- TagHelper attribute naming conventions

## Quick Start Example

```cshtml
@* _ViewImports.cshtml: @addTagHelper *, Syncfusion.EJ2 *@

@{
    var games = new List<object> {
        new { Id = 1, Game = "Football" },
        new { Id = 2, Game = "Basketball" },
        new { Id = 3, Game = "Cricket" },
        new { Id = 4, Game = "Tennis" }
    };
}

<ejs-multiselect id="games"
    dataSource="@games"
    placeholder="Select games"
    mode="Box">
    <e-multiselect-fields text="Game" value="Id"></e-multiselect-fields>
</ejs-multiselect>
```

## Common Patterns

### Checkbox Mode with Select All
```cshtml
<ejs-multiselect id="sports"
    dataSource="@ViewBag.Sports"
    placeholder="Select sports"
    mode="CheckBox"
    showSelectAll="true"
    selectAllText="Select All"
    unSelectAllText="Unselect All"
    filterBarPlaceholder="Search">
    <e-multiselect-fields text="Game" value="Id"></e-multiselect-fields>
</ejs-multiselect>
```

### Filtering with Remote Data
```cshtml
<ejs-multiselect id="customers"
    placeholder="Search customers"
    allowFiltering="true"
    filtering="onFiltering"
    popupHeight="250px">
    <e-data-manager url="url"
        adaptor="ODataV4Adaptor" crossDomain="true">
    </e-data-manager>
    <e-multiselect-fields text="ContactName" value="CustomerID"></e-multiselect-fields>
</ejs-multiselect>
```

### Virtual Scroll for Large Data
```cshtml
<ejs-multiselect id="items"
    dataSource="@ViewBag.LargeDataset"
    enableVirtualization="true"
    allowFiltering="true"
    placeholder="Select items"
    popupHeight="200px">
    <e-multiselect-fields text="text" value="id"></e-multiselect-fields>
</ejs-multiselect>
```

## Key Properties Quick Reference

| Property | TagHelper Attribute | Purpose |
|----------|-------------------|---------|
| `DataSource` | `dataSource` | Local array or DataManager |
| `Value` | `value` | Pre-selected values |
| `Mode` | `mode` | Default, Box, Delimiter, CheckBox |
| `AllowFiltering` | `allowFiltering` | Enable search/filter |
| `ShowSelectAll` | `showSelectAll` | Show "Select All" in checkbox mode |
| `MaximumSelectionLength` | `maximumSelectionLength` | Limit selections |
| `EnableVirtualization` | `enableVirtualization` | Virtual scroll |
| `AllowCustomValue` | `allowCustomValue` | Accept new typed values |
| `Placeholder` | `placeholder` | Input placeholder text |
| `PopupHeight` | `popupHeight` | Popup list height |
| `PopupWidth` | `popupWidth` | Popup list width |
| `AllowResize` | `allowResize` | Resizable popup |

## Common Use Cases

- **Multi-tag input** → Use `mode="Box"` with `tagging` event for chip styling
- **Permission selector** → Checkbox mode with `maximumSelectionLength`
- **Country/city picker** → Remote data with `allowFiltering` and groupBy
- **Category filter** → Grouping with `enableGroupCheckBox`
- **Large dataset dropdown** → `enableVirtualization` with remote data
- **Form field** → Use `ejs-multiselect-for` with model binding

---

**Start here:** Read [references/getting-started.md](references/multiselect-getting-started.md) to set up your first MultiSelect.

For the parent library overview and other components, see [Implementing Syncfusion ASP.NET Core Components](./SKILL.md).

---

## Security & Trust Boundary

### ⚠️ Key Security Findings & Mitigations

**Snyk W011 (Third-party Content Exposure)** & **W012 (Malicious External URL)** require enforcing a strict trust boundary between external sources and your application.

---

### **1. Remote Data Ingestion (Cascading Dropdowns, Remote Filtering)**

**✅ SECURE PATTERN:** Always proxy through your **same-origin server**. Do NOT:
- Direct `fetch()` to untrusted external URLs
- Use `crossDomain="true"` with external OData/WebAPI endpoints
- Bind dropdowns directly to public third-party APIs

**Implementation Guidelines:**
- Create server-side `/api/` endpoints that act as proxies (see Pattern 2 & 3 secure examples above)
- Server validates, authenticates, and sanitizes all data before returning
- Client binds only to these same-origin endpoints
- **Input validation:** whitelist allowed values, reject out-of-range IDs
- **Schema validation:** reject unexpected JSON fields or types
- **Error handling:** set timeouts, size limits, catch fetch failures gracefully

---

### **2. Runtime CDN Scripts (ej2.min.js, stylesheets)**

**Option A: Use Subresource Integrity (SRI) + HTTPS (if CDN is mandatory)**
```html
<!-- ✅ Pin version & use SRI hash to verify script integrity -->
<link rel="stylesheet" 
      href="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/material.css"
      integrity="sha384-[REPLACE_WITH_ACTUAL_SRI_HASH]"
      crossorigin="anonymous">
<script src="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/ej2.min.js"
        integrity="sha384-[REPLACE_WITH_ACTUAL_SRI_HASH]"
        crossorigin="anonymous"></script>
```
*Find SRI hashes on the CDN provider's documentation or use an SRI hash generator.*

**Option B: Host Scripts Locally (RECOMMENDED for production)**
```html
<!-- ✅ Best security: host vendor files locally from NuGet -->
<link rel="stylesheet" href="~/lib/syncfusion/material.css">
<script src="~/lib/syncfusion/ej2.min.js"></script>
```

**Setup via NuGet:**
```bash
Install-Package Syncfusion.EJ2.AspNet.Core
```
Then add to `_Layout.cshtml`:
```html
<link rel="stylesheet" href="~/css/ej2/material.css" />
<script src="~/js/ej2/ej2.min.js"></script>
<ejs-script-manager></ejs-script-manager>
```

---

### **3. Content Security Policy (CSP) Configuration**

**Add to `_Layout.cshtml` to restrict script/style sources:**
```html
<meta http-equiv="Content-Security-Policy" 
      content="default-src 'self'; 
               script-src 'self'; 
               style-src 'self' 'unsafe-inline'; 
               font-src 'self' data:; 
               img-src 'self' data: https:; 
               connect-src 'self';">
```
Or implement in **Startup.cs / Program.cs**:
```csharp
app.Use(async (context, next) =>
{
    context.Response.Headers["Content-Security-Policy"] = 
        "default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'; connect-src 'self'";
    await next();
});
```

---

### **4. General Controls & Best Practices**

- **Authorization:** Sensitive dropdown actions (e.g., delete, submit) must be re-validated server-side; never trust client-side selection alone.
- **Input Sanitization:** Use `System.Web.HttpUtility.HtmlEncode()` or Razor's `@Html.Encode()` for all user-generated or external data rendered in templates.
- **Logging & Monitoring:** Log remote-data fetch failures and unexpected response shapes for audit trails and rapid incident detection.
- **Trusted Endpoints Whitelist:** Maintain a documented list of approved API endpoints in your deployment configuration.
- **Vendor Scanning:** Periodically scan NuGet/CDN artifacts with `snyk test --file=packages.config` or equivalent.

---

### **Summary: How These Steps Resolve Snyk Findings**

| Snyk Finding | Root Cause | Mitigation |
|---|---|---|
| **W011: Third-party exposure** | Untrusted external data influences UI/app behavior | Use server-proxy pattern (Pattern 2 & 3); validate & sanitize server-side before binding |
| **W012: Malicious CDN URL** | Runtime execution of remote vendor scripts | Host scripts locally from NuGet; or use SRI + version pinning if CDN is required |

**The secure examples in Patterns 2 and 3 above (with checkmark comments) demonstrate the recommended implementation.**

``` 
