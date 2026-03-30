# Advanced Features in QueryBuilder for ASP.NET MVC

## Table of Contents
- [Overview](#overview)
- [Virtual Scrolling](#virtual-scrolling)
- [Drag and Drop](#drag-and-drop)
- [Clone and Lock Groups](#clone-and-lock-groups)
- [Import and Export](#import-and-export)
- [Global vs Local Settings](#global-vs-local-settings)
- [Disable Specific Rules](#disable-specific-rules)
- [Performance Optimization](#performance-optimization)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

Advanced features enable:
- **Performance:** Virtual scrolling for large datasets
- **User Experience:** Drag-and-drop, clone/lock rules, import/export
- **Customization:** Global and local settings, disable specific features
- **Optimization:** Lazy loading, query caching, batch operations

## Virtual Scrolling

Virtual scrolling improves performance with large datasets by rendering only visible rows.

### Example: Enable Virtual Scrolling

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(largeDataSet) // 10,000+ records
    .AllowVirtualization(true)
    .Render()

<script>
var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];

// Virtual scrolling automatically renders only visible rows
// This improves rendering speed and memory usage

// When scrolling, only visible 20-30 rows are in DOM
// Other rows are created/destroyed dynamically
</script>
```

### Performance Comparison

| Feature | Without Virtual Scroll | With Virtual Scroll |
|---------|------------------------|---------------------|
| Initial Load | 5000ms+ | <500ms |
| Memory Usage | 50MB+ | <5MB |
| Scroll Smoothness | Stutters | Smooth |
| Optimal Size | <1000 rows | 10,000+ rows |

### Configuration

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(data)
    .AllowVirtualization(true)
    .VirtualHeight("500px" ) // Height of viewport
    .ItemHeight(40)            // Height of each row
    .Render()
```

## Drag and Drop

Allow users to reorder rules and groups by dragging.

### Example: Enable Drag and Drop

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(data)
    .AllowDragDrop(true)
    .Render()

<script>
// Users can now drag rules/groups to reorder them
// Drag indicator appears while dragging
// Drop zone highlights on hover
</script>
```

### Drag Events

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(data)
    .AllowDragDrop(true)
    .RuleDragStart("onRuleDragStart")
    .RuleDragStop("onRuleDragStop")
    .Render()

<script>
function onRuleDragStart(args) {
    console.log("Rule drag started:", args);
    // args.rule = dragged rule
    // args.groupIndex = source group index
    // Can prevent drag by setting args.cancel = true
}

function onRuleDragStop(args) {
    console.log("Rule dropped:", args);
    // args.rule = dragged rule
    // args.fromIndex = original position
    // args.toIndex = new position
}
</script>
```

## Clone and Lock Groups

Clone (duplicate) groups and lock them to prevent modifications.

### Example: Clone and Lock

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(data)
    .ShowButtons(new QueryBuilderShowButtons
    {
        CloneGroup = true,  // Show clone button
        LockGroup = true    // Show lock button
    })
    .Render()
```

### Programmatic Clone

```html
<button onclick="cloneSelectedGroup()">Clone Group</button>

<script>
function cloneSelectedGroup() {
    var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];
    
    // Clone group at index 0
    queryBuilder.cloneGroups([0], 0);
}
</script>
```

### Lock Group (Disable Edits)

```html
<script>
var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];

// Lock group to prevent rule additions/deletions
// Users can still view but not modify
queryBuilder.lockGroups([0], true);

// Unlock group
queryBuilder.lockGroups([0], false);
</script>
```

**Use Cases:**
- Archive filters (prevent accidental changes)
- Template filters (show predefined conditions)
- Read-only audit trail

## Import and Export

Serialize and deserialize filter rules for persistence and sharing.

### Example: Export Rules as JSON

```html
<button onclick="exportRules()">Export Filters</button>

<script>
function exportRules() {
    var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];
    
    // Get current rules as JSON
    var rules = queryBuilder.getRule();
    var json = JSON.stringify(rules, null, 2);
    
    console.log(json);
    // Output:
    // {
    //   "condition": "and",
    //   "rules": [
    //     {
    //       "label": "FirstName",
    //       "field": "FirstName",
    //       "type": "string",
    //       "operator": "equal",
    //       "value": "Nancy"
    //     }
    //   ]
    // }
    
    // Download as file
    downloadJSON(json, "filters.json");
}

function downloadJSON(json, filename) {
    var dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(json);
    var downloadAnchorNode = document.createElement('a');
    downloadAnchorNode.setAttribute("href", dataStr);
    downloadAnchorNode.setAttribute("download", filename);
    document.body.appendChild(downloadAnchorNode);
    downloadAnchorNode.click();
    downloadAnchorNode.remove();
}
</script>
```

### Example: Import Rules from JSON

```html
<input type="file" id="fileInput" onchange="importRules()" />

<script>
function importRules() {
    var file = document.getElementById("fileInput").files[0];
    var reader = new FileReader();
    
    reader.onload = function(e) {
        var json = JSON.parse(e.target.result);
        var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];
        
        // Set rules from imported JSON
        queryBuilder.setRule(json);
    };
    
    reader.readAsText(file);
}
</script>
```

### Save Rules to Server

```html
<button onclick="saveRulesToServer()">Save Filters</button>

<script>
function saveRulesToServer() {
    var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];
    var rules = queryBuilder.getRule();
    
    // Send to server via AJAX
    $.ajax({
        url: '/api/filters/save',
        type: 'POST',
        contentType: 'application/json',
        data: JSON.stringify({ rules: rules }),
        success: function(response) {
            console.log("Filters saved:", response);
            alert("Filters saved successfully!");
        },
        error: function(error) {
            console.error("Error saving filters:", error);
        }
    });
}

// Server endpoint (C#)
[HttpPost]
[Route("api/filters/save")]
public JsonResult SaveFilters(QueryFilterRequest request)
{
    // Save request.Rules to database
    // request.Rules contains serialized QueryBuilder rule object
    
    return Json(new { message = "Filters saved successfully" });
}

public class QueryFilterRequest
{
    public object Rules { get; set; }
}
</script>
```

## Global vs Local Settings

Configure settings globally or per-group.

### Global Settings

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(data)
    .ShowButtons(new QueryBuilderShowButtons
    {
        GroupInsert = true,
        RuleDelete = true,
        CloneGroup = true
    })
    .AllowValidation(true)
    .EnableNotCondition(true)
    .Render()
```

**Applies to:** All groups and rules

### Local Settings (Per-Group)

```html
<script>
var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];

// Get first group
var rule = queryBuilder.getRule();
var firstGroup = rule.rules[0];

// Override for specific group
if (firstGroup.condition === 'and') {
    // Apply group-specific logic
    firstGroup.readonly = true; // Lock this group
}

queryBuilder.setRule(rule);
</script>
```

## Disable Specific Rules

Prevent certain rules or columns from being used.

### Example: Disable Column

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns.Where(c => c.Field != "Password")) // Exclude Password column
    .DataSource(data)
    .Render()
```

### Example: Disable Operators for a Column

```html
@{
    List<QueryBuilderColumn> columns = new List<QueryBuilderColumn>
    {
        new QueryBuilderColumn
        {
            Field = "FirstName",
            Label = "First Name",
            Type = "string",
            Operators = new List<QueryBuilderOperator>
            {
                new QueryBuilderOperator { Text = "Equal", Value = "equal" },
                new QueryBuilderOperator { Text = "Not Equal", Value = "notequal" }
                // Exclude: startswith, endswith, contains
            }
        }
    };
}

@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(data)
    .Render()
```

### Example: Disable Features Programmatically

```html
<script>
var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];

