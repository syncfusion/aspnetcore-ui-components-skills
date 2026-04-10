# Data Binding

## Table of Contents
- [Local Array Binding](#local-array-binding)
- [Object Array Binding](#object-array-binding)
- [Complex Object Binding](#complex-object-binding)
- [Remote Data Binding](#remote-data-binding)
- [Field Mapping](#field-mapping)
- [Dynamic Data Loading](#dynamic-data-loading)

## Local Array Binding

### Array of Strings

Simplest binding approach - each string is both text and value:

```razor
<ejs-multiselect id="colors" 
    dataSource="ViewBag.ColorList"
    placeholder="Select colors">
</ejs-multiselect>

<ejs-scriptmanager></ejs-scriptmanager>
```

```csharp
public IActionResult Index()
{
    ViewBag.ColorList = new List<string>
    {
        "Red",
        "Green",
        "Blue",
        "Yellow",
        "Purple",
        "Orange"
    };
    
    return View();
}
```

**Result:**
- Text displayed: "Red", "Green", "Blue", etc.
- Values posted: "Red", "Green", "Blue", etc.

### Array of Numbers

```razor
<ejs-multiselect id="ratings" 
    dataSource="ViewBag.RatingList"
    placeholder="Select ratings">
</ejs-multiselect>
```

```csharp
public IActionResult Index()
{
    ViewBag.RatingList = new List<int> { 1, 2, 3, 4, 5 };
    return View();
}
```

## Object Array Binding

### Simple Object Binding

Bind object array with field mapping:

```razor
<ejs-multiselect id="employees" 
    dataSource="ViewBag.EmployeeList"
    fields-text="EmployeeName"
    fields-value="EmployeeId"
    placeholder="Select employees">
</ejs-multiselect>

<ejs-scriptmanager></ejs-scriptmanager>
```

```csharp
public class Employee
{
    public int EmployeeId { get; set; }
    public string EmployeeName { get; set; }
    public string Department { get; set; }
    public string Email { get; set; }
}

public IActionResult Index()
{
    ViewBag.EmployeeList = new List<Employee>
    {
        new Employee 
        { 
            EmployeeId = 1, 
            EmployeeName = "Alice Johnson", 
            Department = "Engineering" 
        },
        new Employee 
        { 
            EmployeeId = 2, 
            EmployeeName = "Bob Smith", 
            Department = "Sales" 
        },
        new Employee 
        { 
            EmployeeId = 3, 
            EmployeeName = "Carol Davis", 
            Department = "Engineering" 
        }
    };
    
    return View();
}
```

**Result:**
- Text displayed: "Alice Johnson", "Bob Smith", "Carol Davis"
- Values posted: 1, 2, 3

### Object Array with Icon

```razor
<ejs-multiselect id="frameworks" 
    dataSource="ViewBag.FrameworkList"
    fields-text="FrameworkName"
    fields-value="FrameworkId"
    fields-iconCss="IconClass">
</ejs-multiselect>
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
        new Framework 
        { 
            FrameworkId = 1, 
            FrameworkName = "React", 
            IconClass = "icon-react" 
        },
        new Framework 
        { 
            FrameworkId = 2, 
            FrameworkName = "Angular", 
            IconClass = "icon-angular" 
        },
        new Framework 
        { 
            FrameworkId = 3, 
            FrameworkName = "Vue", 
            IconClass = "icon-vue" 
        }
    };
    
    return View();
}
```

## Complex Object Binding

### Nested Field Binding

For objects with nested properties, use dot notation:

```razor
<ejs-multiselect id="products" 
    dataSource="ViewBag.ProductList"
    fields-text="Supplier.CompanyName"
    fields-value="ProductId">
</ejs-multiselect>
```

```csharp
public class Supplier
{
    public int SupplierId { get; set; }
    public string CompanyName { get; set; }
    public string Country { get; set; }
}

public class Product
{
    public int ProductId { get; set; }
    public string ProductName { get; set; }
    public Supplier Supplier { get; set; }
    public decimal Price { get; set; }
}

public IActionResult Index()
{
    var suppliers = new List<Supplier>
    {
        new Supplier { SupplierId = 1, CompanyName = "TechCorp", Country = "USA" },
        new Supplier { SupplierId = 2, CompanyName = "GlobalTech", Country = "Germany" }
    };
    
    ViewBag.ProductList = new List<Product>
    {
        new Product 
        { 
            ProductId = 1, 
            ProductName = "Laptop",
            Supplier = suppliers[0]
        },
        new Product 
        { 
            ProductId = 2, 
            ProductName = "Desktop",
            Supplier = suppliers[1]
        }
    };
    
    return View();
}
```

**Result:**
- Text displayed: "TechCorp", "GlobalTech" (from nested property)
- Values posted: 1, 2

### Multiple Nested Levels

```razor
<ejs-multiselect id="locations" 
    dataSource="ViewBag.LocationList"
    fields-text="Address.City"
    fields-value="LocationId">
</ejs-multiselect>
```

```csharp
public class Address
{
    public string City { get; set; }
    public string Country { get; set; }
}

public class Location
{
    public int LocationId { get; set; }
    public Address Address { get; set; }
}

public IActionResult Index()
{
    ViewBag.LocationList = new List<Location>
    {
        new Location 
        { 
            LocationId = 1, 
            Address = new Address { City = "New York", Country = "USA" } 
        },
        new Location 
        { 
            LocationId = 2, 
            Address = new Address { City = "London", Country = "UK" } 
        }
    };
    
    return View();
}
```

## Remote Data Binding

### Remote URL Data Source

```razor
<ejs-multiselect id="countries" 
    dataSource="YOUR_API_ENDPOINT"
    fields-text="ContactName"
    fields-value="CustomerID"
    placeholder="Select customer">
</ejs-multiselect>

<ejs-scriptmanager></ejs-scriptmanager>
```

### Controller Action Data Source

```razor
<ejs-multiselect id="products" 
    dataSource="@Url.Action('GetProducts', 'Home')"
    fields-text="ProductName"
    fields-value="ProductId"
    placeholder="Select products">
</ejs-multiselect>

<ejs-scriptmanager></ejs-scriptmanager>
```

```csharp
public class ProductsController : Controller
{
    private readonly ApplicationDbContext _context;
    
    public ProductsController(ApplicationDbContext context)
    {
        _context = context;
    }
    
    [HttpGet]
    public IActionResult GetProducts()
    {
        var products = _context.Products
            .Select(p => new { p.ProductId, p.ProductName })
            .ToList();
        
        return Json(products);
    }
}
```

### With Parameters

```razor
<ejs-multiselect id="products" 
    dataSource="@Url.Action('GetProductsByCategory', 'Home', new { categoryId = ViewBag.CategoryId })"
    fields-text="ProductName"
    fields-value="ProductId">
</ejs-multiselect>
```

```csharp
public IActionResult GetProductsByCategory(int categoryId)
{
    var products = _context.Products
        .Where(p => p.CategoryId == categoryId)
        .Select(p => new { p.ProductId, p.ProductName })
        .ToList();
    
    return Json(products);
}
```

## Field Mapping

### Text and Value Fields

```razor
<ejs-multiselect id="employees" 
    dataSource="ViewBag.EmployeeList"
    fields-text="EmployeeName"
    fields-value="EmployeeId">
</ejs-multiselect>
```

| Property | Purpose |
|----------|---------|
| `fields-text` | Display text in dropdown |
| `fields-value` | Value to submit in form |

### Group By Field

Group related items:

```razor
<ejs-multiselect id="products" 
    dataSource="ViewBag.ProductList"
    fields-text="ProductName"
    fields-value="ProductId"
    fields-groupBy="Category">
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
        new Product { ProductId = 3, ProductName = "Desk", Category = "Furniture" },
        new Product { ProductId = 4, ProductName = "Chair", Category = "Furniture" }
    };
    
    return View();
}
```

**Result:**
```
Electronics
  ☐ Laptop
  ☐ Mouse
Furniture
  ☐ Desk
  ☐ Chair
```

### Icon CSS Field

```razor
<ejs-multiselect id="priorities" 
    dataSource="ViewBag.PriorityList"
    fields-text="PriorityName"
    fields-value="PriorityId"
    fields-iconCss="IconClass">
</ejs-multiselect>
```

```csharp
public class Priority
{
    public int PriorityId { get; set; }
    public string PriorityName { get; set; }
    public string IconClass { get; set; }
}

public IActionResult Index()
{
    ViewBag.PriorityList = new List<Priority>
    {
        new Priority { PriorityId = 1, PriorityName = "Low", IconClass = "e-icon-low" },
        new Priority { PriorityId = 2, PriorityName = "Medium", IconClass = "e-icon-medium" },
        new Priority { PriorityId = 3, PriorityName = "High", IconClass = "e-icon-high" }
    };
    
    return View();
}
```

## Dynamic Data Loading

### Load on Demand

```razor
<ejs-multiselect id="departments" 
    dataSource="@Url.Action('GetDepartments', 'Home')"
    fields-text="DepartmentName"
    fields-value="DepartmentId"
    open="onDropdownOpen">
</ejs-multiselect>

@section Scripts {
    <script>
        function onDropdownOpen(args) {
            // Data loads when dropdown opens
            console.log("Loading data...");
        }
    </script>
}
```

### Refresh Data

```razor
<ejs-multiselect id="products" 
    dataSource="ViewBag.ProductList"
    fields-text="ProductName"
    fields-value="ProductId">
</ejs-multiselect>

<button onclick="refreshProducts()">Refresh</button>

@section Scripts {
    <script>
        function refreshProducts() {
            $.ajax({
                url: "@Url.Action('GetProducts', 'Home')",
                success: function(data) {
                    var multiObj = document.getElementById('products').ej2_instances[0];
                    multiObj.dataSource = data;
                }
            });
        }
    </script>
}
```

### Cascading Selection

Two MultiSelects where second depends on first:

```razor
<!-- Category Selection -->
<ejs-multiselect id="categories" 
    dataSource="ViewBag.CategoryList"
    fields-text="CategoryName"
    fields-value="CategoryId"
    change="onCategoryChange"
    placeholder="Select category">
</ejs-multiselect>

<!-- Product Selection (depends on category) -->
<ejs-multiselect id="products" 
    dataSource="ViewBag.ProductList"
    fields-text="ProductName"
    fields-value="ProductId"
    placeholder="Select product">
</ejs-multiselect>

@section Scripts {
    <script>
        function onCategoryChange(args) {
            if (args.value) {
                $.ajax({
                    url: "@Url.Action('GetProductsByCategory', 'Home')",
                    data: { categoryId: args.value },
                    success: function(data) {
                        var productMulti = document.getElementById('products').ej2_instances[0];
                        productMulti.dataSource = data;
                        productMulti.value = null; // Clear previous selection
                    }
                });
            }
        }
    </script>
}
```

```csharp
public IActionResult GetProductsByCategory(int categoryId)
{
    var products = _context.Products
        .Where(p => p.CategoryId == categoryId)
        .Select(p => new { p.ProductId, p.ProductName })
        .ToList();
    
    return Json(products);
}
```

## Data Binding Best Practices

### 1. Always Provide Value Fields

```razor
<!-- Good -->
<ejs-multiselect 
    fields-text="Name"
    fields-value="Id">
</ejs-multiselect>

<!-- Avoid -->
<ejs-multiselect fields-text="Name"></ejs-multiselect>
```

### 2. Validate Server-Side

```csharp
[HttpPost]
public IActionResult SaveSelection(int[] selectedIds)
{
    // Validate that selected IDs exist in database
    var validIds = _context.Items
        .Where(i => selectedIds.Contains(i.Id))
        .Select(i => i.Id)
        .ToList();
    
    if (validIds.Count != selectedIds.Length)
    {
        return BadRequest("Invalid selection");
    }
    
    // Process valid selections
    return Ok();
}
```

### 3. Use Appropriate Data Types

```csharp
// Good: Specific model class
public class Product
{
    public int ProductId { get; set; }
    public string ProductName { get; set; }
}

// Avoid: Anonymous object with inconsistent types
var data = new { id = 1, name = "Item" };
```

### 4. Handle Large Datasets

```csharp
// For 1000+ items, use pagination
public IActionResult GetProducts(int pageNumber = 1)
{
    const int pageSize = 50;
    
    var products = _context.Products
        .OrderBy(p => p.ProductName)
        .Skip((pageNumber - 1) * pageSize)
        .Take(pageSize)
        .Select(p => new { p.ProductId, p.ProductName })
        .ToList();
    
    return Json(products);
}
```

### 5. Cache Remote Data

```csharp
public IActionResult GetCountries()
{
    const string cacheKey = "countries_list";
    
    if (!_cache.TryGetValue(cacheKey, out List<Country> countries))
    {
        countries = _context.Countries
            .OrderBy(c => c.CountryName)
            .ToList();
        
        _cache.Set(cacheKey, countries, TimeSpan.FromHours(1));
    }
    
    return Json(countries);
}
```

## Next Steps

✅ **Learn More:**
- [Filtering and Search](filtering-and-search.md) - Search through bound data
- [Templates](templates-and-customization.md) - Customize data display
- [Advanced Features](advanced-features.md) - Grouping, sorting, and more
