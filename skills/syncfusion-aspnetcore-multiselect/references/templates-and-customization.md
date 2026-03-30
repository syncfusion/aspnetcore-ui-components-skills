# Templates and Customization

## Table of Contents
- [Item Template](#item-template)
- [Value Template](#value-template)
- [Header and Footer Templates](#header-and-footer-templates)
- [Group Header Template](#group-header-template)
- [Icon Integration](#icon-integration)
- [CSS Customization](#css-customization)
- [Practical Examples](#practical-examples)

## Item Template

Customize how items appear in the dropdown list:

### Basic Item Template

```razor
<ejs-multiselect id="products" 
    dataSource="ViewBag.ProductList"
    fields-text="ProductName"
    fields-value="ProductId"
    itemTemplate="itemTemplate">
</ejs-multiselect>

@section Scripts {
    <script>
        var itemTemplate = '<div><strong>${ProductName}</strong><br/><small>$${Price}</small></div>';
    </script>
}

<ejs-scriptmanager></ejs-scriptmanager>
```

```csharp
public class Product
{
    public int ProductId { get; set; }
    public string ProductName { get; set; }
    public decimal Price { get; set; }
}

public IActionResult Index()
{
    ViewBag.ProductList = new List<Product>
    {
        new Product { ProductId = 1, ProductName = "Laptop", Price = 999 },
        new Product { ProductId = 2, ProductName = "Mouse", Price = 25 }
    };
    
    return View();
}
```

**Result:**
```
□ Laptop
  $999
□ Mouse
  $25
```

### HTML Template with Styling

```razor
<ejs-multiselect id="employees" 
    dataSource="ViewBag.EmployeeList"
    fields-text="EmployeeName"
    fields-value="EmployeeId"
    itemTemplate="employeeTemplate">
</ejs-multiselect>

@section Scripts {
    <script>
        var employeeTemplate = '<div class="employee-item">' +
                               '<img src="images/${EmployeePhoto}" class="emp-photo">' +
                               '<div class="emp-details">' +
                               '<strong>${EmployeeName}</strong>' +
                               '<br/><small>${Department}</small>' +
                               '</div></div>';
    </script>
}
```

```css
.employee-item {
    display: flex;
    align-items: center;
    padding: 10px;
}

.emp-photo {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    margin-right: 10px;
}

.emp-details {
    flex-grow: 1;
}

.emp-details small {
    color: #999;
}
```

## Value Template

Customize how selected items are displayed:

### Basic Value Template

```razor
<ejs-multiselect id="countries" 
    dataSource="ViewBag.CountryList"
    fields-text="CountryName"
    fields-value="CountryCode"
    valueTemplate="countryValue">
</ejs-multiselect>

@section Scripts {
    <script>
        var countryValue = '<span class="country-flag">${CountryCode}</span> ${CountryName}';
    </script>
}
```

**Result:**
```
[US] United States [GB] United Kingdom [Remove]
```

### Custom Chip Styling

```razor
<ejs-multiselect id="tags" 
    dataSource="ViewBag.Tags"
    valueTemplate="tagValue"
    mode="Default">
</ejs-multiselect>

@section Scripts {
    <script>
        var tagValue = '<span class="tag-chip tag-${TagCategory}">${TagName}</span>';
    </script>
}
```

```css
.tag-chip {
    display: inline-block;
    padding: 5px 12px;
    border-radius: 20px;
    margin-right: 5px;
    color: white;
}

.tag-technology {
    background-color: #2196F3;
}

.tag-language {
    background-color: #4CAF50;
}

.tag-skill {
    background-color: #FF9800;
}
```

## Header and Footer Templates

### Header Template

Display header content above the dropdown list:

```razor
<ejs-multiselect id="products" 
    dataSource="ViewBag.ProductList"
    fields-text="ProductName"
    fields-value="ProductId"
    headerTemplate="headerTemplate">
</ejs-multiselect>

@section Scripts {
    <script>
        var headerTemplate = '<div class="header-custom">' +
                            '<strong>Available Products</strong>' +
                            '<p>Select items to add to cart</p>' +
                            '</div>';
    </script>
}
```

```css
.header-custom {
    padding: 15px;
    border-bottom: 1px solid #ddd;
    background-color: #f9f9f9;
}

.header-custom strong {
    display: block;
    margin-bottom: 5px;
}

.header-custom p {
    margin: 0;
    font-size: 12px;
    color: #999;
}
```

### Footer Template

Display content below the dropdown list:

```razor
<ejs-multiselect id="items" 
    dataSource="ViewBag.ItemList"
    fields-text="ItemName"
    fields-value="ItemId"
    footerTemplate="footerTemplate">
</ejs-multiselect>

@section Scripts {
    <script>
        var footerTemplate = '<div class="footer-custom">' +
                            '<a href="#" onclick="addNewItem(event)">+ Add New Item</a>' +
                            '</div>';
    </script>
}
```

```css
.footer-custom {
    padding: 15px;
    border-top: 1px solid #ddd;
    text-align: center;
}

.footer-custom a {
    color: #2196F3;
    text-decoration: none;
}

.footer-custom a:hover {
    text-decoration: underline;
}
```

## Group Header Template

Customize how group headers appear:

```razor
<ejs-multiselect id="products" 
    dataSource="ViewBag.ProductList"
    fields-text="ProductName"
    fields-value="ProductId"
    fields-groupBy="Category"
    groupTemplate="groupTemplate">
</ejs-multiselect>

@section Scripts {
    <script>
        var groupTemplate = '<strong style="color: #2196F3;">${Category}</strong> ' +
                           '<span style="float: right; color: #999;">${Count} items</span>';
    </script>
}
```

```csharp
public class Product
{
    public int ProductId { get; set; }
    public string ProductName { get; set; }
    public string Category { get; set; }
}

public IActionResult Index()
{
    ViewBag.ProductList = new List<Product>
    {
        new Product { ProductId = 1, ProductName = "Laptop", Category = "Electronics" },
        new Product { ProductId = 2, ProductName = "Mouse", Category = "Electronics" },
        new Product { ProductId = 3, ProductName = "Desk", Category = "Furniture" },
        new Product { ProductId = 4, ProductName = "Chair", Category = "Furniture" }
    };
    
    return View();
}
```

**Result:**
```
Electronics          4 items
  ☐ Laptop
  ☐ Mouse
Furniture            2 items
  ☐ Desk
  ☐ Chair
```

## Icon Integration

### Font Awesome Icons

```razor
<ejs-multiselect id="frameworks" 
    dataSource="ViewBag.FrameworkList"
    fields-text="FrameworkName"
    fields-value="FrameworkId"
    fields-iconCss="IconClass"
    itemTemplate="frameworkTemplate">
</ejs-multiselect>

@section Scripts {
    <script>
        var frameworkTemplate = '<i class="${IconClass}"></i> ${FrameworkName}';
    </script>
}
```

```csharp
public class Framework
{
    public int FrameworkId { get; set; }
    public string FrameworkName { get; set; }
    public string IconClass { get; set; }
}

public IActionResult Index()
{
    ViewBag.FrameworkList = new List<Framework>
    {
        new Framework { FrameworkId = 1, FrameworkName = "React", IconClass = "fab fa-react" },
        new Framework { FrameworkId = 2, FrameworkName = "Angular", IconClass = "fab fa-angular" },
        new Framework { FrameworkId = 3, FrameworkName = "Vue", IconClass = "fab fa-vuejs" }
    };
    
    return View();
}
```

**Result:**
```
☐ ⚛ React
☐ ⬆ Angular
☐ ⬇ Vue
```

### Material Icons

```razor
<ejs-multiselect id="priorities" 
    dataSource="ViewBag.PriorityList"
    fields-text="PriorityName"
    fields-value="PriorityId"
    itemTemplate="priorityTemplate">
</ejs-multiselect>

@section Scripts {
    <script>
        var priorityTemplate = '<span class="material-icons">${IconName}</span> ${PriorityName}';
    </script>
}
```

```html
<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
```

```css
.material-icons {
    vertical-align: middle;
    margin-right: 8px;
}
```

## CSS Customization

### Container Styling

```css
/* MultiSelect container */
.e-multiselect {
    width: 100%;
    font-size: 16px;
    border-radius: 4px;
}

/* Input area */
.e-multiselect .e-input-group {
    border: 1px solid #ccc;
    border-radius: 4px;
    transition: border-color 0.3s;
}

/* Input focus */
.e-multiselect .e-input-group:focus-within {
    border-color: #2196F3;
    box-shadow: 0 0 0 3px rgba(33, 150, 243, 0.1);
}

/* Selected chips/tags */
.e-multiselect .e-chip {
    background-color: #e0e0e0;
    color: #333;
    border-radius: 20px;
    padding: 4px 8px;
    margin: 2px;
}

/* Remove button on chip */
.e-multiselect .e-icon-close {
    cursor: pointer;
    margin-left: 5px;
}

/* Dropdown list */
.e-multiselect .e-list-item {
    padding: 10px;
    border-bottom: 1px solid #f0f0f0;
}

/* List item hover */
.e-multiselect .e-list-item:hover {
    background-color: #f5f5f5;
}

/* Selected list item */
.e-multiselect .e-list-item.e-active {
    background-color: #e3f2fd;
    color: #2196F3;
}

/* Dropdown arrow */
.e-multiselect .e-ddl-icon {
    color: #999;
}
```

### Theme Customization

```css
/* Custom theme - Blue color scheme */
.custom-theme .e-multiselect .e-chip {
    background-color: #2196F3;
    color: white;
}

.custom-theme .e-multiselect .e-list-item.e-active {
    background-color: #1976D2;
    color: white;
}

.custom-theme .e-multiselect .e-input-group:focus-within {
    border-color: #1976D2;
}

/* Custom theme - Green color scheme */
.success-theme .e-multiselect .e-chip {
    background-color: #4CAF50;
    color: white;
}

.success-theme .e-multiselect .e-list-item.e-active {
    background-color: #388E3C;
    color: white;
}
```

### Disabled State

```css
/* Disabled MultiSelect */
.e-multiselect.e-disabled {
    opacity: 0.6;
    pointer-events: none;
}

.e-multiselect.e-disabled .e-input-group {
    background-color: #f5f5f5;
}
```

## Practical Examples

### Example 1: Employee Selection with Avatar

```razor
<ejs-multiselect id="team-members" 
    dataSource="ViewBag.EmployeeList"
    fields-text="EmployeeName"
    fields-value="EmployeeId"
    itemTemplate="employeeTemplate"
    valueTemplate="employeeValue">
</ejs-multiselect>

@section Scripts {
    <script>
        var employeeTemplate = '<div class="emp-option">' +
                              '<img src="images/${AvatarUrl}" class="emp-avatar">' +
                              '<div><strong>${EmployeeName}</strong><br/>' +
                              '<small>${Department}</small></div></div>';
        
        var employeeValue = '<img src="images/${AvatarUrl}" class="emp-avatar-small"> ${EmployeeName}';
    </script>
}
```

```css
.emp-option {
    display: flex;
    align-items: center;
    padding: 10px;
}

.emp-avatar {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    margin-right: 10px;
    object-fit: cover;
}

.emp-avatar-small {
    width: 24px;
    height: 24px;
    border-radius: 50%;
    margin-right: 5px;
    object-fit: cover;
    vertical-align: middle;
}
```

### Example 2: Product with Stock Status

```razor
<ejs-multiselect id="products" 
    dataSource="ViewBag.ProductList"
    fields-text="ProductName"
    fields-value="ProductId"
    itemTemplate="productTemplate">
</ejs-multiselect>

@section Scripts {
    <script>
        var productTemplate = '<div class="product-item">' +
                             '<strong>${ProductName}</strong>' +
                             '<span class="stock-badge stock-${StockStatus}">${StockStatus}</span>' +
                             '</div>';
    </script>
}
```

```css
.product-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px;
}

.stock-badge {
    font-size: 12px;
    padding: 3px 8px;
    border-radius: 3px;
    color: white;
}

.stock-InStock {
    background-color: #4CAF50;
}

.stock-LowStock {
    background-color: #FF9800;
}

.stock-OutOfStock {
    background-color: #f44336;
}
```

### Example 3: Category Grouped with Icons

```razor
<ejs-multiselect id="skills" 
    dataSource="ViewBag.SkillList"
    fields-text="SkillName"
    fields-value="SkillId"
    fields-groupBy="Category"
    itemTemplate="skillTemplate"
    groupTemplate="categoryTemplate">
</ejs-multiselect>

@section Scripts {
    <script>
        var skillTemplate = '<i class="${IconClass}"></i> ${SkillName} ' +
                           '<span class="skill-level">${Level}</span>';
        
        var categoryTemplate = '<strong>${Category}</strong>';
    </script>
}
```

## Best Practices

### 1. Keep Templates Simple

```javascript
// Good: Simple, fast template
var template = '<div>${Name}</div>';

// Avoid: Complex logic in template
var template = '<div>${Name} - ' +
               (status === 'active' ? 'Active' : 'Inactive') +
               '</div>';
```

### 2. Use Consistent Icon Sizes

```css
/* Consistent sizing */
.item-icon {
    width: 20px;
    height: 20px;
    margin-right: 8px;
    vertical-align: middle;
}
```

### 3. Ensure Template Accessibility

```razor
<!-- Add proper labels and roles -->
<div class="emp-option" role="option">
    <img src="images/${AvatarUrl}" alt="${EmployeeName}" class="emp-avatar">
    <div>
        <strong>${EmployeeName}</strong>
        <br/>
        <small>${Department}</small>
    </div>
</div>
```

### 4. Handle Missing Data

```javascript
// Safe template with fallback
var template = '<div>${ProductName || "Unnamed"}</div> ' +
              '<span>${Price ? "$" + Price : "N/A"}</span>';
```

## Next Steps

✅ **Learn More:**
- [Advanced Features](advanced-features.md) - Combine templates with other features
- [Data Binding](data-binding.md) - Prepare data for templates
- [Filtering and Search](filtering-and-search.md) - Work with filtered results
