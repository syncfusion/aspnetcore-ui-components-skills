# Getting Started with ComboBox

## Table of Contents
- [Installation](#installation)
- [Basic Setup](#basic-setup)
- [TagHelper Registration](#taghelper-registration)
- [Component Implementation](#component-implementation)
- [Custom Values](#custom-values)
- [Popup Configuration](#popup-configuration)

---

## Installation

### Step 1: Install NuGet Package

Install the Syncfusion ASP.NET Core package via NuGet Package Manager Console:

```powershell
Install-Package Syncfusion.EJ2.AspNet.Core
```

Or using .NET CLI:

```bash
dotnet add package Syncfusion.EJ2.AspNet.Core
```

### Step 2: Verify Installation

Check the `.csproj` file for the package reference:

```xml
<PackageReference Include="Syncfusion.EJ2.AspNet.Core" Version="*" />
```

---

## Basic Setup

### Register Service in Startup

In `Program.cs`, add Syncfusion services:

```csharp
var builder = WebApplication.CreateBuilder(args);

// Add Syncfusion services
builder.Services.AddSyncfusionEJ2();

var app = builder.Build();
app.Run();
```

---

## TagHelper Registration

### Add TagHelper Reference

Edit `_ViewImports.cshtml` to include the Syncfusion TagHelper:

```cshtml
@addTagHelper *, Syncfusion.EJ2
```

### Available TagHelper Namespace

The ComboBox component is available under the `Syncfusion.EJ2` namespace.

---

## Component Implementation

### Basic ComboBox with Static Data

**Controller (`HomeController.cs`):**

```csharp
public IActionResult Index()
{
    ViewBag.sportsList = new List<string>
    {
        "Badminton",
        "Cricket",
        "Football",
        "Golf",
        "Tennis"
    };
    return View();
}
```

**View (`Index.cshtml`):**

```cshtml
<ejs-combobox id="sports-combo"
    dataSource="@ViewBag.sportsList"
    placeholder="Choose a sport...">
</ejs-combobox>
```

### ComboBox with Object Data

**Controller (`HomeController.cs`):**

```csharp
public IActionResult Index()
{
    ViewBag.gamesList = new List<object>
    {
        new { Id = "game1", Name = "Chess" },
        new { Id = "game2", Name = "Carrom" },
        new { Id = "game3", Name = "Badminton" }
    };
    return View();
}
```

**View (`Index.cshtml`):**

```cshtml
<ejs-combobox id="games-combo"
    dataSource="@ViewBag.gamesList"
    placeholder="Select or type a game...">
    <e-combobox-fields text="Name" value="Id"></e-combobox-fields>
</ejs-combobox>
```

---

## Custom Values

### Enable User-Defined Values

By default, `allowCustom="true"` lets users enter values not in the dropdown list.

```cshtml
<ejs-combobox id="games-combo"
    dataSource="@ViewBag.gamesList"
    allowCustom="true"
    placeholder="Select or type a game..."
    change="onComboBoxChange">
    <e-combobox-fields text="Name" value="Id"></e-combobox-fields>
</ejs-combobox>

<script>
function onComboBoxChange(args) {
    console.log('Selected value:', args.value);
    console.log('Custom value:', args.isCustom);
}
</script>
```

**Behavior:**
- ✅ User can select from list
- ✅ User can type and create a new value
- ✅ Custom value is submitted with form

**Example Workflow:**
1. User types "Scrabble" (not in list)
2. Presses Enter or leaves field
3. "Scrabble" becomes the selected value
4. Form submission includes "Scrabble"

---

## Popup Configuration

### Customize Dropdown Appearance

Control the popup list size and behavior:

```cshtml
<ejs-combobox id="combobox"
    dataSource="@ViewBag.largeDataset"
    placeholder="Select an item..."
    popupHeight="250px"
    popupWidth="300px"
    allowFiltering="true">
</ejs-combobox>
```

### Responsive Popup

For mobile-friendly design, use percentage-based widths:

```cshtml
<ejs-combobox id="combobox"
    dataSource="@ViewBag.sportsList"
    popupHeight="auto"
    popupWidth="100%"
    placeholder="Choose a sport...">
</ejs-combobox>
```

### Default Popup Behavior

| Property | Default | Purpose |
|----------|---------|---------|
| `popupHeight` | 300px | Dropdown height with scrollbar |
| `popupWidth` | 100% | Dropdown width (matches input width) |
| `showClearButton` | true | Show X button to clear selection |

---

## First Implementation Checklist

- [ ] NuGet package installed: `Syncfusion.EJ2.AspNet.Core`
- [ ] Service registered in `Program.cs`: `builder.Services.AddSyncfusionEJ2();`
- [ ] TagHelper added to `_ViewImports.cshtml`: `@addTagHelper *, Syncfusion.EJ2`
- [ ] Script Manager in layout: Included in `_Layout.cshtml` or per-page view
- [ ] ComboBox component added to view with `<ejs-combobox>`
- [ ] dataSource populated with data via controller `ViewBag` or model
- [ ] placeholder text added for UX
- [ ] change event handler (optional) for selection events
- [ ] Application runs: Build and run project, navigate to view
- [ ] ComboBox appears and is clickable

---

## Next Steps

- **Binding complex data?** → Go to [data-binding.md](combobox-data-binding.md)
- **Want search capability?** → See [filtering-and-search.md](combobox-filtering-and-search.md)
- **Need grouping?** → Check [grouping-and-sorting.md](combobox-grouping-and-sorting.md)
- **Want custom UI?** → Explore [templates-and-customization.md](combobox-templates-and-customization.md)
