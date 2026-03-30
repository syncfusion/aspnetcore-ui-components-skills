# Advanced Features

## Table of Contents
- [Checkbox Mode](#checkbox-mode)
- [Grouping](#grouping)
- [Sorting](#sorting)
- [Custom Values](#custom-values)
- [Chip Customization](#chip-customization)
- [Virtual Scrolling](#virtual-scrolling)
- [Cascading MultiSelect](#cascading-multiselect)

## Checkbox Mode

Display checkboxes for each item instead of tags:

### Basic Checkbox Mode

```razor
<ejs-multiselect id="languages" 
    dataSource="ViewBag.LanguageList"
    mode="CheckBox"
    placeholder="Select languages">
</ejs-multiselect>

<ejs-scriptmanager></ejs-scriptmanager>
```

```csharp
public IActionResult Index()
{
    ViewBag.LanguageList = new List<string>
    {
        "English",
        "Spanish",
        "French",
        "German",
        "Japanese"
    };
    
    return View();
}
```

**Result:**
```
☐ English
☐ Spanish
☐ French
☐ German
☐ Japanese
```

### With Select All Checkbox

```razor
<ejs-multiselect id="permissions" 
    dataSource="ViewBag.PermissionList"
    fields-text="PermissionName"
    fields-value="PermissionId"
    mode="CheckBox"
    showSelectAll="true"
    placeholder="Select permissions">
</ejs-multiselect>
```

**Result:**
```
☐ Select All
──────────
☐ Read
☐ Write
☐ Delete
☐ Create
```

### Pre-Selected with Checkboxes

```razor
<ejs-multiselect id="roles" 
    dataSource="ViewBag.RoleList"
    fields-text="RoleName"
    fields-value="RoleId"
    mode="CheckBox"
    value="ViewBag.SelectedRoles"
    showSelectAll="true">
</ejs-multiselect>
```

```csharp
public IActionResult Index()
{
    ViewBag.RoleList = new List<Role>
    {
        new Role { RoleId = 1, RoleName = "Admin" },
        new Role { RoleId = 2, RoleName = "Editor" },
        new Role { RoleId = 3, RoleName = "Viewer" }
    };
    
    ViewBag.SelectedRoles = new int[] { 1, 2 }; // Pre-select Admin and Editor
    
    return View();
}
```

### With Maximum Selection

```razor
<ejs-multiselect id="skills" 
    dataSource="ViewBag.SkillList"
    mode="CheckBox"
    maximumSelectionLength="5"
    placeholder="Select up to 5 skills">
</ejs-multiselect>

@section Scripts {
    <script>
        var multiObj = document.getElementById('skills').ej2_instances[0];
        multiObj.actionComplete = function(args) {
            if (multiObj.value && multiObj.value.length >= 5) {
                alert("Maximum 5 skills allowed");
            }
        };
    </script>
}
```

## Grouping

Organize items into logical categories:

### Basic Grouping

```razor
<ejs-multiselect id="products" 
    dataSource="ViewBag.ProductList"
    fields-text="ProductName"
    fields-value="ProductId"
    fields-groupBy="Category"
    placeholder="Select products">
</ejs-multiselect>
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
        new Product { ProductId = 3, ProductName = "Keyboard", Category = "Electronics" },
        new Product { ProductId = 4, ProductName = "Desk", Category = "Furniture" },
        new Product { ProductId = 5, ProductName = "Chair", Category = "Furniture" },
        new Product { ProductId = 6, ProductName = "Monitor Stand", Category = "Furniture" }
    };
    
    return View();
}
```

**Result:**
```
Electronics (3)
  ☐ Laptop
  ☐ Mouse
  ☐ Keyboard
Furniture (3)
  ☐ Desk
  ☐ Chair
  ☐ Monitor Stand
```

### Nested Grouping

```razor
<ejs-multiselect id="employees" 
    dataSource="ViewBag.EmployeeList"
    fields-text="EmployeeName"
    fields-value="EmployeeId"
    fields-groupBy="Department"
    placeholder="Select employees">
</ejs-multiselect>
```

```csharp
public class Employee
{
    public int EmployeeId { get; set; }
    public string EmployeeName { get; set; }
    public string Department { get; set; }
}

public IActionResult Index()
{
    ViewBag.EmployeeList = new List<Employee>
    {
        new Employee { EmployeeId = 1, EmployeeName = "Alice", Department = "Engineering" },
        new Employee { EmployeeId = 2, EmployeeName = "Bob", Department = "Engineering" },
        new Employee { EmployeeId = 3, EmployeeName = "Carol", Department = "Sales" },
        new Employee { EmployeeId = 4, EmployeeName = "David", Department = "Sales" },
        new Employee { EmployeeId = 5, EmployeeName = "Eve", Department = "HR" }
    };
    
    return View();
}
```

## Sorting

Sort items alphabetically or by custom order:

### Ascending Sort

```razor
<ejs-multiselect id="countries" 
    dataSource="ViewBag.CountryList"
    sortOrder="Ascending"
    placeholder="Select countries">
</ejs-multiselect>
```

**Result:** Items sorted A-Z

### Descending Sort

```razor
<ejs-multiselect id="items" 
    dataSource="ViewBag.ItemList"
    sortOrder="Descending"
    placeholder="Select items">
</ejs-multiselect>
```

**Result:** Items sorted Z-A

### Custom Sort Order

```csharp
public IActionResult Index()
{
    // Define data with priority values
    var items = new List<Item>
    {
        new Item { ItemId = 3, ItemName = "Low Priority", SortOrder = 3 },
        new Item { ItemId = 1, ItemName = "High Priority", SortOrder = 1 },
        new Item { ItemId = 2, ItemName = "Medium Priority", SortOrder = 2 }
    };
    
    // Sort by custom priority
    var sortedItems = items.OrderBy(i => i.SortOrder).ToList();
    
    ViewBag.ItemList = sortedItems;
    return View();
}
```

## Custom Values

Allow users to create new values not in the list:

### Enable Custom Values

```razor
<ejs-multiselect id="tags" 
    dataSource="ViewBag.TagList"
    allowCustom="true"
    placeholder="Type to create new tags">
</ejs-multiselect>

<ejs-scriptmanager></ejs-scriptmanager>
```

```csharp
public IActionResult Index()
{
    ViewBag.TagList = new List<string>
    {
        "Important",
        "Urgent",
        "Follow-up",
        "Reference"
    };
    
    return View();
}
```

**Result:** Users can type custom values, and they'll be added to selections

### Custom Value with Validation

```razor
<ejs-multiselect id="emails" 
    dataSource="ViewBag.EmailList"
    allowCustom="true"
    customValueHandler="validateEmail"
    placeholder="Enter email addresses">
</ejs-multiselect>

@section Scripts {
    <script>
        function validateEmail(args) {
            var emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            
            if (emailRegex.test(args.text)) {
                args.item = { text: args.text, value: args.text };
            } else {
                alert("Invalid email format");
                args.cancel = true;
            }
        }
    </script>
}
```

### Custom Value with Suggestion

```razor
<ejs-multiselect id="skills" 
    dataSource="ViewBag.SkillList"
    allowCustom="true"
    customValueHandler="onCustomSkill"
    placeholder="Select or add skills">
</ejs-multiselect>

@section Scripts {
    <script>
        function onCustomSkill(args) {
            // Suggest similar existing skills
            var similar = args.dataSource.filter(s => 
                s.toLowerCase().includes(args.text.toLowerCase())
            );
            
            if (similar.length === 0) {
                args.item = { text: args.text, value: args.text };
            }
        }
    </script>
}
```

## Chip Customization

Customize the appearance of selected items:

### Styled Chips

```razor
<ejs-multiselect id="technologies" 
    dataSource="ViewBag.TechList"
    fields-text="TechName"
    fields-value="TechId"
    cssClass="tech-multiselect">
</ejs-multiselect>
```

```css
.tech-multiselect .e-chip {
    background-color: #2196F3;
    color: white;
    border-radius: 20px;
    padding: 6px 12px;
    margin: 3px;
    font-weight: 500;
}

.tech-multiselect .e-icon-close {
    cursor: pointer;
    margin-left: 8px;
    font-weight: bold;
}

.tech-multiselect .e-icon-close:hover {
    opacity: 0.8;
}
```

### Colored Chips by Category

```razor
<ejs-multiselect id="items" 
    dataSource="ViewBag.ItemList"
    fields-text="ItemName"
    fields-value="ItemId"
    valueTemplate="itemChip">
</ejs-multiselect>

@section Scripts {
    <script>
        var itemChip = '<span class="chip-item chip-${Category}">${ItemName}</span>';
    </script>
}
```

```css
.chip-item {
    display: inline-block;
    padding: 6px 12px;
    border-radius: 20px;
    margin: 3px;
    color: white;
    font-weight: 500;
}

.chip-technology {
    background-color: #2196F3;
}

.chip-language {
    background-color: #4CAF50;
}

.chip-framework {
    background-color: #FF9800;
}
```

### Icon Chips

```razor
<ejs-multiselect id="features" 
    dataSource="ViewBag.FeatureList"
    fields-text="FeatureName"
    fields-value="FeatureId"
    valueTemplate="featureChip">
</ejs-multiselect>

@section Scripts {
    <script>
        var featureChip = '<span class="feature-chip">' +
                         '<i class="${IconClass}"></i> ${FeatureName}' +
                         '</span>';
    </script>
}
```

```css
.feature-chip {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    padding: 6px 12px;
    border-radius: 20px;
    background-color: #f0f0f0;
    color: #333;
    margin: 3px;
}

.feature-chip i {
    font-size: 14px;
}
```

## Virtual Scrolling

Efficiently handle large lists (1000+ items):

### Enable Virtual Scrolling

```razor
<ejs-multiselect id="countries" 
    dataSource="ViewBag.CountryList"
    enableVirtualization="true"
    popupHeight="300px"
    placeholder="Select countries">
</ejs-multiselect>

<ejs-scriptmanager></ejs-scriptmanager>
```

```csharp
public IActionResult Index()
{
    // Load all countries - virtual scrolling handles efficiently
    var countries = _context.Countries
        .OrderBy(c => c.CountryName)
        .ToList();
    
    ViewBag.CountryList = countries;
    return View();
}
```

### Virtual Scrolling with Filtering

```razor
<ejs-multiselect id="products" 
    dataSource="ViewBag.ProductList"
    fields-text="ProductName"
    fields-value="ProductId"
    enableVirtualization="true"
    allowFiltering="true"
    popupHeight="400px">
</ejs-multiselect>
```

**Benefits:**
- Only renders visible items (improves performance)
- Smooth scrolling with large datasets
- Combined with filtering for optimal UX

## Cascading MultiSelect

Link multiple MultiSelect components where one depends on another:

### Two-Level Cascading

**View:**

```razor
<!-- Department Selection -->
<ejs-multiselect id="departments" 
    dataSource="ViewBag.DepartmentList"
    fields-text="DepartmentName"
    fields-value="DepartmentId"
    change="onDepartmentChange"
    placeholder="Select departments">
</ejs-multiselect>

<!-- Employee Selection (depends on selected departments) -->
<ejs-multiselect id="employees" 
    fields-text="EmployeeName"
    fields-value="EmployeeId"
    placeholder="Select employees">
</ejs-multiselect>

@section Scripts {
    <script>
        function onDepartmentChange(args) {
            if (args.value && args.value.length > 0) {
                $.ajax({
                    url: "@Url.Action('GetEmployeesByDepartments', 'Home')",
                    type: "POST",
                    data: { departmentIds: args.value },
                    success: function(data) {
                        var empMulti = document.getElementById('employees').ej2_instances[0];
                        empMulti.dataSource = data;
                        empMulti.value = null; // Clear previous selection
                    }
                });
            }
        }
    </script>
}

<ejs-scriptmanager></ejs-scriptmanager>
```

**Controller:**

```csharp
public IActionResult GetEmployeesByDepartments(int[] departmentIds)
{
    var employees = _context.Employees
        .Where(e => departmentIds.Contains(e.DepartmentId))
        .Select(e => new { e.EmployeeId, e.EmployeeName })
        .OrderBy(e => e.EmployeeName)
        .ToList();
    
    return Json(employees);
}
```

### Three-Level Cascading

```razor
<!-- Category -->
<ejs-multiselect id="categories" 
    dataSource="ViewBag.CategoryList"
    change="onCategoryChange"
    placeholder="Select category">
</ejs-multiselect>

<!-- Product (depends on category) -->
<ejs-multiselect id="products" 
    change="onProductChange"
    placeholder="Select product">
</ejs-multiselect>

<!-- Variant (depends on product) -->
<ejs-multiselect id="variants" 
    placeholder="Select variant">
</ejs-multiselect>

@section Scripts {
    <script>
        function onCategoryChange(args) {
            if (args.value) {
                $.ajax({
                    url: "@Url.Action('GetProductsByCategory', 'Home')",
                    data: { categoryId: args.value },
                    success: function(data) {
                        var prodMulti = document.getElementById('products').ej2_instances[0];
                        prodMulti.dataSource = data;
                        prodMulti.value = null;
                    }
                });
            }
        }
        
        function onProductChange(args) {
            if (args.value && args.value.length > 0) {
                $.ajax({
                    url: "@Url.Action('GetVariantsByProducts', 'Home')",
                    type: "POST",
                    data: { productIds: args.value },
                    success: function(data) {
                        var varMulti = document.getElementById('variants').ej2_instances[0];
                        varMulti.dataSource = data;
                        varMulti.value = null;
                    }
                });
            }
        }
    </script>
}
```

## Practical Examples

### Example 1: Technology Stack Selection

```razor
<ejs-multiselect id="tech-stack" 
    dataSource="ViewBag.TechList"
    fields-text="TechName"
    fields-value="TechId"
    fields-groupBy="Category"
    itemTemplate="techTemplate"
    placeholder="Select technologies">
</ejs-multiselect>

@section Scripts {
    <script>
        var techTemplate = '<span class="tech-badge tech-${Category}">' +
                          '<i class="${IconClass}"></i> ${TechName}' +
                          '</span>';
    </script>
}
```

### Example 2: User Permissions with Max Limit

```razor
<ejs-multiselect id="permissions" 
    dataSource="ViewBag.PermissionList"
    fields-text="PermissionName"
    fields-value="PermissionId"
    fields-groupBy="Module"
    mode="CheckBox"
    showSelectAll="true"
    maximumSelectionLength="10"
    placeholder="Select permissions (max 10)">
</ejs-multiselect>
```

### Example 3: Large Dataset with Virtual Scrolling

```csharp
public IActionResult Index()
{
    // Load 10,000 cities
    var cities = Enumerable.Range(1, 10000)
        .Select(i => new City { CityId = i, CityName = $"City {i}" })
        .OrderBy(c => c.CityName)
        .ToList();
    
    ViewBag.CityList = cities;
    return View();
}
```

```razor
<ejs-multiselect id="cities" 
    dataSource="ViewBag.CityList"
    fields-text="CityName"
    fields-value="CityId"
    enableVirtualization="true"
    allowFiltering="true"
    placeholder="Search cities...">
</ejs-multiselect>
```

## Best Practices

### 1. Use Virtual Scrolling for 500+ Items
```csharp
if (items.Count > 500) {
    // Enable virtual scrolling
}
```

### 2. Validate Custom Values
```javascript
function validateCustom(args) {
    // Always validate user input
    if (!isValidValue(args.text)) {
        args.cancel = true;
    }
}
```

### 3. Handle Large Cascading Lists
```csharp
// Cache dependent data
var cached = _cache.Get($"employees-dept-{deptId}");
if (cached == null) {
    cached = GetEmployees(deptId);
    _cache.Set($"employees-dept-{deptId}", cached, TimeSpan.FromHours(1));
}
```

### 4. User Feedback for Selection Limits
```javascript
multiObj.actionComplete = function(args) {
    if (multiObj.value && multiObj.value.length >= maxLimit) {
        showMessage(`Maximum ${maxLimit} items allowed`);
    }
};
```

## Next Steps

✅ **Learn More:**
- [Accessibility](accessibility.md) - Ensure keyboard and screen reader support
- [Templates](templates-and-customization.md) - Customize with templates
- [Getting Started](getting-started.md) - Build your first MultiSelect
