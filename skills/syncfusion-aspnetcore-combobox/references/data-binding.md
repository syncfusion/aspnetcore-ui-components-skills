# Data Binding in ComboBox

## Table of Contents

- [Overview](#overview)
- [Local Data Binding](#local-data-binding)
  - [Array of Strings](#array-of-strings)
  - [Array of Objects](#array-of-objects)
  - [Complex Data Structures](#complex-data-structures)
- [Remote Data Binding](#remote-data-binding)
  - [DataManager with OData](#datamanager-with-odata)
  - [Web API Integration](#web-api-integration)
  - [Query and Adaptor](#query-and-adaptor)
- [Field Mapping](#field-mapping)
- [Custom Values](#custom-values)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

The ComboBox supports multiple data sources:
- **Local:** In-memory arrays, Lists, or collections
- **Remote:** OData services, Web APIs, REST endpoints
- Uses the `dataSource` property to bind data
- Uses `fields` object to map data properties to ComboBox display

## Local Data Binding

### Array of Strings

Simplest form: array of primitive values. Both text and value are the same.

**Controller:**
```csharp
public IActionResult Index()
{
    var sports = new List<string> 
    { 
        "Cricket", "Badminton", "Basketball", "Tennis" 
    };
    return View(sports);
}
```

**View:**
```html
@model List<string>

<ejs-combobox id="sports" dataSource="@Model" placeholder="Select a sport">
</ejs-combobox>
```

**JavaScript (if needed):**
```javascript
// Access selected value
var combobox = document.getElementById('sports').ej2_instances[0];
console.log(combobox.value); // Returns selected string
```

### Array of Objects

Most common: binding objects with text/value properties.

**Model:**
```csharp
public class Country
{
    public int CountryId { get; set; }
    public string CountryName { get; set; }
    public string CountryCode { get; set; }
}
```

**Controller:**
```csharp
public IActionResult Index()
{
    var countries = new List<Country>
    {
        new Country { CountryId = 1, CountryName = "United States", CountryCode = "US" },
        new Country { CountryId = 2, CountryName = "United Kingdom", CountryCode = "UK" },
        new Country { CountryId = 3, CountryName = "India", CountryCode = "IN" }
    };
    return View(countries);
}
```

**View (Map properties with fields):**
```html
@model List<Country>

<ejs-combobox id="country"
              dataSource="@Model"
              fields-text="CountryName"
              fields-value="CountryId"
              placeholder="Select a country">
</ejs-combobox>

<script>
function onSelect(args) {
    console.log("Selected ID:", args.itemData.CountryId);
    console.log("Selected Name:", args.itemData.CountryName);
}
</script>
```

**Key Points:**
- `fields-text`: Property to display in dropdown list
- `fields-value`: Property to use as the internal value
- When user selects, `value` is set to the `fields-value` property

### Complex Data Structures

Map nested properties using dot notation.

**Model with nested object:**
```csharp
public class Employee
{
    public int EmpId { get; set; }
    public string EmpName { get; set; }
    public Department Dept { get; set; }
}

public class Department
{
    public int DeptId { get; set; }
    public string DeptName { get; set; }
}
```

**Controller:**
```csharp
var employees = new List<Employee>
{
    new Employee 
    { 
        EmpId = 1, 
        EmpName = "John", 
        Dept = new Department { DeptId = 101, DeptName = "Engineering" } 
    },
    new Employee 
    { 
        EmpId = 2, 
        EmpName = "Jane", 
        Dept = new Department { DeptId = 102, DeptName = "Sales" } 
    }
};
return View(employees);
```

**View (use dot notation for nested properties):**
```html
@model List<Employee>

<ejs-combobox id="employee"
              dataSource="@Model"
              fields-text="EmpName"
              fields-value="Dept.DeptId"
              placeholder="Select employee">
</ejs-combobox>

<!-- Display department name -->
<ejs-combobox id="department"
              dataSource="@Model"
              fields-text="Dept.DeptName"
              fields-value="EmpId"
              placeholder="Select by department">
</ejs-combobox>
```

## Remote Data Binding

### DataManager with OData

Bind to remote OData services using DataManager.

**View:**
```html
<ejs-combobox id="remotedata"
              dataSource="@(new Syncfusion.EJ2.Base.DataManager { Url = \"https://js.syncfusion.com/demos/ejServices/Wcf/Northwind.svc/Orders\" })"
              fields-text="CustomerName"
              fields-value="OrderID"
              placeholder="Select a customer"
              allowFiltering="true">
</ejs-combobox>

<script>
function onActionFailure(args) {
    console.log("Error loading remote data:", args);
}
</script>
```

**Key Properties:**
- `Url`: OData endpoint URL
- `CrossDomain`: Set to true for cross-domain requests (usually not needed for same domain)
- `Adaptor`: Type of adaptor (ODataV4Adaptor by default)

### Web API Integration

Bind to custom Web API endpoints.

**Web API Controller (e.g., CountriesController.cs):**
```csharp
[ApiController]
[Route("api/[controller]")]
public class CountriesController : ControllerBase
{
    [HttpGet]
    public IActionResult GetCountries()
    {
        var countries = new List<Country>
        {
            new Country { CountryId = 1, CountryName = "USA", Code = "US" },
            new Country { CountryId = 2, CountryName = "UK", Code = "UK" },
            new Country { CountryId = 3, CountryName = "India", Code = "IN" }
        };
        return Ok(countries);
    }

    [HttpGet("{id}")]
    public IActionResult GetCountry(int id)
    {
        // Find and return specific country
        return Ok(new { id, name = "USA" });
    }
}
```

**View (consume API):**
```html
<ejs-combobox id="country"
              dataSource="@(new Syncfusion.EJ2.Base.DataManager { Url = \"/api/countries\" })"
              fields-text="CountryName"
              fields-value="CountryId"
              placeholder="Select country">
</ejs-combobox>
```

### Query and Adaptor

Use DataManager Query for filtering/paging remote data.

**View with Query:**
```html
@{
    var dataManager = new Syncfusion.EJ2.Base.DataManager
    {
        Url = "https://js.syncfusion.com/demos/ejServices/Wcf/Northwind.svc/Orders",
        Adaptor = "ODataAdaptor"
    };
    var query = new Syncfusion.EJ2.Base.Query()
        .Select(new List<string> { "OrderID", "CustomerName", "ShippedDate" })
        .Take(10);
}

<ejs-combobox id="remotedata"
              dataSource="@dataManager"
              query="@query"
              fields-text="CustomerName"
              fields-value="OrderID">
</ejs-combobox>
```

## Field Mapping

Use the `fields` object to map data properties. Useful when your data properties don't exactly match ComboBox expectations.

**Standard Field Options:**
```html
<ejs-combobox id="combobox"
              dataSource="@data"
              fields-text="DisplayName"
              fields-value="UniqueId"
              fields-groupBy="Category"
              fields-iconCss="IconClass">
</ejs-combobox>
```

**Detailed Field Properties:**

| Field | Type | Purpose |
|-------|------|---------|
| `text` | string | Property for display text |
| `value` | string or int | Property for selected value |
| `groupBy` | string | Property to group items by |
| `iconCss` | string | Property for CSS class of icon |

**Example with all fields:**
```csharp
// Model
public class Product
{
    public int ProductId { get; set; }
    public string ProductName { get; set; }
    public string Category { get; set; }
    public string IconClass { get; set; }
}

// Usage in view
<ejs-combobox id="product"
              dataSource="@Model"
              fields-text="ProductName"
              fields-value="ProductId"
              fields-groupBy="Category"
              fields-iconCss="IconClass">
</ejs-combobox>
```

## Custom Values

Allow users to enter values not present in the data source.

**View:**
```html
<ejs-combobox id="combobox"
              dataSource="@data"
              allowCustom="true"
              placeholder="Select or type custom value">
</ejs-combobox>
```

**JavaScript to capture custom values:**
```javascript
var combobox = document.getElementById('combobox').ej2_instances[0];

// Listen to change event
combobox.actionComplete = function(args) {
    if (args.isInteracted) {
        console.log("User entered:", combobox.value);
        // Check if it's in original data or custom
        var isCustom = !args.itemData; // No itemData = custom value
        if (isCustom) {
            console.log("Custom value entered:", combobox.value);
        }
    }
};
```

**When submitted in a form:**
```html
<form method="post">
    <ejs-combobox id="sport" 
                  name="sport"
                  dataSource="@data"
                  allowCustom="true">
    </ejs-combobox>
    <button type="submit">Submit</button>
</form>
```

The custom value will be posted to the server even if not in the original list.

## Common Patterns

**Pattern 1: Load data via AJAX after page load**
```html
<ejs-combobox id="dynamic" placeholder="Select..."></ejs-combobox>

<script>
document.addEventListener('DOMContentLoaded', function() {
    fetch('/api/items')
        .then(r => r.json())
        .then(data => {
            var combobox = document.getElementById('dynamic').ej2_instances[0];
            combobox.dataSource = data;
        });
});
</script>
```

**Pattern 2: Dependent ComboBoxes**
```html
<!-- First ComboBox -->
<ejs-combobox id="category" 
              dataSource="@categories"
              change="onCategoryChange">
</ejs-combobox>

<!-- Second ComboBox (filtered by first) -->
<ejs-combobox id="product" placeholder="Select product"></ejs-combobox>

<script>
function onCategoryChange(args) {
    if (args.value) {
        var category = args.value;
        fetch(`/api/products?category=${category}`)
            .then(r => r.json())
            .then(data => {
                var product = document.getElementById('product').ej2_instances[0];
                product.dataSource = data;
            });
    }
}
</script>
```

## Troubleshooting

**Issue: Data not displaying**
- ✓ Verify `fields-text` property name matches your data object
- ✓ Check `dataSource` is not null/empty (debug in Controller)
- ✓ Inspect browser Network tab to confirm data was sent
- ✓ Use F12 Console to check for JavaScript errors

**Issue: Selected value not showing correctly**
- ✓ Ensure `fields-value` property name is correct
- ✓ Verify selected value exists in the data source
- ✓ For complex nested data, use dot notation: `fields-value="Dept.Id"`

**Issue: Remote data not loading**
- ✓ Check API endpoint is returning valid JSON
- ✓ Verify CORS if calling cross-domain
- ✓ Use browser Network tab to see the actual response
- ✓ Add `actionFailure` event to catch errors

**Issue: Custom values not being saved**
- ✓ Ensure `allowCustom="true"` is set
- ✓ When posting form, the value is sent (may differ from displayed text)
- ✓ Handle both predefined and custom values in backend controller
