# Switch Events and State Management

## Overview

The Switch component fires events when user interactions occur. Use events to:

- **Detect state changes** - Know when user toggles the switch
- **Trigger actions** - Perform operations when switch is toggled
- **Validate changes** - Confirm state change before committing
- **Update UI** - Refresh related components based on switch state
- **Send to server** - Persist state changes to database

**Primary Event:** `change` - Fires when user toggles the switch

## Change Event

The `change` event is the main interaction event for the Switch component.

### Basic Usage

```cshtml
<ejs-switch id="mySwitch" change="onSwitchChange"></ejs-switch>

<script>
    function onSwitchChange(args) {
        console.log('Switch toggled!');
        console.log('New state:', args.checked);
    }
</script>
```

### Event Handler Parameters

```javascript
function onSwitchChange(args) {
    // args.checked: boolean - New state (true = on, false = off)
    // args.event: Event object - Original DOM event
    // args.previousValue: boolean - Previous state
    // args.isInteracted: boolean - Was toggled by user (true) or programmatically (false)
}
```

### Complete Example

```cshtml
<div class="example">
    <label for="feature">Enable Feature:</label>
    <ejs-switch id="feature" change="handleFeatureToggle"></ejs-switch>
    
    <div id="featureStatus" class="status-message"></div>
</div>

<script>
    function handleFeatureToggle(args) {
        var statusDiv = document.getElementById('featureStatus');
        
        if (args.checked) {
            statusDiv.innerHTML = '✓ Feature is now ENABLED';
            statusDiv.style.color = 'green';
        } else {
            statusDiv.innerHTML = '✗ Feature is now DISABLED';
            statusDiv.style.color = 'red';
        }
    }
</script>

<style>
    .status-message {
        margin-top: 10px;
        font-weight: bold;
    }
</style>
```

## State Detection Patterns

### Pattern 1: Track State Changes

```javascript
document.addEventListener('DOMContentLoaded', function() {
    var switchInstance = document.getElementById('notifySwitch').ej2_instances[0];
    
    // Get current state
    console.log('Initial state:', switchInstance.checked);
    
    // Listen to changes
    switchInstance.addEventListener('change', function(args) {
        if (args.isInteracted) {
            console.log('User changed switch to:', args.checked);
        } else {
            console.log('Switch changed programmatically to:', args.checked);
        }
    });
});
```

### Pattern 2: Conditional Actions

```cshtml
<ejs-switch id="darkMode" change="toggleDarkMode"></ejs-switch>

<script>
    function toggleDarkMode(args) {
        if (args.checked) {
            // Enable dark mode
            document.body.classList.add('dark-theme');
            localStorage.setItem('theme', 'dark');
        } else {
            // Disable dark mode
            document.body.classList.remove('dark-theme');
            localStorage.setItem('theme', 'light');
        }
    }
</script>
```

### Pattern 3: Multiple Switches with Shared Handler

```cshtml
<!-- Multiple preferences -->
<div class="preferences">
    <div>
        <label>Notifications</label>
        <ejs-switch class="preference-switch" data-setting="notifications"></ejs-switch>
    </div>
    
    <div>
        <label>Marketing Emails</label>
        <ejs-switch class="preference-switch" data-setting="marketing"></ejs-switch>
    </div>
    
    <div>
        <label>Analytics</label>
        <ejs-switch class="preference-switch" data-setting="analytics"></ejs-switch>
    </div>
</div>

<script>
    document.addEventListener('DOMContentLoaded', function() {
        var switches = document.querySelectorAll('.preference-switch');
        
        switches.forEach(function(switchElement) {
            var instance = switchElement.ej2_instances[0];
            var setting = switchElement.getAttribute('data-setting');
            
            instance.addEventListener('change', function(args) {
                updatePreference(setting, args.checked);
            });
        });
    });
    
    function updatePreference(setting, enabled) {
        console.log('Updating ' + setting + ' to ' + enabled);
        // Send to server
    }
</script>
```

## Server Communication

### Send State to Server Immediately

```cshtml
<ejs-switch id="newsletter" change="saveToServer"></ejs-switch>

<script>
    function saveToServer(args) {
        // Send to server API
        fetch('/api/preferences/update', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({
                setting: 'newsletter',
                enabled: args.checked
            })
        })
        .then(response => response.json())
        .then(data => {
            if (data.success) {
                console.log('Saved to server');
            } else {
                console.error('Failed to save');
                // Revert switch
                var switchInstance = document.getElementById('newsletter').ej2_instances[0];
                switchInstance.checked = !args.checked;
            }
        })
        .catch(error => {
            console.error('Error:', error);
            // Revert switch on error
            var switchInstance = document.getElementById('newsletter').ej2_instances[0];
            switchInstance.checked = !args.checked;
        });
    }
</script>
```

### Batch Updates on Form Submit

