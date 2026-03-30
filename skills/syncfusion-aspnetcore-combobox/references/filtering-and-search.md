# Filtering & Search in ComboBox

## Table of Contents

- [Overview](#overview)
- [Enable Basic Filtering](#enable-basic-filtering)
- [Filter Types](#filter-types)
- [Minimum Filter Characters](#minimum-filter-characters)
- [Case-Sensitive Filtering](#case-sensitive-filtering)
- [Remote Filtering](#remote-filtering)
- [Custom Filter Handler](#custom-filter-handler)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

Filtering enables users to search and narrow down ComboBox items as they type. This makes it easy to find items in large datasets.

**Key Features:**
- Autocomplete as user types
- Filter type options (contains, startsWith, endsWith)
- Case-sensitive or insensitive search
- Remote filtering from server
- Custom filter logic

## Enable Basic Filtering

Add `allowFiltering="true"` to enable the search field.

**View:**
```html
@{
    var countries = new List<Country>
    {
        new Country { CountryId = 1, CountryName = "United States" },
        new Country { CountryId = 2, CountryName = "United Kingdom" },
        new Country { CountryId = 3, CountryName = "United Arab Emirates" },
        new Country { CountryId = 4, CountryName = "Uruguay" }
    };
}

<ejs-combobox id="country"
              dataSource="@countries"
              fields-text="CountryName"
              fields-value="CountryId"
              allowFiltering="true"
              placeholder="Type to search">
</ejs-combobox>
```

**Behavior:**
- A search box appears at the top of the dropdown
- As user types "United", shows "United States", "United Kingdom", "United Arab Emirates"
- Filter updates dropdown list in real-time
- Uses "contains" filter by default

## Filter Types

Change how filtering works with the `filterType` property.

**Available Filter Types:**
- **contains** (default) - Text appears anywhere in item
- **startsWith** - Text is at the beginning
- **endsWith** - Text is at the end

**Example with different filter types:**

```html
<!-- Filter items that START with typed text -->
<ejs-combobox id="country1"
              dataSource="@data"
              allowFiltering="true"
              filterType="StartsWith"
              placeholder="Type (searches start of name)">
</ejs-combobox>

<!-- Filter items that END with typed text -->
<ejs-combobox id="country2"
              dataSource="@data"
              allowFiltering="true"
              filterType="EndsWith"
              placeholder="Type (searches end of name)">
</ejs-combobox>

<!-- Filter items that CONTAIN typed text (default) -->
<ejs-combobox id="country3"
              dataSource="@data"
              allowFiltering="true"
              filterType="Contains"
              placeholder="Type (searches anywhere)">
</ejs-combobox>
```

**Example Behavior (searching "land"):**
- **Contains:** Shows "Ireland", "Iceland", "England"
- **StartsWith:** Shows nothing (no country starts with "land")
- **EndsWith:** Shows "Ireland", "Iceland"

## Minimum Filter Characters

Require a minimum number of characters before filtering starts. Useful for remote data to reduce server requests.

```html
<ejs-combobox id="country"
              dataSource="@remoteData"
              allowFiltering="true"
              minFilterChars="2"
              placeholder="Type at least 2 characters">
</ejs-combobox>
```

**Behavior:**
- User types "U" → No filtering, shows all items
- User types "UN" → Filtering starts, shows "United States", "United Kingdom", etc.

**With event handler for validation:**
```html
<ejs-combobox id="country"
              dataSource="@remoteData"
              allowFiltering="true"
              filtering="onFiltering">
</ejs-combobox>

<script>
function onFiltering(args) {
    // Check minimum character count
    if (args.text.length < 2) {
        args.preventDefaultAction = true; // Stop filtering
    }
}
</script>
```

## Case-Sensitive Filtering

By default, filtering is **case-insensitive** (e.g., "usa" matches "USA"). Use DataManager for case-sensitive filtering.

**Case-Insensitive (default):**
```html
<ejs-combobox id="country"
              dataSource="@data"
              allowFiltering="true"
              filterType="Contains">
</ejs-combobox>
<!-- Typing "usa" finds "United States of America" -->
```

**Case-Sensitive (using DataManager):**
```csharp
// In View
@{
    var dm = new Syncfusion.EJ2.Base.DataManager();
    dm.Adaptor = "JsonAdaptor";
}

<ejs-combobox id="country"
              dataSource="@Model"
              allowFiltering="true"
              filtering="onFiltering">
</ejs-combobox>

<script>
function onFiltering(args) {
    // Perform case-sensitive filter manually
    if (args.text) {
        var filteredData = args.baseEventArgs.model.dataSource.filter(
            item => item.CountryName.indexOf(args.text) !== -1
        );
        args.baseEventArgs.model.dataSource = filteredData;
    }
}
</script>
```

## Remote Filtering

Filter large datasets by making server requests. User types → request to server → updated list.

**View:**
```html
<ejs-combobox id="customer"
              dataSource="@(new Syncfusion.EJ2.Base.DataManager { Url = \"/api/customers\" })"
              fields-text="CustomerName"
              fields-value="CustomerId"
              allowFiltering="true"
              filtering="onRemoteFiltering">
</ejs-combobox>

<script>
function onRemoteFiltering(args) {
    console.log("User typed:", args.text);
    var dataManager = args.baseEventArgs.model.dataSource;
    
    // Filter on server via query parameter
    dataManager.url = `/api/customers?search=${args.text}`;
}
</script>
```

**Web API Controller (api/customers):**
```csharp
[ApiController]
[Route("api/[controller]")]
public class CustomersController : ControllerBase
{
    [HttpGet]
    public IActionResult GetCustomers([FromQuery] string search = "")
    {
        var allCustomers = GetAllCustomers(); // Your data source
        
        if (!string.IsNullOrEmpty(search))
        {
            // Filter on server
            var filtered = allCustomers
                .Where(c => c.CustomerName.Contains(search, StringComparison.OrdinalIgnoreCase))
                .ToList();
            return Ok(filtered);
        }
        return Ok(allCustomers);
    }
    
    private List<Customer> GetAllCustomers()
    {
        return new List<Customer>
        {
            new Customer { CustomerId = 1, CustomerName = "Acme Corp" },
            new Customer { CustomerId = 2, CustomerName = "Beta Industries" },
            new Customer { CustomerId = 3, CustomerName = "Acme Tech" }
        };
    }
}

public class Customer
{
    public int CustomerId { get; set; }
    public string CustomerName { get; set; }
}
```

## Custom Filter Handler

Implement custom filtering logic via the `filtering` event.

**Example: Filter by multiple fields**
```html
<ejs-combobox id="employee"
              dataSource="@data"
              fields-text="EmployeeName"
              fields-value="EmployeeId"
              allowFiltering="true"
              filtering="onCustomFilter">
</ejs-combobox>

<script>
function onCustomFilter(args) {
    var searchText = args.text.toLowerCase();
    var combobox = args.baseEventArgs.model;
    
    // Filter by multiple fields: name OR department
    var customFiltered = combobox.dataSource.filter(item => 
        item.EmployeeName.toLowerCase().includes(searchText) ||
        item.Department.toLowerCase().includes(searchText)
    );
    
    combobox.dataSource = customFiltered;
}
</script>
```

**Example: Filter by word boundary (match start of words)**
```html
<script>
function onWordBoundaryFilter(args) {
    var searchText = args.text.toLowerCase();
    var words = searchText.split(' ');
    
    var filtered = args.baseEventArgs.model.dataSource.filter(item => {
        var itemName = item.CountryName.toLowerCase();
        // Check if any word matches the start
        return words.every(word => itemName.includes(word));
    });
    
    args.baseEventArgs.model.dataSource = filtered;
}
</script>
```

## Common Patterns

**Pattern 1: Search with debounce (reduce server calls)**
```html
<ejs-combobox id="search" 
              allowFiltering="true" 
              filtering="onDebounceFilter">
</ejs-combobox>

<script>
var debounceTimer;

function onDebounceFilter(args) {
    clearTimeout(debounceTimer);
    debounceTimer = setTimeout(() => {
        console.log("Filtered for:", args.text);
        // Now make server request
        fetch(`/api/search?q=${args.text}`)
            .then(r => r.json())
            .then(data => {
                var cb = args.baseEventArgs.model;
                cb.dataSource = data;
            });
    }, 300); // Wait 300ms after user stops typing
}
</script>
```

**Pattern 2: Filter with highlighting**
```html
<ejs-combobox id="country"
              allowFiltering="true"
              filtering="onFilterAndHighlight">
</ejs-combobox>

<script>
function onFilterAndHighlight(args) {
    var searchText = args.text;
    var filtered = args.baseEventArgs.model.dataSource.filter(item => 
        item.CountryName.toLowerCase().includes(searchText.toLowerCase())
    );
    args.baseEventArgs.model.dataSource = filtered;
    
    // Highlight matched text (requires template)
    // See templates reference for item template highlighting
}
</script>
```

**Pattern 3: No results message**
```html
<ejs-combobox id="country"
              allowFiltering="true"
              filtering="onFilter">
</ejs-combobox>

<script>
function onFilter(args) {
    var cb = args.baseEventArgs.model;
    var filtered = cb.dataSource.filter(item =>
        item.CountryName.toLowerCase().includes(args.text.toLowerCase())
    );
    
    if (filtered.length === 0) {
        console.log("No results for:", args.text);
        // Optionally show noRecordsTemplate (see templates reference)
    }
    cb.dataSource = filtered;
}
</script>
```

## Troubleshooting

**Issue: Filtering too slow with large dataset**
- ✓ Implement remote filtering (move filter logic to server)
- ✓ Add `minFilterChars` to require minimum text length
- ✓ Use debouncing to reduce filter calls
- ✓ Enable `virtualization` for large lists

**Issue: Filter not showing all matches**
- ✓ Check `filterType` setting (maybe should be "Contains" not "StartsWith")
- ✓ Verify field name matches actual data property
- ✓ Check console for JavaScript errors
- ✓ Debug filter event: `console.log("Filtering for:", args.text);`

**Issue: Case-sensitive filtering not working**
- ✓ Case-insensitive is default
- ✓ Implement custom filter handler with exact string comparison for case-sensitive
- ✓ See Custom Filter Handler section above

**Issue: Remote filter requests too frequent**
- ✓ Add `minFilterChars="2"` to wait for 2+ characters
- ✓ Implement debouncing in `filtering` event (300ms delay)
- ✓ Monitor server logs to see filter requests pattern
