# Data Binding

## Table of Contents
- [Overview](#overview)
- [Array Data Binding](#array-data-binding)
- [Object Array Binding](#object-array-binding)
- [Field Mapping](#field-mapping)
- [Dynamic Data Binding](#dynamic-data-binding)
- [Refresh and Update Strategies](#refresh-and-update-strategies)
- [Data Source Patterns](#data-source-patterns)

## Overview

Data binding connects your ListBox to data sources, enabling:
- Display lists from arrays, objects, or databases
- Map complex object properties to display text and values
- Update data dynamically without page refresh
- Handle real-time data changes

The `dataSource` property accepts any IEnumerable collection.

## Array Data Binding

### String Array

Simplest form: direct array of strings.

```csharp
// Page Model
public List<string> Fruits { get; set; }

public void OnGet()
{
    Fruits = new List<string> 
    { 
        "Apple", 
        "Banana", 
        "Cherry", 
        "Date", 
        "Elderberry" 
    };
}
```

```cshtml
<!-- Display -->
<ejs-listbox id="fruitListBox" dataSource="@Model.Fruits"></ejs-listbox>
```

**Result:** Each string appears as both display text and value.

### Numeric Array

```csharp
public List<int> Numbers { get; set; }

public void OnGet()
{
    Numbers = new List<int> { 1, 2, 3, 4, 5, 10, 20, 50, 100 };
}
```

```cshtml
<ejs-listbox id="numberListBox" dataSource="@Model.Numbers"></ejs-listbox>
```

### Empty Array Handling

```csharp
public List<string> Items { get; set; } = new List<string>();

public void OnGet()
{
    if (someCondition)
    {
        Items = FetchItemsFromDatabase();
    }
    // else: remains empty
}
```

```cshtml
<ejs-listbox id="itemListBox" dataSource="@Model.Items" placeholder="No items available"></ejs-listbox>
```

## Object Array Binding

For rich data with multiple properties.

### Define Data Model

```csharp
// Models/Product.cs
public class Product
{
    public int ProductId { get; set; }
    public string ProductName { get; set; }
    public decimal Price { get; set; }
    public string Category { get; set; }
}
```

### Populate in Page Model

```csharp
// Pages/Products.cshtml.cs
public List<Product> Products { get; set; }

public void OnGet()
{
    Products = new List<Product>
    {
        new Product { ProductId = 1, ProductName = "Laptop", Price = 999.99m, Category = "Electronics" },
        new Product { ProductId = 2, ProductName = "Mouse", Price = 29.99m, Category = "Accessories" },
        new Product { ProductId = 3, ProductName = "Keyboard", Price = 79.99m, Category = "Accessories" }
    };
}
```

### Bind with Default Mapping

By default, object's first property is used for display:

```cshtml
<ejs-listbox id="productListBox" dataSource="@Model.Products"></ejs-listbox>
```

**Result:** Displays product IDs (first property), not product names.

**Solution:** Use field mapping (see next section).

## Field Mapping

Map object properties to display text and value.

### Basic Field Mapping

```cshtml
<ejs-listbox id="productListBox" 
             dataSource="@Model.Products"
             fields="@(new Syncfusion.EJ2.DropDowns.ListBoxFieldSettings 
             { 
                 Text = "ProductName",      // Display property
                 Value = "ProductId"         // Value property
             })">
</ejs-listbox>
```

**Result:** Shows product names, stores product IDs as values.

### Get Selected Product

```javascript
var listbox = document.getElementById('productListBox').ej2_instances[0];
var selectedProductId = listbox.value;      // ProductId
var selectedProductName = listbox.text;     // ProductName

// Note: To get full object, you need to find it in dataSource
var selectedProduct = listbox.dataSource.find(p => p.ProductId === selectedProductId);
console.log(selectedProduct); // Full product object
```

### Complex Display: Concatenate Fields

```cshtml
@{
    // Map to property that contains combined text
    // Or use template (see Features section)
}
```

For complex display text, use **templates** in [references/features.md](features.md).

## Dynamic Data Binding

### Load Data from Database

```csharp
// Page Model with database call
public List<Department> Departments { get; set; }

public void OnGet()
{
    using (var context = new CompanyDbContext())
    {
        Departments = context.Departments
            .OrderBy(d => d.DepartmentName)
            .ToList();
    }
}
```

```cshtml
<ejs-listbox id="deptListBox" 
             dataSource="@Model.Departments"
             fields="@(new Syncfusion.EJ2.DropDowns.ListBoxFieldSettings 
             { 
                 Text = "DepartmentName", 
                 Value = "DepartmentId" 
             })">
</ejs-listbox>
```

### Load via AJAX (Async)

```csharp
// API endpoint
public IActionResult OnGetCategories()
{
    var categories = new List<Category>
    {
        new Category { Id = 1, Name = "Electronics" },
        new Category { Id = 2, Name = "Books" }
    };
    return new JsonResult(categories);
}
```

```cshtml
<ejs-listbox id="categoryListBox"></ejs-listbox>

<script>
    // Load data after page loads
    document.addEventListener('DOMContentLoaded', function() {
        fetch('/YourPage?handler=Categories')
            .then(response => response.json())
            .then(data => {
                var listbox = document.getElementById('categoryListBox').ej2_instances[0];
                listbox.dataSource = data;
            });
    });
</script>
```

### Real-Time Data Updates

```javascript
// Update ListBox with new data
function updateListBox(newData) {
    var listbox = document.getElementById('myListBox').ej2_instances[0];
    
    // Clear existing data
    listbox.dataSource = [];
    
    // Set new data
    listbox.dataSource = newData;
}

// Example: Refresh every 5 seconds
setInterval(function() {
    fetch('/api/items')
        .then(r => r.json())
        .then(data => updateListBox(data));
}, 5000);
```

## Refresh and Update Strategies

### Strategy 1: Replace Entire DataSource

Use when data changes significantly:

```javascript
var listbox = document.getElementById('listBox').ej2_instances[0];

// Fetch fresh data
fetch('/api/items')
    .then(r => r.json())
    .then(data => {
        listbox.dataSource = data;
        listbox.refresh();  // Refresh UI
    });
```

### Strategy 2: Update Individual Items

Use for minor changes:

```javascript
// Add item
listbox.dataSource.push({ Id: 5, Name: 'New Item' });
listbox.refresh();

// Remove item
listbox.dataSource = listbox.dataSource.filter(item => item.Id !== 3);
listbox.refresh();

// Update item
var item = listbox.dataSource.find(item => item.Id === 2);
if (item) {
    item.Name = 'Updated Item';
    listbox.refresh();
}
```

### Strategy 3: Smart Refresh with DataBound Event

```cshtml
<ejs-listbox id="listBox" 
             dataSource="@Model.Items"
             dataBound="onDataBound">
</ejs-listbox>

<script>
    function onDataBound(args) {
        console.log('Data binding complete. Items count: ' + args.component.dataSource.length);
        // Restore selection state or apply formatting
    }
    
    // Update data and trigger dataBound
    function refreshData() {
        var listbox = document.getElementById('listBox').ej2_instances[0];
        listbox.dataSource = newData;
    }
</script>
```

## Data Source Patterns

### Pattern 1: Hierarchical/Grouped Data

While ListBox doesn't support built-in grouping, simulate with separators:

```csharp
public class GroupedItem
{
    public int Id { get; set; }
    public string Name { get; set; }
    public bool IsHeader { get; set; }
}

public List<GroupedItem> GroupedItems { get; set; }

public void OnGet()
{
    GroupedItems = new List<GroupedItem>
    {
        new GroupedItem { Id = 0, Name = "--- Electronics ---", IsHeader = true },
        new GroupedItem { Id = 1, Name = "Laptop", IsHeader = false },
        new GroupedItem { Id = 2, Name = "Phone", IsHeader = false },
        
        new GroupedItem { Id = 0, Name = "--- Accessories ---", IsHeader = true },
        new GroupedItem { Id = 3, Name = "Mouse", IsHeader = false },
        new GroupedItem { Id = 4, Name = "Keyboard", IsHeader = false }
    };
}
```

```cshtml
<ejs-listbox id="groupedListBox" dataSource="@Model.GroupedItems" change="onItemSelect"></ejs-listbox>

<script>
    function onItemSelect(args) {
        if (args.isInteracted) {
            // Prevent selecting header items
            var listbox = document.getElementById('groupedListBox').ej2_instances[0];
            if (args.index === 0 || args.index === 3) {
                listbox.value = listbox.previousValue;  // Revert selection
            }
        }
    }
</script>
```

### Pattern 2: Master-Dependent Lists

First list drives data in second list:

```csharp
public List<Category> Categories { get; set; }

public void OnGet()
{
    Categories = new List<Category>
    {
        new Category { Id = 1, Name = "Electronics" },
        new Category { Id = 2, Name = "Books" }
    };
}

public IActionResult OnPostGetProducts(int categoryId)
{
    var products = GetProductsByCategory(categoryId);
    return new JsonResult(products);
}
```

```cshtml
<div class="row">
    <div class="col-md-6">
        <h5>Categories</h5>
        <ejs-listbox id="categoryList" 
                     dataSource="@Model.Categories"
                     fields="@(new Syncfusion.EJ2.DropDowns.ListBoxFieldSettings { Text = "Name", Value = "Id" })"
                     change="onCategoryChange">
        </ejs-listbox>
    </div>
    <div class="col-md-6">
        <h5>Products</h5>
        <ejs-listbox id="productList"></ejs-listbox>
    </div>
</div>

<script>
    function onCategoryChange(args) {
        fetch('/Index?handler=GetProducts&categoryId=' + args.value)
            .then(r => r.json())
            .then(data => {
                var productList = document.getElementById('productList').ej2_instances[0];
                productList.dataSource = data;
            });
    }
</script>
```

### Pattern 3: Filtered Data from Single Source

```javascript
// All items
var allItems = [
    { id: 1, name: 'Active Item 1', status: 'active' },
    { id: 2, name: 'Inactive Item 1', status: 'inactive' },
    { id: 3, name: 'Active Item 2', status: 'active' }
];

var listbox = document.getElementById('listBox').ej2_instances[0];

// Filter to active only
function showActive() {
    listbox.dataSource = allItems.filter(item => item.status === 'active');
}

// Filter to inactive only
function showInactive() {
    listbox.dataSource = allItems.filter(item => item.status === 'inactive');
}

// Show all
function showAll() {
    listbox.dataSource = allItems;
}
```

## Next Steps

- **Selection** → Work with selection events in [references/selection-modes.md](selection-modes.md)
- **Features** → Use templates and filters in [references/features.md](features.md)
- **Styling** → Style based on data in [references/styling-and-appearance.md](styling-and-appearance.md)
- **Common Tasks** → Practical patterns in [references/howto-common-tasks.md](howto-common-tasks.md)
