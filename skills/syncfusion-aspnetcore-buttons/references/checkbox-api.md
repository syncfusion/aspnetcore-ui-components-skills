# API Reference – ASP.NET Core Checkbox

Complete API reference for the Syncfusion ASP.NET Core Checkbox component.

---

## Table of Contents
- [Properties](#properties)
- [Methods](#methods)
- [Events](#events)
- [Tag Helper Attributes](#tag-helper-attributes)
- [Complete Example](#complete-example)

---

## Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `id` | `string` | - | Unique identifier for the checkbox |
| `label` | `string` | `''` | Checkbox label text |
| `checked` | `bool` | `false` | Initial checked state |
| `disabled` | `bool` | `false` | Disable checkbox (prevent interaction) |
| `indeterminate` | `bool` | `false` | Indeterminate state (partial selection) |
| `name` | `string` | `''` | Form field name for submission |
| `value` | `string` | `''` | Form field value when checked |
| `cssClass` | `string` | `''` | Additional CSS classes (e.g., `e-primary`, `e-small`) |
| `labelPosition` | `string` | `'After'` | Label placement (`'Before'` or `'After'`) |
| `enableRtl` | `bool` | `false` | Enable right-to-left layout |
| `enableHtmlSanitizer` | `bool` | `true` | Sanitize HTML content in label for security |
| `enablePersistence` | `bool` | `false` | Persist checkbox state across page reloads |
| `htmlAttributes` | `Dictionary` | - | Additional HTML attributes |

---

## Property Usage Examples

### enableHtmlSanitizer

Control HTML content sanitization in labels for security:

```cshtml
<!-- HTML sanitized (safe, default) -->
<ejs-checkbox 
    id="sanitized" 
    label="<b>Bold Label</b>"
    enableHtmlSanitizer="true">
</ejs-checkbox>

<!-- HTML NOT sanitized (use with caution) -->
<ejs-checkbox 
    id="unsanitized" 
    label="<b>Bold Label</b>"
    enableHtmlSanitizer="false">
</ejs-checkbox>
```

**Property:** `enableHtmlSanitizer` — `bool`, defaults to `true`

### enablePersistence

Persist checkbox state across page reloads:

```cshtml
<!-- State persists using localStorage -->
<ejs-checkbox 
    id="persistent" 
    label="Remember my preference"
    enablePersistence="true"
    name="userPreference"
    value="remembered">
</ejs-checkbox>
```

**Property:** `enablePersistence` — `bool`, defaults to `false`

> When enabled, the checkbox state is saved to browser localStorage and restored on page reload.

---

## Methods

| Method | Description |
|--------|-------------|
| `click()` | Programmatically click the checkbox |
| `focusIn()` | Set focus to the checkbox |
| `destroy()` | Destroy the component |

**Usage Example:**
```cshtml
<ejs-checkbox id="myCheckbox" label="Toggle Me"></ejs-checkbox>

<ejs-button id="toggleBtn" onclick="toggleCheckbox()">Toggle Checkbox</ejs-button>

<script>
    function toggleCheckbox() {
        var checkbox = document.getElementById('myCheckbox').ej2_instances[0];
        checkbox.click(); // Programmatically click
    }
</script>
```

---

## Events

| Event | Description |
|-------|-------------|
| `created` | Fired when component is created |
| `beforeChange` | Fired before state changes (can be cancelled) |
| `change` | Fired after state change |

**Event Usage Example:**
```cshtml
<ejs-checkbox id="eventCheckbox" label="Change Event Checkbox"></ejs-checkbox>

<div id="eventLog"></div>

<script>
    var checkbox = document.getElementById('eventCheckbox');
    
    // Handle change event
    checkbox.addEventListener('change', function(e) {
        var log = document.getElementById('eventLog');
        log.textContent = 'Checkbox changed to: ' + e.target.checked;
    });
    
    // Handle before change (can prevent change)
    checkbox.addEventListener('beforeChange', function(e) {
        console.log('About to change to:', e.checked);
        // e.cancel = true; // Cancel the change if needed
    });
</script>
```

---

## Tag Helper Attributes

All properties are available as tag helper attributes:

```cshtml
<ejs-checkbox 
    id="complete-checkbox"
    label="I accept all terms"
    checked="true"
    disabled="false"
    indeterminate="false"
    name="acceptance"
    value="yes"
    cssClass="e-primary"
    labelPosition="After"
    enableRtl="false"
    enableHtmlSanitizer="true"
    enablePersistence="false"
    htmlAttributes="@new { data_custom = 'value' }">
</ejs-checkbox>
```

---

## Complete Example

```cshtml
@{
    // Controller or Page Handler can set initial values
    ViewData["TermsAccepted"] = false;
}

<form method="post">
    <fieldset>
        <legend>Account Settings</legend>
        
        <!-- Basic checkbox -->
        <div>
            <ejs-checkbox 
                id="emailUpdates" 
                name="emailUpdates"
                value="yes"
                label="Receive email updates"
                checked="false">
            </ejs-checkbox>
        </div>
        
        <!-- Disabled checkbox -->
        <div>
            <ejs-checkbox 
                id="twoFactor" 
                name="twoFactor"
                value="enabled"
                label="Two-factor authentication (must be enabled)"
                checked="true"
                disabled="true">
            </ejs-checkbox>
        </div>
        
        <!-- Required checkbox with small size -->
        <div>
            <ejs-checkbox 
                id="terms" 
                name="agreeTerms"
                value="agreed"
                label="I agree to the terms of service"
                cssClass="e-small"
                labelPosition="After">
            </ejs-checkbox>
        </div>
        
        <!-- Indeterminate (parent) checkbox -->
        <div>
            <ejs-checkbox 
                id="selectAll" 
                label="Select All Notifications"
                indeterminate="true">
            </ejs-checkbox>
            
            <ul style="margin-left: 30px;">
                <li><ejs-checkbox name="notifications" value="email" label="Email Notifications"></ejs-checkbox></li>
                <li><ejs-checkbox name="notifications" value="sms" label="SMS Notifications"></ejs-checkbox></li>
                <li><ejs-checkbox name="notifications" value="push" label="Push Notifications" checked="true"></ejs-checkbox></li>
            </ul>
        </div>
        
        <ejs-button id="submit" type="submit" cssClass="e-primary">Save Settings</ejs-button>
    </fieldset>
</form>

<script>
    // Enable ripple effect on all checkboxes
    Syncfusion.enableRipple(true);
</script>
```

**C# Page Handler:**
```csharp
public async Task<IActionResult> OnPostAsync()
{
    // Retrieve checkbox values from form submission
    bool emailUpdates = Request.Form["emailUpdates"] == "yes";
    bool agreeTerms = Request.Form["agreeTerms"] == "agreed";
    
    var notifications = Request.Form["notifications"].ToList();
    // notifications contains: ["email", "sms", "push"] etc.
    
    // Process and save settings
    // ...
    
    return RedirectToPage("./Success");
}
```

---

## See Also

- [Checkbox Getting Started](checkbox-getting-started.md)
- [Checkbox States and Features](checkbox-features-and-state.md)
- [Checkbox Label and Size](checkbox-label-and-size.md)
- [Checkbox Customization](checkbox-customization.md)
- [Checkbox Accessibility](checkbox-accessibility.md)
