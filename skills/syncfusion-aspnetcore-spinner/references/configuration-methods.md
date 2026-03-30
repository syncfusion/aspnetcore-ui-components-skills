# Configuration Methods Reference

## Table of Contents
- [Overview](#overview)
- [createSpinner()](#createspinner)
- [showSpinner()](#showspinner)
- [hideSpinner()](#hidespinner)
- [setSpinner()](#setspinner)
- [Method Chaining Pattern](#method-chaining-pattern)
- [Real-World Examples](#real-world-examples)
- [Error Handling](#error-handling)

## Overview

The Syncfusion Spinner provides four main methods for configuration and lifecycle management:

| Method | Purpose | When to Use |
|--------|---------|------------|
| `createSpinner()` | Initialize spinner for a target | Once, before showing |
| `showSpinner()` | Display the spinner overlay | Before async operations |
| `hideSpinner()` | Hide the spinner overlay | After operations complete |
| `setSpinner()` | Configure type/template | Before creating ANY spinners |

## createSpinner()

**Purpose:** Initialize a spinner instance for a specific target element.

### Syntax

```javascript
ej.popups.createSpinner(config);
```

### Parameters

```javascript
{
    target: Element,  // Required: DOM element where spinner appears
    label: String     // Optional: Text label for spinner
}
```

### Required Parameter: target

The target element that the spinner will overlay:

```javascript
// Get target element
var target = document.getElementById('container');

// Create spinner for that target
ej.popups.createSpinner({
    target: target
});
```

**Target Requirements:**
- Must be a valid DOM element
- Should have defined dimensions (height/width)
- Better with `position: relative` or `position: absolute` parent
- Can be `document.body` for full-page spinner

### Optional Parameter: label

Add a text label (rarely used):

```javascript
ej.popups.createSpinner({
    target: target,
    label: 'Loading data'  // Optional label
});
```

### Examples

**Simple target:**
```javascript
var target = document.getElementById('container');
ej.popups.createSpinner({ target: target });
```

**Full-page spinner:**
```javascript
ej.popups.createSpinner({ target: document.body });
```

**Multiple spinners:**
```javascript
ej.popups.createSpinner({ target: document.getElementById('area1') });
ej.popups.createSpinner({ target: document.getElementById('area2') });
// Each has independent show/hide control
```

### Important Notes

- Call ONCE per target element
- Must call BEFORE `showSpinner()` for that target
- Must call AFTER `setSpinner()` if setting type/template
- Spinner is created but hidden until `showSpinner()` is called

## showSpinner()

**Purpose:** Display the spinner overlay on a target element.

### Syntax

```javascript
ej.popups.showSpinner(target);
```

### Parameters

```javascript
target  // Required: DOM element (must match createSpinner target)
```

### Basic Usage

```javascript
var target = document.getElementById('container');
ej.popups.createSpinner({ target: target });

// Later, show the spinner
ej.popups.showSpinner(target);
```

### Behavior

When called, `showSpinner()`:
1. Makes spinner overlay visible (opacity: 1)
2. Starts spinner animation
3. Disables user interaction with target
4. Centers spinner within target
5. Sets z-index above target content

### Examples

**Show on button click:**
```javascript
document.getElementById('loadBtn').addEventListener('click', function() {
    ej.popups.showSpinner(target);
});
```

**Show before async operation:**
```javascript
function fetchData() {
    ej.popups.showSpinner(target);
    
    fetch('/api/data')
        .then(r => r.json())
        .then(data => {
            ej.popups.hideSpinner(target);
            updateUI(data);
        });
}
```

**Multiple spinners:**
```javascript
// Show specific spinners
ej.popups.showSpinner(area1);  // Only area1 shows
ej.popups.showSpinner(area2);  // Only area2 shows
// area3 remains hidden
```

### Prerequisites

- Must call `createSpinner()` first for that target
- Target element must be in DOM
- Spinner type/template must be set BEFORE `createSpinner()`

## hideSpinner()

**Purpose:** Hide the spinner overlay on a target element.

### Syntax

```javascript
ej.popups.hideSpinner(target);
```

### Parameters

```javascript
target  // Required: DOM element (must match createSpinner target)
```

### Basic Usage

```javascript
var target = document.getElementById('container');
ej.popups.createSpinner({ target: target });
ej.popups.showSpinner(target);

// Later, hide the spinner
ej.popups.hideSpinner(target);
```

### Behavior

When called, `hideSpinner()`:
1. Makes spinner overlay invisible (opacity: 0)
2. Stops spinner animation
3. Restores user interaction with target
4. Keeps spinner in DOM for later reuse
5. Can be shown again without recreating

### Examples

**Hide after timeout:**
```javascript
ej.popups.showSpinner(target);

setTimeout(function() {
    ej.popups.hideSpinner(target);
}, 3000);  // Hide after 3 seconds
```

**Hide after async operation:**
```javascript
fetch('/api/data')
    .then(r => r.json())
    .then(data => {
        updateUI(data);
    })
    .finally(() => {
        ej.popups.hideSpinner(target);  // Hide in all cases
    });
```

**Hide multiple spinners:**
```javascript
ej.popups.hideSpinner(area1);
ej.popups.hideSpinner(area2);
ej.popups.hideSpinner(area3);
```

### Reusability

Spinners can be shown/hidden multiple times:

```javascript
var target = document.getElementById('container');
ej.popups.createSpinner({ target: target });

// First use
ej.popups.showSpinner(target);
setTimeout(() => ej.popups.hideSpinner(target), 2000);

// Can reuse same spinner
setTimeout(() => ej.popups.showSpinner(target), 5000);
setTimeout(() => ej.popups.hideSpinner(target), 8000);
```

## setSpinner()

**Purpose:** Configure spinner type or template globally.

### Syntax

```javascript
ej.popups.setSpinner(config);
```

### Parameters

```javascript
{
    type: String,      // Optional: 'Material', 'Fabric', 'Bootstrap', 'Bootstrap4', 'HighContrast'
    template: String   // Optional: HTML/SVG string for custom appearance
}
```

### Type Configuration

Set spinner type for all subsequent spinners:

```javascript
// Set Bootstrap4 type
ej.popups.setSpinner({ type: 'Bootstrap4' });

// Now all createSpinner() calls use Bootstrap4
ej.popups.createSpinner({ target: element1 });
ej.popups.createSpinner({ target: element2 });
// Both use Bootstrap4 type
```

**Valid Type Values:**
```javascript
ej.popups.setSpinner({ type: 'Material' });
ej.popups.setSpinner({ type: 'Fabric' });
ej.popups.setSpinner({ type: 'Bootstrap' });
ej.popups.setSpinner({ type: 'Bootstrap4' });
ej.popups.setSpinner({ type: 'HighContrast' });
```

### Template Configuration

Set custom template for all spinners:

```javascript
ej.popups.setSpinner({
    template: '<div style="width:100%;height:100%" class="custom-spinner">' +
              '<p>Loading...</p>' +
              '</div>'
});

// All spinners now use custom template
ej.popups.createSpinner({ target: element1 });
ej.popups.createSpinner({ target: element2 });
```

### Combined Configuration

Set both type and template:

```javascript
ej.popups.setSpinner({
    type: 'Bootstrap4',
    template: '<div style="width:100%;height:100%">' +
              '<img src="custom-spinner.svg" />' +
              '</div>'
});
```

### Critical Timing

`setSpinner()` MUST be called before `createSpinner()`:

```javascript
// ✅ CORRECT
ej.popups.setSpinner({ type: 'Bootstrap4' });
ej.popups.createSpinner({ target: target });
ej.popups.showSpinner(target);

// ❌ WRONG - Type ignored
ej.popups.createSpinner({ target: target });
ej.popups.setSpinner({ type: 'Bootstrap4' });
ej.popups.showSpinner(target);
```

### Global Configuration

Best practice: Set globally at app startup:

```html
<!-- _Layout.cshtml -->
<body>
    @RenderBody()
    
    <ejs-scripts></ejs-scripts>
    
    <script>
        // Set spinner configuration once
        ej.popups.setSpinner({ type: 'Bootstrap4' });
    </script>
    
    <script src="~/js/app.js"></script>
</body>
```

Then use in your app without worrying about type:

```javascript
// app.js
ej.popups.createSpinner({ target: target });  // Uses configured type
```

## Method Chaining Pattern

While Syncfusion spinner methods don't return the object for chaining, you can chain them logically:

### Initialization Sequence

```javascript
// Proper initialization order (pseudo-chaining)
ej.popups.setSpinner({ type: 'Bootstrap4' });
ej.popups.createSpinner({ target: target });
ej.popups.showSpinner(target);
```

### Wrapped Chaining

Create helper functions for chainable patterns:

```javascript
function showSpinnerFor(target, duration) {
    ej.popups.createSpinner({ target: target });
    ej.popups.showSpinner(target);
    
    if (duration) {
        setTimeout(() => ej.popups.hideSpinner(target), duration);
    }
    
    return target;  // Return for potential future chaining
}

// Usage
showSpinnerFor(myElement, 3000);
```

### Promise-Based Pattern

```javascript
function showSpinnerAsync(target, operation) {
    ej.popups.createSpinner({ target: target });
    ej.popups.showSpinner(target);
    
    return operation().finally(() => {
        ej.popups.hideSpinner(target);
    });
}

// Usage
showSpinnerAsync(target, () => fetch('/api/data'))
    .then(data => updateUI(data))
    .catch(error => showError(error));
```

## Real-World Examples

### Form Submission

```javascript
document.getElementById('submitBtn').addEventListener('click', function(e) {
    e.preventDefault();
    
    var target = document.getElementById('formContainer');
    ej.popups.createSpinner({ target: target });
    ej.popups.showSpinner(target);
    
    const formData = new FormData(document.getElementById('myForm'));
    
    fetch('/api/submit', { method: 'POST', body: formData })
        .then(response => response.json())
        .then(data => {
            showMessage('Success: ' + data.message);
        })
        .catch(error => {
            showError('Error: ' + error.message);
        })
        .finally(() => {
            ej.popups.hideSpinner(target);
        });
});
```

### Data Grid with Spinner

```javascript
var gridTarget = document.getElementById('gridContainer');
ej.popups.setSpinner({ type: 'Bootstrap4' });
ej.popups.createSpinner({ target: gridTarget });

// Initialize grid
var grid = new ej.grids.Grid({
    dataSource: {
        url: '/api/employees',
        adaptor: 'UrlAdaptor'
    },
    dataBound: function() {
        ej.popups.hideSpinner(gridTarget);  // Hide when data loaded
    }
});

// Show while loading
ej.popups.showSpinner(gridTarget);
```

### Nested Async Operations

```javascript
var target = document.getElementById('container');
ej.popups.createSpinner({ target: target });

async function processData() {
    ej.popups.showSpinner(target);
    
    try {
        // Step 1
        const users = await fetch('/api/users').then(r => r.json());
        
        // Step 2
        const details = await fetch('/api/details').then(r => r.json());
        
        // Step 3
        await saveResults({ users, details });
        
        showMessage('All operations complete');
    } catch (error) {
        showError(error);
    } finally {
        ej.popups.hideSpinner(target);
    }
}

processData();
```

## Error Handling

### Check Element Exists

```javascript
function safeShowSpinner(elementId) {
    var target = document.getElementById(elementId);
    
    if (!target) {
        console.error('Target element not found: ' + elementId);
        return;
    }
    
    ej.popups.createSpinner({ target: target });
    ej.popups.showSpinner(target);
}
```

### Try-Catch Pattern

```javascript
try {
    ej.popups.setSpinner({ type: 'Bootstrap4' });
    ej.popups.createSpinner({ target: target });
    ej.popups.showSpinner(target);
} catch (error) {
    console.error('Spinner error:', error);
    // Fallback UI
    target.classList.add('loading');
}
```

### Verify Spinner State

```javascript
function showSpinnerSafely(target) {
    try {
        // Ensure spinner created
        if (!target._spinnerElement) {
            ej.popups.createSpinner({ target: target });
        }
        ej.popups.showSpinner(target);
    } catch (error) {
        console.error('Failed to show spinner:', error);
        // Implement fallback
    }
}
```

### Cleanup on Errors

```javascript
fetch('/api/data')
    .then(r => {
        if (!r.ok) throw new Error('Network error');
        return r.json();
    })
    .then(data => {
        updateUI(data);
    })
    .catch(error => {
        console.error(error);
        showUserError('Failed to load data');
    })
    .finally(() => {
        ej.popups.hideSpinner(target);  // Always cleanup
    });
```

## Performance Considerations

**Reuse spinners for repeated operations:**
```javascript
// Good: Create once, reuse
ej.popups.createSpinner({ target: target });

for (let i = 0; i < 100; i++) {
    ej.popups.showSpinner(target);
    // operation
    ej.popups.hideSpinner(target);
}

// Bad: Recreate each time
for (let i = 0; i < 100; i++) {
    ej.popups.createSpinner({ target: target });  // Wasteful
    ej.popups.showSpinner(target);
    // operation
    ej.popups.hideSpinner(target);
}
```
