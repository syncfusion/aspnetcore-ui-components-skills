# Tag Helper Syntax

## Table of Contents
- [Basic Structure](#basic-structure)
- [Essential Properties](#essential-properties)
- [Data Binding](#data-binding)
- [Events](#events)
- [CSS Classes](#css-classes)
- [Best Practices](#best-practices)

## Basic Structure

MultiSelect tag helper syntax follows ASP.NET Core conventions:

```razor
<ejs-multiselect id="multiselect-id" 
    dataSource="@ViewBag.DataList"
    fields-text="TextProperty"
    fields-value="ValueProperty"
    placeholder="Select items...">
</ejs-multiselect>

<ejs-scriptmanager></ejs-scriptmanager>
```

### Minimal Example

```razor
<!-- Minimal MultiSelect with string array -->
<ejs-multiselect id="colors" dataSource="ViewBag.Colors"></ejs-multiselect>
```

## Essential Properties

### Display Properties

**id** - Unique identifier (required)
```razor
<ejs-multiselect id="languages"></ejs-multiselect>
```

**placeholder** - Hint text when empty
```razor
<ejs-multiselect id="skills" placeholder="Select your skills"></ejs-multiselect>
```

**popupHeight** - Height of dropdown list (default: "300px")
```razor
<ejs-multiselect id="items" popupHeight="400px"></ejs-multiselect>
```

**width** - Width of the control
```razor
<ejs-multiselect id="items" width="100%"></ejs-multiselect>
```

**cssClass** - Custom CSS classes
```razor
<ejs-multiselect id="items" cssClass="custom-multiselect"></ejs-multiselect>
```

### Data Properties

**dataSource** - Array or remote data
```razor
<!-- Local array -->
<ejs-multiselect id="skills" dataSource="ViewBag.SkillsList"></ejs-multiselect>

<!-- List from controller -->
<ejs-multiselect id="products" dataSource="@Model.ProductList"></ejs-multiselect>
```

**fields-text** - Field to display
```razor
<ejs-multiselect id="products" 
    dataSource="ViewBag.ProductList"
    fields-text="ProductName">
</ejs-multiselect>
```

**fields-value** - Field to submit as value
```razor
<ejs-multiselect id="products" 
    dataSource="ViewBag.ProductList"
    fields-value="ProductId">
</ejs-multiselect>
```

**fields-groupBy** - Field to group items
```razor
<ejs-multiselect id="products" 
    dataSource="ViewBag.ProductList"
    fields-text="ProductName"
    fields-value="ProductId"
    fields-groupBy="Category">
</ejs-multiselect>
```

### Selection Properties

**value** - Initially selected values (array)
```razor
<ejs-multiselect id="skills" 
    dataSource="ViewBag.SkillsList"
    value="ViewBag.PreSelectedSkills">
</ejs-multiselect>
```

```csharp
// Controller
public IActionResult Index()
{
    ViewBag.SkillsList = new List<string> { "C#", "JavaScript", "Python", "Java" };
    ViewBag.PreSelectedSkills = new string[] { "C#", "JavaScript" };
    return View();
}
```

**maximumSelectionLength** - Maximum items that can be selected
```razor
<!-- Allow selecting up to 3 items -->
<ejs-multiselect id="skills" 
    dataSource="ViewBag.SkillsList"
    maximumSelectionLength="3">
</ejs-multiselect>
```

### State Properties

**disabled** - Disable the control
```razor
<ejs-multiselect id="skills" dataSource="ViewBag.SkillsList" disabled="true"></ejs-multiselect>
```

**readonly** - Prevent editing (but allow opening dropdown)
```razor
<ejs-multiselect id="skills" dataSource="ViewBag.SkillsList" readonly="true"></ejs-multiselect>
```

**allowFiltering** - Enable search/filter box
```razor
<ejs-multiselect id="countries" 
    dataSource="ViewBag.CountryList"
    allowFiltering="true">
</ejs-multiselect>
```

**filterType** - How to filter (StartsWith, Contains, EndsWith)
```razor
<ejs-multiselect id="items" 
    dataSource="ViewBag.ItemList"
    allowFiltering="true"
    filterType="Contains">
</ejs-multiselect>
```

### Display Mode Properties

**mode** - Selection mode (Default, CheckBox, Delimiter)
```razor
<!-- Default mode: tags/chips -->
<ejs-multiselect id="skills" 
    dataSource="ViewBag.SkillsList"
    mode="Default">
</ejs-multiselect>

<!-- Checkbox mode: checkboxes for each item -->
<ejs-multiselect id="languages" 
    dataSource="ViewBag.Languages"
    mode="CheckBox">
</ejs-multiselect>

<!-- Delimiter mode: comma-separated in textbox -->
<ejs-multiselect id="tags" 
    dataSource="ViewBag.TagList"
    mode="Delimiter">
</ejs-multiselect>
```

**showSelectAll** - Show "Select All" checkbox (for checkbox mode)
```razor
<ejs-multiselect id="items" 
    dataSource="ViewBag.ItemList"
    mode="CheckBox"
    showSelectAll="true">
</ejs-multiselect>
```

## Data Binding

### Pattern 1: String Array

```razor
<!-- View -->
<ejs-multiselect id="colors" dataSource="ViewBag.Colors"></ejs-multiselect>

@section Scripts {
    <script>
        // Programmatic access
        var multiObj = document.getElementById('colors').ej2_instances[0];
        console.log(multiObj.value);
    </script>
}
```

```csharp
// Controller
public IActionResult Index()
{
    ViewBag.Colors = new List<string> { "Red", "Green", "Blue", "Yellow", "Purple" };
    return View();
}
```

### Pattern 2: Object Array

```razor
<ejs-multiselect id="employees" 
    dataSource="ViewBag.EmployeeList"
    fields-text="EmployeeName"
    fields-value="EmployeeId">
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
        new Employee { EmployeeId = 1, EmployeeName = "John Doe", Department = "IT" },
        new Employee { EmployeeId = 2, EmployeeName = "Jane Smith", Department = "HR" },
        new Employee { EmployeeId = 3, EmployeeName = "Bob Johnson", Department = "IT" }
    };
    return View();
}
```

### Pattern 3: Model Binding

```razor
<ejs-multiselect id="selectedProducts" dataSource="@Model.AvailableProducts">
</ejs-multiselect>
```

```csharp
public class ProductViewModel
{
    public List<Product> AvailableProducts { get; set; }
    public List<string> SelectedProductIds { get; set; }
}
```

## Events

### Common Events

**created** - Fires when MultiSelect is created
```razor
<ejs-multiselect id="skills" 
    dataSource="ViewBag.SkillsList"
    created="onMultiSelectCreated">
</ejs-multiselect>

@section Scripts {
    <script>
        function onMultiSelectCreated(args) {
            console.log("MultiSelect created");
        }
    </script>
}
```

**select** - Fires when item is selected
```razor
<ejs-multiselect id="items" 
    dataSource="ViewBag.ItemList"
    select="onItemSelect">
</ejs-multiselect>

@section Scripts {
    <script>
        function onItemSelect(args) {
            console.log("Selected: " + args.itemData);
        }
    </script>
}
```

**removed** - Fires when item is removed
```razor
<ejs-multiselect id="skills" 
    dataSource="ViewBag.SkillsList"
    removed="onItemRemoved">
</ejs-multiselect>

@section Scripts {
    <script>
        function onItemRemoved(args) {
            console.log("Removed: " + args.itemData);
        }
    </script>
}
```

**change** - Fires when selection changes (select or remove)
```razor
<ejs-multiselect id="items" 
    dataSource="ViewBag.ItemList"
    change="onSelectionChange">
</ejs-multiselect>

@section Scripts {
    <script>
        function onSelectionChange(args) {
            console.log("Current selection: " + args.value);
        }
    </script>
}
```

**open** - Fires when dropdown opens
```razor
<ejs-multiselect id="items" 
    dataSource="ViewBag.ItemList"
    open="onDropdownOpen">
</ejs-multiselect>

@section Scripts {
    <script>
        function onDropdownOpen(args) {
            console.log("Dropdown opened");
        }
    </script>
}
```

**close** - Fires when dropdown closes
```razor
<ejs-multiselect id="items" 
    dataSource="ViewBag.ItemList"
    close="onDropdownClose">
</ejs-multiselect>

@section Scripts {
    <script>
        function onDropdownClose(args) {
            console.log("Dropdown closed");
        }
    </script>
}
```

## CSS Classes

### Styling with CSS Classes

```razor
<ejs-multiselect id="skills" 
    dataSource="ViewBag.SkillsList"
    cssClass="custom-multiselect">
</ejs-multiselect>
```

```css
/* Style the container */
.custom-multiselect {
    font-size: 16px;
    border-radius: 4px;
}

/* Style the chips */
.custom-multiselect .e-chip {
    background-color: #2196F3;
    color: white;
}

/* Style the input area */
.custom-multiselect .e-input-group {
    border: 1px solid #ccc;
}

/* Style dropdown items */
.custom-multiselect .e-list-item {
    padding: 10px;
}

/* Style on hover */
.custom-multiselect .e-list-item:hover {
    background-color: #f5f5f5;
}

/* Style selected item */
.custom-multiselect .e-list-item.e-active {
    background-color: #e3f2fd;
}
```

### Built-in CSS Classes

| Class | Purpose |
|-------|---------|
| `.e-input-group` | Container wrapper |
| `.e-chip` | Selected item chip |
| `.e-icon-close` | Chip remove button |
| `.e-list-item` | Dropdown list item |
| `.e-list-item.e-active` | Selected list item |
| `.e-list-group` | Item group header |
| `.e-ddl-icon` | Dropdown arrow icon |

## Practical Examples

### Example 1: Basic Selection

```razor
<ejs-multiselect id="categories" 
    dataSource="ViewBag.Categories"
    placeholder="Select categories">
</ejs-multiselect>
```

### Example 2: With Grouping

```razor
<ejs-multiselect id="products" 
    dataSource="ViewBag.ProductList"
    fields-text="ProductName"
    fields-value="ProductId"
    fields-groupBy="Category"
    placeholder="Select products">
</ejs-multiselect>
```

### Example 3: With Filtering and Checkboxes

```razor
<ejs-multiselect id="employees" 
    dataSource="ViewBag.EmployeeList"
    fields-text="EmployeeName"
    fields-value="EmployeeId"
    allowFiltering="true"
    filterType="Contains"
    mode="CheckBox"
    showSelectAll="true"
    placeholder="Search and select employees">
</ejs-multiselect>
```

### Example 4: With Selection Limit

```razor
<ejs-multiselect id="skills" 
    dataSource="ViewBag.SkillsList"
    maximumSelectionLength="3"
    placeholder="Select up to 3 skills">
</ejs-multiselect>

@section Scripts {
    <script>
        var multiObj = document.getElementById('skills').ej2_instances[0];
        multiObj.actionComplete = function(args) {
            if (multiObj.value && multiObj.value.length >= 3) {
                console.log("Maximum selection reached");
            }
        };
    </script>
}
```

## Best Practices

### 1. Always Provide Labels

```razor
<div class="form-group">
    <label for="skills">Skills <span class="text-danger">*</span></label>
    <ejs-multiselect id="skills" 
        dataSource="ViewBag.SkillsList"
        placeholder="Select your skills">
    </ejs-multiselect>
</div>
```

### 2. Use aria-label for Accessibility

```razor
<ejs-multiselect id="skills" 
    dataSource="ViewBag.SkillsList"
    aria-label="Select your professional skills">
</ejs-multiselect>
```

### 3. Validate Server-Side

```csharp
[HttpPost]
public IActionResult SaveSelection(string[] selectedIds)
{
    if (selectedIds == null || selectedIds.Length == 0)
    {
        ModelState.AddModelError("selectedIds", "At least one item must be selected");
        return View(model);
    }
    
    // Process selection
    return Ok();
}
```

### 4. Handle Empty State

```razor
<ejs-multiselect id="items" 
    dataSource="ViewBag.ItemList"
    noRecordsTemplate="No items found"
    placeholder="Select items...">
</ejs-multiselect>
```

### 5. Set Reasonable Popup Height

```razor
<!-- For small lists (<10 items) -->
<ejs-multiselect id="colors" dataSource="ViewBag.Colors" popupHeight="200px"></ejs-multiselect>

<!-- For large lists (>50 items) -->
<ejs-multiselect id="countries" dataSource="ViewBag.Countries" popupHeight="400px"></ejs-multiselect>
```

## Next Steps

✅ **Learn More:**
- [Data Binding](data-binding.md) - Complex data source patterns
- [Filtering and Search](filtering-and-search.md) - Search functionality
- [Templates](templates-and-customization.md) - Customize appearance
- [Advanced Features](advanced-features.md) - Checkboxes, grouping, sorting
