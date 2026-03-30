# Getting Started with ASP.NET Core ListBox

## Table of Contents
- [Prerequisites](#prerequisites)
- [NuGet Installation](#nuget-installation)
- [Project Setup](#project-setup)
- [TagHelper Registration](#taghelper-registration)
- [Stylesheet and Script Resources](#stylesheet-and-script-resources)
- [Script Manager Registration](#script-manager-registration)
- [Basic ListBox Implementation](#basic-listbox-implementation)
- [Data Binding Basics](#data-binding-basics)

## Prerequisites

Before starting, ensure your system meets these requirements:

- **.NET SDK**: ASP.NET Core 6.0 or later
- **Visual Studio**: 2022 or later (recommended)
- **Package Manager**: NuGet (included with Visual Studio)
- **Browser**: Modern browser with JavaScript enabled

Check [System Requirements](https://ej2.syncfusion.com/aspnetcore/documentation/system-requirements) for detailed compatibility information.

## NuGet Installation

### Step 1: Open NuGet Package Manager

In Visual Studio:
1. **Tools** → **NuGet Package Manager** → **Manage NuGet Packages for Solution**
2. Search for `Syncfusion.EJ2.AspNet.Core`
3. Select the latest version
4. Click **Install**

### Step 2: Package Manager Console (Alternative)

```csharp
Install-Package Syncfusion.EJ2.AspNet.Core -Version 25.1.35
```

Replace `25.1.35` with the current version.

### Dependencies

The Syncfusion.EJ2.AspNet.Core package automatically installs:
- **Newtonsoft.Json** - For JSON serialization
- **Syncfusion.Licensing** - For license validation

**Note:** Add your Syncfusion license key to `appsettings.json` to avoid license warnings:

```json
{
  "Syncfusion": {
    "LicenseKey": "YOUR_LICENSE_KEY_HERE"
  }
}
```

## Project Setup

### Create ASP.NET Core Project (If New)

Using **Microsoft Templates**:
1. **File** → **New** → **Project**
2. Select **ASP.NET Core Web App (Razor Pages)**
3. Configure project name and location
4. Select **.NET 6.0+** as target framework
5. Click **Create**

Using **Syncfusion Extension** (recommended):
- Use Syncfusion's Visual Studio extension for pre-configured templates
- Automatically includes theme and script references

## TagHelper Registration

TagHelpers enable clean CSHTML syntax for Syncfusion controls.

### Register in _ViewImports.cshtml

```cshtml
<!-- ~/Pages/_ViewImports.cshtml -->
@addTagHelper *, Syncfusion.EJ2
```

**Location:** This file must be in the `~/Pages/` directory for Razor Pages applications.

**Result:** Now you can use `<ejs-*>` tags throughout your application.

### Usage Example

```cshtml
<!-- Now available in any .cshtml file -->
<ejs-listbox id="myListBox"></ejs-listbox>
```

## Stylesheet and Script Resources

### Add to Layout File

Open `~/Pages/Shared/_Layout.cshtml` and add resources in the `<head>` and `<body>`:

```cshtml
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>@ViewData["Title"]</title>
    
    <!-- Syncfusion Theme CSS (CDN) -->
    <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/25.1.35/fluent.css" />
    
    <!-- Your custom styles -->
    <link rel="stylesheet" href="~/css/site.css" />
</head>

<body>
    <!-- Page content -->
    @RenderBody()
    
    <!-- Syncfusion EJ2 Script (CDN) -->
    <script src="https://cdn.syncfusion.com/ej2/25.1.35/dist/ej2.min.js"></script>
    
    <!-- Your custom scripts -->
    <script src="~/js/site.js"></script>
    
    @RenderSection("Scripts", required: false)
</body>
</html>
```

### Theme Options

Available themes from CDN:
- `fluent.css` - Modern, clean design
- `bootstrap4.css` - Bootstrap 4 style
- `bootstrap5.css` - Bootstrap 5 style
- `material.css` - Material Design
- `tailwind.css` - Tailwind CSS inspired
- `highcontrast.css` - Accessibility focused

### Using NPM Packages (Alternative)

```bash
npm install @syncfusion/ej2-dropdowns @syncfusion/ej2-base
```

Import in your TypeScript/JavaScript:

```javascript
import '@syncfusion/ej2-base/styles/material.css';
import { ListBox } from '@syncfusion/ej2-dropdowns';
```

## Script Manager Registration

The script manager ensures proper initialization of all Syncfusion components.

### Add to Layout File

```cshtml
<body>
    <!-- Page content -->
    @RenderBody()
    
    <!-- Other scripts -->
    <script src="https://cdn.syncfusion.com/ej2/25.1.35/dist/ej2.min.js"></script>
    
    <!-- CRITICAL: Syncfusion Script Manager -->
    <ejs-scripts></ejs-scripts>
</body>
```

**Must be placed:**
- After all control declarations
- Before closing `</body>` tag
- Only once per page

**If missing:** Controls won't initialize; you'll see JavaScript errors in console.

## Basic ListBox Implementation

### 1. Create Model/Data

```csharp
// ~/Pages/Index.cshtml.cs
using Microsoft.AspNetCore.Mvc.RazorPages;
using System.Collections.Generic;

public class IndexModel : PageModel
{
    public List<string> Fruits { get; set; }
    
    public void OnGet()
    {
        Fruits = new List<string> 
        { 
            "Apple", 
            "Banana", 
            "Cherry", 
            "Date", 
            "Elderberry",
            "Fig",
            "Grape"
        };
    }
}
```

### 2. Add ListBox to View

```cshtml
<!-- ~/Pages/Index.cshtml -->
@page
@model IndexModel

<h2>Select Your Favorite Fruit</h2>

<ejs-listbox id="fruitListBox" 
             dataSource="@Model.Fruits"
             placeholder="Choose a fruit...">
</ejs-listbox>

<!-- Script to access the component -->
@section Scripts {
    <script>
        var listboxInstance = document.getElementById('fruitListBox').ej2_instances[0];
    </script>
}
```

### 3. Run Application

```powershell
dotnet run
```

The ListBox will display in your browser with all fruits listed.

## Data Binding Basics

### Simple Array Binding

```cshtml
<!-- Simple string array -->
<ejs-listbox id="simpleList" dataSource="@new List<string> { 'Item 1', 'Item 2', 'Item 3' }">
</ejs-listbox>
```

### Object Array Binding

For complex data, use objects with text/value properties:

```csharp
// Model
public class Item
{
    public string Text { get; set; }
    public string Value { get; set; }
}

// Page Model
public List<Item> Items { get; set; }

public void OnGet()
{
    Items = new List<Item>
    {
        new Item { Text = "Option 1", Value = "val1" },
        new Item { Text = "Option 2", Value = "val2" }
    };
}
```

```cshtml
<!-- CSHTML -->
<ejs-listbox id="objectList" 
             dataSource="@Model.Items"
             fields="@(new Syncfusion.EJ2.DropDowns.ListBoxFieldSettings 
             { 
                 Text = "Text", 
                 Value = "Value" 
             })">
</ejs-listbox>
```

### Accessing Selected Value

```cshtml
<ejs-listbox id="myListBox" dataSource="@Model.Items" change="onListBoxChange"></ejs-listbox>

<script>
    function onListBoxChange(args) {
        var selectedValue = args.value;
        var selectedText = args.text;
        console.log('Selected: ' + selectedText + ' (' + selectedValue + ')');
    }
</script>
```

## Common Issues and Solutions

| Issue | Solution |
|-------|----------|
| ListBox not appearing | Verify CDN scripts/styles are loaded; check browser console for errors |
| TagHelper not recognized | Ensure `@addTagHelper` is in `_ViewImports.cshtml` |
| Data not binding | Check property names match in C# model and CSHTML |
| Events not firing | Ensure `ejs-scripts` manager is registered |
| Styling looks wrong | Verify theme CSS file is loaded before custom styles |

## Next Steps

- **Selection Modes** → Configure single or multiple selection in [references/selection-modes.md](selection-modes.md)
- **Data Binding** → Learn advanced binding in [references/data-binding.md](data-binding.md)
- **Styling** → Customize appearance in [references/styling-and-appearance.md](styling-and-appearance.md)
- **Features** → Explore drag-drop, templates in [references/features.md](features.md)
