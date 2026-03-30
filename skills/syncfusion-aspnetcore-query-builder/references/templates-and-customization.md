# Templates and Customization in QueryBuilder for ASP.NET MVC

## Table of Contents
- [Overview](#overview)
- [Header Template](#header-template)
- [Item Template](#item-template)
- [Value Template](#value-template)
- [Group Template](#group-template)
- [Custom Operator UI](#custom-operator-ui)
- [Error Handling Templates](#error-handling-templates)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

Templates allow you to customize the appearance and behavior of QueryBuilder UI elements. You can define:
- **Header Template:** Customize top-level controls
- **Item Template:** Customize individual rule rows
- **Value Template:** Customize input fields
- **Group Template:** Customize group headers
- **Operator Template:** Customize operator dropdowns

## Header Template

The header template customizes the top-level UI showing AND/OR/NOT options and action buttons.

### Example: Custom Header with Bootstrap Icons

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(data)
    .HeaderTemplate("headerTemplate")
    .Render()

<script type="text/x-template" id="headerTemplate">
    <div class="e-header-container">
        <label class="e-label">Filter Conditions</label>
        
        <!-- Condition Toggle -->
        <div class="e-condition-container">
            <span class="e-condition-label">Condition:</span>
            <select class="e-condition-dropdown" onchange="changeCondition(this)">
                <option value="and">AND</option>
                <option value="or">OR</option>
            </select>
        </div>
        
        <!-- Action Buttons -->
        <div class="e-action-buttons">
            <button class="e-btn-icon" title="Add Condition">
                <i class="bi bi-plus-circle"></i>
            </button>
            <button class="e-btn-icon" title="Delete Group">
                <i class="bi bi-trash"></i>
            </button>
            <button class="e-btn-icon" title="Reset Filters">
                <i class="bi bi-arrow-clockwise"></i>
            </button>
        </div>
    </div>
</script>

<style>
.e-header-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px;
    background-color: #f5f5f5;
    border-bottom: 1px solid #ddd;
}

.e-label {
    font-weight: bold;
    margin: 0 10px;
}

.e-condition-container {
    flex: 1;
    margin: 0 10px;
}

.e-condition-dropdown {
    padding: 5px 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
}

.e-action-buttons {
    display: flex;
    gap: 5px;
}

.e-btn-icon {
    padding: 5px 10px;
    border: none;
    background: #007bff;
    color: white;
    border-radius: 4px;
    cursor: pointer;
}

.e-btn-icon:hover {
    background: #0056b3;
}
</style>
```

## Item Template

The item template customizes individual filter rows (field, operator, value).

### Example: Custom Item Template with Styling

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(data)
    .RuleTemplate("itemTemplate")
    .Render()

<script type="text/x-template" id="itemTemplate">
    <div class="e-rule-container">
        <!-- Field Selection -->
        <div class="e-rule-field">
            <select class="e-field-select" onchange="updateField(this)">
                <option value="">Select Field</option>
                ${columns.map(col => `<option value="${col.Field}">${col.Label}</option>`).join('')}
            </select>
        </div>
        
        <!-- Operator Selection -->
        <div class="e-rule-operator">
            <select class="e-operator-select" onchange="updateOperator(this)">
                <option value="">Select Operator</option>
                <option value="equal">Equal</option>
                <option value="contains">Contains</option>
                <option value="startswith">Starts With</option>
                <option value="between">Between</option>
            </select>
        </div>
        
        <!-- Value Input -->
        <div class="e-rule-value">
            <input type="text" class="e-value-input" placeholder="Enter value" />
        </div>
        
        <!-- Actions -->
        <div class="e-rule-actions">
            <button class="e-btn-delete" onclick="deleteRule(this)" title="Delete">
                <i class="bi bi-trash"></i>
            </button>
        </div>
    </div>
</script>

<style>
.e-rule-container {
    display: flex;
    gap: 10px;
    padding: 10px;
    background: #fff;
    border: 1px solid #ddd;
    border-radius: 4px;
    align-items: center;
}

.e-rule-field,
.e-rule-operator,
.e-rule-value {
    flex: 1;
}

.e-field-select,
.e-operator-select,
.e-value-input {
    width: 100%;
    padding: 8px;
    border: 1px solid #ddd;
    border-radius: 4px;
}

.e-rule-actions {
    flex: 0.2;
}

.e-btn-delete {
    padding: 8px 12px;
    background: #dc3545;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.e-btn-delete:hover {
    background: #c82333;
}
</style>
```

## Value Template

Customize the input field where users enter filter values.

### Example: Custom Value Input with Dropdown

```html
<script type="text/x-template" id="valueTemplate">
    <div class="e-value-wrapper">
        <!-- Conditional Input Based on Operator -->
        ${operator === 'between' ? `
            <div class="e-range-container">
                <input type="text" class="e-value-from" placeholder="From" />
                <span class="e-range-separator">to</span>
                <input type="text" class="e-value-to" placeholder="To" />
            </div>
        ` : `
            <input type="text" class="e-value-single" placeholder="Value" />
        `}
        
        <!-- Optional: Dropdown for Predefined Values -->
        ${field === 'Department' ? `
            <select class="e-value-select">
                <option value="">Select Department</option>
                <option value="Sales">Sales</option>
                <option value="Engineering">Engineering</option>
                <option value="Marketing">Marketing</option>
            </select>
        ` : ''}
    </div>
</script>

<style>
.e-value-wrapper {
    width: 100%;
}

.e-range-container {
    display: flex;
    gap: 8px;
    align-items: center;
}

.e-value-from,
.e-value-to,
.e-value-single,
.e-value-select {
    flex: 1;
    padding: 8px;
    border: 1px solid #ddd;
    border-radius: 4px;
}

.e-range-separator {
    padding: 0 5px;
    color: #666;
}
</style>
```

## Group Template

Customize the appearance of filter groups.

### Example: Custom Group Header

```html
<script type="text/x-template" id="groupTemplate">
    <div class="e-group-container">
        <!-- Group Condition (AND/OR) -->
        <div class="e-group-header">
            <select class="e-group-condition" onchange="updateGroupCondition(this)">
                <option value="and">AND</option>
                <option value="or">OR</option>
            </select>
            
            <span class="e-group-label">Filter Group</span>
            
            <!-- NOT Checkbox -->
            <label class="e-not-container">
                <input type="checkbox" class="e-not-checkbox" onchange="toggleNot(this)" />
                <span>NOT</span>
            </label>
        </div>
        
        <!-- Nested Rules -->
        <div class="e-group-rules">
            <!-- Rules will be inserted here -->
        </div>
        
        <!-- Add Rule Button -->
        <button class="e-btn-add-rule" onclick="addRule(this)">
            + Add Rule
        </button>
    </div>
</script>

<style>
.e-group-container {
    padding: 10px;
    margin: 10px 0;
    background: #f9f9f9;
    border-left: 4px solid #007bff;
    border-radius: 4px;
}

.e-group-header {
    display: flex;
    gap: 10px;
    align-items: center;
    margin-bottom: 10px;
    padding-bottom: 10px;
    border-bottom: 1px solid #ddd;
}

.e-group-condition {
    padding: 5px 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
}

.e-group-label {
    font-weight: bold;
    flex: 1;
}

.e-not-container {
    display: flex;
    align-items: center;
    gap: 5px;
}

.e-not-checkbox {
    cursor: pointer;
}

.e-group-rules {
    margin: 10px 0;
}

.e-btn-add-rule {
    width: 100%;
    padding: 10px;
    background: #28a745;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-weight: bold;
}

.e-btn-add-rule:hover {
    background: #218838;
}
</style>
```

## Custom Operator UI

Customize the operator dropdown with icons and descriptions.

### Example: Enhanced Operator Dropdown

```html
<script type="text/x-template" id="operatorTemplate">
    <select class="e-operator-custom">
        <option value="equal">
            <i class="bi bi-equal"></i> Equal (=)
        </option>
        <option value="notequal">
            <i class="bi bi-exclamation"></i> Not Equal (≠)
        </option>
        <option value="startswith">
            <i class="bi bi-arrow-right"></i> Starts With
        </option>
        <option value="endswith">
            <i class="bi bi-arrow-left"></i> Ends With
        </option>
        <option value="contains">
            <i class="bi bi-search"></i> Contains
        </option>
        <option value="between">
            <i class="bi bi-arrow-left-right"></i> Between
        </option>
        <option value="greaterthan">
            <i class="bi bi-chevron-right"></i> Greater Than (>)
        </option>
        <option value="lessthan">
            <i class="bi bi-chevron-left"></i> Less Than (<)
        </option>
    </select>
</script>

<style>
.e-operator-custom {
    width: 100%;
    padding: 8px;
    border: 1px solid #ddd;
    border-radius: 4px;
    font-size: 14px;
}

.e-operator-custom option {
    padding: 5px;
}

.e-operator-custom i {
    margin-right: 5px;
}
</style>
```

## Error Handling Templates

Customize messages when validation or data errors occur.

### Example: Error and No-Data Messages

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(data)
    .AllowValidation(true)
    .Render()

<script>
var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];

// Handle validation errors
queryBuilder.addEventListener('actionFailure', function(args) {
    if (args.error && args.error.includes('validation')) {
        showErrorMessage("Invalid filter configuration. Please check your rules.");
    }
});

function showErrorMessage(message) {
    var errorDiv = document.createElement('div');
    errorDiv.className = 'e-error-message';
    errorDiv.textContent = message;
    errorDiv.style.cssText = `
        padding: 12px;
        margin: 10px 0;
        background: #f8d7da;
        color: #721c24;
        border: 1px solid #f5c6cb;
        border-radius: 4px;
    `;
    document.body.insertBefore(errorDiv, queryBuilder.element);
}
</script>
```

## Common Patterns

### Pattern 1: Material Design Template

```html
<script type="text/x-template" id="materialTemplate">
    <div class="material-rule">
        <div class="mdc-text-field mdc-text-field--filled">
            <select class="mdc-text-field__input e-field-select"></select>
            <span class="mdc-floating-label">Field</span>
        </div>
        
        <div class="mdc-text-field mdc-text-field--filled">
            <select class="mdc-text-field__input e-operator-select"></select>
            <span class="mdc-floating-label">Operator</span>
        </div>
        
        <div class="mdc-text-field mdc-text-field--filled">
            <input type="text" class="mdc-text-field__input e-value-input" />
            <span class="mdc-floating-label">Value</span>
        </div>
    </div>
</script>
```

### Pattern 2: Inline Editing Template

```html
<script type="text/x-template" id="inlineTemplate">
    <div class="e-inline-rule" contenteditable="true">
        <span class="e-field-name" data-field="${field}">${fieldLabel}</span>
        <span class="e-operator-name" data-operator="${operator}">${operatorLabel}</span>
        <span class="e-value-name" data-value="${value}">${value}</span>
    </div>
</script>

<style>
.e-inline-rule {
    padding: 8px;
    background: #f5f5f5;
    border-radius: 4px;
    outline: none;
}

.e-inline-rule:focus {
    background: #fff;
    border: 2px solid #007bff;
}

.e-field-name,
.e-operator-name,
.e-value-name {
    margin: 0 5px;
    padding: 2px 5px;
    border-radius: 3px;
    cursor: pointer;
}

.e-field-name:hover {
    background: #e7f3ff;
}

.e-operator-name:hover {
    background: #fff3cd;
}

.e-value-name:hover {
    background: #d4edda;
}
</style>
```

## Troubleshooting

### Issue: "Template not rendering"
**Cause:** Template ID doesn't match template reference or script tag syntax error  
**Solution:**
1. Verify template ID matches property name
2. Check script type is `text/x-template`
3. Ensure template HTML is valid

### Issue: "Custom buttons not working"
**Cause:** Event handlers not bound or wrong selector  
**Solution:**
1. Verify onclick handlers match function names
2. Use event delegation for dynamic elements
3. Check console for JavaScript errors

### Issue: "Styling not applied to template"
**Cause:** CSS not scoped correctly or styles loaded after template  
**Solution:**
1. Move `<style>` block to end of view after template
2. Use specific class selectors
3. Check for conflicting global styles

---

## Next Steps

- Review [Advanced Features](advanced-features.md) for import/export
- Explore [Styling and Appearance](styling-and-appearance.md) for theme customization
- Check [Accessibility and Localization](accessibility-and-localization.md) for WCAG compliance
