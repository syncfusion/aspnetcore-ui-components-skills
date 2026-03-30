# Filtering and Search

## Table of Contents
- [Enable Filtering](#enable-filtering)
- [Filter Types](#filter-types)
- [Custom Filtering](#custom-filtering)
- [Server-Side Filtering](#server-side-filtering)
- [Filter Templates](#filter-templates)
- [No Records Handling](#no-records-handling)
- [Performance Tips](#performance-tips)

## Enable Filtering

### Basic Filtering

Enable the built-in search box:

```razor
<ejs-multiselect id="countries" 
    dataSource="ViewBag.CountryList"
    fields-text="CountryName"
    fields-value="CountryCode"
    allowFiltering="true"
    placeholder="Search and select countries">
</ejs-multiselect>
```

```csharp
public IActionResult Index()
{
    var countries = new List<Country>
    {
        new Country { CountryCode = "US", CountryName = "United States" },
        new Country { CountryCode = "GB", CountryName = "United Kingdom" },
        new Country { CountryCode = "CA", CountryName = "Canada" },
        new Country { CountryCode = "AU", CountryName = "Australia" },
        new Country { CountryCode = "IN", CountryName = "India" }
    };
    
    ViewBag.CountryList = countries;
    return View();
}
```

### Result

When `allowFiltering="true"`, a search box appears in the dropdown. Users can type to filter items in real-time.

```
Search: [united_________]

☐ United States
☐ United Kingdom
☐ United Arab Emirates
```

## Filter Types

Control how filtering matches:

### StartsWith (default)

Matches items that start with the search text:

```razor
<ejs-multiselect id="products" 
    dataSource="ViewBag.ProductList"
    fields-text="ProductName"
    allowFiltering="true"
    filterType="StartsWith">
</ejs-multiselect>
```

**Example:**
- Search: "lapi"
- Results: "Laptop", "Laptop Stand"
- Skipped: "Gaming Laptop" (doesn't start with "lapi")

### Contains

Matches items anywhere in the text:

```razor
<ejs-multiselect id="products" 
    dataSource="ViewBag.ProductList"
    fields-text="ProductName"
    allowFiltering="true"
    filterType="Contains">
</ejs-multiselect>
```

**Example:**
- Search: "top"
- Results: "Laptop", "Desktop", "Laptop Stand", "Desktop Monitor"
- Matches: Contains "top" anywhere

### EndsWith

Matches items that end with the search text:

```razor
<ejs-multiselect id="items" 
    dataSource="ViewBag.ItemList"
    allowFiltering="true"
    filterType="EndsWith">
</ejs-multiselect>
```

**Example:**
- Search: "top"
- Results: "Laptop", "Desktop"

### Choosing Filter Type

| Type | Best For | Example |
|------|----------|---------|
| `StartsWith` | Quick search with predictable prefix | Product codes: "AB123", "AB456" |
| `Contains` | Flexible search anywhere | Generic search: "phone", "land" |
| `EndsWith` | Less common, specialized searches | File extensions: ".pdf", ".xlsx" |

## Custom Filtering

### Client-Side Custom Filter

Implement custom logic with JavaScript:

```razor
<ejs-multiselect id="employees" 
    dataSource="ViewBag.EmployeeList"
    fields-text="EmployeeName"
    fields-value="EmployeeId"
    allowFiltering="true"
    filtering="onEmployeeFilter">
</ejs-multiselect>

@section Scripts {
    <script>
        function onEmployeeFilter(args) {
            // Custom filter logic: only show employees from IT department
            if (args.text) {
                var itEmployees = args.dataSource.filter(function(emp) {
                    return emp.Department === "IT" && 
                           emp.EmployeeName.toLowerCase().includes(args.text.toLowerCase());
                });
                
                args.dataSource = itEmployees;
            }
        }
    </script>
}
```

### Filter by Multiple Fields

```razor
<ejs-multiselect id="products" 
    dataSource="ViewBag.ProductList"
    fields-text="ProductName"
    allowFiltering="true"
    filtering="onProductFilter">
</ejs-multiselect>

@section Scripts {
    <script>
        function onProductFilter(args) {
            // Filter by both ProductName and Category
            if (args.text) {
                var searchText = args.text.toLowerCase();
                args.dataSource = args.dataSource.filter(function(product) {
                    return product.ProductName.toLowerCase().includes(searchText) ||
                           product.Category.toLowerCase().includes(searchText);
                });
            }
        }
    </script>
}
```

## Server-Side Filtering

For large datasets, filter on the server:

**View:**

```razor
<ejs-multiselect id="users" 
    dataSource="@Url.Action('GetFilteredUsers', 'Home')"
    fields-text="UserName"
    fields-value="UserId"
    allowFiltering="true"
    filtering="onUserFilter">
</ejs-multiselect>

@section Scripts {
    <script>
        function onUserFilter(args) {
            args.preventDefaultAction = true;
            
            if (args.text) {
                $.ajax({
                    type: "GET",
                    url: "@Url.Action('GetFilteredUsers', 'Home')",
                    data: { searchText: args.text },
                    success: function(result) {
                        var multiObj = document.getElementById('users').ej2_instances[0];
                        multiObj.dataSource = result;
                    }
                });
            }
        }
    </script>
}
```

**Controller:**

```csharp
public IActionResult GetFilteredUsers(string searchText)
{
    // Simulate database query
    var allUsers = new List<User>
    {
        new User { UserId = 1, UserName = "Alice Johnson" },
        new User { UserId = 2, UserName = "Bob Smith" },
        new User { UserId = 3, UserName = "Charlie Brown" },
        new User { UserId = 4, UserName = "Diana Prince" }
    };
    
    if (string.IsNullOrEmpty(searchText))
    {
        return Json(allUsers);
    }
    
    var filtered = allUsers
        .Where(u => u.UserName.ToLower().Contains(searchText.ToLower()))
        .ToList();
    
    return Json(filtered);
}
```

## Filter Templates

### Custom Filter UI with Template

```razor
<ejs-multiselect id="employees" 
    dataSource="ViewBag.EmployeeList"
    fields-text="EmployeeName"
    fields-value="EmployeeId"
    allowFiltering="true"
    filterBarPlaceholder="Filter by name or department">
    <e-multiselect-filtertemplate>
        <ng-template #filterTemplate let-data="data">
            <div class="filter-custom">
                <span>@Html.Raw("{{ data.EmployeeName }} - {{ data.Department }}")</span>
            </div>
        </ng-template>
    </e-multiselect-filtertemplate>
</ejs-multiselect>
```

### Using Filter Template for Enhanced UI

```razor
<ejs-multiselect id="products" 
    dataSource="ViewBag.ProductList"
    fields-text="ProductName"
    filterBarPlaceholder="Search products..."
    allowFiltering="true"
    filtering="onFilter">
    <e-multiselect-filtertemplate>
        <ng-template #filterTemplate let-data="data">
            <div style="padding: 5px; display: flex; justify-content: space-between;">
                <span>@Html.Raw("{{ data.ProductName }}")</span>
                <span style="color: #999;">@Html.Raw("${{ data.Price }}")</span>
            </div>
        </ng-template>
    </e-multiselect-filtertemplate>
</ejs-multiselect>
```

## No Records Handling

### Show Message When No Matches

```razor
<ejs-multiselect id="items" 
    dataSource="ViewBag.ItemList"
    allowFiltering="true"
    noRecordsTemplate="@Html.Raw("<div style='text-align: center; padding: 20px;'><p>No matching items found</p></div>")">
</ejs-multiselect>
```

### Custom No Records Template

```razor
<ejs-multiselect id="items" 
    dataSource="ViewBag.ItemList"
    allowFiltering="true"
    noRecordsTemplate="noRecordsTemplate">
</ejs-multiselect>

@section Scripts {
    <script>
        var noRecordsTemplate = '<div class="no-records">' +
                                 '<i class="icon-search"></i>' +
                                 '<p>No items found</p>' +
                                 '<small>Try different keywords</small>' +
                                 '</div>';
    </script>
}
```

### CSS for No Records

```css
.no-records {
    text-align: center;
    padding: 30px 10px;
    color: #999;
}

.no-records i {
    font-size: 32px;
    display: block;
    margin-bottom: 10px;
    opacity: 0.5;
}

.no-records p {
    margin: 10px 0;
    font-weight: 500;
}

.no-records small {
    display: block;
    font-size: 12px;
}
```

## Practical Examples

### Example 1: Search Products by Name or Category

```razor
<ejs-multiselect id="products" 
    dataSource="ViewBag.ProductList"
    fields-text="ProductName"
    fields-value="ProductId"
    allowFiltering="true"
    filterType="Contains"
    filtering="onProductSearch">
</ejs-multiselect>

@section Scripts {
    <script>
        function onProductSearch(args) {
            if (args.text) {
                var searchTerm = args.text.toLowerCase();
                args.dataSource = args.dataSource.filter(function(product) {
                    return product.ProductName.toLowerCase().includes(searchTerm) ||
                           (product.Category && product.Category.toLowerCase().includes(searchTerm));
                });
            }
        }
    </script>
}
```

```csharp
public class Product
{
    public int ProductId { get; set; }
    public string ProductName { get; set; }
    public string Category { get; set; }
    public decimal Price { get; set; }
}

public IActionResult Index()
{
    ViewBag.ProductList = new List<Product>
    {
        new Product { ProductId = 1, ProductName = "Laptop Pro", Category = "Electronics" },
        new Product { ProductId = 2, ProductName = "USB Cable", Category = "Accessories" },
        new Product { ProductId = 3, ProductName = "Wireless Mouse", Category = "Electronics" },
        new Product { ProductId = 4, ProductName = "Laptop Stand", Category = "Accessories" }
    };
    
    return View();
}
```

### Example 2: Live Search with Debounce

```razor
<ejs-multiselect id="users" 
    dataSource="@Url.Action('SearchUsers', 'Home')"
    fields-text="UserName"
    fields-value="UserId"
    allowFiltering="true"
    filtering="onUserSearch">
</ejs-multiselect>

@section Scripts {
    <script>
        var debounceTimer;
        
        function onUserSearch(args) {
            clearTimeout(debounceTimer);
            
            debounceTimer = setTimeout(function() {
                if (args.text && args.text.length > 2) {
                    $.ajax({
                        url: "@Url.Action('SearchUsers', 'Home')?q=" + args.text,
                        success: function(data) {
                            var multiObj = document.getElementById('users').ej2_instances[0];
                            multiObj.dataSource = data;
                        }
                    });
                }
            }, 300); // 300ms debounce
            
            args.preventDefaultAction = true;
        }
    </script>
}
```

### Example 3: Case-Insensitive Search

```razor
<ejs-multiselect id="cities" 
    dataSource="ViewBag.CityList"
    allowFiltering="true"
    filtering="onCitySearch">
</ejs-multiselect>

@section Scripts {
    <script>
        function onCitySearch(args) {
            if (args.text) {
                var searchLower = args.text.toLowerCase();
                args.dataSource = args.dataSource.filter(function(city) {
                    return city.toLowerCase().includes(searchLower);
                });
            }
        }
    </script>
}
```

## Performance Tips

### 1. Use Server-Side Filtering for Large Lists

```csharp
// Good: Only 10-50 items in dropdown at a time
public IActionResult GetFilteredItems(string searchText)
{
    var query = _context.Items.AsQueryable();
    
    if (!string.IsNullOrEmpty(searchText))
    {
        query = query.Where(i => i.Name.Contains(searchText));
    }
    
    return Json(query.Take(50).ToList());
}
```

### 2. Implement Debouncing for Remote Search

```javascript
var searchTimeout;

function onFilter(args) {
    clearTimeout(searchTimeout);
    
    searchTimeout = setTimeout(function() {
        // Make server request
    }, 500); // Wait 500ms after user stops typing
    
    args.preventDefaultAction = true;
}
```

### 3. Virtual Scrolling for Large Local Lists

```razor
<ejs-multiselect id="items" 
    dataSource="ViewBag.ItemList"
    allowFiltering="true"
    enableVirtualization="true">
</ejs-multiselect>
```

### 4. Limit Initial Data

```csharp
public IActionResult Index()
{
    // Only load first 50 items initially
    var items = _context.Items.Take(50).ToList();
    ViewBag.ItemList = items;
    return View();
}
```

## Troubleshooting

### Issue: Filtering Too Slow

**Solution:** Use server-side filtering for datasets >500 items

### Issue: Search Not Working

**Solution:** Ensure `allowFiltering="true"` is set on the MultiSelect tag

### Issue: Case Sensitivity

**Solution:** Use `.toLowerCase()` for case-insensitive matching

### Issue: No Results Message Not Showing

**Solution:** Verify `noRecordsTemplate` is properly formatted HTML

## Next Steps

✅ **Learn More:**
- [Data Binding](data-binding.md) - Bind to various data sources
- [Templates](templates-and-customization.md) - Customize search results appearance
- [Advanced Features](advanced-features.md) - Combine with other features
