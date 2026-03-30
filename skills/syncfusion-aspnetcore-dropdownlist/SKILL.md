---
name: syncfusion-aspnetcore-dropdownlist
description: Implement Syncfusion DropdownList component in ASP.NET Core immediately. Single selection control with data binding, filtering, grouping, templates, and accessibility features. Essential for building professional dropdown interfaces with server-side data binding.
license: "SEE LICENSE IN license"
metadata:
  author: "Syncfusion"
  category: "Dropdowns"
version: "33.1.44"
---

# Implementing DropdownList in ASP.NET Core

The DropdownList is a single-selection control that displays a list of predefined values. Users can select one value from the dropdown list. It supports data binding, filtering, grouping, custom templates, and accessibility features.

## When to Use This Skill

Use this skill when you need to:
- Create a single-selection dropdown in an ASP.NET Core application
- Bind dropdown data from server-side sources (array, database, API)
- Enable filtering for large lists
- Group related items
- Customize item appearance with templates
- Ensure accessibility compliance
- Cascade dropdowns based on parent selection
- Handle user selections with events

## Component Overview

**Key Features:**
- Data binding from local arrays and remote sources
- Real-time filtering with searchbox
- Automatic grouping of items
- Customizable item and value templates
- WCAG accessibility support with keyboard navigation
- Cascading dropdown support
- Virtual scrolling for large datasets

**Common Properties:**
- `DataSource` - Data array or DataManager
- `Fields` - Field mapping (text, value, groupBy, iconCss)
- `AllowFiltering` - Enable/disable filtering
- `FilterType` - Filtering algorithm (StartsWith, Contains, EndsWith)
- `GroupBy` - Field name for grouping
- `ItemTemplate` - Custom item rendering
- `ValueTemplate` - Custom selected value rendering
- `Placeholder` - Hint text
- `Value` - Selected value
- `ReadOnly` - Read-only state

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and NuGet package setup
- TagHelper registration and configuration
- Basic DropdownList implementation
- HTML structure and minimal examples
- CSS theme imports
- Click and change event handlers

### Data Binding and Sources
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Binding array of simple data (strings, numbers)
- Binding complex JSON objects
- Nested data structure mapping
- Remote data binding with DataManager
- OData and Web API integration
- Field mapping and value binding
- Understanding text and value fields

### Filtering and Grouping
📄 **Read:** [references/filtering-grouping.md](references/filtering-grouping.md)
- Enable and configure filtering
- Filter types (StartsWith, Contains, EndsWith)
- Case-insensitive filtering
- Grouping items by category
- Custom group headers
- Sort order configuration (ascending/descending)
- Virtual grouping for performance

### Templates and Styling
📄 **Read:** [references/templates-styling.md](references/templates-styling.md)
- Item template customization
- Selected value template
- Header and footer templates
- Group header templates
- CSS class customization
- Theme styling and color schemes
- Responsive sizing and positioning

### Accessibility and Features
📄 **Read:** [references/accessibility-features.md](references/accessibility-features.md)
- WCAG 2.1 Level AA compliance
- Keyboard navigation (arrow keys, Enter, Tab)
- ARIA attributes and screen reader support
- Focus management and focus indicators
- Disabled state handling
- RTL (Right-to-Left) language support
- Tooltip and help text integration

### Advanced Scenarios
📄 **Read:** [references/advanced-scenarios.md](references/advanced-scenarios.md)
- Cascading dropdowns (dependent selection)
- Multi-select dropdown patterns
- Virtual scrolling for large datasets
- Search and autocomplete patterns
- API integration and async data loading
- Error handling and empty state
- Performance optimization tips

## Quick Start Example

**Controller (HomeController.cs):**
```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        ViewBag.Fruits = new List<string> { "Apple", "Orange", "Banana", "Mango" };
        return View();
    }
}
```

**View (Index.cshtml):**
```csharp
@Html.EJ2().DropDownList()
    .Id("DropdownList")
    .DataSource(ViewBag.Fruits)
    .Placeholder("Select fruit")
    .Render()
```

## Common Patterns

### Pattern 1: Basic Selection with Event Handling

**Model:**
```csharp
public class Category
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```

**Controller:**
```csharp
public IActionResult Index()
{
    var categories = new List<Category>
    {
        new Category { Id = 1, Name = "Electronics" },
        new Category { Id = 2, Name = "Furniture" },
        new Category { Id = 3, Name = "Clothing" }
    };
    ViewBag.Categories = categories;
    return View();
}
```

**View:**
```csharp
@Html.EJ2().DropDownList()
    .Id("Category")
    .DataSource(ViewBag.Categories)
    .Fields(fields => fields.Text("Name").Value("Id"))
    .Change("onCategoryChange")
    .Placeholder("Select category")
    .Render()

<script>
function onCategoryChange(args) {
    console.log('Selected ID:', args.value);
    console.log('Selected Text:', args.text);
    // Perform action based on selection
}
</script>
```

