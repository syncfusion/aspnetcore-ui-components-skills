# Spinner Types and Configuration

## Table of Contents
- [Overview](#overview)
- [Available Types](#available-types)
- [Material Type](#material-type)
- [Fabric Type](#fabric-type)
- [Bootstrap Type](#bootstrap-type)
- [Bootstrap4 Type](#bootstrap4-type)
- [High Contrast Type](#high-contrast-type)
- [Setting Spinner Type](#setting-spinner-type)
- [Theme-Based Selection](#theme-based-selection)
- [Type Configuration](#type-configuration)

## Overview

By default, the Spinner type is automatically set based on the theme imported into your page. However, you can explicitly set the spinner type using the `setSpinner()` method to override the default behavior.

**Key Point:** The spinner type must be set BEFORE creating the component. Changing type after component creation requires recreating the spinner.

## Available Types

The Syncfusion Spinner supports five distinct types, each designed to match different design themes:

| Type | Use Case | Default For |
|------|----------|-------------|
| **Material** | Modern, minimalist design | Material theme |
| **Fabric** | Office-inspired design | Fabric theme |
| **Bootstrap** | Bootstrap 3/4 design | Bootstrap themes |
| **Bootstrap4** | Modern Bootstrap 4 design | Bootstrap 4+ |
| **High Contrast** | Accessibility and visibility | High Contrast theme |

## Material Type

The Material type renders a spinner with a modern, minimalist appearance using a rotating ring pattern.

**Characteristics:**
- Circular rotating ring
- Smooth animation
- Works well on modern, flat UI designs
- Default when Material theme is imported

**Visual Example:**
- Single animated ring rotating around a center point
- Thin stroke with smooth curves

**CSS Class Used:**
```css
.e-spin-material
```

## Fabric Type

The Fabric type provides an Office-inspired spinner design with a specific animation pattern.

**Characteristics:**
- Office 365-style animation
- Square-based design elements
- Works well with Fabric/Office UI
- Professional appearance

**Visual Example:**
- Nested squares with rotating animation
- More structured than Material

**CSS Class Used:**
```css
.e-spin-fabric
```

## Bootstrap Type

The Bootstrap type renders a standard Bootstrap-style spinner with a circular progress pattern.

**Characteristics:**
- Compatible with Bootstrap 3/4 designs
- Classic rotating circle with offset arc
- Filled areas rather than stroked
- Works across Bootstrap versions

**Visual Example:**
- Circular with partial fill rotating around
- Similar to Bootstrap's native spinner

**CSS Class Used:**
```css
.e-spin-bootstrap
```

## Bootstrap4 Type

The Bootstrap4 type is optimized for Bootstrap 4+ frameworks with updated visual design.

**Characteristics:**
- Modern Bootstrap 4 styling
- Refined animation patterns
- Better performance
- Recommended for new Bootstrap 4+ projects

**Visual Example:**
- Updated Bootstrap spinner design
- Smoother animation curves

**CSS Class Used:**
```css
.e-spin-bootstrap4
```

## High Contrast Type

The High Contrast type ensures maximum visibility and accessibility in high-contrast mode.

**Characteristics:**
- Enhanced visibility for accessibility
- Stronger contrast ratios
- Works well in low-light environments
- Meets WCAG accessibility standards

**Visual Example:**
- Bold, high-contrast design
- Clear distinction between animated and static parts

**CSS Classes Used:**
```css
.e-spin-high-contrast
.e-path-arc
```

## Setting Spinner Type

### Method 1: Using setSpinner()

Explicitly set the spinner type before component creation:

```javascript
// Set type BEFORE creating spinner
ej.popups.setSpinner({ type: 'Bootstrap4' });

// Then create spinner
var target = document.getElementById('container');
ej.popups.createSpinner({ target: target });
```

**Important:** Call `setSpinner()` immediately after your scripts load, before any `createSpinner()` calls.

### Method 2: Automatic Type Detection

If you don't call `setSpinner()`, the type is automatically determined by the imported theme:

```javascript
// No setSpinner() call needed
var target = document.getElementById('container');
ej.popups.createSpinner({ target: target });

// Type matches the CSS theme in <head>
```

### All Valid Type Values

```javascript
// Valid type parameters:
ej.popups.setSpinner({ type: 'Material' });
ej.popups.setSpinner({ type: 'Fabric' });
ej.popups.setSpinner({ type: 'Bootstrap' });
ej.popups.setSpinner({ type: 'Bootstrap4' });
ej.popups.setSpinner({ type: 'HighContrast' });
```

**Note:** Type names are case-sensitive. Use exact capitalization as shown above.

## Theme-Based Selection

### Material Theme

```html
<!-- In _Layout.cshtml -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/material.css" />

<script>
    // Material theme → Spinner type = Material (automatic)
    var target = document.getElementById('container');
    ej.popups.createSpinner({ target: target });
</script>
```

### Bootstrap5 Theme

```html
<!-- In _Layout.cshtml -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/bootstrap5.css" />

<script>
    // Bootstrap5 theme → Spinner type = Bootstrap4 (automatic)
    var target = document.getElementById('container');
    ej.popups.createSpinner({ target: target });
</script>
```

### Fabric Theme

```html
<!-- In _Layout.cshtml -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/fabric.css" />

<script>
    // Fabric theme → Spinner type = Fabric (automatic)
    var target = document.getElementById('container');
    ej.popups.createSpinner({ target: target });
</script>
```

### Fluent Theme

```html
<!-- In _Layout.cshtml -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/fluent.css" />

<script>
    // Fluent theme → Spinner type = Bootstrap4 (automatic)
    var target = document.getElementById('container');
    ej.popups.createSpinner({ target: target });
</script>
```

## Type Configuration

### Complete Configuration Example

```html
<script>
    // Set spinner type and optionally other configurations
    ej.popups.setSpinner({
        type: 'Bootstrap4'  // Type value
    });
    
    // Create spinner after setting type
    var target = document.getElementById('container');
    ej.popups.createSpinner({
        target: target
    });
    
    // Show spinner
    ej.popups.showSpinner(target);
</script>
```

### Switching Types in Your Application

If you need different types in different sections:

```javascript
// Section 1: Use Bootstrap4
ej.popups.setSpinner({ type: 'Bootstrap4' });
ej.popups.createSpinner({ target: section1 });

// Section 2: Use Material (requires resetting)
ej.popups.setSpinner({ type: 'Material' });
ej.popups.createSpinner({ target: section2 });

// Note: Switching types mid-app requires careful state management
```

**Recommendation:** Set type once at app startup based on your selected theme, then use throughout.

### Order of Operations (Critical)

```javascript
// ✅ CORRECT ORDER:
// 1. Set spinner type
ej.popups.setSpinner({ type: 'Material' });

// 2. Create spinner
var target = document.getElementById('container');
ej.popups.createSpinner({ target: target });

// 3. Show spinner
ej.popups.showSpinner(target);


// ❌ INCORRECT - Type will be ignored:
var target = document.getElementById('container');
ej.popups.createSpinner({ target: target });
ej.popups.setSpinner({ type: 'Material' });  // Too late!
ej.popups.showSpinner(target);
```

## Type Consistency

### Within a Single Page

For consistent appearance on a single page, set type once:

```javascript
// app.js - Called once at app startup
ej.popups.setSpinner({ type: 'Bootstrap4' });

// Then throughout your app:
// index.js
ej.popups.createSpinner({ target: document.getElementById('area1') });

// data.js
ej.popups.createSpinner({ target: document.getElementById('area2') });

// Both spinners will use Bootstrap4 type
```

### Across Multiple Pages

Set type in the `_Layout.cshtml` script section:

```html
<!-- _Layout.cshtml -->
<body>
    @RenderBody()
    
    <ejs-scripts></ejs-scripts>
    
    <script>
        // Set globally once
        ej.popups.setSpinner({ type: 'Bootstrap4' });
    </script>
    
    <script src="~/js/app.js"></script>
</body>
```

Now all pages inherit the same spinner type.

## Troubleshooting Type Issues

**Spinner doesn't match theme:**
- Verify CSS theme in `<head>` is loaded
- Check that `setSpinner()` was called before `createSpinner()`
- Inspect element to see actual CSS classes applied

**Type name not recognized:**
- Verify exact capitalization: 'Material', 'Bootstrap4', 'HighContrast'
- Check browser console for errors
- Try removing `setSpinner()` to use automatic detection

**Multiple spinner types on same page:**
- Consider redesign to use single consistent type
- If required, manage state carefully
- Document the reasoning for mixed types