```cshtml
<form id="settingsForm" method="post">
    <div class="settings">
        <div>
            <label>Enable Notifications</label>
            <ejs-switch id="notif" name="enableNotifications"></ejs-switch>
        </div>
        
        <div>
            <label>Enable Analytics</label>
            <ejs-switch id="analytics" name="enableAnalytics"></ejs-switch>
        </div>
        
        <div>
            <label>Marketing Emails</label>
            <ejs-switch id="marketing" name="enableMarketing"></ejs-switch>
        </div>
    </div>
    
    <button type="submit" class="btn btn-primary">Save Settings</button>
</form>

<script>
    document.getElementById('settingsForm').addEventListener('submit', function(e) {
        e.preventDefault();
        
        // Get all switch values
        var settings = {
            enableNotifications: document.getElementById('notif').ej2_instances[0].checked,
            enableAnalytics: document.getElementById('analytics').ej2_instances[0].checked,
            enableMarketing: document.getElementById('marketing').ej2_instances[0].checked
        };
        
        // Submit to server
        fetch('/settings/save', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(settings)
        })
        .then(response => response.json())
        .then(data => {
            if (data.success) {
                alert('Settings saved successfully!');
            } else {
                alert('Error saving settings');
            }
        });
    });
</script>
```

## Form Submission

### Automatic Form Binding

When using `asp-for` attribute, Switch values are automatically sent with form submission:

```cshtml
@model SettingsModel

<form method="post">
    <div class="form-group">
        <label asp-for="EnableNotifications"></label>
        <ejs-switch id="notifications" asp-for="EnableNotifications"></ejs-switch>
    </div>
    
    <div class="form-group">
        <label asp-for="EnableMarketing"></label>
        <ejs-switch id="marketing" asp-for="EnableMarketing"></ejs-switch>
    </div>
    
    <button type="submit" class="btn btn-primary">Save</button>
</form>
```

**Server-Side Handler:**
```csharp
public async Task<IActionResult> OnPost(SettingsModel model)
{
    // model.EnableNotifications - automatically bound from switch
    // model.EnableMarketing - automatically bound from switch
    
    await _settings.UpdateAsync(User.Id, model);
    return RedirectToPage();
}
```

### Manual Form Data Collection

```javascript
function collectFormData() {
    var notifSwitch = document.getElementById('notif').ej2_instances[0];
    var analyticsSwitch = document.getElementById('analytics').ej2_instances[0];
    
    var formData = new FormData();
    formData.append('EnableNotifications', notifSwitch.checked);
    formData.append('EnableAnalytics', analyticsSwitch.checked);
    
    return formData;
}
```

## Validation and Confirmation

### Prevent State Change Until Confirmed

```cshtml
<ejs-switch id="deleteData" change="confirmDeletion"></ejs-switch>

<script>
    function confirmDeletion(args) {
        // Only proceed if user confirms
        if (args.checked) {
            var confirmed = confirm('Warning: This action cannot be undone. Continue?');
            if (!confirmed) {
                // Revert the switch
                var switchInstance = document.getElementById('deleteData').ej2_instances[0];
                switchInstance.checked = false;
            }
        }
    }
</script>
```

### Undo Functionality

```cshtml
<div class="setting-with-undo">
    <label>Enable Experimental Feature</label>
    <ejs-switch id="experimental" change="onExperimentalChange"></ejs-switch>
    
    <div id="undoContainer" style="display: none; margin-top: 10px;">
        <button type="button" onclick="undoExperimentalChange()">
            Undo Change
        </button>
        <span id="undoTimer"></span>
    </div>
</div>

<script>
    var previousState = null;
    var undoTimer = null;
    
    function onExperimentalChange(args) {
        previousState = !args.checked;
        
        // Show undo button for 5 seconds
        document.getElementById('undoContainer').style.display = 'block';
        
        // Auto-hide undo after 5 seconds
        let countdown = 5;
        document.getElementById('undoTimer').textContent = 'Undo in ' + countdown + 's';
        
        undoTimer = setInterval(function() {
            countdown--;
            document.getElementById('undoTimer').textContent = 'Undo in ' + countdown + 's';
            
            if (countdown <= 0) {
                clearInterval(undoTimer);
                document.getElementById('undoContainer').style.display = 'none';
            }
        }, 1000);
    }
    
    function undoExperimentalChange() {
        clearInterval(undoTimer);
        var switchInstance = document.getElementById('experimental').ej2_instances[0];
        switchInstance.checked = previousState;
        document.getElementById('undoContainer').style.display = 'none';
    }
</script>
```

## Advanced Event Handling

### Debouncing Multiple Changes

```javascript
var changeTimeout;

document.addEventListener('DOMContentLoaded', function() {
    var switchInstance = document.getElementById('autoSave').ej2_instances[0];
    
    switchInstance.addEventListener('change', function(args) {
        // Clear previous timeout
        clearTimeout(changeTimeout);
        
        // Wait 1 second before saving (in case user toggles multiple times)
        changeTimeout = setTimeout(function() {
            saveToServer(args.checked);
        }, 1000);
    });
});
```

### Dependent Switches (Enable/Disable Based on Parent)

