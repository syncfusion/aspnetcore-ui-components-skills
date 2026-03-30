# Getting Started with ASP.NET Core NumericTextBox

This guide explains how to include the Syncfusion NumericTextBox control in an ASP.NET Core application and create your first numeric input using Tag Helpers.

## Prerequisites

- Visual Studio 2022 or later
- ASP.NET Core 6.0 or higher
- Basic knowledge of ASP.NET Core and Razor syntax
- [System requirements for ASP.NET Core controls](https://ej2.syncfusion.com/aspnetcore/documentation/system-requirements)

## Step 1: Create ASP.NET Core Web Application

1. **Using Visual Studio:**
   - Open Visual Studio
   - File → New Project → ASP.NET Core Web App (Model-View-Controller)
   - Choose .NET 6.0 or higher
   - Create the project

2. **Using Syncfusion Extension (Recommended):**
   - Install [Syncfusion Extension for Visual Studio](https://marketplace.visualstudio.com/items?itemName=SyncfusionInc.SyncfusionEssentialStudioforASPNETCore)
   - Use Syncfusion → Create Syncfusion Project to generate pre-configured project

## Step 2: Install Syncfusion.EJ2.AspNet.Core NuGet Package

### Via NuGet Package Manager UI

1. Tools → NuGet Package Manager → Manage NuGet Packages for Solution
2. Search for "Syncfusion.EJ2.AspNet.Core"
3. Select the package and click "Install"
4. Accept license agreements
5. Wait for installation to complete

### Via Package Manager Console

```powershell
Install-Package Syncfusion.EJ2.AspNet.Core -Version 24.1.48
```

**Note:** The latest version may differ. Check [NuGet packages page](https://www.nuget.org/packages?q=syncfusion.EJ2) for current version.

## Step 3: Add Tag Helpers

Open `Pages/_ViewImports.cshtml` or `Views/_ViewImports.cshtml` and add the Syncfusion Tag Helper import:

```html
@addTagHelper *, Syncfusion.EJ2
```

This single line enables all Syncfusion Tag Helpers across your application.

## Step 4: Add Stylesheet and Script Resources

Open `Pages/Shared/_Layout.cshtml` or `Views/Shared/_Layout.cshtml` and add CDN references in the `<head>` section:

```html
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>@ViewData["Title"] - Syncfusion NumericTextBox Demo</title>
    
    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0/dist/css/bootstrap.min.css" />
    
    <!-- Syncfusion Fluent Theme CSS -->
    <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/24.1.48/fluent.css" />
    
    @await RenderSectionAsync("Styles", required: false)
</head>

<body>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <div class="container-fluid">
            <a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">
                NumericTextBox Demo
            </a>
        </div>
    </nav>

    <div class="container">
        @RenderBody()
    </div>

    <!-- jQuery -->
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    
    <!-- Bootstrap JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0/dist/js/bootstrap.bundle.min.js"></script>
    
    <!-- Syncfusion EJ2 Core JS -->
    <script src="https://cdn.syncfusion.com/ej2/24.1.48/dist/ej2.min.js"></script>
    
    <!-- Syncfusion Script Manager (REQUIRED) -->
    <ejs-scripts></ejs-scripts>
    
    @await RenderSectionAsync("Scripts", required: false)
</body>
</html>
```

**Important:** The `<ejs-scripts></ejs-scripts>` tag MUST be placed at the end of the `<body>` tag. This initializes all Syncfusion controls.

## Step 5: Create Your First NumericTextBox

### Simple Example

Open `Pages/Index.cshtml` or `Views/Home/Index.cshtml`:

```html
@{
    ViewData["Title"] = "NumericTextBox Demo";
}

<div class="container mt-5">
    <div class="row">
        <div class="col-md-6">
            <h3>Employee Salary Form</h3>
            <form method="post">
                <div class="form-group mb-3">
                    <label for="salary" class="form-label">Monthly Salary</label>
                    <ejs-numerictextbox id="salary"
                        name="salary"
                        placeholder="Enter salary"
                        value="5000"
                        format="c2"
                        min="0"
                        max="1000000"
                        step="1000"
                        float-label-type="Auto"
                        css-class="form-control">
                    </ejs-numerictextbox>
                    <small class="form-text text-muted">Format: Currency with 2 decimals</small>
                </div>

                <div class="form-group mb-3">
                    <label for="bonus" class="form-label">Annual Bonus (%)</label>
                    <ejs-numerictextbox id="bonus"
                        name="bonus"
                        placeholder="Enter bonus percentage"
                        value="10"
                        format="p2"
                        min="0"
                        max="100"
                        step="5"
                        float-label-type="Auto"
                        css-class="form-control">
                    </ejs-numerictextbox>
                </div>

                <button type="submit" class="btn btn-primary">Calculate Total Compensation</button>
            </form>
        </div>
    </div>
</div>
```

### With Model Binding

**Model (Models/Employee.cs):**

```csharp
using System.ComponentModel.DataAnnotations;

namespace NumericTextBoxDemo.Models
{
    public class Employee
    {
        public int Id { get; set; }

        [Required]
        public string Name { get; set; }

        [Display(Name = "Monthly Salary")]
        [Range(0, 1000000)]
        public decimal MonthlySalary { get; set; }

        [Display(Name = "Annual Bonus")]
        [Range(0, 100)]
        public decimal AnnualBonus { get; set; }
    }
}
```

**Controller (Controllers/HomeController.cs):**

```csharp
using Microsoft.AspNetCore.Mvc;
using NumericTextBoxDemo.Models;

namespace NumericTextBoxDemo.Controllers
{
    public class HomeController : Controller
    {
        public IActionResult Index()
        {
            var employee = new Employee
            {
                MonthlySalary = 5000,
                AnnualBonus = 10
            };
            return View(employee);
        }

        [HttpPost]
        public IActionResult Index(Employee employee)
        {
            if (ModelState.IsValid)
            {
                // Process employee data
                ViewBag.Message = $"Salary: {employee.MonthlySalary:C2}, Bonus: {employee.AnnualBonus:P2}";
                return View(employee);
            }
            return View(employee);
        }
    }
}
```

**View (Views/Home/Index.cshtml):**

```html
@model NumericTextBoxDemo.Models.Employee

@{
    ViewData["Title"] = "Employee Form";
}

<div class="container mt-5">
    <div class="row">
        <div class="col-md-6">
            <h3>Employee Information</h3>
            <form asp-action="Index" asp-controller="Home" method="post">
                <div class="form-group mb-3">
                    <label asp-for="MonthlySalary" class="form-label"></label>
                    <ejs-numerictextbox asp-for="MonthlySalary"
                        format="c2"
                        min="0"
                        max="1000000"
                        step="1000"
                        float-label-type="Auto"
                        css-class="form-control">
                    </ejs-numerictextbox>
                    <span asp-validation-for="MonthlySalary" class="text-danger"></span>
                </div>

                <div class="form-group mb-3">
                    <label asp-for="AnnualBonus" class="form-label"></label>
                    <ejs-numerictextbox asp-for="AnnualBonus"
                        format="p2"
                        min="0"
                        max="100"
                        step="5"
                        float-label-type="Auto"
                        css-class="form-control">
                    </ejs-numerictextbox>
                    <span asp-validation-for="AnnualBonus" class="text-danger"></span>
                </div>

                <button type="submit" class="btn btn-primary">Save</button>
            </form>

            @if (!string.IsNullOrEmpty(ViewBag.Message))
            {
                <div class="alert alert-success mt-3">@ViewBag.Message</div>
            }
        </div>
    </div>
</div>
```

## Step 6: Event Binding with Tag Helpers

### Binding to JavaScript Event Handlers

```html
<ejs-numerictextbox id="salary"
    name="salary"
    value="5000"
    format="c2"
    change="onSalaryChange"
    blur="onSalaryBlur"
    focus="onSalaryFocus"
    created="onSalaryCreated">
</ejs-numerictextbox>

<script>
    function onSalaryCreated(args) {
        console.log("NumericTextBox created successfully");
    }

    function onSalaryChange(args) {
        console.log("New value: " + args.value);
        console.log("Previous value: " + args.previousValue);
    }

    function onSalaryBlur(args) {
        console.log("NumericTextBox lost focus, value: " + args.value);
    }

    function onSalaryFocus(args) {
        console.log("NumericTextBox gained focus");
    }
</script>
```

### Common Events

| Event | Trigger | Use Case |
|-------|---------|----------|
| `created` | After control initialization | Initialize related controls |
| `change` | When value changes | Validate or calculate dependent values |
| `blur` | When focus leaves control | Format or save value |
| `focus` | When control gains focus | Show help text or highlight field |

## Step 7: Tag Helper Attributes Reference

### Common Attributes

```html
<!-- Basic attributes -->
<ejs-numerictextbox id="myNumeric"
    name="myNumeric"
    value="100"
    placeholder="Enter value">
</ejs-numerictextbox>

<!-- Formatting and validation -->
<ejs-numerictextbox id="salary"
    value="50000"
    format="c2"
    min="0"
    max="1000000"
    step="1000"
    decimals="2">
</ejs-numerictextbox>

<!-- Display options -->
<ejs-numerictextbox id="quantity"
    value="0"
    format="n0"
    show-spin-button="true"
    read-only="false"
    disabled="false">
</ejs-numerictextbox>

<!-- Labels and styling -->
<ejs-numerictextbox id="price"
    value="0"
    float-label-type="Auto"
    css-class="e-outline"
    placeholder="Enter price">
</ejs-numerictextbox>

<!-- Globalization -->
<ejs-numerictextbox id="international"
    value="1234.56"
    format="c2"
    locale="de-DE"
    enable-rtl="false">
</ejs-numerictextbox>

<!-- Events -->
<ejs-numerictextbox id="calculated"
    value="0"
    format="c2"
    created="onCreate"
    change="onChange"
    blur="onBlur"
    focus="onFocus">
</ejs-numerictextbox>
```

## Step 8: Run and Test

1. Press **Ctrl+F5** (or Debug → Start Without Debugging)
2. Browse to `http://localhost:port/Home/Index`
3. You should see NumericTextBox with:
   - Placeholder text
   - Spinner up/down buttons
   - Currency formatting
   - Floating label

### Expected Output

```
Employee Salary Form
┌─────────────────────────────────────┐
│ Monthly Salary                      │
│ $5,000.00                      [↑↓] │  ← Spinner buttons
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ Annual Bonus (%)                    │
│ 10.00%                         [↑↓] │
└─────────────────────────────────────┘

[Calculate Total Compensation]
```

## Troubleshooting

### Issue: NumericTextBox not rendering

**Solution:** 
- Ensure `@addTagHelper *, Syncfusion.EJ2` is in _ViewImports.cshtml
- Ensure `<ejs-scripts></ejs-scripts>` is at end of _Layout.cshtml body
- Check browser console for JavaScript errors
- Verify all CDN links are accessible

### Issue: Spinner buttons not showing

**Solution:**
- Check CSS is loaded: `<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/24.1.48/fluent.css" />`
- Add `show-spin-button="true"` explicitly to Tag Helper
- Verify browser hasn't cached old CSS

### Issue: Model binding not working

**Solution:**
- Property name in model MUST match `asp-for` or `name` attribute exactly
- Use `<ejs-numerictextbox asp-for="MonthlySalary" />` for property `MonthlySalary`
- Verify model properties are public
- Check controller receives correct model

### Issue: Format not applied

**Solution:**
- Ensure `format="c2"` or similar is set on Tag Helper
- Format is applied on **blur** (loses focus), not during input
- Verify format string syntax is valid

### Issue: Validation messages not showing

**Solution:**
- Add `<span asp-validation-for="PropertyName" class="text-danger"></span>` in view
- Ensure controller validates `ModelState.IsValid`
- Model properties must have `[Required]` or `[Range]` attributes

## Next Steps

1. **Number Formats**: Learn currency and percentage formatting → [number-formats.md](number-formats.md)
2. **Range Validation**: Set min/max constraints and precision → [range-validation.md](range-validation.md)
3. **Styling**: Customize appearance → [appearance-styling.md](appearance-styling.md)
4. **Globalization**: Support multiple cultures → [globalization-localization.md](globalization-localization.md)
5. **Advanced**: Event handling and integration patterns → [advanced-patterns.md](advanced-patterns.md)
