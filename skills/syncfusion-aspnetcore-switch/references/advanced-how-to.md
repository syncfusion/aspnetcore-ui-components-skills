# Advanced How-To: Switch Features and Patterns

## Table of Contents
- [Change Switch Size](#change-switch-size)
- [Enable Right-to-Left (RTL) Support](#enable-right-to-left-rtl-support)
- [Prevent Change Event](#prevent-change-event)
- [Set Disabled State](#set-disabled-state)
- [Submit Form with Switch Values](#submit-form-with-switch-values)

## Change Switch Size

### Overview

The Switch component provides two size options:
- **Default** - Standard size (approximately 60px wide, 34px tall)
- **Small** - Compact size (approximately 50px wide, 30px tall)

Reduce the switch to small size using the `cssClass` property set to `e-small`.

### Implementation

**Basic Small Switch:**
```cshtml
<!-- Add e-small class to switch -->
<ejs-switch id="smallSwitch" cssClass="e-small"></ejs-switch>
```

**Multiple Sizes on Same Page:**
```cshtml
<div class="size-comparison">
    <div class="size-item">
        <label>Default Size</label>
        <ejs-switch id="defaultSwitch"></ejs-switch>
    </div>
    
    <div class="size-item">
        <label>Small Size</label>
        <ejs-switch id="smallSwitch" cssClass="e-small"></ejs-switch>
    </div>
</div>

<style>
    .size-comparison {
        display: flex;
        gap: 30px;
    }
    
    .size-item {
        display: flex;
        align-items: center;
        gap: 10px;
    }
</style>
```

### Use Cases

**Compact UI:**
- Dense form layouts with many switches
- Mobile applications with limited space
- Sidebar or compact panels

**Mixed Sizes:**
- Prominent switches (default) for primary options
- Secondary switches (small) for related features

### Properties for Size Control

| Property | Value | Effect |
|----------|-------|--------|
| `cssClass` | `"e-small"` | Reduces switch dimensions |
| `cssClass` | `""` (empty) | Default size |

### Custom Size Variations

For sizes other than default and small, create custom CSS:

```css
/* Extra small switch */
.extra-small.e-switch-wrapper {
    width: 40px;
    height: 24px;
}

.extra-small .e-switch-handle {
    width: 20px;
    height: 20px;
    top: 2px;
}

.extra-small .e-switch-handle.e-switch-active {
    right: 2px;
}

/* Extra large switch */
.extra-large.e-switch-wrapper {
    width: 80px;
    height: 50px;
}

.extra-large .e-switch-handle {
    width: 44px;
    height: 44px;
    top: 3px;
}

.extra-large .e-switch-handle.e-switch-active {
    right: 3px;
}
```

**Usage:**
```cshtml
<ejs-switch id="extraSmall" cssClass="extra-small"></ejs-switch>
<ejs-switch id="extraLarge" cssClass="extra-large"></ejs-switch>
```

---

## Enable Right-to-Left (RTL) Support

### Overview

Switch component supports right-to-left (RTL) layout for languages like Arabic, Hebrew, and Persian. Enable RTL by setting the `enableRtl` property to `true`.

### Implementation

**Basic RTL Switch:**
```cshtml
<!-- Enable RTL -->
<ejs-switch id="rtlSwitch" enableRtl="true"></ejs-switch>
```

**RTL Container:**
```cshtml
<!-- Apply to container with dir attribute -->
<div dir="rtl" class="rtl-container">
    <label>تفعيل الإشعارات</label>  <!-- Arabic for "Enable Notifications" -->
    <ejs-switch id="notificationsRtl" 
                enableRtl="true"
                onLabel="تشغيل"
                offLabel="إيقاف">
    </ejs-switch>
</div>
```

**CSS for RTL:**
```css
.rtl-container {
    direction: rtl;
    text-align: right;
}

.rtl-container label {
    margin-left: 10px;
    margin-right: 0;
}
```

### Use Cases

**Multi-Language Applications:**
- Automatically detect language and apply RTL
- Store user language preference
- Switch layout based on culture

**Bilingual Support:**
```csharp
// Page handler
public class PreferencesModel : PageModel
{
    public string SelectedLanguage { get; set; }
    
    public void OnGet()
    {
        SelectedLanguage = HttpContext.GetUserLanguage() ?? "en";
    }
}
```

```cshtml
@{
    var isRtl = Model.SelectedLanguage == "ar" || Model.SelectedLanguage == "he";
}

<div dir="@(isRtl ? "rtl" : "ltr")">
    <ejs-switch id="langSwitch" enableRtl="@isRtl"></ejs-switch>
</div>
```

### Properties for RTL

| Property | Type | Values | Default |
|----------|------|--------|---------|
| `enableRtl` | boolean | `true` / `false` | `false` |

### RTL with Other Features

**RTL + Custom Labels:**
```cshtml
<ejs-switch id="rtlLabeled" 
            enableRtl="true"
            onLabel="✓ نعم"
            offLabel="✗ لا">
</ejs-switch>
```

**RTL + Small Size:**
```cshtml
<ejs-switch id="rtlSmall" 
            enableRtl="true"
            cssClass="e-small">
</ejs-switch>
```

---

## Prevent Change Event

### Overview

The `beforeChange` event fires **before** the switch state changes, allowing you to:
- Validate the state change
- Cancel the change based on conditions
- Show confirmation dialogs
- Prevent state change without user confirmation

This differs from the `change` event, which fires **after** the state has changed.

### Implementation

**Prevent State Change:**
```cshtml
<ejs-switch id="preventSwitch" beforeChange="onBeforeChange"></ejs-switch>

<script>
    function onBeforeChange(args) {
        // Return false to prevent the change
        if (!confirm('Are you sure you want to change this setting?')) {
            args.cancel = true;  // Prevent state change
        }
    }
</script>
```

**Conditional Prevention:**
```cshtml
<ejs-switch id="criticalSwitch" beforeChange="validateCriticalChange"></ejs-switch>

<script>
    function validateCriticalChange(args) {
        // Only allow changes if system is not busy
        if (isSystemBusy) {
            args.cancel = true;
            alert('System is busy. Please wait before making changes.');
        }
        
        // Only allow if user has permission
        if (!userHasPermission()) {
            args.cancel = true;
            alert('You do not have permission to change this setting.');
        }
    }
</script>
```

**Confirmation with Reason:**
```cshtml
<ejs-switch id="confirmSwitch" beforeChange="confirmWithReason"></ejs-switch>

<div id="confirmDialog">
    <p>Why do you want to disable this feature?</p>
    <select id="reasonSelect">
        <option value="">Select reason...</option>
        <option value="not-needed">Not needed</option>
        <option value="performance">Performance issues</option>
        <option value="other">Other</option>
    </select>
</div>

<script>
    function confirmWithReason(args) {
        var reason = document.getElementById('reasonSelect').value;
        
        if (!reason) {
            alert('Please select a reason');
            args.cancel = true;
        } else {
            // Log the reason
            console.log('Feature disabled because:', reason);
        }
    }
</script>
```

### Properties for Control

| Property | Type | Description |
|----------|------|-------------|
| `beforeChange` | function | Event handler called before state changes |
| `args.cancel` | boolean | Set to `true` to prevent the change |
| `args.checked` | boolean | New state being set |
| `args.previousValue` | boolean | Current state |

### Use Cases

**Destructive Operations:**
- Prevent accidental deletion of data
- Confirm system shutdown or restart
- Confirm feature removal

**Validation Rules:**
```cshtml
<ejs-switch id="adultContent" beforeChange="validateAdultAccess"></ejs-switch>

<script>
    function validateAdultAccess(args) {
        if (args.checked && !isUserAdult()) {
            args.cancel = true;
            alert('You must be 18+ to enable this feature.');
        }
    }
</script>
```

**Permission Checks:**
```csharp
public class AdminPanel : PageModel
{
    public bool CanToggleFeature { get; set; }
}
```

```cshtml
@model AdminPanel

<ejs-switch id="featureSwitch" 
            beforeChange="@(Model.CanToggleFeature ? "null" : "preventUnauthorized")">
</ejs-switch>

<script>
    function preventUnauthorized(args) {
        args.cancel = true;
        alert('You do not have permission to change this setting.');
    }
</script>
```

---

## Set Disabled State

### Overview

Disable the switch to prevent user interaction while maintaining visibility. Use the `disabled` property or set programmatically.

### Implementation

**Markup-based Disable:**
```cshtml
<!-- Disabled in markup -->
<ejs-switch id="disabledSwitch" disabled="true"></ejs-switch>

<!-- Conditional disable based on model -->
<ejs-switch id="conditionalSwitch" 
            disabled="@(!Model.IsAdmin)">
</ejs-switch>
```

**Programmatic Disable:**
```javascript
document.addEventListener('DOMContentLoaded', function() {
    var switchInstance = document.getElementById('mySwitch').ej2_instances[0];
    
    // Disable the switch
    switchInstance.disabled = true;
    
    // Enable the switch
    switchInstance.disabled = false;
});
```

**Toggle Disable on Condition:**
```cshtml
<ejs-switch id="parentSwitch" change="onParentChange"></ejs-switch>
<ejs-switch id="childSwitch" disabled="true"></ejs-switch>

<script>
    function onParentChange(args) {
        var childSwitch = document.getElementById('childSwitch').ej2_instances[0];
        
        // Enable child switch only if parent is checked
        childSwitch.disabled = !args.checked;
    }
</script>
```

### Use Cases

**Feature Dependencies:**
- Child features disabled until parent is enabled
- Premium features disabled for free users
- Experimental features disabled in production

**Loading States:**
```javascript
function disableDuringLoad(switchId) {
    var switchInstance = document.getElementById(switchId).ej2_instances[0];
    switchInstance.disabled = true;
    
    // Simulate loading
    setTimeout(function() {
        switchInstance.disabled = false;
    }, 2000);
}
```

**Permission-based Disable:**
```csharp
public class SettingsModel : PageModel
{
    public bool IsPremiumUser { get; set; }
    public bool IsAdmin { get; set; }
}
```

```cshtml
@model SettingsModel

<!-- Premium feature (only enabled for premium users) -->
<ejs-switch id="advancedAnalytics" 
            disabled="@(!Model.IsPremiumUser)">
</ejs-switch>

<!-- Admin feature (only enabled for admins) -->
<ejs-switch id="systemMaintenance" 
            disabled="@(!Model.IsAdmin)">
</ejs-switch>
```

---

## Submit Form with Switch Values

### Overview

Submit switch values through HTML forms using the `name` attribute or JavaScript form data collection.

### Implementation

**HTML Form Submission:**
```cshtml
<form method="post" action="/settings/update">
    <div class="form-group">
        <label>Enable Notifications</label>
        <ejs-switch id="notifications" name="enableNotifications"></ejs-switch>
    </div>
    
    <div class="form-group">
        <label>Enable Analytics</label>
        <ejs-switch id="analytics" name="enableAnalytics"></ejs-switch>
    </div>
    
    <button type="submit" class="btn btn-primary">Save</button>
</form>
```

**Server-side Handler:**
```csharp
public async Task<IActionResult> OnPostAsync(
    bool enableNotifications, 
    bool enableAnalytics)
{
    // enableNotifications and enableAnalytics are automatically bound
    var settings = new UserSettings
    {
        Notifications = enableNotifications,
        Analytics = enableAnalytics
    };
    
    await _context.UpdateSettingsAsync(User.Id, settings);
    return RedirectToPage();
}
```

**Model Binding:**
```cshtml
@model SettingsFormModel

<form method="post">
    <ejs-switch id="notif" asp-for="EnableNotifications"></ejs-switch>
    <ejs-switch id="email" asp-for="EnableEmails"></ejs-switch>
    
    <button type="submit">Save</button>
</form>
```

```csharp
public class SettingsFormModel
{
    public bool EnableNotifications { get; set; }
    public bool EnableEmails { get; set; }
}
```

### Use Cases

**Batch Updates:**
```cshtml
<form id="preferencesForm" method="post">
    <fieldset>
        <legend>Notification Preferences</legend>
        <ejs-switch name="pushNotifications"></ejs-switch>
        <ejs-switch name="emailNotifications"></ejs-switch>
        <ejs-switch name="smsNotifications"></ejs-switch>
    </fieldset>
    
    <fieldset>
        <legend>Privacy Settings</legend>
        <ejs-switch name="shareProfile"></ejs-switch>
        <ejs-switch name="allowTracking"></ejs-switch>
    </fieldset>
    
    <button type="submit">Save All Preferences</button>
</form>
```

**Dynamic Form Submission:**
```javascript
document.getElementById('preferencesForm').addEventListener('submit', function(e) {
    e.preventDefault();
    
    // Collect all switch values
    var formData = new FormData(this);
    
    // Get all switches and their states
    var switches = document.querySelectorAll('.preferences-switch');
    switches.forEach(function(switchElement) {
        var instance = switchElement.ej2_instances[0];
        formData.set(switchElement.name, instance.checked);
    });
    
    // Submit via API
    fetch('/api/preferences', {
        method: 'POST',
        body: formData
    })
    .then(response => response.json())
    .then(data => {
        alert('Preferences saved successfully!');
    });
});
```

---

## Troubleshooting

### Issue: beforeChange event doesn't fire
**Cause:** Event handler not properly registered
**Solution:**
```cshtml
<!-- Ensure handler is assigned -->
<ejs-switch id="mySwitch" beforeChange="handleBeforeChange"></ejs-switch>

<script>
    // Define handler at global scope
    function handleBeforeChange(args) {
        console.log('Before change triggered');
    }
</script>
```

### Issue: Disabled switches still submit values
**Cause:** Disabled inputs may still include their value
**Solution:**
```cshtml
<!-- Prevent submission of disabled values -->
<ejs-switch id="disabledSwitch" disabled="true" name="disabledSetting"></ejs-switch>

<!-- Use hidden input for explicit control -->
<input type="hidden" name="disabledSetting" value="false" />
```

### Issue: RTL layout not working
**Cause:** Missing `dir` attribute or RTL not enabled on component
**Solution:**
```cshtml
<!-- Add to html tag or container -->
<div dir="rtl">
    <ejs-switch id="rtlSwitch" enableRtl="true"></ejs-switch>
</div>
```

### Issue: Size class not applying
**Cause:** CSS class misspelled or conflicting styles
**Solution:**
```cshtml
<!-- Use exact Syncfusion class name -->
<ejs-switch id="switch" cssClass="e-small"></ejs-switch>

<!-- Verify in browser DevTools that class is applied -->
```

## Next Steps

- Basic setup: [Getting Started](../getting-started.md)
- Configure properties: [Switch Properties](../switch-properties.md)
- Handle events: [Switch Events](../switch-events.md)
- Customize appearance: [Styling](../styling-customization.md)
