# Switch Properties and Configuration

## Table of Contents
- [Overview](#overview)
- [Core Properties](#core-properties)
- [Property Binding](#property-binding)
- [Model Integration](#model-integration)
- [Form Submission](#form-submission)
- [Default Values](#default-values)
- [Advanced Configuration](#advanced-configuration)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

Switch properties control the component's behavior, appearance, and state. Properties can be set declaratively in markup or programmatically through JavaScript.

**Two approaches:**
1. **Markup** - Set properties directly in TagHelper (simplest)
2. **JavaScript** - Access and modify properties at runtime (more control)

## Core Properties

### checked
**Type:** `boolean`  
**Default:** `false`  
**Description:** Sets the initial state of the switch (true = on, false = off)

**Example:**
```cshtml
<!-- Switch starts in "on" position -->
<ejs-switch id="enableFeature" checked="true"></ejs-switch>

<!-- Switch starts in "off" position (default) -->
<ejs-switch id="disableFeature"></ejs-switch>
```

**Use When:**
- Loading existing settings from database
- Showing current user preferences
- Pre-checking confirmation options

### disabled
**Type:** `boolean`  
**Default:** `false`  
**Description:** Disables user interaction with the switch (appears grayed out)

**Example:**
```cshtml
<!-- Disabled based on condition -->
<ejs-switch id="premiumFeature" 
            disabled="@(!Model.IsPremiumUser)"
            checked="@Model.IsPremiumUser">
</ejs-switch>
```

**Use When:**
- Feature requires permission (premium user, admin only)
- Loading or processing data
- Showing disabled options temporarily
- Preventing changes during validation

### name
**Type:** `string`  
**Default:** Empty string  
**Description:** HTML name attribute for form submission and data binding

**Example:**
```cshtml
<!-- For form submission -->
<ejs-switch id="newsletter" name="subscribeNewsletter"></ejs-switch>

<!-- Sends "subscribeNewsletter=true" or "subscribeNewsletter=false" to server -->
```

**Use When:**
- Switch is inside a form
- Submitting data to ASP.NET Core action method
- Binding to model properties

### value
**Type:** `string`  
**Default:** `"on"`  
**Description:** Custom value sent to server when checked (default is "on")

**Example:**
```cshtml
<!-- Default behavior: sends "on" when checked -->
<ejs-switch id="agree" name="userAgreement"></ejs-switch>

<!-- Custom value: sends "yes" when checked -->
<ejs-switch id="agreeCustom" 
            name="userAgreement" 
            value="yes">
</ejs-switch>
```

**Server Receives:**
- When checked: "yes"
- When unchecked: Not sent (or "off" depending on implementation)

**Use When:**
- Sending specific values to server
- Multiple checkboxes representing different options
- Database or enum mappings

### onLabel
**Type:** `string`  
**Default:** Empty string  
**Description:** Text displayed when switch is in "on" state

**Example:**
```cshtml
<!-- Shows "ON" when enabled -->
<ejs-switch id="notifications" onLabel="ON"></ejs-switch>

<!-- Context-specific label -->
<ejs-switch id="darkMode" onLabel="Dark"></ejs-switch>

<!-- Shows "Enabled" -->
<ejs-switch id="feature" onLabel="Enabled" offLabel="Disabled"></ejs-switch>
```

**Use When:**
- Clarifying what the "on" state means
- Non-English labels needed
- Two-word descriptions (limit for space constraints)

### offLabel
**Type:** `string`  
**Default:** Empty string  
**Description:** Text displayed when switch is in "off" state

**Example:**
```cshtml
<!-- Shows "OFF" when disabled -->
<ejs-switch id="notifications" offLabel="OFF"></ejs-switch>

<!-- Shows "Disabled" when off -->
<ejs-switch id="feature" onLabel="Enabled" offLabel="Disabled"></ejs-switch>
```

**Best Practices:**
- Use 1-2 word labels (longer labels may not display properly)
- Keep on/off labels symmetrical in length
- Avoid long labels on Material theme

### cssClass
**Type:** `string`  
**Default:** Empty string  
**Description:** Custom CSS class for styling and theming

**Example:**
```cshtml
<!-- Apply custom styling -->
<ejs-switch id="custom" cssClass="custom-switch"></ejs-switch>

<!-- Apply multiple classes -->
<ejs-switch id="multi" cssClass="large-switch dark-mode"></ejs-switch>
```

**Custom CSS Example:**
```css
/* Increase switch size */
.custom-switch.e-switch-wrapper {
    width: 60px;
    height: 40px;
}

.custom-switch.e-switch-wrapper .e-switch-handle {
    width: 36px;
    height: 36px;
}

/* Custom colors -->
.custom-switch.e-switch-wrapper .e-switch-on {
    background-color: #4CAF50;
}
```

### title
**Type:** `string`  
**Default:** Empty string  
**Description:** HTML title attribute for tooltip on hover

**Example:**
```cshtml
<ejs-switch id="experimental" 
            title="Enable experimental features">
</ejs-switch>
```

## Property Binding

### Binding to Model Properties

Connect Switch to ASP.NET Core model properties for automatic data binding.

**Page Model:**
```csharp
public class UserPreferencesModel
{
    public bool NotificationsEnabled { get; set; }
    public bool DarkModeEnabled { get; set; }
    public bool EmailUpdates { get; set; }
}
```

**Razor Page:**
```cshtml
@model UserPreferencesModel

<form method="post">
    <div class="preferences">
        <div class="preference-item">
            <label>Notifications</label>
            <ejs-switch id="notifications" 
                        asp-for="NotificationsEnabled">
            </ejs-switch>
        </div>
        
        <div class="preference-item">
            <label>Dark Mode</label>
            <ejs-switch id="darkMode" 
                        asp-for="DarkModeEnabled">
            </ejs-switch>
        </div>
        
        <div class="preference-item">
            <label>Email Updates</label>
            <ejs-switch id="emailUpdates" 
                        asp-for="EmailUpdates">
            </ejs-switch>
        </div>
    </div>
    
    <button type="submit">Save Preferences</button>
</form>

@section Scripts {
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Switches are automatically bound to Model properties
            console.log('Notifications enabled:', @Json.Serialize(Model.NotificationsEnabled));
        });
    </script>
}
```

**Page Handler:**
```csharp
public void OnPost(UserPreferencesModel model)
{
    // model.NotificationsEnabled - automatically populated from switch state
    // model.DarkModeEnabled - automatically populated from switch state
    
    // Update database
    _context.UserPreferences.Update(model);
    _context.SaveChanges();
}
```

## Model Integration

### Display Data from Server

Load initial Switch state from database:

**Controller/Page Handler:**
```csharp
public async Task<IActionResult> OnGetAsync(int userId)
{
    var user = await _context.Users.FindAsync(userId);
    var preferences = new UserPreferencesModel
    {
        NotificationsEnabled = user.Preferences.Notifications,
        DarkModeEnabled = user.Preferences.DarkMode
    };
    
    return Page();
}
```

**View:**
```cshtml
@model UserPreferencesModel

<ejs-switch id="notifications" 
            checked="@Model.NotificationsEnabled"
            name="NotificationsEnabled">
</ejs-switch>
```

### Dynamic Property Assignment

Set properties programmatically at runtime:

```javascript
document.addEventListener('DOMContentLoaded', function() {
    // Get switch instance
    var switchElement = document.getElementById('mySwitch');
    var switchInstance = switchElement.ej2_instances[0];
    
    // Get current state
    var isChecked = switchInstance.checked;
    console.log('Switch is:', isChecked ? 'ON' : 'OFF');
    
    // Set checked state
    switchInstance.checked = true;
    
    // Get/Set disabled state
    switchInstance.disabled = false;
});
```

## Form Submission

### Basic Form with Switch

```cshtml
<form id="settingsForm" method="post">
    <fieldset>
        <legend>Account Settings</legend>
        
        <div class="form-group">
            <label for="twoFactorAuth">Enable Two-Factor Authentication</label>
            <ejs-switch id="twoFactorAuth" 
                        name="TwoFactorEnabled">
            </ejs-switch>
        </div>
        
        <div class="form-group">
            <label for="emailNotifications">Email Notifications</label>
            <ejs-switch id="emailNotifications" 
                        name="EmailNotificationsEnabled">
            </ejs-switch>
        </div>
        
        <button type="submit" class="btn btn-primary">Save Settings</button>
    </fieldset>
</form>
```

### Capture Form Data with JavaScript

```javascript
document.getElementById('settingsForm').addEventListener('submit', function(e) {
    e.preventDefault();
    
    var twoFactorSwitch = document.getElementById('twoFactorAuth').ej2_instances[0];
    var emailSwitch = document.getElementById('emailNotifications').ej2_instances[0];
    
    var formData = {
        TwoFactorEnabled: twoFactorSwitch.checked,
        EmailNotificationsEnabled: emailSwitch.checked
    };
    
    console.log('Submitting:', formData);
    
    // Send to server
    fetch('/settings/update', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify(formData)
    });
});
```

## Default Values

### Set Initial State on Page Load

**Option 1: In Markup**
```cshtml
<!-- Most common: set via model property -->
<ejs-switch id="feature" checked="@Model.IsFeatureEnabled"></ejs-switch>
```

**Option 2: In JavaScript**
```javascript
document.addEventListener('DOMContentLoaded', function() {
    var switchInstance = document.getElementById('feature').ej2_instances[0];
    
    // Load from localStorage
    var savedState = localStorage.getItem('featureState');
    if (savedState !== null) {
        switchInstance.checked = savedState === 'true';
    }
});
```

**Option 3: From Server API**
```javascript
fetch('/api/user/preferences')
    .then(response => response.json())
    .then(data => {
        var switchInstance = document.getElementById('feature').ej2_instances[0];
        switchInstance.checked = data.featureEnabled;
    });
```

## Advanced Configuration

### Accessibility Attributes

```cshtml
<ejs-switch id="accessible" 
            title="Enable accessibility features"
            aria-label="Accessibility Options"
            aria-describedby="accessibilityHelp">
</ejs-switch>

<p id="accessibilityHelp">Enable enhanced keyboard navigation and screen reader support</p>
```

### Combining Multiple Properties

```cshtml
<ejs-switch id="premium" 
            checked="@Model.IsPremiumUser"
            disabled="@(!Model.CanChangePlan)"
            onLabel="Active"
            offLabel="Inactive"
            name="PremiumStatus"
            cssClass="premium-switch"
            title="Premium subscription status">
</ejs-switch>
```

## Common Patterns

### Pattern 1: Conditional Disable Based on Parent Setting

```cshtml
<!-- Main feature toggle -->
<div>
    <label>Enable Analytics</label>
    <ejs-switch id="analyticsMain" change="onAnalyticsChange"></ejs-switch>
</div>

<!-- Only available when analytics is enabled -->
<div>
    <label>Share Data Anonymously</label>
    <ejs-switch id="anonymousShare" disabled="true"></ejs-switch>
</div>

<script>
    function onAnalyticsChange(args) {
        var shareSwitch = document.getElementById('anonymousShare').ej2_instances[0];
        shareSwitch.disabled = !args.checked;
    }
</script>
```

### Pattern 2: Save Switch State to Browser Storage

```javascript
document.addEventListener('DOMContentLoaded', function() {
    var switchInstance = document.getElementById('darkMode').ej2_instances[0];
    
    // Load saved preference
    var savedDarkMode = localStorage.getItem('darkMode');
    if (savedDarkMode !== null) {
        switchInstance.checked = savedDarkMode === 'true';
    }
    
    // Save on change
    switchInstance.addEventListener('change', function(args) {
        localStorage.setItem('darkMode', args.checked.toString());
    });
});
```

### Pattern 3: Multiple Switches with Shared Logic

```cshtml
@for (int i = 1; i <= 3; i++) {
    <div class="switch-item">
        <label>Feature @i</label>
        <ejs-switch id="feature@i" name="feature@i" class="feature-switch"></ejs-switch>
    </div>
}

<script>
    document.addEventListener('DOMContentLoaded', function() {
        // Get all feature switches
        var switches = document.querySelectorAll('.feature-switch');
        
        switches.forEach(function(switchElement) {
            var instance = switchElement.ej2_instances[0];
            
            instance.addEventListener('change', function(args) {
                console.log(switchElement.id + ' changed to:', args.checked);
                // Perform common action for all switches
            });
        });
    });
</script>
```

## Troubleshooting

### Issue: Switch checked state not binding to model
**Cause:** `asp-for` directive not used or model property not public
**Solution:**
```cshtml
<!-- ❌ Wrong -->
<ejs-switch id="feature" checked="@Model.FeatureEnabled"></ejs-switch>

<!-- ✅ Correct -->
<ejs-switch id="feature" asp-for="FeatureEnabled"></ejs-switch>
```

### Issue: Switch property changes don't trigger change event
**Cause:** Using property assignment instead of UI interaction
**Solution:**
```javascript
// ❌ Property change doesn't trigger event
switchInstance.checked = true;

// ✅ To trigger change event:
switchInstance.checked = true;
switchInstance.change({ checked: true, isInteracted: true });
```

### Issue: Switch state reverts after form submission
**Cause:** Page reloads with original model data from server
**Solution:**
```csharp
public async Task<IActionResult> OnPostAsync()
{
    // Process the new values
    await _userService.UpdatePreferences(User.Id, model);
    
    // Reload updated data to reflect changes
    return RedirectToPage();
}
```

### Issue: Disabled switches still send values in form submission
**Cause:** HTML form behavior - disabled inputs don't send values
**Solution:**
```cshtml
<!-- Disabled switches don't submit values -->
<ejs-switch id="disabled" disabled="true" name="setting"></ejs-switch>

<!-- Use hidden input if you need to send a value from disabled switch -->
<ejs-switch id="disabled" disabled="true" name="settingDisplay"></ejs-switch>
<input type="hidden" name="setting" value="false" />
```

## Next Steps

- Handle switch changes: [Switch Events Reference](../switch-events.md)
- Customize appearance: [Styling and Customization](../styling-customization.md)
- Setup guide: [Getting Started](../getting-started.md)
