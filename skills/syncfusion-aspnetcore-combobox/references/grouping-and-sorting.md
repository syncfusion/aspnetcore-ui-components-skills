# Grouping & Sorting in ComboBox

## Overview

ComboBox supports organizing items through:
- **Grouping** - Organize items by category (e.g., group countries by continent)
- **Sorting** - Sort items in ascending or descending order
- **Combined** - Use grouping AND sorting together for organized lists

## Grouping Items

Group related items under category headers using the `groupBy` field.

**Basic Grouping:**
```html
@{
    var countries = new List<Country>
    {
        new Country { CountryId = 1, CountryName = "United States", Continent = "North America" },
        new Country { CountryId = 2, CountryName = "Canada", Continent = "North America" },
        new Country { CountryId = 3, CountryName = "India", Continent = "Asia" },
        new Country { CountryId = 4, CountryName = "China", Continent = "Asia" },
        new Country { CountryId = 5, CountryName = "France", Continent = "Europe" }
    };
}

<ejs-combobox id="country"
              dataSource="@countries"
              fields-text="CountryName"
              fields-value="CountryId"
              fields-groupBy="Continent"
              placeholder="Select country">
</ejs-combobox>
```

**Display Result:**
```
North America
├── United States
├── Canada
Asia
├── India
├── China
Europe
├── France
```

**Model:**
```csharp
public class Country
{
    public int CountryId { get; set; }
    public string CountryName { get; set; }
    public string Continent { get; set; }
}
```

## Sorting Items

Sort items alphabetically using the `sortOrder` property.

**Sort Ascending (A to Z):**
```html
<ejs-combobox id="country"
              dataSource="@countries"
              fields-text="CountryName"
              fields-value="CountryId"
              sortOrder="Ascending"
              placeholder="Countries A to Z">
</ejs-combobox>
```

**Sort Descending (Z to A):**
```html
<ejs-combobox id="country"
              dataSource="@countries"
              fields-text="CountryName"
              fields-value="CountryId"
              sortOrder="Descending"
              placeholder="Countries Z to A">
</ejs-combobox>
```

**Without sorting (default order):**
```html
<ejs-combobox id="country"
              dataSource="@countries"
              fields-text="CountryName"
              fields-value="CountryId"
              placeholder="Original order">
</ejs-combobox>
```

## Grouping + Sorting Combined

Use both grouping and sorting together for organized lists.

**Group by Continent, Sort Countries A-Z:**
```html
<ejs-combobox id="country"
              dataSource="@countries"
              fields-text="CountryName"
              fields-value="CountryId"
              fields-groupBy="Continent"
              sortOrder="Ascending"
              placeholder="Grouped & sorted">
</ejs-combobox>
```

**Result:**
```
Asia (alphabetically sorted group)
├── China
├── India
├── Japan
North America (alphabetically sorted group)
├── Canada
├── Mexico
├── United States
Europe (alphabetically sorted group)
├── France
├── Germany
├── Spain
```

## Custom Grouping Logic

Group items using complex logic via the controller.

**Complex Group Example (Group by First Letter):**
```csharp
public IActionResult Index()
{
    var countries = new List<Country>
    {
        new Country { CountryId = 1, CountryName = "United States", FirstLetter = "U" },
        new Country { CountryId = 2, CountryName = "Canada", FirstLetter = "C" },
        new Country { CountryId = 3, CountryName = "China", FirstLetter = "C" },
        new Country { CountryId = 4, CountryName = "India", FirstLetter = "I" }
    };
    return View(countries);
}

public class Country
{
    public int CountryId { get; set; }
    public string CountryName { get; set; }
    public string FirstLetter { get; set; }
}
```

**View:**
```html
@model List<Country>

<ejs-combobox id="country"
              dataSource="@Model"
              fields-text="CountryName"
              fields-value="CountryId"
              fields-groupBy="FirstLetter"
              placeholder="Grouped by first letter">
</ejs-combobox>
```

**Result:**
```
C
├── Canada
├── China
I
├── India
U
├── United States
```

## Styling Groups

Customize the appearance of group headers using templates.

**Example: Add Icons and Styling to Groups**
```html
<ejs-combobox id="product"
              dataSource="@products"
              fields-text="ProductName"
              fields-value="ProductId"
              fields-groupBy="Category">
    <e-combobox-templates>
        <e-template type="GroupTemplate">
            <div class="custom-group-header">
                <span class="group-icon">
                    ${GetIconForCategory(Category)}
                </span>
                <strong>${Category}</strong>
                <span class="group-count">(${ItemCount})</span>
            </div>
        </e-template>
    </e-combobox-templates>
</ejs-combobox>

<style>
    .custom-group-header {
        display: flex;
        align-items: center;
        gap: 10px;
        padding: 10px 15px;
        background-color: #f5f5f5;
        font-weight: bold;
        border-bottom: 1px solid #ddd;
    }
    .group-icon {
        font-size: 18px;
    }
    .group-count {
        margin-left: auto;
        font-size: 12px;
        color: #999;
        font-weight: normal;
    }
</style>
```

