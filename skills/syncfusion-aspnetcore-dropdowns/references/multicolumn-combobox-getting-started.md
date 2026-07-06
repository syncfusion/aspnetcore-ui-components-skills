# Getting Started with MultiColumn ComboBox

## Table of Contents
- [Installation](#installation)
- [TagHelper Registration](#taghelper-registration)
- [Basic Component Setup](#basic-component-setup)
- [Binding Data with Fields and Columns](#binding-data-with-fields-and-columns)
- [Configuring Popup Size](#configuring-popup-size)
- [Running the Application](#running-the-application)

---

## Installation

Install the Syncfusion ASP.NET Core package via NuGet:

**Package Manager Console:**

```powershell
Install-Package Syncfusion.EJ2.AspNet.Core
```

**Or using .NET CLI:**

```bash
dotnet add package Syncfusion.EJ2.AspNet.Core
```

---

## TagHelper Registration

### Register Service in Program.cs

Add Syncfusion services:

```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllersWithViews();
builder.Services.AddSyncfusionEJ2();  // ✅ Add this

var app = builder.Build();
app.Run();
```

### Add TagHelper Reference

Edit `Views/_ViewImports.cshtml`:

```cshtml
@addTagHelper *, Syncfusion.EJ2
```

---

## Basic Component Setup

The minimum required setup: `dataSource`, `fields`, and at least one `<e-column>`.

**Controller (`HomeController.cs`):**

```csharp
public class Employee
{
    public int EmpID { get; set; }
    public string Name { get; set; }
    public string Designation { get; set; }
    public string Country { get; set; }
}

public IActionResult Index()
{
    ViewBag.empData = new List<Employee>
    {
        new Employee { EmpID = 1001, Name = "Andrew Fuller", Designation = "Team Lead", Country = "England" },
        new Employee { EmpID = 1002, Name = "Robert", Designation = "Developer", Country = "USA" },
        new Employee { EmpID = 1003, Name = "Michael", Designation = "HR", Country = "Russia" }
    };
    return View();
}
```

**View (`Index.cshtml`):**

```cshtml
<ejs-multicolumncombobox id="multicolumn"
    dataSource="@ViewBag.empData"
    placeholder="Select an employee">
    <e-multicolumncombobox-fields text="Name" value="EmpID"></e-multicolumncombobox-fields>
    <e-multicolumncombobox-columns>
        <e-multicolumncombobox-column field="EmpID" header="Employee ID" width="120"></e-multicolumncombobox-column>
        <e-multicolumncombobox-column field="Name" header="Name" width="120"></e-multicolumncombobox-column>
        <e-multicolumncombobox-column field="Designation" header="Designation" width="120"></e-multicolumncombobox-column>
        <e-multicolumncombobox-column field="Country" header="Country" width="100"></e-multicolumncombobox-column>
    </e-multicolumncombobox-columns>
</ejs-multicolumncombobox>
```

---

## Binding Data with Fields and Columns

### Fields Property

The `fields` property maps your data:

- `text` — The field shown in the input box after selection
- `value` — The hidden value bound to the component

```cshtml
<e-multicolumn-combobox-fields 
    text="Name" 
    value="EmpID">
</e-multicolumn-combobox-fields>
```

### Columns Definition

Each `<e-multicolumncombobox-column>` renders one column in the popup grid:

```cshtml
<e-multicolumncombobox-columns>
    <e-multicolumncombobox-column field="EmpID" header="Employee ID" width="90"></e-multicolumncombobox-column>
    <e-multicolumncombobox-column field="Name" header="Name" width="90"></e-multicolumncombobox-column>
    <e-multicolumncombobox-column field="Designation" header="Designation" width="90"></e-multicolumncombobox-column>
    <e-multicolumncombobox-column field="Country" header="Country" width="70"></e-multicolumncombobox-column>
</e-multicolumncombobox-columns>
```

| Property | Description |
|----------|-------------|
| `field` | Maps to data property (must match your object keys) |
| `header` | Column header text |
| `width` | Column width in pixels |

---

## Configuring Popup Size

### Default Popup Behavior

The popup defaults to `300px` height and matches the component width. Customize with `popupHeight` and `popupWidth`:

**View:**

```cshtml
<ejs-multicolumncombobox id="multicolumn"
    dataSource="@ViewBag.empData"
    popupHeight="250px"
    popupWidth="500px"
    placeholder="Select an employee">
    <e-multicolumncombobox-fields text="Name" value="EmpID"></e-multicolumncombobox-fields>
    <e-multicolumncombobox-columns>
        <e-multicolumncombobox-column field="EmpID" header="Employee ID" width="90"></e-multicolumncombobox-column>
        <e-multicolumncombobox-column field="Name" header="Name" width="90"></e-multicolumncombobox-column>
        <e-multicolumncombobox-column field="Designation" header="Designation" width="90"></e-multicolumncombobox-column>
        <e-multicolumncombobox-column field="Country" header="Country" width="70"></e-multicolumncombobox-column>
    </e-multicolumncombobox-columns>
</ejs-multicolumncombobox>
```

Both properties accept `string` (e.g., `'250px'`) or `number` (e.g., `250`).

### Column Width Adjustment

Ensure total column width fits in popup width:

```cshtml
<e-multicolumncombobox-columns>
    <e-multicolumncombobox-column field="EmpID" header="ID" width="80"></e-multicolumncombobox-column>
    <e-multicolumncombobox-column field="Name" header="Name" width="120"></e-multicolumncombobox-column>
    <e-multicolumncombobox-column field="Designation" header="Designation" width="120"></e-multicolumncombobox-column>
    <e-multicolumncombobox-column field="Country" header="Country" width="100"></e-multicolumncombobox-column>
    <!-- Total: 420px -->
</e-multicolumncombobox-columns>
```

---

## Running the Application

Start the development server:

**Using .NET CLI:**

```bash
dotnet run
```

**Using Visual Studio:**
- Press F5 or click "Start Debugging"
- Navigate to the view containing the MultiColumn ComboBox

---

## First Implementation Checklist

- [ ] NuGet package installed: `Syncfusion.EJ2.AspNet.Core`
- [ ] Service registered in `Program.cs`: `builder.Services.AddSyncfusionEJ2();`
- [ ] TagHelper added to `_ViewImports.cshtml`: `@addTagHelper *, Syncfusion.EJ2`
- [ ] MultiColumn ComboBox added to view with `<ejs-multicolumn-combobox>`
- [ ] dataSource populated with data via controller `ViewBag`
- [ ] `<e-multicolumn-combobox-fields>` configured with `text` and `value`
- [ ] At least one `<e-column>` defined
- [ ] placeholder text added for UX
- [ ] popupHeight and popupWidth configured appropriately
- [ ] Application runs and component renders properly

---

## Next Steps

- **Binding complex data?** → Go to [data-binding.md](multicolumn-combobox-data-binding.md)
- **Want advanced features?** → See [advanced-features.md](multicolumn-combobox-advanced-features.md)
- **Need filtering?** → Check [filtering.md](multicolumn-combobox-filtering.md)
- **Want custom templates?** → Explore [templates.md](multicolumn-combobox-templates.md)
