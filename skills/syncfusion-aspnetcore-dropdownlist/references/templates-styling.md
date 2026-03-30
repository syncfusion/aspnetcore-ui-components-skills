# Templates and Styling in DropdownList

## Table of Contents
- [Item Template](#item-template)
- [Value Template](#value-template)
- [Header and Footer Templates](#header-and-footer-templates)
- [Group Templates](#group-templates)
- [CSS Customization](#css-customization)
- [Theme Styling](#theme-styling)
- [Responsive Sizing](#responsive-sizing)
- [Advanced Styling Examples](#advanced-styling-examples)

## Item Template

Customize how items appear in the dropdown list.

### Basic Item Template

```csharp
@Html.EJ2().DropDownList()
    .Id("Employees")
    .DataSource(ViewBag.Employees)
    .Fields(f => f.Text("Name").Value("Id"))
    .ItemTemplate("<div class='emp-item'>${Name} - ${Department}</div>")
    .Render()
```

### Item Template with Rich Content

Display images, icons, and formatted content:

```csharp
public class Employee
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
    public string PhotoUrl { get; set; }
    public string Department { get; set; }
}

// View with rich item template
@Html.EJ2().DropDownList()
    .Id("Employees")
    .DataSource(ViewBag.Employees)
    .ItemTemplate(@"
        <div style='display: flex; gap: 10px; padding: 8px;'>
            <img src='${PhotoUrl}' style='width: 40px; height: 40px; border-radius: 50%;' />
            <div>
                <div style='font-weight: bold;'>${Name}</div>
                <div style='color: #666; font-size: 12px;'>${Email}</div>
            </div>
        </div>
    ")
    .Render()
```

### Item Template with Badges

```csharp
public class Task
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Priority { get; set; }  // High, Medium, Low
}

// View - color-coded priority badges
@Html.EJ2().DropDownList()
    .Id("Tasks")
    .DataSource(ViewBag.Tasks)
    .ItemTemplate(@"
        <div style='display: flex; justify-content: space-between; padding: 8px;'>
            <span>${Title}</span>
            <span class='priority-badge priority-${Priority}'>${Priority}</span>
        </div>
    ")
    .Render()

<style>
.priority-badge {
    padding: 4px 8px;
    border-radius: 3px;
    font-size: 12px;
    font-weight: bold;
}

.priority-High {
    background-color: #ff6b6b;
    color: white;
}

.priority-Medium {
    background-color: #ffc107;
    color: black;
}

.priority-Low {
    background-color: #51cf66;
    color: white;
}
</style>
```

## Value Template

Customize how selected item displays in the dropdown field.

### Basic Value Template

```csharp
@Html.EJ2().DropDownList()
    .Id("Status")
    .DataSource(ViewBag.Statuses)
    .Fields(f => f.Text("StatusName").Value("StatusCode"))
    .ValueTemplate("<span class='status-tag status-${StatusCode}'>${StatusName}</span>")
    .Render()

<style>
.status-tag {
    padding: 4px 8px;
    border-radius: 4px;
    font-weight: bold;
}

.status-Active { background-color: #28a745; color: white; }
.status-Inactive { background-color: #dc3545; color: white; }
.status-Pending { background-color: #ffc107; color: black; }
</style>
```

### Value Template with Icon

```csharp
public class Department
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string IconClass { get; set; }
}

// Display selected department with icon
@Html.EJ2().DropDownList()
    .Id("Departments")
    .DataSource(ViewBag.Departments)
    .Fields(f => f.Text("Name").Value("Id").IconCss("IconClass"))
    .ValueTemplate("<i class='${IconClass}'></i> ${Name}")
    .Render()
```

### Conditional Value Display

```csharp
@Html.EJ2().DropDownList()
    .Id("Items")
    .DataSource(ViewBag.Items)
    .ValueTemplate(@"
        <div id='valueTemplate'>
            <span class='item-count'>${Count} items</span>
        </div>
    ")
    .Render()
```

## Header and Footer Templates

### Header Template

Add content above the dropdown list:

```csharp
@Html.EJ2().DropDownList()
    .Id("Countries")
    .DataSource(ViewBag.Countries)
    .HeaderTemplate(@"
        <div style='padding: 10px; background-color: #f0f0f0; border-bottom: 1px solid #ddd;'>
            <span style='font-weight: bold;'>Select a Country</span>
            <span style='float: right; color: #666;'>${Count} available</span>
        </div>
    ")
    .Render()
```

### Footer Template

Add content below the dropdown list:

```csharp
@Html.EJ2().DropDownList()
    .Id("Products")
    .DataSource(ViewBag.Products)
    .FooterTemplate(@"
        <div style='padding: 10px; background-color: #f9f9f9; border-top: 1px solid #ddd; text-align: center;'>
            <a href='/products/view-all' style='color: #0078d4;'>View All Products</a>
        </div>
    ")
    .Render()
```

### Combined Header and Footer

```csharp
@Html.EJ2().DropDownList()
    .Id("Customers")
    .DataSource(ViewBag.Customers)
    .HeaderTemplate(@"<div style='padding: 8px; font-weight: bold;'>Active Customers</div>")
    .FooterTemplate(@"<div style='padding: 8px; text-align: center; color: #999;'>End of list</div>")
    .Render()
```

## Group Templates

Customize group header display in grouped dropdowns.

### Custom Group Header

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Category { get; set; }
}

@Html.EJ2().DropDownList()
    .Id("Products")
    .DataSource(ViewBag.Products)
    .Fields(f => f
        .Text("Name")
        .Value("Id")
        .GroupBy("Category")
    )
    .GroupBy("Category")
    .GroupTemplate(@"
        <strong style='color: #0078d4;'>${Category}</strong>
        <span style='float: right; color: #999; font-size: 12px;'>${Count} products</span>
    ")
    .Render()
```

### Group Template with Icons

```csharp
@Html.EJ2().DropDownList()
    .Id("Tasks")
    .DataSource(ViewBag.Tasks)
    .Fields(f => f
        .Text("TaskName")
        .Value("TaskId")
        .GroupBy("Status")
    )
    .GroupBy("Status")
    .GroupTemplate(@"
        <div style='display: flex; align-items: center; gap: 8px;'>
            <i class='e-icons ${StatusIcon}'></i>
            <strong>${Status}</strong>
            <span style='background: #ddd; padding: 2px 6px; border-radius: 12px; font-size: 12px;'>${Count}</span>
        </div>
    ")
    .Render()
```

## CSS Customization

### Using CSS Classes

Apply custom CSS classes to dropdown:

```csharp
@Html.EJ2().DropDownList()
    .Id("Category")
    .DataSource(ViewBag.Categories)
    .CssClass("custom-dropdown custom-style")
    .Render()

<style>
.custom-dropdown {
    width: 100%;
    max-width: 500px;
}

.custom-dropdown .e-input {
    font-size: 14px;
    border-color: #ddd;
    border-radius: 4px;
    padding: 10px;
}

.custom-dropdown .e-input:focus {
    border-color: #0078d4;
    box-shadow: 0 0 0 3px rgba(0, 120, 212, 0.1);
}

.custom-style .e-list-item {
    padding: 12px;
    border-bottom: 1px solid #f0f0f0;
}

.custom-style .e-list-item:hover {
    background-color: #f5f5f5;
}

.custom-style .e-list-item.e-item-focus {
    background-color: #e3f2fd;
}
</style>
```

### Styling the Input Field

```csharp
@Html.EJ2().DropDownList()
    .Id("Priority")
    .DataSource(ViewBag.Priorities)
    .CssClass("priority-input")
    .Render()

<style>
.priority-input .e-input {
    background-color: #f9f9f9;
    border: 2px solid #e0e0e0;
    font-weight: 500;
}

.priority-input .e-input:hover {
    border-color: #0078d4;
}

.priority-input .e-input:focus {
    background-color: white;
    border-color: #0078d4;
}
</style>
```

### Styling the Popup List

```csharp
<style>
.e-dropdownlist .e-list {
    background-color: white;
    border: 1px solid #ddd;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.e-dropdownlist .e-list-item {
    color: #333;
    padding: 10px 12px;
    line-height: 1.5;
}

.e-dropdownlist .e-list-item:hover {
    background-color: #f5f5f5;
    color: #000;
}

.e-dropdownlist .e-list-item.e-item-selected {
    background-color: #e3f2fd;
    color: #0078d4;
    font-weight: 500;
}

.e-dropdownlist .e-list-item.e-item-focus {
    background-color: #e8e8e8;
}
</style>
```

## Theme Styling

Syncfusion provides built-in themes:

### Available Themes

```html
<!-- Material Theme (Default) -->
<link rel="stylesheet" href="~/lib/ej2/material.css" />

<!-- Bootstrap Theme -->
<link rel="stylesheet" href="~/lib/ej2/bootstrap.css" />

<!-- Fabric Theme -->
<link rel="stylesheet" href="~/lib/ej2/fabric.css" />

<!-- Tailwind Theme -->
<link rel="stylesheet" href="~/lib/ej2/tailwind.css" />

<!-- Fluent Theme -->
<link rel="stylesheet" href="~/lib/ej2/fluent.css" />
```

### Theme Variables (Material Theme)

Customize theme colors:

```css
:root {
    --e-primary: #0078d4;           /* Primary color */
    --e-primary-dark: #006db3;      /* Dark primary */
    --e-primary-light: #4da6ff;     /* Light primary */
    --e-success: #28a745;           /* Success state */
    --e-danger: #dc3545;            /* Danger/Error */
    --e-warning: #ffc107;           /* Warning state */
    --e-info: #17a2b8;              /* Info state */
}
```

## Responsive Sizing

### Width and Height

```csharp
@Html.EJ2().DropDownList()
    .Id("Dropdown")
    .DataSource(ViewBag.Data)
    .Width("100%")
    .PopupHeight("300px")
    .Render()
```

### Mobile-Responsive Dropdown

```csharp
@Html.EJ2().DropDownList()
    .Id("Categories")
    .DataSource(ViewBag.Categories)
    .CssClass("responsive-dropdown")
    .Render()

<style>
.responsive-dropdown {
    width: 100%;
}

@media (max-width: 768px) {
    .responsive-dropdown .e-input {
        font-size: 16px;  /* Prevents zoom on iOS */
    }
}

@media (max-width: 480px) {
    .responsive-dropdown {
        width: 100%;
    }
    
    .responsive-dropdown .e-list-item {
        padding: 12px;
        font-size: 14px;
    }
}
</style>
```

## Advanced Styling Examples

### Modern Card-Style Dropdown

```csharp
public class Service
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Description { get; set; }
    public decimal Price { get; set; }
}

@Html.EJ2().DropDownList()
    .Id("Services")
    .DataSource(ViewBag.Services)
    .ItemTemplate(@"
        <div class='service-card'>
            <div class='service-header'>
                <strong>${Name}</strong>
                <span class='service-price'>${Price}</span>
            </div>
            <div class='service-description'>${Description}</div>
        </div>
    ")
    .ValueTemplate(@"<strong>${Name}</strong> <span style='color: #0078d4;'>${Price}</span>")
    .CssClass("service-dropdown")
    .Render()

<style>
.service-dropdown .e-list-item {
    padding: 12px !important;
    border-bottom: 1px solid #f0f0f0;
}

.service-card {
    width: 100%;
}

.service-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 4px;
}

.service-price {
    background-color: #e3f2fd;
    color: #0078d4;
    padding: 2px 8px;
    border-radius: 12px;
    font-size: 12px;
    font-weight: bold;
}

.service-description {
    color: #666;
    font-size: 12px;
    margin-top: 4px;
}
</style>
```

### Colored Priority Dropdown

```csharp
@Html.EJ2().DropDownList()
    .Id("TaskPriority")
    .DataSource(ViewBag.Priorities)
    .ItemTemplate(@"
        <div class='priority-item priority-${Value}'>
            <span>${Text}</span>
        </div>
    ")
    .ValueTemplate(@"<div class='priority-selected priority-${Value}'>${Text}</div>")
    .CssClass("priority-dropdown")
    .Render()

<style>
.priority-item {
    padding: 8px 12px;
    margin: 4px;
    border-radius: 4px;
}

.priority-item.priority-1 {
    background-color: #ffebee;
    color: #c62828;
}

.priority-item.priority-2 {
    background-color: #fff3e0;
    color: #e65100;
}

.priority-item.priority-3 {
    background-color: #e8f5e9;
    color: #1b5e20;
}

.priority-selected {
    padding: 4px 12px;
    border-radius: 3px;
    font-weight: bold;
}
</style>
```

## Styling Best Practices

- Use CSS classes for maintainability
- Keep templates simple for performance
- Test responsive design on mobile
- Follow theme color conventions
- Ensure sufficient color contrast for accessibility

---

## Next Steps

- Review [Accessibility and Features](accessibility-features.md) for accessible styling
- Explore [Advanced Scenarios](advanced-scenarios.md) for complex templates
- Check [Data Binding](data-binding.md) for dynamic template data
