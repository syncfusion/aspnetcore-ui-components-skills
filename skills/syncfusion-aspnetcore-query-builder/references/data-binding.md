# Data Binding in QueryBuilder for ASP.NET MVC

## Table of Contents
- [Overview](#overview)
- [Local Data Binding](#local-data-binding)
- [Remote Data with DataManager](#remote-data-with-datamanager)
- [OData Services](#odata-services)
- [ODatav4 Services](#odatav4-services)
- [Field Mapping](#field-mapping)
- [Asynchronous Data Loading](#asynchronous-data-loading)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

QueryBuilder uses the `DataSource` property to bind data. It supports:
- **Local data**: JavaScript arrays or C# lists
- **Remote data**: REST API endpoints with DataManager
- **OData services**: OData v3 and v4 protocols
- **Custom adaptors**: JsonAdaptor, ODataAdaptor, ODataV4Adaptor

The component maps column fields to data source properties and uses operators to filter results.

## Local Data Binding

Local data binding is ideal for small datasets or when data is already in memory.

### Example: Bind Array of Objects

**Controller:**
```csharp
public ActionResult Index()
{
    List<QueryBuilderColumn> columns = new List<QueryBuilderColumn>
    {
        new QueryBuilderColumn { Field = "EmployeeID", Label = "Employee ID", Type = "number" },
        new QueryBuilderColumn { Field = "FirstName", Label = "First Name", Type = "string" },
        new QueryBuilderColumn { Field = "Department", Label = "Department", Type = "string" },
        new QueryBuilderColumn { Field = "Salary", Label = "Salary", Type = "number" }
    };

    List<Employee> employees = new List<Employee>
    {
        new Employee { EmployeeID = 1, FirstName = "Nancy", Department = "Sales", Salary = 60000 },
        new Employee { EmployeeID = 2, FirstName = "Andrew", Department = "Engineering", Salary = 75000 },
        new Employee { EmployeeID = 3, FirstName = "Janet", Department = "Marketing", Salary = 65000 },
        new Employee { EmployeeID = 4, FirstName = "Margaret", Department = "Sales", Salary = 62000 }
    };

    ViewBag.Columns = columns;
    ViewBag.Data = employees;
    return View();
}
```

**View:**
```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns((IEnumerable<QueryBuilderColumn>)ViewBag.Columns)
    .DataSource((IEnumerable<object>)ViewBag.Data)
    .Render()
```

**Result:** QueryBuilder displays all employees with filtering on any column.

### Example: Filter by Department

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns((IEnumerable<QueryBuilderColumn>)ViewBag.Columns)
    .DataSource((IEnumerable<object>)ViewBag.Data)
    .Rule(new QueryBuilderRule
    {
        Condition = "and",
        Rules = new List<QueryBuilderRule>
        {
            new QueryBuilderRule
            {
                Label = "Department",
                Field = "Department",
                Type = "string",
                Operator = "equal",
                Value = "Sales"
            }
        }
    })
    .Render()
```

## Remote Data with DataManager

Remote data binding connects to REST API endpoints using DataManager.

### Example: Connect to API Endpoint

**Controller (API):**
```csharp
[HttpGet]
[Route("api/employees")]
public JsonResult GetEmployees()
{
    List<Employee> employees = new List<Employee>
    {
        new Employee { EmployeeID = 1, FirstName = "Nancy", Department = "Sales", Salary = 60000 },
        new Employee { EmployeeID = 2, FirstName = "Andrew", Department = "Engineering", Salary = 75000 },
        new Employee { EmployeeID = 3, FirstName = "Janet", Department = "Marketing", Salary = 65000 }
    };

    return Json(employees, JsonRequestBehavior.AllowGet);
}
```

**View:**
```html
@{
    List<QueryBuilderColumn> columns = new List<QueryBuilderColumn>
    {
        new QueryBuilderColumn { Field = "EmployeeID", Label = "Employee ID", Type = "number" },
        new QueryBuilderColumn { Field = "FirstName", Label = "First Name", Type = "string" },
        new QueryBuilderColumn { Field = "Department", Label = "Department", Type = "string" },
        new QueryBuilderColumn { Field = "Salary", Label = "Salary", Type = "number" }
    };
}

@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(new DataManager
    {
        Url = "/api/employees",
        Adaptor = "JsonAdaptor"
    })
    .Render()
```

**Result:** QueryBuilder fetches data from `/api/employees` endpoint and displays it.

## OData Services

OData (Open Data Protocol) is a standardized protocol for RESTful APIs. Use the `ODataAdaptor` for OData v3 services.

### Example: Connect to OData Service

**View:**
```html
@{
    List<QueryBuilderColumn> columns = new List<QueryBuilderColumn>
    {
        new QueryBuilderColumn { Field = "CustomerID", Label = "Customer ID", Type = "string" },
        new QueryBuilderColumn { Field = "CompanyName", Label = "Company Name", Type = "string" },
        new QueryBuilderColumn { Field = "ContactName", Label = "Contact Name", Type = "string" },
        new QueryBuilderColumn { Field = "Country", Label = "Country", Type = "string" }
    };
}

@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(new DataManager
    {
        Url = "https://services.odata.org/V3/Northwind/Northwind.svc/Customers",
        Adaptor = "ODataAdaptor"
    })
    .Render()
```

**Result:** QueryBuilder connects to Northwind OData service and displays customer records.

### Example: Filter with OData Query

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(new DataManager
    {
        Url = "https://services.odata.org/V3/Northwind/Northwind.svc/Customers",
        Adaptor = "ODataAdaptor"
    })
    .Rule(new QueryBuilderRule
    {
        Condition = "and",
        Rules = new List<QueryBuilderRule>
        {
            new QueryBuilderRule
            {
                Label = "Country",
                Field = "Country",
                Type = "string",
                Operator = "equal",
                Value = "USA"
            }
        }
    })
    .Render()
```

## ODatav4 Services

ODatav4 is the modern, improved version of OData. Use the `ODataV4Adaptor` for OData v4 services.

### Example: Connect to ODatav4 Service

**View:**
```html
@{
    List<QueryBuilderColumn> columns = new List<QueryBuilderColumn>
    {
        new QueryBuilderColumn { Field = "OrderID", Label = "Order ID", Type = "number" },
        new QueryBuilderColumn { Field = "CustomerID", Label = "Customer ID", Type = "string" },
        new QueryBuilderColumn { Field = "OrderDate", Label = "Order Date", Type = "date" },
        new QueryBuilderColumn { Field = "ShippedDate", Label = "Shipped Date", Type = "date" },
        new QueryBuilderColumn { Field = "Freight", Label = "Freight", Type = "number" }
    };
}

@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(new DataManager
    {
        Url = "https://services.odata.org/V4/Northwind/Northwind.svc/Orders",
        Adaptor = "ODataV4Adaptor"
    })
    .Render()
```

**Result:** QueryBuilder connects to ODatav4 endpoint and displays orders.

### Key Differences: OData v3 vs v4

| Aspect | OData v3 | OData v4 |
|--------|----------|---------|
| Adaptor | ODataAdaptor | ODataV4Adaptor |
| Query Syntax | `?$filter=FirstName eq 'Nancy'` | `?$filter=FirstName eq 'Nancy'` (same) |
| Date Format | `datetime'2020-01-01T00:00:00'` | `2020-01-01T00:00:00Z` |
| Naming Convention | PascalCase | camelCase (though flexible) |
| Maturity | Legacy | Current standard |

## Field Mapping

Field mapping ensures QueryBuilder columns correctly map to data source properties.

### Example: Map Column Fields to Data Properties

**Data Model:**
```csharp
public class Product
{
    public int ProductId { get; set; }        // Database ID
    public string ProductName { get; set; }    // Display name
    public decimal UnitPrice { get; set; }     // Price
    public int QuantityInStock { get; set; }   // Inventory
}
```

**Column Mapping:**
```html
@{
    List<QueryBuilderColumn> columns = new List<QueryBuilderColumn>
    {
        new QueryBuilderColumn 
        { 
            Field = "ProductId",        // Matches data property exactly
            Label = "Product ID", 
            Type = "number" 
        },
        new QueryBuilderColumn 
        { 
            Field = "ProductName",      // Matches data property exactly
            Label = "Name", 
            Type = "string" 
        },
        new QueryBuilderColumn 
        { 
            Field = "UnitPrice",        // Matches data property exactly
            Label = "Price", 
            Type = "number" 
        },
        new QueryBuilderColumn 
        { 
            Field = "QuantityInStock",  // Matches data property exactly
            Label = "Stock", 
            Type = "number" 
        }
    };
}

@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(products)
    .Render()
```

**Important:** Field names are **case-sensitive**. `ProductId` and `productId` are different fields.

## Asynchronous Data Loading

For large datasets, asynchronously load data to avoid blocking the UI.

### Example: Async Data Fetch

**Controller:**
```csharp
public ActionResult Index()
{
    List<QueryBuilderColumn> columns = new List<QueryBuilderColumn>
    {
        new QueryBuilderColumn { Field = "EmployeeID", Label = "Employee ID", Type = "number" },
        new QueryBuilderColumn { Field = "FirstName", Label = "First Name", Type = "string" },
        new QueryBuilderColumn { Field = "Salary", Label = "Salary", Type = "number" }
    };

    ViewBag.Columns = columns;
    // Don't load data here, let DataManager handle it via API
    return View();
}

[HttpGet]
[Route("api/employees/async")]
public async Task<JsonResult> GetEmployeesAsync()
{
    // Simulate async data fetch (e.g., from database)
    await Task.Delay(500);

    List<Employee> employees = new List<Employee>
    {
        new Employee { EmployeeID = 1, FirstName = "Nancy", Salary = 60000 },
        new Employee { EmployeeID = 2, FirstName = "Andrew", Salary = 75000 }
    };

    return Json(employees, JsonRequestBehavior.AllowGet);
}
```

**View:**
```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns((IEnumerable<QueryBuilderColumn>)ViewBag.Columns)
    .DataSource(new DataManager
    {
        Url = "/api/employees/async",
        Adaptor = "JsonAdaptor"
    })
    .Render()
```

## Common Patterns

### Pattern 1: Preload with Initial Filter

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(data)
    .Rule(new QueryBuilderRule
    {
        Condition = "and",
        Rules = new List<QueryBuilderRule>
        {
            new QueryBuilderRule
            {
                Label = "Salary",
                Field = "Salary",
                Type = "number",
                Operator = "greaterthan",
                Value = 50000
            }
        }
    })
    .Render()
```

### Pattern 2: Pagination with Remote Data

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(new DataManager
    {
        Url = "/api/employees?pageSize=20&pageIndex=1",
        Adaptor = "JsonAdaptor"
    })
    .Render()
```

### Pattern 3: Cache Remote Data

```html
<script>
var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];
var cachedData = null;

// Cache data after first load
queryBuilder.created = function () {
    var dm = queryBuilder.dataSource;
    dm.executeQuery(new Query()).then(function (e) {
        cachedData = e.result;
    });
};
</script>
```

## Troubleshooting

### Issue: "Column field returns empty values"
**Cause:** Field name doesn't match data source property  
**Solution:** 
1. Verify field names are exact match (case-sensitive)
2. Inspect data in browser console: `console.log(data)`
3. Use browser DevTools Network tab to verify API response

### Issue: "DataManager returns 404"
**Cause:** Endpoint URL is incorrect or API doesn't exist  
**Solution:**
1. Verify route configuration in controller
2. Test URL directly in browser: `http://localhost:port/api/employees`
3. Check CORS configuration if API is on different domain

### Issue: "OData query fails silently"
**Cause:** Service URL incorrect or adaptor mismatch  
**Solution:**
1. Test OData URL in browser: `https://services.odata.org/V3/Northwind/Northwind.svc/$metadata`
2. Verify adaptor matches service version (ODataAdaptor vs ODataV4Adaptor)
3. Check browser console for CORS or network errors

---

## Next Steps

- Review [Columns and Operators](columns-and-operators.md) to customize filtering options
- Explore [Filtering and Groups](filtering-and-groups.md) for UI-based rule creation
- Check [Advanced Features](advanced-features.md) for import/export and serialization
