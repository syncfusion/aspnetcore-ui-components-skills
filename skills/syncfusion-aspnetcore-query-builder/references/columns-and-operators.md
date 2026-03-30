# Columns and Operators in QueryBuilder for ASP.NET MVC

## Table of Contents
- [Column Definitions](#column-definitions)
- [Auto-Generation](#auto-generation)
- [Manual Configuration](#manual-configuration)
- [Operator Types](#operator-types)
- [Type-Specific Operators](#type-specific-operators)
- [Format and Step Properties](#format-and-step-properties)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Column Definitions

Columns define the fields available for filtering in QueryBuilder. Each column maps to a data source property and specifies:
- Field name (required, must match data property)
- Label (display name)
- Type (string, number, date, boolean)
- Operators (available filtering options)
- Format (for display)
- Step (for numeric inputs)

## Auto-Generation

QueryBuilder can automatically generate columns from the data source if you don't specify them manually.

### Example: Auto-Generate Columns

**Controller:**
```csharp
public ActionResult Index()
{
    List<Employee> employees = new List<Employee>
    {
        new Employee { EmployeeID = 1, FirstName = "Nancy", HireDate = new DateTime(1992, 4, 1), Salary = 60000 },
        new Employee { EmployeeID = 2, FirstName = "Andrew", HireDate = new DateTime(1994, 6, 15), Salary = 75000 }
    };

    ViewBag.Data = employees;
    return View();
}
```

**View (No Columns Specified):**
```html
@Html.EJS().QueryBuilder("querybuilder")
    .DataSource((IEnumerable<object>)ViewBag.Data)
    .Render()
```

**Result:** QueryBuilder analyzes the first data object and creates columns for all properties:
- EmployeeID → number type
- FirstName → string type
- HireDate → date type
- Salary → number type

**Note:** Auto-generated columns use property names as labels. To customize, specify columns manually.

## Manual Configuration

Manually define columns for better control over labels, operators, and display format.

### Example: Define Columns with Labels and Operators

```csharp
List<QueryBuilderColumn> columns = new List<QueryBuilderColumn>
{
    new QueryBuilderColumn
    {
        Field = "EmployeeID",
        Label = "Employee ID",
        Type = "number",
        Operators = new List<QueryBuilderOperator>
        {
            new QueryBuilderOperator { Text = "Equal", Value = "equal" },
            new QueryBuilderOperator { Text = "Greater Than", Value = "greaterthan" },
            new QueryBuilderOperator { Text = "Less Than", Value = "lessthan" }
        }
    },
    new QueryBuilderColumn
    {
        Field = "FirstName",
        Label = "First Name",
        Type = "string",
        Operators = new List<QueryBuilderOperator>
        {
            new QueryBuilderOperator { Text = "Starts With", Value = "startswith" },
            new QueryBuilderOperator { Text = "Ends With", Value = "endswith" },
            new QueryBuilderOperator { Text = "Contains", Value = "contains" }
        }
    },
    new QueryBuilderColumn
    {
        Field = "HireDate",
        Label = "Hire Date",
        Type = "date",
        Operators = new List<QueryBuilderOperator>
        {
            new QueryBuilderOperator { Text = "Equal", Value = "equal" },
            new QueryBuilderOperator { Text = "Between", Value = "between" }
        }
    },
    new QueryBuilderColumn
    {
        Field = "Salary",
        Label = "Salary",
        Type = "number",
        Format = "C2", // Currency format
        Operators = new List<QueryBuilderOperator>
        {
            new QueryBuilderOperator { Text = "Between", Value = "between" },
            new QueryBuilderOperator { Text = "Greater Than Or Equal", Value = "greaterthanorequal" }
        }
    }
};

ViewBag.Columns = columns;
```

## Operator Types

QueryBuilder supports various operators depending on the column type. Each operator evaluates conditions for filtering.

### Available Operators

| Operator | Symbol | Supported Types | Description |
|----------|--------|-----------------|-------------|
| **equal** | = | String, Number, Date, Boolean | Exact match |
| **notequal** | ≠ | String, Number, Date, Boolean | Not equal |
| **startswith** | prefix | String | Begins with value |
| **endswith** | suffix | String | Ends with value |
| **contains** | ⊆ | String | Contains substring |
| **greaterthan** | > | Number, Date | Greater than value |
| **greaterthanorequal** | ≥ | Number, Date | Greater than or equal |
| **lessthan** | < | Number, Date | Less than value |
| **lessthanorequal** | ≤ | Number, Date | Less than or equal |
| **between** | ↔ | Number, Date | Within range |
| **notbetween** | ↕ | Number, Date | Outside range |
| **in** | ∈ | String, Number | One of multiple values |
| **notin** | ∉ | String, Number | Not in multiple values |

## Type-Specific Operators

Each data type supports a predefined set of operators.

### String Type Operators

```csharp
new QueryBuilderColumn
{
    Field = "FirstName",
    Label = "First Name",
    Type = "string"
    // Default operators: startswith, endswith, contains, equal, notequal, in, notin
}
```

**Example Conditions:**
- FirstName startswith "A"
- FirstName contains "drew"
- FirstName equal "Nancy"

### Number Type Operators

```csharp
new QueryBuilderColumn
{
    Field = "Salary",
    Label = "Salary",
    Type = "number"
    // Default operators: equal, notequal, greaterthan, greaterthanorequal, lessthan, lessthanorequal, between, notbetween, in, notin
}
```

**Example Conditions:**
- Salary between 50000 and 80000
- Salary greaterthan 60000
- Salary in [50000, 60000, 75000]

### Date Type Operators

```csharp
new QueryBuilderColumn
{
    Field = "HireDate",
    Label = "Hire Date",
    Type = "date"
    // Default operators: equal, notequal, greaterthan, greaterthanorequal, lessthan, lessthanorequal, between, notbetween
}
```

**Example Conditions:**
- HireDate between 1990-01-01 and 2000-12-31
- HireDate greaterthan 1995-01-01

### Boolean Type Operators

```csharp
new QueryBuilderColumn
{
    Field = "IsActive",
    Label = "Is Active",
    Type = "boolean"
    // Default operators: equal, notequal
}
```

**Example Conditions:**
- IsActive equal true
- IsActive equal false

## Format and Step Properties

### Format Property (for Display)

Applies formatting to displayed values, especially useful for dates and currencies.

```csharp
new QueryBuilderColumn
{
    Field = "HireDate",
    Label = "Hire Date",
    Type = "date",
    Format = "yMd"  // Short date format (e.g., 4/1/92)
},
new QueryBuilderColumn
{
    Field = "Salary",
    Label = "Salary",
    Type = "number",
    Format = "C2"   // Currency with 2 decimal places (e.g., $60,000.00)
},
new QueryBuilderColumn
{
    Field = "JoinDate",
    Label = "Join Date",
    Type = "date",
    Format = "MMM d, yyyy"  // e.g., Jan 15, 2020
}
```

**Common Format Codes:**
- **Date Formats:** `yMd`, `MM/dd/yyyy`, `d-MMM-yyyy`, `dd/MM/yyyy`
- **Number Formats:** `C2` (currency), `N2` (2 decimal places), `P` (percentage)

### Step Property (for Numeric Input)

Controls the increment/decrement step for numeric input fields.

```csharp
new QueryBuilderColumn
{
    Field = "Age",
    Label = "Age",
    Type = "number",
    Step = 1  // Increment by 1
},
new QueryBuilderColumn
{
    Field = "Salary",
    Label = "Salary",
    Type = "number",
    Step = 100  // Increment by 100
},
new QueryBuilderColumn
{
    Field = "Price",
    Label = "Price",
    Type = "number",
    Step = 0.01  // Increment by 0.01 (for cents)
}
```

## Common Patterns

### Pattern 1: Mixed Column Types

```csharp
List<QueryBuilderColumn> columns = new List<QueryBuilderColumn>
{
    new QueryBuilderColumn { Field = "EmployeeID", Label = "ID", Type = "number" },
    new QueryBuilderColumn { Field = "FirstName", Label = "Name", Type = "string" },
    new QueryBuilderColumn { Field = "HireDate", Label = "Date", Type = "date", Format = "MM/dd/yyyy" },
    new QueryBuilderColumn { Field = "IsActive", Label = "Status", Type = "boolean" }
};

ViewBag.Columns = columns;
```

### Pattern 2: Restrict Operators

Limit available operators for a specific column:

```csharp
new QueryBuilderColumn
{
    Field = "Status",
    Label = "Status",
    Type = "string",
    Operators = new List<QueryBuilderOperator>
    {
        new QueryBuilderOperator { Text = "Equal", Value = "equal" },
        new QueryBuilderOperator { Text = "Not Equal", Value = "notequal" }
    }
    // Only allow exact match and not equal
}
```

### Pattern 3: Currency Formatting

```csharp
new QueryBuilderColumn
{
    Field = "Price",
    Label = "Unit Price",
    Type = "number",
    Format = "C2",  // Display as currency
    Step = 0.01
}
```

**Display:** When entering value "100", displays as "$100.00"

### Pattern 4: Custom Labels

```csharp
new QueryBuilderColumn
{
    Field = "emp_id",           // Database column name
    Label = "Employee ID",      // Display label (user-friendly)
    Type = "number"
}
```

## Troubleshooting

### Issue: "Operator not available for column"
**Cause:** Selected operator not in column's operator list or type mismatch  
**Solution:**
1. Verify column type matches data (date, number, string, boolean)
2. Check if operator is supported for that type
3. Explicitly define allowed operators if auto-default isn't working

### Issue: "Date format displays incorrectly"
**Cause:** Format code doesn't match intended display or browser locale  
**Solution:**
1. Use standard format codes: `yMd`, `MM/dd/yyyy`, `MMM d, yyyy`
2. Test in browser console: `new Date().toLocaleDateString()`
3. Consider user's locale settings

### Issue: "Numeric input step doesn't work"
**Cause:** Step property not set or type not "number"  
**Solution:**
1. Verify type is "number", not "string"
2. Set step property explicitly (e.g., `Step = 1`)
3. Clear browser cache if change doesn't appear

### Issue: "Column field returns undefined"
**Cause:** Field name doesn't match data source property (case-sensitive)  
**Solution:**
1. Inspect data structure: `console.log(JSON.stringify(data[0]))`
2. Verify exact field name and casing
3. Use field name as it appears in data, not label

---

## Next Steps

- Review [Filtering and Groups](filtering-and-groups.md) to handle rule creation
- Explore [Templates and Customization](templates-and-customization.md) for UI customization
- Check [Advanced Features](advanced-features.md) for performance optimization
