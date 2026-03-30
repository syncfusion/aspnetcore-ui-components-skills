# Data Binding in DropdownList

## Table of Contents
- [Overview](#overview)
- [Array of Simple Data](#array-of-simple-data)
- [Array of Objects](#array-of-objects)
- [Complex Nested Data](#complex-nested-data)
- [Remote Data Binding](#remote-data-binding)
- [Field Mapping](#field-mapping)
- [Text and Value Fields](#text-and-value-fields)
- [Advanced Data Binding](#advanced-data-binding)
- [Troubleshooting](#troubleshooting)

## Overview

DropdownList supports multiple data sources:
- **Local Data:** Arrays in memory
- **Remote Data:** API endpoints, OData services
- **DataManager:** Built-in data source manager for OData, Web API, JSON
- **Database:** Via DataManager with entity framework

The `DataSource` property accepts any enumerable collection. For complex data, use the `Fields` property to map data columns to DropdownList properties.

## Array of Simple Data

### Strings

Simplest data binding - items are strings displayed as both text and value:

```csharp
// Controller
var fruitList = new List<string>
{
    "Apple",
    "Banana",
    "Orange",
    "Mango"
};
ViewBag.Fruits = fruitList;

// View
@Html.EJ2().DropDownList()
    .Id("FruitDropdown")
    .DataSource(ViewBag.Fruits)
    .Placeholder("Select a fruit")
    .Render()
```

When user selects "Apple", both `value` and `text` are "Apple".

### Numbers

```csharp
// Controller
ViewBag.Priorities = new List<int> { 1, 2, 3, 4, 5 };

// View
@Html.EJ2().DropDownList()
    .Id("Priority")
    .DataSource(ViewBag.Priorities)
    .Render()
```

### Accessing Selected Value

```javascript
function onSelectionChange(args) {
    var value = args.value; // The selected value
    var text = args.text;   // The display text
    console.log('Selected: ' + text + ' (' + value + ')');
}
```

## Array of Objects

Most common scenario - bind object properties to dropdown:

### Basic Object Binding

```csharp
// Model class
public class Department
{
    public int Id { get; set; }
    public string Name { get; set; }
}

// Controller
ViewBag.Departments = new List<Department>
{
    new Department { Id = 1, Name = "IT" },
    new Department { Id = 2, Name = "HR" },
    new Department { Id = 3, Name = "Sales" },
    new Department { Id = 4, Name = "Finance" }
};

// View
@Html.EJ2().DropDownList()
    .Id("DepartmentDropdown")
    .DataSource(ViewBag.Departments)
    .Fields(f => f.Text("Name").Value("Id"))
    .Placeholder("Select department")
    .Render()
```

Display shows: "IT", "HR", "Sales", "Finance"
Selected value stores: 1, 2, 3, 4 (numeric IDs)

### Multiple Properties

```csharp
// Model with additional properties
public class Employee
{
    public int EmployeeId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Email { get; set; }
}

// View - use Text property for display
@Html.EJ2().DropDownList()
    .Id("Employees")
    .DataSource(ViewBag.Employees)
    .Fields(f => f
        .Text("FirstName") // What shows in dropdown
        .Value("EmployeeId") // What's stored when selected
    )
    .Render()
```

## Complex Nested Data

### Accessing Nested Properties

Data with nested objects - map nested property paths:

```csharp
// Models with nesting
public class Company
{
    public int Id { get; set; }
    public string CompanyName { get; set; }
    public Address Location { get; set; }
}

public class Address
{
    public string City { get; set; }
    public string Country { get; set; }
}

// Controller
ViewBag.Companies = new List<Company>
{
    new Company { 
        Id = 1, 
        CompanyName = "Tech Corp", 
        Location = new Address { City = "New York", Country = "USA" }
    },
    new Company { 
        Id = 2, 
        CompanyName = "Global Ltd", 
        Location = new Address { City = "London", Country = "UK" }
    }
};

// View - access nested City property
@Html.EJ2().DropDownList()
    .Id("Companies")
    .DataSource(ViewBag.Companies)
    .Fields(f => f
        .Text("Location.City") // Access nested property
        .Value("Id")
    )
    .Render()
```

Display shows: "New York", "London"
Selected values: 1, 2

### Combining Multiple Fields

Create display text from multiple properties:

```csharp
// In view template (using item template)
@Html.EJ2().DropDownList()
    .Id("Employees")
    .DataSource(ViewBag.Employees)
    .Fields(f => f.Text("Name").Value("Id"))
    .ItemTemplate("<span class='e-list-item'>${FullName} - ${Email}</span>")
    .ValueTemplate("<span>${Name}</span>")
    .Render()
```

## Remote Data Binding

### Web API Endpoint

```csharp
// Controller API
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    [HttpGet]
    public IActionResult GetProducts()
    {
        var products = new List<Product>
        {
            new Product { Id = 1, Name = "Laptop", Price = 999 },
            new Product { Id = 2, Name = "Mouse", Price = 25 },
            new Product { Id = 3, Name = "Keyboard", Price = 75 }
        };
        return Ok(products);
    }
}

// View - remote data via URL
@Html.EJ2().DropDownList()
    .Id("Products")
    .DataSource(d => d.Url("/api/products"))
    .Fields(f => f.Text("Name").Value("Id"))
    .AllowFiltering(true)
    .FilterType(FilterType.Contains)
    .Placeholder("Select product")
    .Render()
```

### OData Service

```csharp
// View - bind to OData service
@Html.EJ2().DropDownList()
    .Id("ODataDropdown")
    .DataSource(d => d.Url("https://services.odata.org/V4/Northwind/Northwind.svc/Categories"))
    .Fields(f => f.Text("CategoryName").Value("CategoryID"))
    .Render()
```

### Async Data Loading

Load data after user interaction:

```csharp
@Html.EJ2().DropDownList()
    .Id("Categories")
    .Placeholder("Select category")
    .Change("onCategoryChange")
    .Render()

<script>
async function onCategoryChange(args) {
    try {
        // Fetch dependent data
        const response = await fetch('/api/subcategories?categoryId=' + args.value);
        const subcategories = await response.json();
        
        // Update second dropdown
        var subDropdown = document.getElementById('Subcategories').ej2_instances[0];
        subDropdown.dataSource = subcategories;
    } catch (error) {
        console.error('Error loading data:', error);
    }
}
</script>
```

## Field Mapping

### Basic Field Mapping

Map dropdown properties to data source columns:

```csharp
@Html.EJ2().DropDownList()
    .Id("Status")
    .DataSource(ViewBag.StatusList)
    .Fields(f => f
        .Text("DisplayText")      // Column for display
        .Value("StatusCode")      // Column for value
        .GroupBy("Category")      // Column for grouping
        .IconCss("Icon")          // Column for icon CSS class
    )
    .GroupBy("Category")
    .Render()
```

### Understanding Fields Property

```csharp
// Example data structure
public class Item
{
    public int ItemId { get; set; }           // Maps to Value
    public string ItemName { get; set; }      // Maps to Text
    public string Category { get; set; }      // Maps to GroupBy
    public string IconClass { get; set; }     // Maps to IconCss
    public string Description { get; set; }   // Used in templates
}

// Mapping
.Fields(f => f
    .Text("ItemName")              // Show ItemName in dropdown
    .Value("ItemId")               // Store ItemId when selected
    .GroupBy("Category")           // Organize by Category
    .IconCss("IconClass")          // Show icon from IconClass
)
```

### When Text = Value

If data doesn't need text/value separation:

```csharp
// Simple list of status strings
ViewBag.Statuses = new List<string> { "Active", "Inactive", "Pending" };

// Text and Value automatically match
@Html.EJ2().DropDownList()
    .Id("Status")
    .DataSource(ViewBag.Statuses)
    .Render()

// Both value and text are "Active", "Inactive", etc.
```

## Text and Value Fields

### Key Concepts

**Text Field:** What users see in dropdown list and selected item
**Value Field:** What's stored internally and returned when selected

```csharp
// Display "United States" but store "US"
public class Country
{
    public string CountryCode { get; set; }    // Value: "US"
    public string CountryName { get; set; }    // Text: "United States"
}

.Fields(f => f
    .Text("CountryName")
    .Value("CountryCode")
)

// User sees: "United States"
// Form submits: "US"
```

### Accessing After Selection

```javascript
function onSelect(args) {
    var displayedText = args.text;      // "United States"
    var storedValue = args.value;       // "US"
    console.log(displayedText, storedValue);
}
```

### Required for Complex Objects

**Must** map fields when binding objects:

```csharp
// ❌ WRONG - No field mapping
@Html.EJ2().DropDownList()
    .Id("Employees")
    .DataSource(ViewBag.Employees)  // Object list
    .Render()
    // Result: Shows "[object Object]"

// ✅ CORRECT - With field mapping
@Html.EJ2().DropDownList()
    .Id("Employees")
    .DataSource(ViewBag.Employees)
    .Fields(f => f.Text("Name").Value("Id"))
    .Render()
    // Result: Shows employee names
```

## Advanced Data Binding

### Hierarchical Data

```csharp
public class Category
{
    public int CategoryId { get; set; }
    public string CategoryName { get; set; }
    public List<SubCategory> SubCategories { get; set; }
}

public class SubCategory
{
    public int SubCategoryId { get; set; }
    public string SubCategoryName { get; set; }
}

// In view - flatten for dropdown display
var flatList = ViewBag.Categories
    .SelectMany(c => c.SubCategories.Select(sc => new {
        Id = sc.SubCategoryId,
        Name = sc.SubCategoryName,
        Category = c.CategoryName
    }))
    .ToList();

@Html.EJ2().DropDownList()
    .Id("SubCategories")
    .DataSource(flatList)
    .Fields(f => f.Text("Name").Value("Id").GroupBy("Category"))
    .GroupBy("Category")
    .Render()
```

### Dynamic Data from Database

```csharp
// Controller - query database
public IActionResult GetCategories()
{
    var categories = _context.Categories
        .Select(c => new { c.Id, c.Name })
        .ToList();
    return View(categories);
}

// View
@model List<dynamic>
@Html.EJ2().DropDownList()
    .Id("Categories")
    .DataSource(Model)
    .Fields(f => f.Text("Name").Value("Id"))
    .Render()
```

### Reactive Data Updates

```javascript
// Update dropdown data when external data changes
function updateDropdownData(newData) {
    var dropdown = document.getElementById('Categories').ej2_instances[0];
    dropdown.dataSource = newData;
    dropdown.dataBind();  // Refresh the control
}

// Example: Load new data after filter
function filterByYear(year) {
    fetch('/api/products?year=' + year)
        .then(r => r.json())
        .then(data => {
            updateDropdownData(data);
        });
}
```

## Troubleshooting

### Issue: Shows [object Object]
**Cause:** Fields not mapped for object data
**Solution:** Add Fields property with Text and Value mappings

### Issue: Empty dropdown
**Cause:** DataSource not provided or null
**Solution:** Verify data exists in controller, check Fields mapping

### Issue: Wrong values selected
**Cause:** Text and Value fields reversed
**Solution:** Verify correct field names in Fields mapping

### Issue: Remote data not loading
**Cause:** URL incorrect or CORS issue
**Solution:** Test API endpoint directly, check browser network tab

### Issue: Nested data not showing
**Cause:** Incorrect property path in field mapping
**Solution:** Use dot notation for nested properties: "Location.City"

---

## Next Steps

- Explore [Filtering and Grouping](filtering-grouping.md) for better UX
- Learn [Templates and Styling](templates-styling.md) for custom display
- Review [Advanced Scenarios](advanced-scenarios.md) for complex patterns
