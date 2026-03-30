---
name: syncfusion-aspnetcore-query-builder
description: Implementing the Syncfusion QueryBuilder component for ASP.NET Core. Use this when creating filter builders, query interfaces, filtering UI, rule creation, data-bound columns, or custom templates. Essential for dynamic query generation and complex filtering scenarios.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Forms"
---

# Implementing Syncfusion QueryBuilder for ASP.NET Core

The QueryBuilder component creates and edits filters dynamically. It provides a visual interface for building complex filter conditions with AND/OR logic, nested groups, and customizable operators.

## When to Use This Skill

Use this skill when you need to:
- Build dynamic filter interfaces with rule-based conditions
- Enable users to create AND/OR conditions with nested groups
- Configure columns with specific operators (equality, range, text filtering)
- Customize templates for headers, items, or operator UI
- Implement data binding (local arrays or remote OData services)
- Add accessibility features (WCAG compliance, keyboard navigation, screen readers)
- Apply styling and themes with RTL support
- Import/export or serialize query conditions

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet package installation (`Syncfusion.EJ2.AspNet.Core`)
- CSS and script imports with CDN/local options
- Basic QueryBuilder initialization with TagHelpers
- Program.cs/Startup.cs registration
- Initial data binding with simple example

### Data Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Local data binding with JavaScript arrays
- Remote data with DataManager
- OData and ODatav4 service integration
- Field mapping and schema definition
- Asynchronous data loading patterns

### Columns and Operators
📄 **Read:** [references/columns-and-operators.md](references/columns-and-operators.md)
- Column auto-generation from data source
- Manual column definitions
- Field, label, and type configuration
- Operator types (startswith, endswith, contains, equal, between, etc.)
- Type-specific operator availability
- Format and step properties for display

### Filtering and Groups
📄 **Read:** [references/filtering-and-groups.md](references/filtering-and-groups.md)
- UI-based rule creation and deletion
- AddRules and DeleteRules methods
- Creating and managing groups
- Nested group support
- AND/OR logic and NOT conditions
- ShowButtons property for control visibility

### Templates and Customization
📄 **Read:** [references/templates-and-customization.md](references/templates-and-customization.md)
- Header template customization
- Item template for condition rendering
- Value template for input fields
- Group template structure
- Custom operator UI implementation
- Template event handling with ActionBegin

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Virtual scrolling for performance optimization
- Drag-and-drop rule reordering
- Clone and lock group features
- Import/export query conditions
- Global vs local settings
- Disable specific rules or items

### Styling and Appearance
📄 **Read:** [references/styling-and-appearance.md](references/styling-and-appearance.md)
- CSS class structure and customization
- Theme application (CDN-based vs local files)
- RTL (Right-to-Left) support for Arabic/Hebrew
- Responsive design considerations
- Custom CSS variable overrides
- Dark mode styling

### Accessibility and Localization
📄 **Read:** [references/accessibility-and-localization.md](references/accessibility-and-localization.md)
- WCAG 2.1 Level AA compliance
- ARIA attributes and roles
- Keyboard navigation patterns
- Screen reader announcements
- Localization setup and culture configuration
- Translated strings for conditions, operators, and labels

## Quick Start

```csharp
// Controller: HomeController.cs
using Syncfusion.EJ2.QueryBuilder;

public class HomeController : Controller
{
    public IActionResult Index()
    {
        List<QueryBuilderColumn> columns = new List<QueryBuilderColumn>
        {
            new QueryBuilderColumn { Field = "EmployeeID", Label = "Employee ID", Type = "number" },
            new QueryBuilderColumn { Field = "FirstName", Label = "First Name", Type = "string" },
            new QueryBuilderColumn { Field = "HireDate", Label = "Hire Date", Type = "date" },
            new QueryBuilderColumn { Field = "Salary", Label = "Salary", Type = "number" }
        };
        
        List<Employee> data = new List<Employee>
        {
            new Employee { EmployeeID = 1, FirstName = "Nancy", HireDate = new DateTime(1992, 4, 1), Salary = 60000 },
            new Employee { EmployeeID = 2, FirstName = "Andrew", HireDate = new DateTime(1994, 6, 15), Salary = 65000 },
            new Employee { EmployeeID = 3, FirstName = "Janet", HireDate = new DateTime(1991, 11, 22), Salary = 62000 }
        };
        
        ViewBag.Columns = columns;
        ViewBag.Data = data;
        return View();
    }
}

public class Employee
{
    public int EmployeeID { get; set; }
    public string FirstName { get; set; }
    public DateTime HireDate { get; set; }
    public decimal Salary { get; set; }
}
```

