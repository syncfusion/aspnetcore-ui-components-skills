# Custom Templates

## Table of Contents
- [Overview](#overview)
- [When to Use Templates](#when-to-use-templates)
- [Template Basics](#template-basics)
- [HTML Templates](#html-templates)
- [SVG Templates](#svg-templates)
- [Configuration Patterns](#configuration-patterns)
- [Styling Custom Templates](#styling-custom-templates)
- [Best Practices](#best-practices)

## Overview

Custom templates allow you to replace the default Syncfusion spinner with custom HTML or SVG. Use the `setSpinner()` method with a `template` property to specify your custom content.

**Key Points:**
- Templates must be set BEFORE component creation
- Templates can be HTML strings or SVG markup
- CSS can style template content
- Templates replace entire spinner visual

## When to Use Templates

**Use custom templates when you:**
- Need branded loading indicators matching company colors/logo
- Want custom animations or visual effects
- Require specific text/messaging during loading
- Need animated GIFs or custom SVG animations
- Want loading progress percentage display

**Use default spinner when:**
- Default appearance meets requirements
- Consistent theming with other components
- Performance is critical
- Simple loading indicator is sufficient

## Template Basics

### Template Structure

A template is an HTML or SVG string passed to `setSpinner()`:

```javascript
// Simple HTML template
ej.popups.setSpinner({
    template: '<div style="width:100%;height:100%" class="custom-spinner">'
            + '<div class="spinner-text">Loading...</div>'
            + '</div>'
});
```

### Required Styles

The outer container must fill 100% of target:

```javascript
// Always include these styles in template
template: '<div style="width:100%;height:100%" class="my-spinner">' +
          '  <!-- spinner content -->' +
          '</div>'
```

**Why:** The template container needs full coverage of the target element to prevent user interaction.

### Template Initialization Order

```javascript
// CORRECT ORDER - Template must be set first:
// 1. Set template
ej.popups.setSpinner({
    template: '<div style="width:100%;height:100%">Loading</div>'
});

// 2. Create spinner
ej.popups.createSpinner({ target: document.getElementById('container') });

// 3. Show spinner
ej.popups.showSpinner(target);


// INCORRECT - Template will be ignored:
ej.popups.createSpinner({ target: target });
ej.popups.setSpinner({ template: '...' });  // Too late!
```

## HTML Templates

### Simple Text Template

```javascript
ej.popups.setSpinner({
    template: '<div style="width:100%;height:100%" class="custom-spinner">' +
              '<p style="text-align:center;margin-top:50px;">Please wait...</p>' +
              '</div>'
});
```

### HTML with Logo/Image

```javascript
ej.popups.setSpinner({
    template: '<div style="width:100%;height:100%;display:flex;' +
              'align-items:center;justify-content:center;" class="custom-spinner">' +
              '<div style="text-align:center;">' +
              '<img src="/images/logo.png" style="width:60px;margin-bottom:20px;" />' +
              '<p>Loading...</p>' +
              '</div>' +
              '</div>'
});
```

### HTML with Animated GIF

```javascript
ej.popups.setSpinner({
    template: '<div style="width:100%;height:100%;display:flex;' +
              'align-items:center;justify-content:center;" class="custom-spinner">' +
              '<img src="/images/spinner.gif" style="width:50px;height:50px;" />' +
              '</div>'
});
```

### HTML with Progress Text

```javascript
ej.popups.setSpinner({
    template: '<div style="width:100%;height:100%" class="custom-spinner">' +
              '<div style="position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);">' +
              '<div class="spinner-dot"></div>' +
              '<p id="loading-status">Initializing...</p>' +
              '</div>' +
              '</div>'
});
```

## SVG Templates

### Simple SVG Spinner

```javascript
ej.popups.setSpinner({
    template: '<div style="width:100%;height:100%;display:flex;' +
              'align-items:center;justify-content:center;" class="custom-spinner">' +
              '<svg xmlns="http://www.w3.org/2000/svg" width="50" height="50" viewBox="0 0 50 50">' +
              '<circle cx="25" cy="25" r="20" fill="none" stroke="#007BFF" stroke-width="2" ' +
              'stroke-dasharray="31.4 94.2" ' +
              'style="animation: spin 1s linear infinite;transform-origin:center;">' +
              '</circle>' +
              '</svg>' +
              '</div>'
});
```

### Custom SVG with Animation

```javascript
ej.popups.setSpinner({
    template: '<div style="width:100%;height:100%;display:flex;' +
              'align-items:center;justify-content:center;" class="custom-spinner">' +
              '<svg xmlns="http://www.w3.org/2000/svg" width="60" height="60" viewBox="0 0 60 60">' +
              '<g transform="translate(30, 30)">' +
              '<circle cx="0" cy="-15" r="5" fill="#FF6B6B" ' +
              'style="animation: orbit 2s linear infinite;transform-origin:0 15px;">' +
              '</circle>' +
              '<circle cx="0" cy="-15" r="5" fill="#4ECDC4" ' +
              'style="animation: orbit 2s linear infinite;animation-delay:0.66s;transform-origin:0 15px;">' +
              '</circle>' +
              '<circle cx="0" cy="-15" r="5" fill="#95E1D3" ' +
              'style="animation: orbit 2s linear infinite;animation-delay:1.32s;transform-origin:0 15px;">' +
              '</circle>' +
              '</g>' +
              '</svg>' +
              '</div>'
});
```

### SVG Star Spinner

```javascript
ej.popups.setSpinner({
    template: '<div style="width:100%;height:100%;display:flex;' +
              'align-items:center;justify-content:center;" class="custom-spinner">' +
              '<svg xmlns="http://www.w3.org/2000/svg" width="50" height="50" viewBox="0 0 50 50">' +
              '<g style="animation: rotate 2s linear infinite;transform-origin:center;">' +
              '<polygon points="25,5 30,20 45,20 35,28 40,42 25,35 10,42 15,28 5,20 20,20" ' +
              'fill="none" stroke="#2196F3" stroke-width="2"/>' +
              '</g>' +
              '</svg>' +
              '</div>'
});
```

## Configuration Patterns

### Complete Setup with Custom Template

```html
<div id="container" style="height:300px;border:1px solid #ccc;">
    <p>Content area</p>
    <button id="loadBtn">Start Loading</button>
</div>

<style>
    @keyframes spin {
        from { transform: rotate(0deg); }
        to { transform: rotate(360deg); }
    }
    
    .custom-spinner {
        background-color: rgba(0,0,0,0.5);
        display: flex;
        align-items: center;
        justify-content: center;
    }
</style>

<script>
    // Set custom template FIRST
    ej.popups.setSpinner({
        template: '<div style="width:100%;height:100%" class="custom-spinner">' +
                  '<div style="color:white;text-align:center;">' +
                  '<p>Processing your request...</p>' +
                  '</div>' +
                  '</div>'
    });
    
    // Create spinner
    var target = document.getElementById('container');
    ej.popups.createSpinner({ target: target });
    
    // Use spinner
    document.getElementById('loadBtn').addEventListener('click', function() {
        ej.popups.showSpinner(target);
        setTimeout(() => ej.popups.hideSpinner(target), 3000);
    });
</script>
```

### Multiple Template Styles

If you need different templates in different areas, set template globally and create spinners for each target:

```javascript
// Global template for all spinners
ej.popups.setSpinner({
    template: '<div style="width:100%;height:100%;' +
              'background:rgba(255,255,255,0.9);display:flex;' +
              'align-items:center;justify-content:center;">' +
              '<div class="global-spinner-content"></div>' +
              '</div>'
});

// Create spinners for different targets with same template
ej.popups.createSpinner({ target: document.getElementById('area1') });
ej.popups.createSpinner({ target: document.getElementById('area2') });
ej.popups.createSpinner({ target: document.getElementById('area3') });

// All use the same template
```

## Styling Custom Templates

### CSS Classes for Templates

```html
<style>
    .custom-spinner {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        display: flex;
        align-items: center;
        justify-content: center;
        font-weight: bold;
        color: white;
    }
    
    .spinner-content {
        text-align: center;
        padding: 20px;
    }
</style>

<script>
    ej.popups.setSpinner({
        template: '<div style="width:100%;height:100%" class="custom-spinner">' +
                  '<div class="spinner-content">Loading...</div>' +
                  '</div>'
    });
</script>
```

### Responsive Template Styles

```html
<style>
    .custom-spinner {
        font-size: 16px;
    }
    
    @media (max-width: 768px) {
        .custom-spinner {
            font-size: 14px;
        }
    }
    
    @media (max-width: 480px) {
        .custom-spinner {
            font-size: 12px;
        }
    }
</style>
```

### Dark Mode Template

```html
<style>
    .custom-spinner {
        background-color: rgba(30, 30, 30, 0.8);
        color: #e0e0e0;
    }
    
    @media (prefers-color-scheme: light) {
        .custom-spinner {
            background-color: rgba(255, 255, 255, 0.9);
            color: #333;
        }
    }
</style>
```

## Best Practices

### 1. Keep Templates Simple

❌ **Too Complex:**
```javascript
// Avoid heavy DOM manipulation
template: '<div>' + generateComplexUI() + '</div>'
```

✅ **Better:**
```javascript
// Use simple HTML string
template: '<div style="width:100%;height:100%" class="spinner">' +
          '<p>Loading...</p>' +
          '</div>'
```

### 2. Always Include Full-Size Container

❌ **Wrong:**
```javascript
template: '<img src="spinner.gif" />'  // Missing container
```

✅ **Correct:**
```javascript
template: '<div style="width:100%;height:100%;display:flex;' +
          'align-items:center;justify-content:center;">' +
          '<img src="spinner.gif" />' +
          '</div>'
```

### 3. Set Template Before Creation

❌ **Wrong Order:**
```javascript
ej.popups.createSpinner({ target: target });
ej.popups.setSpinner({ template: '...' });
```

✅ **Correct Order:**
```javascript
ej.popups.setSpinner({ template: '...' });
ej.popups.createSpinner({ target: target });
```

### 4. Use External CSS for Complex Styling

❌ **Inline Styles:**
```javascript
template: '<div style="width:100%;height:100%;background:linear-gradient(...);">'
```

✅ **With CSS Classes:**
```javascript
template: '<div style="width:100%;height:100%" class="branded-spinner"></div>'

// In CSS file:
// .branded-spinner { background: linear-gradient(...); }
```

### 5. Consider Performance

- Avoid complex SVG animations for spinners on many targets
- Use CSS animations instead of JavaScript animations
- Test performance on slower devices
- Keep template string lightweight

### 6. Accessibility

```javascript
// Include ARIA labels for accessibility
template: '<div style="width:100%;height:100%" ' +
          'role="status" aria-label="Loading..." ' +
          'class="custom-spinner">' +
          '<div aria-hidden="true">Loading...</div>' +
          '</div>'
```

## Troubleshooting Templates

**Template not showing:**
- Verify `setSpinner()` called before `createSpinner()`
- Check browser console for errors
- Verify outer div has `width:100%;height:100%`
- Ensure no CSS hiding the spinner

**Template overlapping incorrectly:**
- Check z-index values in CSS
- Verify target element has proper positioning
- Test with different container heights

**Animations not working:**
- Check CSS animation syntax
- Verify browser support for used features
- Test animation separately before adding to spinner
