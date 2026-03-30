# Getting Started with DatePicker in ASP.NET Core

## Table of Contents
- [Prerequisites](#prerequisites)
- [Install NuGet Package](#install-nuget-package)
- [Configure Tag Helpers](#configure-tag-helpers)
- [Add Resources](#add-resources)
- [Register Script Manager](#register-script-manager)
- [Create Your First DatePicker](#create-your-first-datepicker)
- [Model Binding](#model-binding)
- [Tag Helper Attributes](#tag-helper-attributes)
- [Troubleshooting](#troubleshooting)

## Prerequisites

Before implementing DatePicker, ensure you have:
- Visual Studio 2022 or later
- .NET 6.0 or higher (ASP.NET Core)
- Basic knowledge of Razor Pages or ASP.NET Core MVC
- C# and HTML understanding

## Install NuGet Package

### Option 1: Package Manager Console

Open Package Manager Console (Tools → NuGet Package Manager → Package Manager Console) and run:

```csharp
Install-Package Syncfusion.EJ2.AspNet.Core -Version 24.1.48
```

### Option 2: NuGet Package Manager UI

1. Right-click project → Manage NuGet Packages
2. Search for "Syncfusion.EJ2.AspNet.Core"
3. Click Install

### Option 3: .csproj Edit

Edit your `.csproj` file directly:

```xml
<ItemGroup>
    <PackageReference Include="Syncfusion.EJ2.AspNet.Core" Version="24.1.48" />
</ItemGroup>
```

**Dependencies installed automatically:**
- Newtonsoft.Json (JSON serialization)
- Syncfusion.Licensing (license validation)

## Configure Tag Helpers

### Step 1: Update _ViewImports.cshtml

Open `~/Pages/_ViewImports.cshtml` (Razor Pages) or `~/Views/_ViewImports.cshtml` (MVC) and add:

```csharp
@addTagHelper *, Syncfusion.EJ2
```

This enables all Syncfusion Tag Helpers in your views.

### Step 2: Verify _Layout.cshtml Integration

Confirm your `~/Pages/Shared/_Layout.cshtml` has the necessary references (see next section).

## Add Resources

### Step 1: Add Stylesheet (CSS)

In `~/Pages/Shared/_Layout.cshtml` (or your layout file), add the theme CSS in the `<head>` section:

```html
<head>
    <!-- Other meta tags -->
    
    <!-- Syncfusion EJ2 Fluent Theme CSS -->
    <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/24.1.48/fluent.css" />
    
    <!-- Alternative themes (choose one):
    <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/24.1.48/bootstrap.css" />
    <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/24.1.48/material.css" />
    <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/24.1.48/highcontrast.css" />
    -->
</head>
```

### Step 2: Add JavaScript Resources

In `~/Pages/Shared/_Layout.cshtml`, add the script at the end of the `<body>` section (before `<ejs-scripts>`):

```html
<body>
    <!-- Your page content -->
    
    <!-- Syncfusion EJ2 JavaScript -->
    <script src="https://cdn.syncfusion.com/ej2/24.1.48/dist/ej2.min.js"></script>
    
    <!-- Page scripts -->
    @await RenderSectionAsync("Scripts", required: false)
</body>
```

### Theme Options

| Theme | URL | Best For |
|-------|-----|----------|
| Fluent | `fluent.css` | Modern, clean design |
| Bootstrap | `bootstrap.css` | Bootstrap framework integration |
| Material | `material.css` | Material Design look |
| High Contrast | `highcontrast.css` | Accessibility, vision impairment |
| Tailwind | `tailwind.css` | Tailwind CSS projects |

## Register Script Manager

The Syncfusion script manager must be registered at the end of `_Layout.cshtml`:

```html
</head>
<body>
    <!-- Your content -->
    
    <!-- Syncfusion ASP.NET Core Script Manager (END OF BODY) -->
    <ejs-scripts></ejs-scripts>
</body>
</html>
```

**Important:** Place `<ejs-scripts>` AFTER all page content but BEFORE the closing `</body>` tag.

## Create Your First DatePicker

### Basic Example (Razor Page)

**PageModel (BookingModel.cshtml.cs):**

```csharp
using Microsoft.AspNetCore.Mvc.RazorPages;
using System;

namespace DatePickerApp.Pages
{
    public class BookingModel : PageModel
    {
        [BindProperty]
        public DateTime AppointmentDate { get; set; } = DateTime.Today;

        public void OnGet()
        {
            // Initialize or load data
        }

        public void OnPost()
        {
            // Process the form with AppointmentDate
            if (ModelState.IsValid)
            {
                // Save appointment
                System.Diagnostics.Debug.WriteLine($"Appointment Date: {AppointmentDate:yyyy-MM-dd}");
            }
        }
    }
}
```

**View (BookingModel.cshtml):**

```html
@page
@model DatePickerApp.Pages.BookingModel

<div class="container mt-5">
    <h1>Book Your Appointment</h1>
    
    <form method="post">
        <!-- DatePicker Input -->
        <div class="form-group">
            <label for="appointmentDate">Select Date:</label>
            <ejs-datepicker id="appointmentDate"
                            asp-for="AppointmentDate"
                            placeholder="Choose appointment date">
            </ejs-datepicker>
        </div>
        
        <!-- Submit Button -->
        <button type="submit" class="btn btn-primary">Book Appointment</button>
    </form>
    
    @if (!string.IsNullOrEmpty(ViewData["Message"]?.ToString()))
    {
        <div class="alert alert-success mt-3">
            @ViewData["Message"]
        </div>
    }
</div>

<style>
    .form-group {
        margin-bottom: 20px;
    }
    
    label {
        display: block;
        margin-bottom: 5px;
        font-weight: bold;
    }
</style>
```

**Rendered Output:**
```
A text input with calendar icon allowing date selection through popup calendar.
Bound value accessible in PageModel via AppointmentDate property.
```

## Model Binding

### Two-Way Binding with asp-for

The `asp-for` attribute automatically binds the DatePicker to your model property:

```csharp
// PageModel
public DateTime BirthDate { get; set; }
public DateTime? OptionalDate { get; set; }  // Nullable for optional dates
```

```html
<!-- Razor Page -->
<ejs-datepicker asp-for="BirthDate"></ejs-datepicker>
<ejs-datepicker asp-for="OptionalDate"></ejs-datepicker>
```

**Binding Features:**
- Automatic `id` and `name` generation from property name
- Automatic `value` binding from model
- Server-side validation integration
- Form submission with selected date value

### Nullable DateTime Handling

For optional date selections, use `DateTime?`:

```csharp
public DateTime? ReleaseDate { get; set; }  // Can be null

public void OnPost()
{
    if (ReleaseDate.HasValue)
    {
        // Date selected
    }
    else
    {
        // No date selected
    }
}
```

## Tag Helper Attributes

### Common Attributes

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `asp-for` | string | - | Model property binding (required) |
| `value` | DateTime | Today | Initial selected date |
| `placeholder` | string | "" | Input placeholder text |
| `readonly` | bool | false | Disable editing, display only |
| `enabled` | bool | true | Enable/disable the component |
| `format` | string | Culture dependent | Display date format |
| `input-formats` | string[] | - | Accepted input formats |
| `min` | DateTime | - | Minimum selectable date |
| `max` | DateTime | - | Maximum selectable date |
| `start-view` | string | "Month" | Initial calendar view |
| `strict-mode` | bool | false | Enforce strict validation |

### Example with Multiple Attributes

```html
<ejs-datepicker id="appointmentPicker"
                 asp-for="AppointmentDate"
                 min="@DateTime.Today"
                 max="@DateTime.Today.AddMonths(1)"
                 format="yyyy-MM-dd"
                 placeholder="mm/dd/yyyy"
                 readonly="false"
                 enabled="true">
</ejs-datepicker>
```

## Practical Scenarios

### Scenario 1: Past Date Selection (Birth Date)

```csharp
public class PersonModel : PageModel
{
    [BindProperty]
    public DateTime BirthDate { get; set; }
}
```

```html
<ejs-datepicker asp-for="BirthDate"
                 max="@DateTime.Today"
                 format="MM/dd/yyyy"
                 placeholder="Birth date">
</ejs-datepicker>
```

### Scenario 2: Future Date Selection (Appointment)

```csharp
public class AppointmentModel : PageModel
{
    [BindProperty]
    public DateTime AppointmentDate { get; set; }
}
```

```html
<ejs-datepicker asp-for="AppointmentDate"
                 min="@DateTime.Today"
                 max="@DateTime.Today.AddDays(90)"
                 format="yyyy-MM-dd"
                 placeholder="Select appointment within 90 days">
</ejs-datepicker>
```

### Scenario 3: Custom Format

```html
<!-- Display as "March 19, 2026" -->
<ejs-datepicker asp-for="EventDate"
                 format="MMMM dd, yyyy"
                 placeholder="Full date format">
</ejs-datepicker>
```

## Troubleshooting

### Issue: "Tag Helper not recognized"
**Solution:** Verify `@addTagHelper *, Syncfusion.EJ2` is in `_ViewImports.cshtml`

### Issue: "DatePicker not rendering"
**Solution:** Ensure `<ejs-scripts>` is present at end of `_Layout.cshtml`

### Issue: "CSS/Styling missing"
**Solution:** Verify theme CSS link in `<head>` section of layout file

### Issue: "Binding not working (asp-for shows null)"
**Solution:** Ensure property name matches exactly, use `[BindProperty]` attribute

### Issue: "Date value not submitting"
**Solution:** Verify property is `DateTime` (not string) for proper binding

---

**Next:** [Date Formats and Parsing](../date-formats-and-parsing.md) - Learn custom date formatting
