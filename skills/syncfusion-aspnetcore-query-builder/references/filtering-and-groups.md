# Filtering and Groups in QueryBuilder for ASP.NET MVC

## Table of Contents
- [Overview](#overview)
- [Creating Conditions](#creating-conditions)
- [AddRules and DeleteRules](#addrules-and-deleterules)
- [Creating Groups](#creating-groups)
- [Nested Groups](#nested-groups)
- [AND/OR Logic](#andor-logic)
- [NOT Conditions](#not-conditions)
- [ShowButtons Property](#showbuttons-property)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

Filtering in QueryBuilder involves:
1. **Rules:** Individual filter conditions (e.g., "Salary > 50000")
2. **Groups:** Collections of rules with AND/OR logic
3. **Conditions:** The logical operators connecting rules (AND, OR)
4. **UI Controls:** Add/Delete buttons, group management

## Creating Conditions

Conditions are filter rules that users create through the QueryBuilder UI or programmatically.

### Example: Initial Condition via UI

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(data)
    .ShowButtons(new QueryBuilderShowButtons
    {
        GroupInsert = true,
        RuleDelete = true,
        GroupDelete = true
    })
    .Render()
```

**User Action:** Click "Add Condition" button → Select field → Select operator → Enter value

**Resulting Rule:**
```json
{
  "condition": "and",
  "rules": [
    {
      "label": "Salary",
      "field": "Salary",
      "type": "number",
      "operator": "greaterthan",
      "value": 50000
    }
  ]
}
```

## AddRules and DeleteRules

### AddRules Method

Programmatically add new filter rules to the QueryBuilder.

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(data)
    .Render()

<button onclick="addNewRule()">Add Rule</button>

<script>
function addNewRule() {
    var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];
    
    // Add a single rule
    queryBuilder.addRules([{
        label: "FirstName",
        field: "FirstName",
        type: "string",
        operator: "equal",
        value: "Nancy"
    }], 0); // Add to group 0 (root group)
}
</script>
```

### AddRules with Multiple Rules

```html
<script>
function addMultipleRules() {
    var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];
    
    queryBuilder.addRules([
        {
            label: "FirstName",
            field: "FirstName",
            type: "string",
            operator: "equal",
            value: "Nancy"
        },
        {
            label: "Salary",
            field: "Salary",
            type: "number",
            operator: "greaterthan",
            value: 50000
        },
        {
            label: "Department",
            field: "Department",
            type: "string",
            operator: "equal",
            value: "Sales"
        }
    ], 0);
}
</script>
```

### DeleteRules Method

Remove filter rules from the QueryBuilder.

```html
<script>
function deleteFirstRule() {
    var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];
    
    // Delete rule at index 0 from group 0
    queryBuilder.deleteRules([0], 0);
}

function deleteMultipleRules() {
    var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];
    
    // Delete rules at indices 0, 1, 2 from group 0
    queryBuilder.deleteRules([0, 1, 2], 0);
}
</script>
```

## Creating Groups

Groups combine multiple rules with a shared logical operator (AND or OR).

### Example: Create Initial Group

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
                Label = "FirstName",
                Field = "FirstName",
                Type = "string",
                Operator = "equal",
                Value = "Nancy"
            },
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

**Result:** Two rules connected with AND: "FirstName = Nancy AND Salary > 50000"

### AddGroups Method

Programmatically add new filter groups.

```html
<script>
function addNewGroup() {
    var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];
    
    queryBuilder.addGroups([{
        condition: "or",
        rules: [{
            label: "Department",
            field: "Department",
            type: "string",
            operator: "equal",
            value: "Engineering"
        }]
    }], 0); // Add to group 0
}
</script>
```

### DeleteGroups Method

Remove entire groups and all contained rules.

```html
<script>
function deleteGroup() {
    var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];
    
    // Delete group at index 1
    queryBuilder.deleteGroups([1], 0);
}
</script>
```

## Nested Groups

Create complex filter hierarchies with nested AND/OR groups.

### Example: Nested Group Structure

```
Main Group (AND)
├── Rule 1: FirstName = Nancy
└── Sub-Group (OR)
    ├── Rule 2: Department = Sales
    └── Rule 3: Department = Marketing
```

**JSON Representation:**
```json
{
  "condition": "and",
  "rules": [
    {
      "label": "FirstName",
      "field": "FirstName",
      "type": "string",
      "operator": "equal",
      "value": "Nancy"
    },
    {
      "condition": "or",
      "rules": [
        {
          "label": "Department",
          "field": "Department",
          "type": "string",
          "operator": "equal",
          "value": "Sales"
        },
        {
          "label": "Department",
          "field": "Department",
          "type": "string",
          "operator": "equal",
          "value": "Marketing"
        }
      ]
    }
  ]
}
```

**Initialization:**
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
                Label = "FirstName",
                Field = "FirstName",
                Type = "string",
                Operator = "equal",
                Value = "Nancy"
            },
            new QueryBuilderRule
            {
                Condition = "or",
                Rules = new List<QueryBuilderRule>
                {
                    new QueryBuilderRule
                    {
                        Label = "Department",
                        Field = "Department",
                        Type = "string",
                        Operator = "equal",
                        Value = "Sales"
                    },
                    new QueryBuilderRule
                    {
                        Label = "Department",
                        Field = "Department",
                        Type = "string",
                        Operator = "equal",
                        Value = "Marketing"
                    }
                }
            }
        }
    })
    .Render()
```

**Result:** "FirstName = Nancy AND (Department = Sales OR Department = Marketing)"

## AND/OR Logic

Switch between AND and OR operators to combine conditions.

### AND Operator
- **Logic:** All conditions must be true
- **SQL:** `WHERE FirstName = 'Nancy' AND Salary > 50000`
- **Result:** Returns records matching BOTH conditions

### OR Operator
- **Logic:** At least one condition must be true
- **SQL:** `WHERE FirstName = 'Nancy' OR FirstName = 'Andrew'`
- **Result:** Returns records matching EITHER condition

### Example: Toggle AND/OR Programmatically

```html
<script>
function toggleCondition() {
    var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];
    
    // Get current rule
    var rule = queryBuilder.getRule();
    
    // Toggle condition
    if (rule.condition === "and") {
        rule.condition = "or";
    } else {
        rule.condition = "and";
    }
    
    // Update QueryBuilder
    queryBuilder.setRule(rule);
}
</script>
```

## NOT Conditions

Enable NOT operator to invert conditions (negation).

**Setup:**
```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(data)
    .EnableNotCondition(true)
    .Render()
```

**Example:**
- NOT (FirstName = "Nancy") → All employees except Nancy
- NOT (Salary between 50000 and 60000) → Salaries outside that range

### NOT in JSON

```json
{
  "not": true,
  "condition": "and",
  "rules": [
    {
      "label": "FirstName",
      "field": "FirstName",
      "type": "string",
      "operator": "equal",
      "value": "Nancy"
    }
  ]
}
```

## ShowButtons Property

Control which action buttons appear in the QueryBuilder UI.

### Example: Show/Hide Buttons

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(data)
    .ShowButtons(new QueryBuilderShowButtons
    {
        GroupInsert = true,      // Show "Add Group" button
        GroupDelete = true,      // Show delete button on groups
        CloneGroup = false,      // Hide "Clone Group" button
        RuleDelete = true,       // Show delete button on rules
        Edit = true              // Show edit option
    })
    .Render()
```

### All ShowButtons Options

| Property | Default | Purpose |
|----------|---------|---------|
| GroupInsert | true | Add new group button |
| GroupDelete | true | Delete group button |
| CloneGroup | false | Clone/duplicate group |
| RuleDelete | true | Delete rule button |
| Edit | false | Edit existing rules |
| Lock | false | Lock/unlock rules |

## Common Patterns

### Pattern 1: Sales Filter

```html
<script>
var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];

// Sales department with salary > 60000
queryBuilder.addRules([
    {
        label: "Department",
        field: "Department",
        type: "string",
        operator: "equal",
        value: "Sales"
    },
    {
        label: "Salary",
        field: "Salary",
        type: "number",
        operator: "greaterthan",
        value: 60000
    }
], 0);
</script>
```

### Pattern 2: Date Range Filter

```html
<script>
var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];

// Hired between 2015 and 2020
queryBuilder.addRules([{
    label: "HireDate",
    field: "HireDate",
    type: "date",
    operator: "between",
    value: [new Date(2015, 0, 1), new Date(2020, 11, 31)]
}], 0);
</script>
```

### Pattern 3: Multi-Department Filter

```html
<script>
var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];

// Sales OR Engineering OR Marketing
queryBuilder.addGroups([{
    condition: "or",
    rules: [
        { label: "Department", field: "Department", type: "string", operator: "equal", value: "Sales" },
        { label: "Department", field: "Department", type: "string", operator: "equal", value: "Engineering" },
        { label: "Department", field: "Department", type: "string", operator: "equal", value: "Marketing" }
    ]
}], 0);
</script>
```

## Troubleshooting

### Issue: "Rule not appearing after addRules"
**Cause:** Group index incorrect or QueryBuilder not fully initialized  
**Solution:**
1. Ensure QueryBuilder instance exists: `document.getElementById("querybuilder").ej2_instances[0]`
2. Verify group index (0 for root group)
3. Call after QueryBuilder fully renders using `created` event

### Issue: "Deleting rule deletes unintended rule"
**Cause:** Incorrect rule index or deleted from wrong group  
**Solution:**
1. Log all rules to verify indices: `console.log(queryBuilder.getRule())`
2. Double-check group index (0 for root)
3. Use GUI to delete first, then note the index

### Issue: "NOT condition checkbox doesn't appear"
**Cause:** EnableNotCondition not set to true  
**Solution:**
1. Set `EnableNotCondition(true)` in QueryBuilder initialization
2. Clear browser cache
3. Reload page

---

## Next Steps

- Review [Templates and Customization](templates-and-customization.md) for UI customization
- Explore [Advanced Features](advanced-features.md) for import/export
- Check [Styling and Appearance](styling-and-appearance.md) for theme options
