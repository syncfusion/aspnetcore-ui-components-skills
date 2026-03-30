# Getting Started with QueryBuilder for ASP.NET Core

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [CSS and Script Setup](#css-and-script-setup)
- [TagHelper Registration](#taghelper-registration)
- [Basic Initialization](#basic-initialization)
- [Initial Data Binding](#initial-data-binding)
- [Troubleshooting](#troubleshooting)

## Prerequisites

Before implementing QueryBuilder in an ASP.NET Core application, ensure you have:

- **Visual Studio 2017 or later** (Community, Professional, or Enterprise editions)
- **.NET 5.0 or higher** (or .NET Core 3.1+)
- **ASP.NET Core** framework installed
- Basic knowledge of Razor Pages or MVC syntax and C#
- **NuGet Package Manager** available in Visual Studio

## Installation

### Step 1: Install Syncfusion NuGet Package

Open **Package Manager Console** in Visual Studio:
- Go to: `Tools → NuGet Package Manager → Package Manager Console`
- Run the following command:

```powershell
Install-Package Syncfusion.EJ2.AspNet.Core -Version 23.1.36
```

> **Note:** Replace the version number with the latest available from [NuGet.org](https://www.nuget.org/packages/Syncfusion.EJ2.AspNet.Core)

Alternatively, use the **NuGet Package Manager UI:**
1. Right-click on project → Manage NuGet Packages
2. Search for "Syncfusion.EJ2.AspNet.Core"
3. Click Install

### Step 2: Verify Installation

After installation, check that these assemblies are added to your project:
- `Syncfusion.EJ2.dll`
- `Syncfusion.EJ2.AspNet.Core.dll`
- `Newtonsoft.Json.dll` (dependency)

## CSS and Script Setup

### Option 1: CDN Links (Recommended for Quick Start)

Add the following to your `_Layout.cshtml` file in the `<head>` section:

```html
<!-- Syncfusion EJ2 CSS Styles -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/23.1.36/fluent.css" />

<!-- Optional: Different Themes Available -->
<!-- <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/23.1.36/material.css" /> -->
<!-- <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/23.1.36/bootstrap.css" /> -->
```

Add the following to the `<body>` section (at the end):

```html
<!-- Syncfusion EJ2 Scripts -->
<script src="https://cdn.syncfusion.com/ej2/23.1.36/dist/ej2.min.js"></script>
```

### Option 2: Local Files (NPM Package)

Install via NPM in your project root:

```bash
npm install @syncfusion/ej2
```

Update `_Layout.cshtml`:

```html
<head>
    <link rel="stylesheet" href="~/lib/ej2/fluent.css" />
</head>

<body>
    <!-- Content -->
    <script src="~/lib/ej2/ej2.min.js"></script>
</body>
```

### Available Themes

- `fluent.css` — Modern Fluent design (default)
- `material.css` — Material Design
- `bootstrap.css` — Bootstrap styling
- `bootstrap4.css` — Bootstrap 4 theme
- `fabric.css` — Microsoft Fabric design
- `tailwind.css` — Tailwind CSS theme

## TagHelper Registration

TagHelpers allow you to use Syncfusion components with HTML-like syntax.

### Step 1: Register TagHelpers in _ViewImports.cshtml

Create or update `Views/_ViewImports.cshtml` with:

```html
@addTagHelper *, Syncfusion.EJ2
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
```

**Location:** `Views/_ViewImports.cshtml` (or `Pages/_ViewImports.cshtml` for Razor Pages)

### Step 2: Configure Services in Program.cs

In `Program.cs` (.NET 6+) or `Startup.cs` (.NET 5), add Syncfusion services:

**For .NET 6+ (Program.cs):**
```csharp
var builder = WebApplication.CreateBuilder(args);

// Add Syncfusion services
builder.Services.AddSyncfusionBlazor();

// Add MVC services
builder.Services.AddControllersWithViews();

var app = builder.Build();

// Configure the HTTP request pipeline
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Home/Error");
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();
app.UseRouting();

app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");

app.Run();
```

**For .NET 5 (Startup.cs):**
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSyncfusionBlazor();
    services.AddControllersWithViews();
}

public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllerRoute(
            name: "default",
            pattern: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

## Basic Initialization

### Step 1: Create a Simple QueryBuilder with TagHelpers

In your controller (`HomeController.cs`):

```csharp
using Syncfusion.EJ2.QueryBuilder;

public class HomeController : Controller
{
    public IActionResult Index()
    {
        // Define columns
        List<QueryBuilderColumn> columns = new List<QueryBuilderColumn>
        {
            new QueryBuilderColumn { Field = "EmployeeID", Label = "Employee ID", Type = "number" },
            new QueryBuilderColumn { Field = "FirstName", Label = "First Name", Type = "string" },
            new QueryBuilderColumn { Field = "HireDate", Label = "Hire Date", Type = "date" },
            new QueryBuilderColumn { Field = "Salary", Label = "Salary", Type = "number" }
        };

        // Sample data
        List<Employee> data = new List<Employee>
        {
            new Employee { EmployeeID = 1, FirstName = "Nancy", HireDate = new DateTime(1992, 4, 1), Salary = 60000 },
            new Employee { EmployeeID = 2, FirstName = "Andrew", HireDate = new DateTime(1994, 6, 15), Salary = 65000 },
            new Employee { EmployeeID = 3, FirstName = "Janet", HireDate = new DateTime(1991, 11, 22), Salary = 62000 }
        };

        ViewBag.Columns = columns;
        ViewBag.Data = data;
        return View();
    }
}

public class Employee
{
    public int EmployeeID { get; set; }
    public string FirstName { get; set; }
    public DateTime HireDate { get; set; }
    public decimal Salary { get; set; }
}
```

### Step 2: Create View with TagHelper Syntax

In your view (`Index.cshtml`):

```html
@{
    ViewBag.Title = "QueryBuilder Example";
}

<h2>Employee Query Builder</h2>

<div id="querybuilder-container" style="width: 100%; max-width: 900px; margin: 20px auto;">
    <ejs-querybuilder id="querybuilder"
        columns="(IEnumerable<QueryBuilderColumn>)ViewBag.Columns"
        datasource="(IEnumerable<object>)ViewBag.Data">
        <e-querybuilder-rule condition="and"></e-querybuilder-rule>
    </ejs-querybuilder>
</div>

@section Scripts {
    <script>
        document.addEventListener('DOMContentLoaded', function () {
            var qbElement = document.getElementById("querybuilder");
            var queryBuilder = qbElement.ej2_instances[0];
            console.log("QueryBuilder initialized successfully");
        });
    </script>
}
```

## Initial Data Binding

### Binding Local Data

Local data is the simplest approach for testing and small datasets:

```csharp
// Controller
List<Employee> employees = new List<Employee>
{
    new Employee { EmployeeID = 1, FirstName = "Nancy", Salary = 60000 },
    new Employee { EmployeeID = 2, FirstName = "Andrew", Salary = 65000 }
};

ViewBag.Data = employees;
```

```html
<!-- View -->
<ejs-querybuilder id="querybuilder"
    columns="(IEnumerable<QueryBuilderColumn>)ViewBag.Columns"
    datasource="(IEnumerable<object>)ViewBag.Data">
</ejs-querybuilder>
```

**Result:** QueryBuilder displays all employees with filtering on any column.

### Binding Remote Data (DataManager)

For larger datasets, use DataManager with a remote endpoint:

```html
<!-- View -->
<ejs-querybuilder id="querybuilder"
    columns="(IEnumerable<QueryBuilderColumn>)ViewBag.Columns">
    <e-datamanager url="/api/employees" adaptor="JsonAdaptor"></e-datamanager>
</ejs-querybuilder>
```

API endpoint (`EmployeesController.cs`):

```csharp
[HttpGet]
[Route("api/employees")]
public JsonResult GetEmployees()
{
    List<Employee> employees = new List<Employee>
    {
        new Employee { EmployeeID = 1, FirstName = "Nancy", Salary = 60000 },
        new Employee { EmployeeID = 2, FirstName = "Andrew", Salary = 65000 }
    };
    
    return Json(employees);
}
```

## Troubleshooting

### Issue: "QueryBuilder is not defined"
**Cause:** Script files not loaded correctly or TagHelpers not registered  
**Solution:**
1. Verify CDN link is in `<head>` or before closing `</body>`
2. Check browser console for 404 errors on CSS/JS files
3. Ensure version number matches between CSS and JS links
4. Verify TagHelpers registered in `_ViewImports.cshtml`

### Issue: "Column field is not rendering"
**Cause:** Column field doesn't match data source property names  
**Solution:**
1. Verify field names are case-sensitive
2. Ensure data contains all defined columns
3. Check field type matches data type (string, number, date, boolean)

### Issue: "TagHelpers not recognized"
**Cause:** `_ViewImports.cshtml` missing or incorrect registration  
**Solution:**
1. Ensure `@addTagHelper *, Syncfusion.EJ2` is in `_ViewImports.cshtml`
2. Verify file is in correct location (Views/ or Pages/ folder)
3. Clean solution and rebuild
4. Restart Visual Studio if changes don't take effect

### Issue: "Add/Delete buttons not showing"
**Cause:** ShowButtons property not enabled or CSS not loaded  
**Solution:**
1. Explicitly set `ShowButtons` property with desired buttons
2. Verify CSS is loaded before script
3. Check browser console for styling errors

---

## Next Steps

- Review [Data Binding](data-binding.md) to connect remote data sources or OData services
- Explore [Columns and Operators](columns-and-operators.md) to customize filtering options
- Check [Templates and Customization](templates-and-customization.md) for UI customization