// Disable adding new groups
queryBuilder.showButtons = { groupInsert: false };

// Disable deleting rules (but allow adding)
queryBuilder.showButtons = { ruleDelete: false };

// Disable NOT operator
queryBuilder.enableNotCondition = false;

// Refresh UI
queryBuilder.refresh();
</script>
```

## Performance Optimization

### Optimization 1: Lazy Load Large Datasets

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(new DataManager
    {
        Url = "/api/employees/page",
        Adaptor = "JsonAdaptor"
    })
    .AllowVirtualization(true)
    .Render()

<!-- Server returns paginated data -->
[HttpGet]
[Route("api/employees/page")]
public JsonResult GetEmployeesPage(int pageSize = 50, int pageIndex = 1)
{
    var employees = dbContext.Employees
        .Skip((pageIndex - 1) * pageSize)
        .Take(pageSize)
        .ToList();
    
    return Json(new { result = employees, count = dbContext.Employees.Count() });
}
```

### Optimization 2: Debounce Input Changes

```html
<script>
var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];
var debounceTimer;

queryBuilder.addEventListener('ruleChange', function(args) {
    clearTimeout(debounceTimer);
    debounceTimer = setTimeout(function() {
        console.log("Rule changed, updating results...");
        updateResults(queryBuilder.getRule());
    }, 500);
});

function updateResults(rules) {
    // Execute query or fetch results
}
</script>
```

