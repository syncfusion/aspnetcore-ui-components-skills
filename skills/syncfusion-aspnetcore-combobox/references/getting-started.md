# Getting Started with ASP.NET Core ComboBox

## Overview

This guide covers installation, setup, and creating your first ComboBox control in an ASP.NET Core application.

## Installation & Prerequisites

### System Requirements

- .NET 5.0 or higher
- Visual Studio 2019 or later (or VS Code with C# extensions)
- Node.js 14+ (for build tools, optional)

### Install Syncfusion NuGet Package

1. **Via NuGet Package Manager (GUI):**
   - Open Visual Studio
   - Tools → NuGet Package Manager → Manage NuGet Packages for Solution
   - Search for `Syncfusion.EJ2.AspNet.Core`
   - Click Install

2. **Via Package Manager Console:**
   ```powershell
   Install-Package Syncfusion.EJ2.AspNet.Core
   ```

3. **Via .NET CLI:**
   ```bash
   dotnet add package Syncfusion.EJ2.AspNet.Core
   ```

> **Note:** This package includes dependencies on `Newtonsoft.Json` and `Syncfusion.Licensing`

## Project Setup

### Step 1: Add Tag Helper Reference

Open `~/Pages/_ViewImports.cshtml` (for Razor Pages) or `~/Views/Web.config` (for MVC):

```csharp
@addTagHelper *, Syncfusion.EJ2
```

This enables all Syncfusion Tag Helpers in your views.

### Step 2: Add Styles & Scripts

Edit `~/Pages/Shared/_Layout.cshtml` to include Syncfusion resources in the `<head>`:

```html
<head>
    ...
    <!-- Syncfusion ASP.NET Core styles -->
    <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/fluent.css" />
    <!-- Syncfusion ASP.NET Core scripts -->
    <script src="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/dist/ej2.min.js"></script>
</head>
```

In the `<body>` footer, add the script manager:

```html
<body>
    ...
    <!-- Syncfusion ASP.NET Core Script Manager -->
    <ejs-scripts></ejs-scripts>
</body>
```

> **Theme Alternatives:** Replace `fluent.css` with other themes:
> - `bootstrap5.css` - Bootstrap 5 theme
> - `tailwind.css` - Tailwind theme
> - `material.css` - Material Design theme

### Step 3: Create Your First ComboBox

Add this to your page (e.g., `Index.cshtml`):

```html
@{
    List<string> sports = new List<string> 
    { 
        "Cricket", "Badminton", "Basketball", "Tennis", "Football" 
    };
}

<div class="col-lg-8 control-section">
    <div class="control-wrapper">
        <label>Select a Sport:</label>
        <ejs-combobox id="games" 
                      dataSource="@sports" 
                      placeholder="Choose a sport">
        </ejs-combobox>
    </div>
</div>
```

### Step 4: Run Your Application

```bash
dotnet run
```

Visit `https://localhost:5001` and you should see a ComboBox dropdown with the sports list.

## Minimal Working Example

Here's a complete minimal example:

**Index.cshtml:**
```html
@page
@{
    ViewData["Title"] = "ComboBox Example";
    var countries = new List<object>
    {
        new { Name = "United States", Code = "US" },
        new { Name = "United Kingdom", Code = "UK" },
        new { Name = "India", Code = "IN" }
    };
}

<h1>Select a Country</h1>

<ejs-combobox id="country"
              dataSource="@countries"
              fields-text="Name"
              fields-value="Code"
              placeholder="Choose a country"
              change="onChange">
</ejs-combobox>

<p>Selected: <span id="selected">None</span></p>

<script>
function onChange(args) {
    document.getElementById('selected').textContent = args.itemData?.Name || 'None';
}
</script>
```

**Important:** The `fields-text` and `fields-value` properties map to your data object properties.

## Binding Data from Controller

**HomeController.cs:**
```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        var countries = new List<Country>
        {
            new Country { Id = 1, CountryName = "United States" },
            new Country { Id = 2, CountryName = "United Kingdom" },
            new Country { Id = 3, CountryName = "India" }
        };
        return View(countries);
    }
}

public class Country
{
    public int Id { get; set; }
    public string CountryName { get; set; }
}
```

**View (Index.cshtml):**
```html
@model List<Country>

<ejs-combobox id="country"
              dataSource="@Model"
              fields-text="CountryName"
              fields-value="Id"
              placeholder="Select a country">
</ejs-combobox>
```

## Key Tag Helper Attributes

| Attribute | Type | Description |
|-----------|------|-------------|
| `dataSource` | array or List | Data items to display |
| `fields-text` | string | Property name for display text |
| `fields-value` | string | Property name for selected value |
| `placeholder` | string | Hint text |
| `id` | string | Unique control ID |
| `enabled` | bool | Enable/disable control |
| `read-only` | bool | Read-only mode (no typing) |
| `allowFiltering` | bool | Show search field |
| `allowCustom` | bool | Allow user-entered values |

## CSS Styling

Add basic styling to your ComboBox:

```html
<style>
    .e-combobox {
        width: 300px;
    }
    
    .e-combobox.e-outline .e-input-group {
        border: 1px solid #ccc;
        border-radius: 4px;
    }
</style>
```

## Common Issues & Solutions

**Issue 1: ComboBox not rendering**
- ✓ Check that `_ViewImports.cshtml` has `@addTagHelper *, Syncfusion.EJ2`
- ✓ Verify CSS/script tags in `_Layout.cshtml` are loaded
- ✓ Check browser console for JavaScript errors

**Issue 2: Data not showing in dropdown**
- ✓ Verify `fields-text` and `fields-value` match your data property names exactly
- ✓ Check that `dataSource` is correctly populated (not null or empty)
- ✓ Inspect browser network tab to confirm data was sent to view

**Issue 3: Selected value not persisting**
- ✓ Use both `fields-text` and `fields-value` if they differ
- ✓ Ensure selected value matches a value in your data source
- ✓ Check the `change` event is firing (add `console.log(args)`)

## Next Steps

- **Data Binding:** Learn to bind from remote sources and complex objects → [data-binding.md](data-binding.md)
- **Filtering:** Enable search functionality → [filtering-and-search.md](filtering-and-search.md)
- **Templates:** Customize list item appearance → [templates-and-customization.md](templates-and-customization.md)
