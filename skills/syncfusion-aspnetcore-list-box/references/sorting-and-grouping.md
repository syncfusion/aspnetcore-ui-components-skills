# Sorting and Grouping ListBox Data

## Table of Contents
- [Overview](#overview)
- [Sorting Items](#sorting-items)
- [Grouping Items](#grouping-items)
- [Combined Sorting and Grouping](#combined-sorting-and-grouping)
- [Practical Examples](#practical-examples)
- [Troubleshooting](#troubleshooting)

## Overview

The ListBox component supports both **sorting** and **grouping** of items to organize large datasets efficiently:

- **Sorting**: Arrange items in alphabetical order (ascending/descending)
- **Grouping**: Group related items together based on a category field

These features help users navigate and understand large lists more easily.

**When to use:**
- Long lists of items (50+ items)
- Items with natural categories or types
- Need to organize by status, priority, or category
- Users need to quickly find related items

## Sorting Items

### Sorting Configuration

Use the `sortOrder` property to enable sorting. Available options:

| Value | Description |
|-------|-------------|
| `None` | No sorting (default) |
| `Ascending` | A-Z or 0-9 order |
| `Descending` | Z-A or 9-0 order |

### Example: Ascending Order

```cshtml
<!-- Razor TagHelper -->
<ejs-listbox id="sortedList" 
             dataSource="@Model.Items"
             sortOrder="Syncfusion.EJ2.DropDowns.SortOrder.Ascending">
</ejs-listbox>
```

**C# Model:**
```csharp
public class IndexModel : PageModel
{
    public List<string> Items { get; set; }
    
    public void OnGet()
    {
        Items = new List<string> 
        { 
            "Zebra", "Apple", "Mango", "Banana", "Cherry" 
        };
        // Will display: Apple, Banana, Cherry, Mango, Zebra
    }
}
```

### Example: Descending Order

```cshtml
<ejs-listbox id="descendingList" 
             dataSource="@Model.Items"
             sortOrder="Syncfusion.EJ2.DropDowns.SortOrder.Descending">
</ejs-listbox>
```

**Result:** Zebra, Mango, Cherry, Banana, Apple

### Example: Sorting Complex Objects

When binding objects, sorting applies to the Text field:

```csharp
public class Item 
{ 
    public int Value { get; set; }
    public string Text { get; set; }
}

public List<Item> Products { get; set; }

public void OnGet()
{
    Products = new List<Item>
    {
        new Item { Value = 1, Text = "Laptop" },
        new Item { Value = 2, Text = "Desktop" },
        new Item { Value = 3, Text = "Tablet" }
    };
}
```

```cshtml
<ejs-listbox id="productList" 
             dataSource="@Model.Products"
             fields="@(new Syncfusion.EJ2.DropDowns.ListBoxFieldSettings 
             { 
                 Text = "Text",
                 Value = "Value"
             })"
             sortOrder="Syncfusion.EJ2.DropDowns.SortOrder.Ascending">
</ejs-listbox>
```

**Result:** Desktop, Laptop, Tablet (alphabetically sorted by Text field)

## Grouping Items

### Grouping Configuration

Use the `groupBy` field in `ListBoxFieldSettings` to group items by a category field:

```csharp
public class Product 
{ 
    public string Name { get; set; }
    public string Category { get; set; }  // Used for grouping
    public int Price { get; set; }
}

public List<Product> Items { get; set; }

public void OnGet()
{
    Items = new List<Product>
    {
        new Product { Name = "Apple", Category = "Fruits", Price = 100 },
        new Product { Name = "Banana", Category = "Fruits", Price = 80 },
        new Product { Name = "Carrot", Category = "Vegetables", Price = 50 },
        new Product { Name = "Broccoli", Category = "Vegetables", Price = 60 },
        new Product { Name = "Tomato", Category = "Vegetables", Price = 40 }
    };
}
```

```cshtml
<ejs-listbox id="groupedList" 
             dataSource="@Model.Items"
             fields="@(new Syncfusion.EJ2.DropDowns.ListBoxFieldSettings 
             { 
                 Text = "Name",
                 Value = "Name",
                 GroupBy = "Category"  // Group by Category field
             })">
</ejs-listbox>
```

**Display:**
```
Fruits
├─ Apple
├─ Banana

Vegetables
├─ Carrot
├─ Broccoli
├─ Tomato
```

### Understanding Grouping Headers

- Group headers appear as non-selectable dividers
- Items under each group are selectable
- Headers help users understand item organization
- Useful for large datasets with multiple categories

## Combined Sorting and Grouping

Use both features together: **sort items within each group** while keeping groups organized.

### Example: Grouped and Sorted Vegetables

```csharp
public class Vegetable 
{ 
    public string Name { get; set; }
    public string Type { get; set; }  // Root, Leaf, Fruit
}

public List<Vegetable> Vegetables { get; set; }

public void OnGet()
{
    Vegetables = new List<Vegetable>
    {
        new Vegetable { Name = "Tomato", Type = "Fruit" },
        new Vegetable { Name = "Zucchini", Type = "Fruit" },
        new Vegetable { Name = "Cucumber", Type = "Fruit" },
        new Vegetable { Name = "Spinach", Type = "Leaf" },
        new Vegetable { Name = "Lettuce", Type = "Leaf" },
        new Vegetable { Name = "Carrot", Type = "Root" },
        new Vegetable { Name = "Beet", Type = "Root" }
    };
}
```

```cshtml
<ejs-listbox id="vegList" 
             dataSource="@Model.Vegetables"
             fields="@(new Syncfusion.EJ2.DropDowns.ListBoxFieldSettings 
             { 
                 Text = "Name",
                 GroupBy = "Type"
             })"
             sortOrder="Syncfusion.EJ2.DropDowns.SortOrder.Ascending">
</ejs-listbox>
```

**Display (Grouped, then sorted within groups):**
```
Fruit
├─ Cucumber
├─ Tomato
├─ Zucchini

Leaf
├─ Lettuce
├─ Spinach

Root
├─ Beet
├─ Carrot
```

## Practical Examples

### Example 1: Employee Directory by Department

```csharp
public class Employee 
{ 
    public int Id { get; set; }
    public string Name { get; set; }
    public string Department { get; set; }
}

public List<Employee> Employees { get; set; }

public void OnGet()
{
    Employees = new List<Employee>
    {
        new Employee { Id = 1, Name = "Alice Johnson", Department = "Engineering" },
        new Employee { Id = 2, Name = "Bob Smith", Department = "Sales" },
        new Employee { Id = 3, Name = "Charlie Brown", Department = "Engineering" },
        new Employee { Id = 4, Name = "Diana Prince", Department = "Marketing" },
        new Employee { Id = 5, Name = "Eve Wilson", Department = "Sales" }
    };
}
```

```cshtml
<h3>Select Employee by Department</h3>
<ejs-listbox id="employeeList" 
             dataSource="@Model.Employees"
             fields="@(new Syncfusion.EJ2.DropDowns.ListBoxFieldSettings 
             { 
                 Text = "Name",
                 Value = "Id",
                 GroupBy = "Department"
             })"
             sortOrder="Syncfusion.EJ2.DropDowns.SortOrder.Ascending">
</ejs-listbox>

<script>
    document.getElementById('employeeList').addEventListener('change', function(e) {
        console.log('Selected Employee ID: ' + e.detail.value);
    });
</script>
```

**Result:**
```
Engineering
├─ Alice Johnson
├─ Charlie Brown

Marketing
├─ Diana Prince

Sales
├─ Bob Smith
├─ Eve Wilson
```

### Example 2: Product Catalog by Category

```csharp
public class Product 
{ 
    public int ProductId { get; set; }
    public string ProductName { get; set; }
    public string Category { get; set; }
    public decimal Price { get; set; }
}

public List<Product> Products { get; set; }

public void OnGet()
{
    Products = new List<Product>
    {
        new Product { ProductId = 1, ProductName = "Laptop", Category = "Electronics", Price = 800 },
        new Product { ProductId = 2, ProductName = "Mouse", Category = "Electronics", Price = 25 },
        new Product { ProductId = 3, ProductName = "Desk Chair", Category = "Furniture", Price = 200 },
        new Product { ProductId = 4, ProductName = "Bookshelf", Category = "Furniture", Price = 150 },
        new Product { ProductId = 5, ProductName = "Monitor", Category = "Electronics", Price = 300 }
    };
}
```

```cshtml
<h3>Browse Products</h3>
<ejs-listbox id="productCatalog" 
             dataSource="@Model.Products"
             fields="@(new Syncfusion.EJ2.DropDowns.ListBoxFieldSettings 
             { 
                 Text = "ProductName",
                 Value = "ProductId",
                 GroupBy = "Category"
             })"
             sortOrder="Syncfusion.EJ2.DropDowns.SortOrder.Ascending">
</ejs-listbox>

<script>
    const productList = document.getElementById('productCatalog').ej2_instances[0];
    
    productList.change = function(e) {
        const selected = e.itemData;
        console.log('Selected: ' + selected.ProductName + ' ($' + selected.Price + ')');
    };
</script>
```

### Example 3: Multi-Level Filtering with Sorting

```csharp
public class TaskItem 
{ 
    public int TaskId { get; set; }
    public string Title { get; set; }
    public string Status { get; set; }  // Todo, In Progress, Done
    public string Priority { get; set; } // Low, Medium, High
}

public List<TaskItem> Tasks { get; set; }

public void OnGet()
{
    Tasks = new List<TaskItem>
    {
        new TaskItem { TaskId = 1, Title = "Update Profile", Status = "Todo", Priority = "Low" },
        new TaskItem { TaskId = 2, Title = "Fix Bug #123", Status = "In Progress", Priority = "High" },
        new TaskItem { TaskId = 3, Title = "Write Tests", Status = "Todo", Priority = "Medium" },
        new TaskItem { TaskId = 4, Title = "Deploy v2.0", Status = "In Progress", Priority = "High" }
    };
}
```

```cshtml
<h3>Tasks by Status (Sorted by Title)</h3>
<ejs-listbox id="taskList" 
             dataSource="@Model.Tasks"
             fields="@(new Syncfusion.EJ2.DropDowns.ListBoxFieldSettings 
             { 
                 Text = "Title",
                 GroupBy = "Status"
             })"
             sortOrder="Syncfusion.EJ2.DropDowns.SortOrder.Ascending"
             cssClass="task-list">
</ejs-listbox>

<style>
    .task-list .e-list-group-item {
        font-weight: bold;
        background-color: #f0f0f0;
    }
</style>
```

## Troubleshooting

### Issue: Grouping Not Working

**Problem:** Items appear but aren't grouped by category.

**Solution:**
1. Verify `groupBy` field is set correctly in `ListBoxFieldSettings`
2. Ensure the field name matches exactly (case-sensitive):
   ```csharp
   GroupBy = "Category"  // ✓ Correct
   GroupBy = "category"  // ✗ Wrong if field is "Category"
   ```
3. Check that the field exists in your data model

### Issue: Sorting Ignores Custom Order

**Problem:** Items aren't sorting as expected.

**Causes:**
- `sortOrder` is set to `None` (default)
- Data contains numbers that sort as strings: `"1", "10", "2"` sorts as 1, 10, 2
- Mixed data types in the field

**Solution:**
```csharp
// ✓ Correct: Format numbers consistently
Items = new List<string> { "01", "02", "10", "20" };

// ✗ Problem: Mixed types sort incorrectly
Items = new List<string> { "1", "02", "ten", "20" };
```

### Issue: Groups Display but Can't Select Items

**Problem:** Grouping works but all items are disabled.

**Solution:**
1. Ensure `groupBy` field is used only for grouping, not as Value:
   ```csharp
   new ListBoxFieldSettings 
   { 
       Text = "Name",           // Selectable field
       Value = "Id",            // Use unique value
       GroupBy = "Category"     // Separate grouping field
   }
   ```

2. Check that items have unique values
3. Verify selection is enabled (not disabled globally)

### Issue: Unexpected Group Order

**Problem:** Groups appear in random order instead of alphabetical.

**Solution:**
Apply sorting at the data level before binding, or group headers display in the order data appears.

```csharp
Items = Items
    .OrderBy(x => x.Category)    // Sort by group first
    .ThenBy(x => x.Name)         // Then sort within groups
    .ToList();
```

### Issue: Performance Slow with Large Grouped Lists

**Problem:** ListBox sluggish with 1000+ items.

**Solution:**
1. Use virtual scrolling with scroller:
   ```cshtml
   <ejs-listbox id="largeList" 
                dataSource="@Model.Items"
                scrollHeight="300px">
   </ejs-listbox>
   ```

2. Paginate data before binding
3. Consider a different component (DataGrid, Tree) for very large datasets

## Best Practices

✓ **Do:**
- Use grouping for 20+ items with multiple categories
- Keep group names short and meaningful
- Combine sorting for better navigation
- Test with realistic data volume
- Provide a clear visual distinction for group headers

✗ **Don't:**
- Create too many groups (5+ groups may be overwhelming)
- Nest groups (ListBox doesn't support multi-level grouping)
- Use grouping for 2-3 items (too little benefit)
- Mix sorting directions across different features

## See Also

- [Selection Modes](selection-modes.md) - Work with selected grouped items
- [Data Binding](data-binding.md) - Advanced data source configuration
- [Features](features.md) - Virtual scrolling for large lists
