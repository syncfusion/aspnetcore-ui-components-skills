# API Reference – ASP.NET Core Button

Complete reference for all Button properties, methods, and events.

---

## Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `id` | `string` | - | Unique identifier for the button |
| `content` | `string` | - | Text content of the button |
| `cssClass` | `string` | - | Custom CSS classes (e.g., `e-primary`, `e-small`, `e-flat`) |
| `disabled` | `bool` | `false` | Disables the button if set to `true` |
| `type` | `string` | `button` | HTML button type: `button`, `submit`, or `reset` |
| `iconCss` | `string` | - | CSS class for icon (e.g., `e-icons e-save-icon`) |
| `iconPosition` | `string` | `Left` | Icon position: `Left` or `Right` |
| `isPrimary` | `bool` | `false` | Applies primary styling |
| `isToggle` | `bool` | `false` | Enables toggle state behavior |
| `enableRtl` | `bool` | `false` | Enables right-to-left layout |
| `htmlAttributes` | `object` | - | Additional HTML attributes as key-value pairs |

---

## Tag Helper Syntax

### Basic Button

```cshtml
<ejs-button id="button1">Click Me</ejs-button>
```

### Button with All Properties

```cshtml
<ejs-button 
    id="button1" 
    content="Save" 
    cssClass="e-primary e-small"
    disabled="false"
    type="submit"
    iconCss="e-icons e-save-icon"
    iconPosition="Left"
    isPrimary="true"
    isToggle="false"
    enableRtl="false">
</ejs-button>
```

### Button with HTML Attributes

```cshtml
<ejs-button 
    id="button1" 
    cssClass="e-primary"
    htmlAttributes="@new { data_tooltip = 'Save your work', aria_label = 'Save button' }">
    Save
</ejs-button>
```

---

## Events

| Event | Trigger | Callback Signature |
|-------|---------|-------------------|
| `created` | After button is created | `(args: CreateEventArgs) => void` |
| `beforeClick` | Before click action | `(args: ClickEventArgs) => void` |

### Handling Events in JavaScript

**View:**
```cshtml
<ejs-button id="eventBtn">Click Me</ejs-button>

<script>
    const button = ej2_instances['eventBtn'][0];

    // Handle click event
    button.addEventListener('click', function(e) {
        console.log('Button was clicked!', e);
    });

    // Handle focus event
    button.addEventListener('focus', function() {
        console.log('Button received focus');
    });

    // Handle blur event
    button.addEventListener('blur', function() {
        console.log('Button lost focus');
    });
</script>
```

---

## Methods

| Method | Description | Syntax |
|--------|-------------|--------|
| `click()` | Programmatically trigger button click | `button.click()` |
| `focusIn()` | Set focus on button | `button.focusIn()` |
| `destroy()` | Destroy the button instance | `button.destroy()` |

### Using Methods

**View:**
```cshtml
<ejs-button id="targetBtn">Target</ejs-button>
<ejs-button id="controlBtn" onclick="controlTargetButton()">Control</ejs-button>

<script>
    function controlTargetButton() {
        const targetBtn = ej2_instances['targetBtn'][0];
        
        // Programmatically click the target button
        targetBtn.click();
        
        // Or focus it
        targetBtn.focusIn();
    }

    // Destroy button on demand
    function destroyButton() {
        const btn = ej2_instances['targetBtn'][0];
        btn.destroy();
    }
</script>
```

---

## Common Scenarios

### Primary Action Button

```cshtml
<ejs-button id="saveBtn" cssClass="e-primary" type="submit">Save Changes</ejs-button>
```

### Danger Action Button

```cshtml
<ejs-button id="deleteBtn" cssClass="e-danger" onclick="confirmDelete()">Delete</ejs-button>

<script>
    function confirmDelete() {
        if (confirm('Are you sure?')) {
            // Proceed with deletion
        }
    }
</script>
```

### Small Icon Button

```cshtml
<ejs-button id="compactBtn" cssClass="e-small" iconCss="e-icons e-plus-icon"></ejs-button>
```

### Disabled State

```cshtml
<ejs-button id="submitBtn" disabled="@(!Model.IsFormValid)">Submit</ejs-button>
```

### Round Button (Circular)

```cshtml
<ejs-button id="roundBtn" cssClass="e-round e-primary" iconCss="e-icons e-edit-icon"></ejs-button>
```

### Toggle Button

```cshtml
<ejs-button id="toggleBtn" isToggle="true" cssClass="e-flat">Toggle State</ejs-button>
```

---

## CSS Classes for Styling

| Class | Purpose |
|-------|---------|
| `e-primary` | Primary action styling |
| `e-success` | Success/positive action |
| `e-info` | Informational action |
| `e-warning` | Warning action |
| `e-danger` | Danger/destructive action |
| `e-link` | Link appearance |
| `e-flat` | Flat (no background) button |
| `e-outline` | Outline button with border |
| `e-small` | Small size button |
| `e-round` | Round/circular button |
| `e-active` | Active/toggled state (applied automatically) |
| `e-disabled` | Disabled state (applied automatically) |

---

## Complete Example

**View (`~/Pages/Index.cshtml`):**
```cshtml
@{
    ViewData["Title"] = "Button Examples";
}

<div class="button-examples">
    <!-- Primary Actions -->
    <section class="actions">
        <h3>Primary Actions</h3>
        <ejs-button id="saveBtn" cssClass="e-primary" type="submit">Save</ejs-button>
        <ejs-button id="cancelBtn" cssClass="e-outline">Cancel</ejs-button>
    </section>

    <!-- Danger Actions -->
    <section class="actions">
        <h3>Danger Actions</h3>
        <ejs-button id="deleteBtn" cssClass="e-danger">Delete</ejs-button>
        <ejs-button id="resetBtn" type="reset">Reset</ejs-button>
    </section>

    <!-- Icon Buttons -->
    <section class="actions">
        <h3>Icon Buttons</h3>
        <ejs-button id="addBtn" cssClass="e-small" iconCss="e-icons e-plus-icon"></ejs-button>
        <ejs-button id="editBtn" cssClass="e-small" iconCss="e-icons e-edit-icon">Edit</ejs-button>
        <ejs-button id="searchBtn" cssClass="e-small e-primary" iconCss="e-icons e-search-icon"></ejs-button>
    </section>

    <!-- Disabled -->
    <section class="actions">
        <h3>Disabled Button</h3>
        <ejs-button id="disabledBtn" disabled="true">Cannot Click</ejs-button>
    </section>

    <!-- Custom Styling -->
    <section class="actions">
        <h3>Custom Styled</h3>
        <ejs-button id="customBtn" cssClass="custom-style">Custom</ejs-button>
    </section>
</div>

<style>
    .button-examples section {
        margin: 20px 0;
        padding: 10px;
        border: 1px solid #ddd;
        border-radius: 4px;
    }

    .button-examples h3 {
        margin-top: 0;
    }

    .button-examples .e-btn {
        margin: 5px;
    }

    .custom-style {
        background-color: #ff6b6b !important;
        border-color: #ff6b6b !important;
    }

    .custom-style:hover {
        background-color: #ff5252 !important;
    }
</style>
```

---

## See Also

- [Button Getting Started](button-getting-started.md)
- [Button Types and Styles](button-types-and-styles.md)
- [Button How-To Patterns](button-how-to.md)
- [Button Accessibility](button-accessibility.md)
