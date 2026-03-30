# Filtering and Grouping in DropdownList

Enable real-time filtering for searchable dropdowns and organize items into categories using grouping.

## Enabling Filtering

### Basic Filtering

Allow users to search dropdown items:

```csharp
@Html.EJ2().DropDownList()
    .Id("Products")
    .DataSource(ViewBag.Products)
    .Fields(f => f.Text("ProductName").Value("ProductId"))
    .AllowFiltering(true)
    .Placeholder("Search products...")
    .Render()
```

Displays a search box above the dropdown list. Users type to filter items.

### Filter Types

Choose how items are matched during search:

```csharp
// StartsWith - Default: matches beginning of text
@Html.EJ2().DropDownList()
    .Id("Countries")
    .DataSource(ViewBag.Countries)
    .AllowFiltering(true)
    .FilterType(FilterType.StartsWith)  // "Un" matches "United States"
    .Render()

// Contains - matches text anywhere
@Html.EJ2().DropDownList()
    .Id("Cities")
    .DataSource(ViewBag.Cities)
    .AllowFiltering(true)
    .FilterType(FilterType.Contains)    // "yor" matches "New York"
    .Render()

// EndsWith - matches end of text
@Html.EJ2().DropDownList()
    .Id("Files")
    .DataSource(ViewBag.Files)
    .AllowFiltering(true)
    .FilterType(FilterType.EndsWith)    // ".pdf" matches "document.pdf"
    .Render()
```

### Filter Index

Start filtering from specific character index:

```csharp
@Html.EJ2().DropDownList()
    .Id("Products")
    .DataSource(ViewBag.Products)
    .AllowFiltering(true)
    .FilterType(FilterType.StartsWith)
    .FilterBarPlaceholder("Search...")
    .MinFilterLength(2)  // Start filtering after 2 characters
    .Render()
```

## Case-Insensitive Filtering

Filtering is case-insensitive by default:

```csharp
// User input "pro" matches "Product", "PRODUCT", "pRoDuct"
@Html.EJ2().DropDownList()
    .Id("Items")
    .DataSource(ViewBag.Items)
    .AllowFiltering(true)
    .Render()
```

To disable case-insensitive matching, use server-side filtering.

## Grouping Items

### Basic Grouping

Organize items into categories:

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Category { get; set; }  // Grouping column
}

// Controller
ViewBag.Products = new List<Product>
{
    new Product { Id = 1, Name = "Laptop", Category = "Electronics" },
    new Product { Id = 2, Name = "Mouse", Category = "Electronics" },
    new Product { Id = 3, Name = "Desk", Category = "Furniture" },
    new Product { Id = 4, Name = "Chair", Category = "Furniture" }
};

// View - enable grouping
@Html.EJ2().DropDownList()
    .Id("Products")
    .DataSource(ViewBag.Products)
    .Fields(f => f
        .Text("Name")
        .Value("Id")
        .GroupBy("Category")  // Enable grouping by Category
    )
    .GroupBy("Category")  // Required to activate grouping
    .Render()
```

Dropdown displays:
```
Electronics
  ├─ Laptop
  └─ Mouse
Furniture
  ├─ Desk
  └─ Chair
```

### Nested Object Grouping

Group by nested property:

```csharp
public class Item
{
    public int Id { get; set; }
    public string Name { get; set; }
    public Department Dept { get; set; }
}

public class Department
{
    public string Name { get; set; }
}

// Grouping by nested property
@Html.EJ2().DropDownList()
    .Id("Items")
    .DataSource(ViewBag.Items)
    .Fields(f => f
        .Text("Name")
        .Value("Id")
        .GroupBy("Dept.Name")  // Access nested department name
    )
    .GroupBy("Dept.Name")
    .Render()
```

### Custom Group Headers

Customize how group headers display using templates:

```csharp
@Html.EJ2().DropDownList()
    .Id("Products")
    .DataSource(ViewBag.Products)
    .Fields(f => f
        .Text("Name")
        .Value("Id")
        .GroupBy("Category")
    )
    .GroupBy("Category")
    .GroupTemplate("<strong style='color: #0078d4;'>${Category}</strong> (${Count} items)")
    .Render()
```

## Sorting Dropdown Items

### Sort Order

Sort items alphabetically:

```csharp
using Syncfusion.EJ2.DropDowns;

@Html.EJ2().DropDownList()
    .Id("Countries")
    .DataSource(ViewBag.Countries)
    .Fields(f => f.Text("Name").Value("Id"))
    .SortOrder(SortOrder.Ascending)  // A to Z
    .Render()

// Descending sort
@Html.EJ2().DropDownList()
    .Id("Status")
    .DataSource(ViewBag.Statuses)
    .SortOrder(SortOrder.Descending)  // Z to A
    .Render()
