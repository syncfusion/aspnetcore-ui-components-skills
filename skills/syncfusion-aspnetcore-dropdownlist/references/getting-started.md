# Getting Started with DropdownList in ASP.NET Core

This guide covers installation, setup, and creating your first DropdownList component in ASP.NET Core.

## Installation and Setup

### Step 1: Add NuGet Package

Add the Syncfusion EJ2 ASP.NET Core package to your project:

```
Install-Package Syncfusion.EJ2.AspNetCore
```

Or using .NET CLI:
```
dotnet add package Syncfusion.EJ2.AspNetCore
```

### Step 2: Register Services in Startup

In `Program.cs` (ASP.NET Core 6.0+) or `Startup.cs` (earlier versions):

```csharp
// Program.cs - ASP.NET Core 6.0+
var builder = WebApplication.CreateBuilder(args);

// Add Syncfusion services
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();
app.MapRazorPages();
app.Run();
```

For older versions:
```csharp
// Startup.cs - ASP.NET Core 3.1 or earlier
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();
}
```

### Step 3: Register Tag Helpers

In `Views/_ViewImports.cshtml`, add the Syncfusion tag helper:

```csharp
@addTagHelper *, Syncfusion.EJ2
```

### Step 4: Add Theme CSS

In `Views/Shared/_Layout.cshtml`, add the theme CSS reference in the `<head>` section:

```html
<!-- Syncfusion Material Theme -->
<link rel="stylesheet" href="~/lib/ej2/material.css" />

<!-- Or other themes -->
<!-- <link rel="stylesheet" href="~/lib/ej2/bootstrap.css" /> -->
<!-- <link rel="stylesheet" href="~/lib/ej2/fabric.css" /> -->
```

Add script references before closing `</body>`:

```html
<!-- Syncfusion JavaScript Libraries -->
<script src="~/lib/ej2/ej2.all.min.js"></script>
```

## Minimal DropdownList Example

### Controller Setup

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        ViewBag.DropdownData = new List<string>
        {
            "Apple",
            "Orange",
            "Banana",
            "Mango"
        };
        return View();
    }
}
```

### Razor View

```html
@{
    ViewData["Title"] = "DropdownList Demo";
}

<!-- Simple DropdownList with string data -->
@Html.EJ2().DropDownList()
    .Id("Fruits")
    .DataSource(ViewBag.DropdownData)
    .Placeholder("Select a fruit")
    .Render()
```

## HTML Structure

The DropdownList renders the following HTML structure:

```html
<input id="Fruits" type="hidden" data-role="dropdownlist" />
<!-- Renders as a wrapper div with dropdown icon -->
```

When rendered, it creates:
- Hidden input field for value storage
- Dropdown wrapper with popup list
- Search input (if filtering enabled)
- List items with proper ARIA attributes

## Binding Data to DropdownList

### Simple String Array

```csharp
// Controller
ViewBag.Colors = new List<string> { "Red", "Green", "Blue" };

// View
@Html.EJ2().DropDownList()
    .Id("Colors")
    .DataSource(ViewBag.Colors)
    .Render()
```

### List of Objects

```csharp
// Model class
public class Employee
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Department { get; set; }
}

// Controller
var employees = new List<Employee>
{
    new Employee { Id = 1, Name = "John", Department = "IT" },
    new Employee { Id = 2, Name = "Jane", Department = "HR" },
    new Employee { Id = 3, Name = "Bob", Department = "Sales" }
};
ViewBag.Employees = employees;

// View
@Html.EJ2().DropDownList()
    .Id("EmployeeList")
    .DataSource(ViewBag.Employees)
    .Fields(f => f.Text("Name").Value("Id"))
    .Placeholder("Select an employee")
    .Render()
```

## Event Handling

### Change Event

Handle when user selects a different item:

```csharp
@Html.EJ2().DropDownList()
    .Id("Categories")
    .DataSource(ViewBag.Categories)
    .Fields(f => f.Text("Name").Value("Id"))
    .Change("onChangeCategory")
    .Render()

<script>
function onChangeCategory(args) {
    console.log('Selected value:', args.value);
    console.log('Selected text:', args.text);
    // Perform action based on selection
    if (args.value) {
        // Load related data
        loadSubcategories(args.value);
    }
}
</script>
```

### Select Event

Handle when user explicitly selects an item (fires after change):

```csharp
@Html.EJ2().DropDownList()
    .Id("Products")
    .Select("onSelectProduct")
    .Render()

<script>
function onSelectProduct(args) {
    console.log('Product selected:', args.itemData);
}
</script>
```

### Focus and Blur Events

```csharp
@Html.EJ2().DropDownList()
    .Id("Status")
    .Focus("onFocus")
    .Blur("onBlur")
    .Render()

<script>
function onFocus(args) {
    console.log('DropdownList focused');
}

function onBlur(args) {
    console.log('DropdownList lost focus');
}
</script>
```

## Common Properties for Getting Started

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `Id` | string | required | Unique identifier for the control |
| `DataSource` | IEnumerable | null | Items to display |
| `Placeholder` | string | null | Hint text when no item selected |
| `Value` | string | null | Selected value |
| `Enabled` | bool | true | Enable/disable the dropdown |
| `ReadOnly` | bool | false | Prevent user interaction |
| `AllowFiltering` | bool | false | Enable search functionality |
| `PopupHeight` | string | "300px" | Height of the dropdown popup |

## CSS Classes for Styling

Add custom styling to DropdownList:

```csharp
@Html.EJ2().DropDownList()
    .Id("Status")
    .CssClass("custom-dropdown")
    .Render()

<style>
.custom-dropdown {
    width: 300px;
    font-size: 14px;
}

.e-dropdownlist .e-input {
    border-color: #0078d4;
}
</style>
```

## Width and Height Configuration

```csharp
@Html.EJ2().DropDownList()
    .Id("Categories")
    .Width("300px")
    .PopupHeight("250px")
    .Render()

<!-- Or using CSS -->
<style>
#Categories {
    width: 300px !important;
}
</style>
```

## Setting Default Value

```csharp
// Controller
ViewBag.SelectedStatus = "Active";

// View
@Html.EJ2().DropDownList()
    .Id("Status")
    .DataSource(ViewBag.StatusList)
    .Value(ViewBag.SelectedStatus)
    .Render()
```

## Icon CSS Support

Display icons next to items:

```csharp
public class IconItem
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string IconCss { get; set; }
}

// Controller
ViewBag.Items = new List<IconItem>
{
    new IconItem { Id = 1, Name = "Edit", IconCss = "e-icons e-edit" },
    new IconItem { Id = 2, Name = "Delete", IconCss = "e-icons e-delete" },
    new IconItem { Id = 3, Name = "Copy", IconCss = "e-icons e-copy" }
};

// View
@Html.EJ2().DropDownList()
    .Id("Actions")
    .DataSource(ViewBag.Items)
    .Fields(f => f.Text("Name").Value("Id").IconCss("IconCss"))
    .Render()
```

## Troubleshooting Getting Started

**Issue:** DropdownList not displaying
- **Solution:** Verify theme CSS is loaded, check browser console for JavaScript errors

**Issue:** DataSource not binding
- **Solution:** Ensure Fields property is correctly configured for complex objects

**Issue:** Events not firing
- **Solution:** Verify JavaScript functions are defined in global scope

---

## Next Steps

- Learn about [Data Binding](data-binding.md) for different data sources
- Explore [Filtering and Grouping](filtering-grouping.md) for enhanced user experience
- Discover [Templates and Styling](templates-styling.md) for customization