### Pattern 2: Cascading Dropdowns

**Models:**
```csharp
public class Country
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public class City
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int CountryId { get; set; }
}
```

**Controller:**
```csharp
public IActionResult Index()
{
    var countries = new List<Country>
    {
        new Country { Id = 1, Name = "USA" },
        new Country { Id = 2, Name = "Canada" },
        new Country { Id = 3, Name = "Mexico" }
    };
    ViewBag.Countries = countries;
    return View();
}

[HttpGet("api/cities/{countryId}")]
public IActionResult GetCities(int countryId)
{
    var cities = new List<City>
    {
        new City { Id = 1, Name = "New York", CountryId = 1 },
        new City { Id = 2, Name = "Los Angeles", CountryId = 1 },
        new City { Id = 3, Name = "Toronto", CountryId = 2 },
        new City { Id = 4, Name = "Vancouver", CountryId = 2 }
    };
    
    var filtered = cities.Where(c => c.CountryId == countryId)
        .Select(c => new { c.Id, c.Name })
        .ToList();
    
    return Json(filtered);
}
```

**View:**
```csharp
<!-- First dropdown -->
@Html.EJ2().DropDownList()
    .Id("Country")
    .DataSource(ViewBag.Countries)
    .Fields(fields => fields.Text("Name").Value("Id"))
    .Change("onCountryChange")
    .Placeholder("Select country")
    .Render()

<!-- Second dropdown (dependent) -->
@Html.EJ2().DropDownList()
    .Id("City")
    .Placeholder("Select city")
    .Render()

<script>
function onCountryChange(args) {
    var cityDropdown = document.getElementById('City').ej2_instances[0];
    
    if (args.value) {
        // Fetch cities for selected country
        fetch('/home/GetCities/' + args.value)
            .then(response => response.json())
            .then(data => {
                cityDropdown.dataSource = data;
                cityDropdown.value = null;
            })
            .catch(error => console.error('Error:', error));
    } else {
        cityDropdown.dataSource = [];
    }
}
</script>
```

### Pattern 3: Filtered List with Remote Data

**Model:**
```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}
```

**API Controller:**
```csharp
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    [HttpGet]
    public IActionResult GetProducts()
    {
        var products = new List<Product>
        {
            new Product { Id = 1, Name = "Laptop", Price = 999.99m },
            new Product { Id = 2, Name = "Mouse", Price = 25.50m },
            new Product { Id = 3, Name = "Keyboard", Price = 75.00m },
            new Product { Id = 4, Name = "Monitor", Price = 299.99m }
        };
        return Ok(products);
    }
}
```

**View:**
```csharp
@Html.EJ2().DropDownList()
    .Id("Products")
    .DataSource(d => d.Url("/api/products"))
    .Fields(fields => fields.Text("Name").Value("Id"))
    .AllowFiltering(true)
    .FilterType(FilterType.Contains)
    .Placeholder("Search products...")
    .Render()
```

## Key Props and When to Use

| Property | Type | When to Use |
|----------|------|-----------|
| `DataSource` | Array/DataManager | Always required for populating items |
| `Fields` | FieldSettings | When binding complex data objects |
| `Value` | string/number | When pre-selecting an item |
| `AllowFiltering` | bool | For lists with 10+ items |
| `FilterType` | FilterType | Customize search behavior |
| `GroupBy` | string | When items have logical categories |
| `ItemTemplate` | string | For rich item content |
| `Placeholder` | string | For better UX guidance |
| `ReadOnly` | bool | When preventing user changes |
| `Enabled` | bool | For conditional enabling |

## Common Use Cases

### 1. Form Selection Field
Dropdown in a form for selecting category, status, or type

### 2. Cascading Selection
Multiple dropdowns where child depends on parent selection

### 3. Searchable List
Enable filtering for dropdown with large number of items

### 4. Grouped Items
Organize items by category (e.g., Product Category > Subcategory)

### 5. Dynamic Data
Load items from API or database based on user actions

### 6. Template Customization
Display rich content with icons, images, or badges in dropdown items

---

## Next Steps

- **Just starting?** Read [Getting Started](references/getting-started.md)
- **Binding server data?** Read [Data Binding](references/data-binding.md)
- **Need filtering?** Read [Filtering and Grouping](references/filtering-grouping.md)
- **Customizing appearance?** Read [Templates and Styling](references/templates-styling.md)
- **Need accessibility?** Read [Accessibility and Features](references/accessibility-features.md)
- **Advanced scenarios?** Read [Advanced Scenarios](references/advanced-scenarios.md)

---

For the parent library overview and other components, see [Implementing Syncfusion ASP.NET Core Components](../../SKILL.md).