```

### Sorting with Grouping

Sort items within each group:

```csharp
@Html.EJ2().DropDownList()
    .Id("Products")
    .DataSource(ViewBag.Products)
    .Fields(f => f
        .Text("Name")
        .Value("Id")
        .GroupBy("Category")
    )
    .GroupBy("Category")
    .SortOrder(SortOrder.Ascending)  // Items sorted within groups
    .Render()
```

Result:
```
Electronics (sorted A-Z)
  ├─ Keyboard
  ├─ Laptop
  └─ Mouse
Furniture (sorted A-Z)
  ├─ Chair
  └─ Desk
```

## Virtual Grouping

For large datasets, enable virtual rendering:

```csharp
@Html.EJ2().DropDownList()
    .Id("LargeList")
    .DataSource(ViewBag.LargeDataSet)  // 1000+ items
    .Fields(f => f
        .Text("Name")
        .Value("Id")
        .GroupBy("Category")
    )
    .GroupBy("Category")
    .EnableVirtualization(true)  // Virtual rendering
    .PopupHeight("300px")
    .Render()
```

Virtual grouping:
- Renders only visible items in DOM
- Dramatically improves performance
- Essential for 1000+ item dropdowns

## Combining Filtering and Grouping

Use filter with grouped items:

```csharp
@Html.EJ2().DropDownList()
    .Id("Products")
    .DataSource(ViewBag.Products)
    .Fields(f => f
        .Text("ProductName")
        .Value("ProductId")
        .GroupBy("Category")
    )
    .AllowFiltering(true)
    .FilterType(FilterType.Contains)
    .GroupBy("Category")
    .Placeholder("Search and select product..."")
    .Render()
```

User can:
1. Type to filter items
2. See results grouped by category
3. Select from filtered groups

## Advanced Filtering Scenarios

### Server-Side Filtering

Implement custom filtering logic on server:

```csharp
public class ProductController : Controller
{
    [HttpGet]
    public IActionResult FilterProducts(string searchText)
    {
        var products = _context.Products
            .Where(p => p.Name.Contains(searchText))
            .Select(p => new { p.Id, p.Name, p.Category })
            .ToList();
        
        return Json(products);
    }
}

// View
@Html.EJ2().DropDownList()
    .Id("Products")
    .DataSource(d => d
        .Url("/product/filterproducts")
        .Adaptor("UrlAdaptor")
    )
    .Fields(f => f.Text("Name").Value("Id"))
    .AllowFiltering(true)
    .Filtering("onFiltering")
    .Render()

<script>
function onFiltering(args) {
    // This triggers server request with filter text
    args.preventDefaultAction = false;
}
</script>
```

### Highlight Filtered Items

Use item template to highlight search matches:

```csharp
@Html.EJ2().DropDownList()
    .Id("Products")
    .DataSource(ViewBag.Products)
    .AllowFiltering(true)
    .ItemTemplate(@"<div>
        <span class='product-item'>${Name}</span>
    </div>")
    .Render()
```

## Filtering and Grouping Best Practices

### When to Use Filtering

- Lists with 10+ items
- When users need quick access to specific item
- Mobile-friendly interfaces
- Reduces visible options

### When to Use Grouping

- Related items naturally organized
- Logical categories exist
- Users familiar with category structure
- Reduces cognitive load

### Performance Considerations

```csharp
// ❌ Problematic: Large ungrouped, unfiltered list
@Html.EJ2().DropDownList()
    .Id("Items")
    .DataSource(ViewBag.AllItems)  // 5000 items
    .Render()

// ✅ Better: With filtering and grouping
@Html.EJ2().DropDownList()
    .Id("Items")
    .DataSource(ViewBag.AllItems)
    .AllowFiltering(true)
    .GroupBy("Category")
    .EnableVirtualization(true)
    .Render()
```

### Filter with Minimum Characters

Reduce server load for remote data:

```csharp
@Html.EJ2().DropDownList()
    .Id("Countries")
    .DataSource(d => d.Url("/api/countries"))
    .AllowFiltering(true)
    .FilterType(FilterType.StartsWith)
    .MinFilterLength(3)  // Require 3+ chars before filtering
    .Render()
```

## Common Issues

### Grouped items not showing correctly
- Verify GroupBy column exists in data
- Check field mapping for GroupBy
- Ensure data is sorted by group

### Filtering too slow
- Enable virtual rendering for large lists
- Implement server-side filtering
- Use MinFilterLength to reduce requests

### Filter not working
- Verify AllowFiltering(true) is set
- Check data binding
- Confirm FilterType matches your data

---

## Next Steps

- Learn [Templates and Styling](templates-styling.md) for custom group headers
- Review [Advanced Scenarios](advanced-scenarios.md) for complex patterns
- Check [Accessibility and Features](accessibility-features.md) for keyboard support
