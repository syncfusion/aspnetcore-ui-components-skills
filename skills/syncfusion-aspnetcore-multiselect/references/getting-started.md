# Getting Started with MultiSelect

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [First MultiSelect](#first-multiselect)
- [Testing](#testing)

## Prerequisites

Before implementing MultiSelect, ensure you have:
- Visual Studio 2019 or later
- ASP.NET Core 6.0 or higher
- .NET SDK 6.0 or higher
- Basic knowledge of Razor views and tag helpers

## Installation

### Step 1: Install NuGet Package

Open Package Manager Console and run:

```powershell
Install-Package Syncfusion.EJ2.AspNet.Core -Version 24.1.48
```

Or via .NET CLI:

```bash
dotnet add package Syncfusion.EJ2.AspNet.Core --version 24.1.48
```

### Step 2: Register License (Optional)

In `Program.cs`, register your Syncfusion license:

```csharp
using Syncfusion.Licensing;

var builder = WebApplication.CreateBuilder(args);

// Register Syncfusion license
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");

builder.Services.AddControllersWithViews();
var app = builder.Build();
```

## Configuration

### Step 1: Update _ViewImports.cshtml

Add the Syncfusion TagHelper reference at the top of `Views/_ViewImports.cshtml`:

```razor
@* Existing imports *@
@using YourNamespace
@using YourNamespace.Models

@* Add this line *@
@addTagHelper *, Syncfusion.EJ2
```

### Step 2: Add Resources in _Layout.cshtml

Add CSS and JavaScript resources to `Views/Shared/_Layout.cshtml`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>@ViewData["Title"] - Syncfusion MultiSelect</title>
    
    <!-- Syncfusion CSS -->
    <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/24.1.48/fluent.css" />
</head>
<body>
    @RenderBody()

    <!-- jQuery -->
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    
    <!-- Syncfusion Scripts -->
    <script src="https://cdn.syncfusion.com/ej2/24.1.48/ej2.min.js"></script>
    
    @RenderSection("Scripts", required: false)
</body>
</html>
```

### Step 3: Register Script Manager

In your view, add the script manager after the MultiSelect component:

```razor
<ejs-scriptmanager></ejs-scriptmanager>
```

This initializes Syncfusion controls on the page.

## First MultiSelect

### Basic Implementation

Create a simple MultiSelect with string array data:

**View (Index.cshtml):**

```razor
@addTagHelper *, Syncfusion.EJ2

<div class="container mt-4">
    <h2>Select Your Skills</h2>
    
    <form method="post">
        <ejs-multiselect id="skills" 
            dataSource="ViewBag.SkillsList" 
            placeholder="Select your skills"
            popupHeight="300px">
        </ejs-multiselect>
        
        <button type="submit" class="btn btn-primary mt-2">Submit</button>
    </form>
</div>

<ejs-scriptmanager></ejs-scriptmanager>
```

**Controller (HomeController.cs):**

```csharp
using Microsoft.AspNetCore.Mvc;

public class HomeController : Controller
{
    public IActionResult Index()
    {
        List<string> skills = new List<string>
        {
            "C#",
            "JavaScript",
            "Python",
            "Java",
            "TypeScript",
            "React",
            "Angular",
            "Vue.js"
        };
        
        ViewBag.SkillsList = skills;
        return View();
    }
    
    [HttpPost]
    public IActionResult Index(string[] skills)
    {
        // Process selected skills
        string selectedSkills = string.Join(", ", skills);
        ViewBag.Message = $"Selected skills: {selectedSkills}";
        return View();
    }
}
```

**CSS Styling (Optional - add to _Layout.cshtml):**

```css
<style>
    .container {
        max-width: 600px;
        margin: 0 auto;
    }
    
    #skills {
        width: 100%;
    }
</style>
```

## Object Data Binding

For more complex scenarios, bind to object arrays:

**View:**

```razor
<ejs-multiselect id="products" 
    dataSource="ViewBag.ProductList"
    fields-text="ProductName"
    fields-value="ProductId"
    placeholder="Select products"
    popupHeight="300px">
</ejs-multiselect>

<ejs-scriptmanager></ejs-scriptmanager>
```

**Model (Product.cs):**

```csharp
public class Product
{
    public int ProductId { get; set; }
    public string ProductName { get; set; }
    public decimal Price { get; set; }
}
```

**Controller:**

```csharp
public IActionResult Index()
{
    var products = new List<Product>
    {
        new Product { ProductId = 1, ProductName = "Laptop", Price = 999 },
        new Product { ProductId = 2, ProductName = "Mouse", Price = 25 },
        new Product { ProductId = 3, ProductName = "Keyboard", Price = 75 },
        new Product { ProductId = 4, ProductName = "Monitor", Price = 300 }
    };
    
    ViewBag.ProductList = products;
    return View();
}
```

## Complete Working Example

Here's a complete working example:

**Program.cs:**

```csharp
using Syncfusion.Licensing;

var builder = WebApplication.CreateBuilder(args);

// Register Syncfusion license
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");

builder.Services.AddControllersWithViews();
var app = builder.Build();

if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Home/Error");
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();
app.UseRouting();
app.UseAuthorization();

app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");

app.Run();
```

**HomeController.cs:**

```csharp
using Microsoft.AspNetCore.Mvc;

public class HomeController : Controller
{
    public IActionResult Index()
    {
        ViewBag.SkillsList = new List<string>
        {
            "C#", "JavaScript", "Python", "Java", "TypeScript", "React", "Angular"
        };
        return View();
    }
    
    [HttpPost]
    public IActionResult Index(string[] skills)
    {
        if (skills != null && skills.Length > 0)
        {
            ViewBag.Message = $"Selected {skills.Length} skills";
        }
        else
        {
            ViewBag.Message = "No skills selected";
        }
        
        ViewBag.SkillsList = new List<string>
        {
            "C#", "JavaScript", "Python", "Java", "TypeScript", "React", "Angular"
        };
        
        return View();
    }
}
```

**Views/Shared/_Layout.cshtml:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>MultiSelect Demo</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/24.1.48/fluent.css" />
    <style>
        body { padding: 20px; }
        .demo-section { max-width: 600px; margin: 0 auto; }
    </style>
</head>
<body>
    <div class="demo-section">
        @RenderBody()
    </div>
    
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script src="https://cdn.syncfusion.com/ej2/24.1.48/ej2.min.js"></script>
    @RenderSection("Scripts", required: false)
</body>
</html>
```

**Views/Home/Index.cshtml:**

```razor
@addTagHelper *, Syncfusion.EJ2

<h1>Skills Selector</h1>

@if (!string.IsNullOrEmpty(ViewBag.Message))
{
    <div class="alert alert-success">@ViewBag.Message</div>
}

<form method="post" class="mt-4">
    <div class="mb-3">
        <label for="skills" class="form-label">Select Your Skills:</label>
        <ejs-multiselect id="skills" 
            dataSource="ViewBag.SkillsList" 
            placeholder="Choose your skills"
            popupHeight="300px"
            allowFiltering="true">
        </ejs-multiselect>
    </div>
    
    <button type="submit" class="btn btn-primary">Submit</button>
</form>

<ejs-scriptmanager></ejs-scriptmanager>
```

## Testing

### Run the Application

```bash
dotnet run
```

Navigate to `https://localhost:5001/Home/Index`

### Verification Checklist

- [ ] MultiSelect dropdown displays
- [ ] Clicking opens the dropdown list
- [ ] Multiple items can be selected
- [ ] Selected items appear as chips/tags
- [ ] Clicking X on chip removes selection
- [ ] Form submission includes selected values
- [ ] No console errors in browser developer tools

### Common Issues

**Issue: MultiSelect doesn't render**
- Solution: Verify `@addTagHelper *, Syncfusion.EJ2` in _ViewImports.cshtml
- Solution: Ensure CSS/JS files are loaded via CDN

**Issue: Selected values not posting to controller**
- Solution: Check model binding with proper array parameter `string[] skills`
- Solution: Verify form method is POST

**Issue: Styling looks different**
- Solution: Verify correct theme CSS is loaded (fluent, material, bootstrap, etc.)
- Solution: Check for CSS conflicts with other frameworks

## Next Steps

✅ **Now that you have a working MultiSelect, explore:**
- [Tag Helper Syntax](tag-helper-syntax.md) - Learn all properties and events
- [Data Binding](data-binding.md) - Bind to complex data sources
- [Filtering and Search](filtering-and-search.md) - Add search functionality
- [Templates](templates-and-customization.md) - Customize appearance
- [Advanced Features](advanced-features.md) - Checkboxes, grouping, sorting