### Optimization 3: Batch Operations

```html
<script>
var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];

// Disable UI updates during batch operations
queryBuilder.beginUpdate();

// Add multiple rules without refreshing UI
queryBuilder.addRules([rule1, rule2, rule3], 0);
queryBuilder.addGroups([group1, group2], 0);

// Reenable updates and refresh once
queryBuilder.endUpdate();
</script>
```

## Common Patterns

### Pattern 1: Template Filter Library

```html
<select id="templateSelect" onchange="applyTemplate()">
    <option value="">Select a template</option>
    <option value="sales">Sales Department</option>
    <option value="recent">Recent Hires</option>
    <option value="highSalary">High Salary</option>
</select>

<script>
var templates = {
    sales: {
        condition: "and",
        rules: [{
            field: "Department",
            operator: "equal",
            value: "Sales"
        }]
    },
    recent: {
        condition: "and",
        rules: [{
            field: "HireDate",
            operator: "greaterthan",
            value: new Date(2020, 0, 1)
        }]
    },
    highSalary: {
        condition: "and",
        rules: [{
            field: "Salary",
            operator: "greaterthan",
            value: 75000
        }]
    }
};

function applyTemplate() {
    var selected = document.getElementById("templateSelect").value;
    var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];
    
    if (templates[selected]) {
        queryBuilder.setRule(templates[selected]);
    }
}
</script>
```

### Pattern 2: Export to SQL Query

```html
<button onclick="generateSQLQuery()">Generate SQL</button>

<script>
function generateSQLQuery() {
    var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];
    var rules = queryBuilder.getRule();
    
    var sql = rulesToSQL(rules);
    console.log("Generated SQL:", sql);
    alert(sql);
}

function rulesToSQL(rule) {
    if (rule.rules) {
        var conditions = rule.rules.map(r => {
            if (r.rules) {
                return "(" + rulesToSQL(r) + ")";
            } else {
                return `${r.field} ${r.operator} '${r.value}'`;
            }
        });
        return conditions.join(` ${rule.condition.toUpperCase()} `);
    }
}
</script>
```

## Troubleshooting

### Issue: "Virtual scrolling causes jumpy rendering"
**Cause:** ItemHeight property incorrect or large dataset  
**Solution:**
1. Verify ItemHeight matches actual row height
2. Reduce dataset size or implement server pagination
3. Increase buffer for smoother scrolling

### Issue: "Drag-and-drop not working"
**Cause:** AllowDragDrop not set or conflicting event handlers  
**Solution:**
1. Ensure AllowDragDrop is set to true
2. Check for JavaScript errors in console
3. Remove conflicting CSS pointer-events

### Issue: "Import rules fails silently"
**Cause:** JSON structure mismatch or invalid rules  
**Solution:**
1. Validate JSON structure matches export format
2. Log rules before/after: `console.log(rules)`
3. Use setRule() with try-catch for error handling

---

## Next Steps

- Review [Styling and Appearance](styling-and-appearance.md) for theme options
- Explore [Accessibility and Localization](accessibility-and-localization.md) for i18n support
- Check [Data Binding](data-binding.md) for remote data strategies