```html
<!-- View: Index.cshtml -->
@{
    ViewBag.Title = "QueryBuilder - ASP.NET Core";
}

<div id="querybuilder" style="width: 100%; max-width: 900px; margin: 20px auto;">
    <ejs-querybuilder id="querybuilder" 
        columns="(IEnumerable<QueryBuilderColumn>)ViewBag.Columns" 
        datasource="(IEnumerable<object>)ViewBag.Data">
    </ejs-querybuilder>
</div>
```

## Common Patterns

### Pattern 1: Add/Delete Rules Programmatically
```html
<script>
    var queryBuilderInstance = document.getElementById("querybuilder").ej2_instances[0];
    
    // Add a new rule
    queryBuilderInstance.addRules([{
        'label': 'FirstName',
        'field': 'FirstName',
        'type': 'string',
        'operator': 'equal',
        'value': 'Nancy'
    }], 0); // Add to group 0
    
    // Delete a specific rule
    queryBuilderInstance.deleteRules([0], 0); // Delete rule at index 0 from group 0
</script>
```

### Pattern 2: Create and Manage Groups
```html
<script>
    var queryBuilderInstance = document.getElementById("querybuilder").ej2_instances[0];
    
    // Add a new group
    queryBuilderInstance.addGroups([{
        'condition': 'or',
        'rules': [{
            'label': 'Department',
            'field': 'Department',
            'type': 'string',
            'operator': 'equal',
            'value': 'Engineering'
        }]
    }], 0);
</script>
```

### Pattern 3: Export and Serialize Query Conditions
```html
<script>
    var queryBuilderInstance = document.getElementById("querybuilder").ej2_instances[0];
    var rules = queryBuilderInstance.getRules(); // Get all rules as JSON
    console.log(JSON.stringify(rules, null, 2));
    
    // Example output:
    // {
    //   "condition": "and",
    //   "rules": [
    //     {"label": "FirstName", "field": "FirstName", "operator": "equal", "value": "Nancy", "type": "string"},
    //     {"label": "Salary", "field": "Salary", "operator": "greaterthan", "value": 50000, "type": "number"}
    //   ]
    // }
</script>
```

### Pattern 4: Handle Change Events
```html
<ejs-querybuilder id="querybuilder"
    columns="(IEnumerable<QueryBuilderColumn>)ViewBag.Columns"
    datasource="(IEnumerable<object>)ViewBag.Data"
    rulechange="onRuleChange">
</ejs-querybuilder>

<script>
    function onRuleChange(args) {
        console.log("Rule changed:", args);
        // Example: args.type = 'field-change', 'operator-change', 'value-change'
        // Example: args.ruleID = unique rule identifier
    }
</script>
```

## Key Configuration Props

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `Columns` | Array | [] | Define column fields, labels, types, operators |
| `DataSource` | Object/Array | null | Bind local data or DataManager for remote |
| `Rule` | Object | null | Set initial rule/group structure |
| `ShowButtons` | Object | {add, delete, clone, lock, reset} | Show/hide action buttons |
| `EnableNotCondition` | Boolean | false | Allow NOT operator |
| `SortDirection` | String | 'Ascending' | Sort columns ascending/descending |
| `AllowValidation` | Boolean | false | Validate rules before submission |
| `Width` | String | '100%' | Component width |
| `Height` | String | 'auto' | Component height |

## Common Use Cases

**Use Case 1: Employee Filtering**
Users build filters on employee records by salary, hire date, department to generate dynamic queries for reporting.

**Use Case 2: E-commerce Search Refinement**
Customers filter products by price range, category, ratings, color using QueryBuilder to narrow results.

**Use Case 3: Admin Dashboard Monitoring**
Administrators create alert rules on server metrics (CPU > 80%, Memory < 20%) using AND/OR conditions.

---

## Related Skills
- [implementing-dropdown-list](../implementing-dropdown-list/) — When you need column selection dropdowns
- [implementing-multiselect](../implementing-multiselect/) — When you need multi-value selection in operator inputs