```cshtml
<!-- Parent switch -->
<div>
    <label>Enable Advanced Settings</label>
    <ejs-switch id="advancedParent" change="onAdvancedToggle"></ejs-switch>
</div>

<!-- Child switches (disabled until parent enabled) -->
<div id="advancedOptions" style="display: none;">
    <label>Custom Encoding</label>
    <ejs-switch id="encoding" disabled="true"></ejs-switch>
    
    <label>Custom Format</label>
    <ejs-switch id="format" disabled="true"></ejs-switch>
</div>

<script>
    function onAdvancedToggle(args) {
        var encodingSwitch = document.getElementById('encoding').ej2_instances[0];
        var formatSwitch = document.getElementById('format').ej2_instances[0];
        var advancedOptions = document.getElementById('advancedOptions');
        
        if (args.checked) {
            // Enable child switches
            encodingSwitch.disabled = false;
            formatSwitch.disabled = false;
            advancedOptions.style.display = 'block';
        } else {
            // Disable and uncheck child switches
            encodingSwitch.disabled = true;
            encodingSwitch.checked = false;
            formatSwitch.disabled = true;
            formatSwitch.checked = false;
            advancedOptions.style.display = 'none';
        }
    }
</script>
```

### Track User Interaction vs Programmatic Changes

```javascript
var isUserInteracted = true;

document.addEventListener('DOMContentLoaded', function() {
    var switchInstance = document.getElementById('mySwitch').ej2_instances[0];
    
    switchInstance.addEventListener('change', function(args) {
        if (args.isInteracted) {
            console.log('User manually toggled');
            // User changed it - update server, etc.
        } else {
            console.log('Changed programmatically');
            // App changed it - different handling
        }
    });
    
    // Programmatic change (doesn't trigger isInteracted = true)
    document.getElementById('resetButton').addEventListener('click', function() {
        isUserInteracted = false;
        switchInstance.checked = false;
    });
});
```

## Common Event Patterns

### Pattern 1: Real-time Toggle with Visual Feedback

```cshtml
<div class="setting-with-spinner">
    <label>Sync Settings</label>
    <ejs-switch id="syncSwitch" change="handleSync"></ejs-switch>
    <span id="syncSpinner" style="display: none; margin-left: 10px;">Syncing...</span>
</div>

<script>
    function handleSync(args) {
        var spinner = document.getElementById('syncSpinner');
        spinner.style.display = 'inline';
        
        // Simulate sync operation
        setTimeout(function() {
            spinner.style.display = 'none';
            console.log('Sync completed: ' + args.checked);
        }, 2000);
    }
</script>
```

### Pattern 2: Linked Switches (Mutual Exclusivity)

```cshtml
<!-- Mode selection: Manual or Auto (only one can be on) -->
<div>
    <label>Manual Mode</label>
    <ejs-switch id="manual" change="onManualToggle"></ejs-switch>
</div>

<div>
    <label>Auto Mode</label>
    <ejs-switch id="auto" change="onAutoToggle"></ejs-switch>
</div>

<script>
    function onManualToggle(args) {
        if (args.checked) {
            // Turn off auto when manual is enabled
            document.getElementById('auto').ej2_instances[0].checked = false;
        }
    }
    
    function onAutoToggle(args) {
        if (args.checked) {
            // Turn off manual when auto is enabled
            document.getElementById('manual').ej2_instances[0].checked = false;
        }
    }
</script>
```

### Pattern 3: Event Logging

```javascript
var eventLog = [];

document.addEventListener('DOMContentLoaded', function() {
    var switchInstance = document.getElementById('audit').ej2_instances[0];
    
    switchInstance.addEventListener('change', function(args) {
        var timestamp = new Date().toLocaleTimeString();
        eventLog.push({
            time: timestamp,
            previousValue: args.previousValue,
            newValue: args.checked,
            isInteracted: args.isInteracted
        });
        
        console.log('Event logged:', eventLog[eventLog.length - 1]);
    });
});
```

## Troubleshooting

### Issue: Change event fires but state isn't updated

**Cause:** Event handler runs before state change completes
**Solution:**
```javascript
// Use setTimeout to let state update first
switchInstance.addEventListener('change', function(args) {
    setTimeout(function() {
        console.log('Updated state:', args.checked);
    }, 0);
});
```

### Issue: Event doesn't fire when switch state changes programmatically

**Expected behavior:** Setting `.checked = true` doesn't trigger change event
**Solution:** Manually trigger if needed:
```javascript
// Change state
switchInstance.checked = true;

// Trigger change event manually if needed
switchInstance.change({ checked: true, isInteracted: false });
```

### Issue: Server update fails but switch already changed UI

**Cause:** Switch updates before server confirmation
**Solution:**
```javascript
function handleChange(args) {
    // Temporarily revert UI
    var tempState = !args.checked;
    var switchInstance = document.getElementById('mySwitch').ej2_instances[0];
    
    // Try server update
    saveToServer(args.checked)
        .catch(error => {
            // Revert on error
            switchInstance.checked = tempState;
            alert('Failed to update setting');
        });
}
```

## Next Steps

- Configure properties: [Switch Properties Reference](../switch-properties.md)
- Customize appearance: [Styling and Customization](../styling-customization.md)
- Setup guide: [Getting Started](../getting-started.md)