## Sorting Remote Data

When using remote data binding, sort on the server side.

**View:**
```html
<ejs-combobox id="customer"
              dataSource="@(new Syncfusion.EJ2.Base.DataManager { Url = \"/api/customers\" })"
              fields-text="CustomerName"
              fields-value="CustomerId"
              fields-groupBy="Region"
              sortOrder="Ascending">
</ejs-combobox>
```

**Web API Controller (handle sorting on server):**
```csharp
[HttpGet]
public IActionResult GetCustomers([FromQuery] string sortOrder = "")
{
    var customers = GetAllCustomers();
    
    if (sortOrder == "Ascending")
    {
        customers = customers
            .OrderBy(c => c.Region)
            .ThenBy(c => c.CustomerName)
            .ToList();
    }
    else if (sortOrder == "Descending")
    {
        customers = customers
            .OrderByDescending(c => c.Region)
            .ThenByDescending(c => c.CustomerName)
            .ToList();
    }
    
    return Ok(customers);
}
```

## Common Patterns

**Pattern 1: Group employees by department**
```html
@{
    var employees = new List<Employee>
    {
        new Employee { EmpId = 1, EmpName = "Alice", Department = "Engineering" },
        new Employee { EmpId = 2, EmpName = "Bob", Department = "Sales" },
        new Employee { EmpId = 3, EmpName = "Charlie", Department = "Engineering" },
        new Employee { EmpId = 4, EmpName = "Diana", Department = "HR" }
    };
}

<ejs-combobox id="employee"
              dataSource="@employees"
              fields-text="EmpName"
              fields-value="EmpId"
              fields-groupBy="Department"
              sortOrder="Ascending"
              placeholder="Select employee">
</ejs-combobox>

<!-- Result:
Engineering
├── Alice
├── Charlie
HR
├── Diana
Sales
├── Bob
-->
```

**Pattern 2: Group products by category with sorting**
```html
<ejs-combobox id="product"
              dataSource="@products"
              fields-text="ProductName"
              fields-value="ProductId"
              fields-groupBy="Category"
              sortOrder="Ascending"
              placeholder="Browse by category">
    <e-combobox-templates>
        <e-template type="GroupTemplate">
            <div style="font-weight: bold; padding: 8px; background: #eee;">
                📦 ${Category}
            </div>
        </e-template>
    </e-combobox-templates>
</ejs-combobox>
```

**Pattern 3: Multi-level grouping (simulate via display)**
```csharp
// Create hierarchical display in controller
public class CascadingItem
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string MainCategory { get; set; }
    public string SubCategory { get; set; }
}

public IActionResult Index()
{
    var items = new List<CascadingItem>
    {
        new CascadingItem 
        { 
            Id = 1, 
            Name = "Samsung Galaxy", 
            MainCategory = "Electronics",
            SubCategory = "Smartphones"
        },
        new CascadingItem 
        { 
            Id = 2, 
            Name = "Apple iPhone", 
            MainCategory = "Electronics",
            SubCategory = "Smartphones"
        },
        new CascadingItem 
        { 
            Id = 3, 
            Name = "Dell Laptop", 
            MainCategory = "Electronics",
            SubCategory = "Computers"
        }
    };
    return View(items);
}
```

**View (group by main category):**
```html
@model List<CascadingItem>

<ejs-combobox id="product"
              dataSource="@Model"
              fields-text="Name"
              fields-value="Id"
              fields-groupBy="MainCategory"
              sortOrder="Ascending">
</ejs-combobox>

<!-- Result:
Electronics
├── Samsung Galaxy (Smartphones)
├── Apple iPhone (Smartphones)
├── Dell Laptop (Computers)
-->
```

## Troubleshooting

**Issue: Grouping not showing**
- ✓ Verify `fields-groupBy` property name matches your data field exactly
- ✓ Check that your data actually has values for the group field
- ✓ Inspect browser console for errors
- ✓ Ensure model includes the grouping property

**Issue: Sort order not applied**
- ✓ Verify `sortOrder` value is "Ascending" or "Descending" (case-sensitive)
- ✓ For remote data, ensure server-side sorting is implemented
- ✓ If using custom data, check if binding happens after sort is set

**Issue: Groups showing in wrong order**
- ✓ Groups appear in order they first appear in data
- ✓ To control group order, sort source data on server before sending
- ✓ Or manually reorder items in controller before passing to view
