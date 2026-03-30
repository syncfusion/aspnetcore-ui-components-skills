# Styling and Customization

## Table of Contents
- [Overview](#overview)
- [CSS Classes](#css-classes)
- [Built-in Themes](#built-in-themes)
- [Custom CSS Styling](#custom-css-styling)
- [Size Customization](#size-customization)
- [Color Customization](#color-customization)
- [Theme Studio Integration](#theme-studio-integration)
- [Advanced Styling](#advanced-styling)
- [Responsive Design](#responsive-design)
- [Troubleshooting](#troubleshooting)

## Overview

The Syncfusion Switch component uses CSS classes and variables for styling. You can customize:

- **Colors** - Handle, track, and active state colors
- **Size** - Width, height, and handle dimensions
- **States** - Hover, active, disabled, focused states
- **Appearance** - Rounded corners, borders, shadows

**Three Customization Approaches:**
1. **Built-in Themes** - Pre-made themes (Material, Bootstrap, Fabric)
2. **Custom CSS** - Override default styles with your own CSS
3. **Theme Studio** - Visual editor to generate custom themes

## CSS Classes

### Switch Structure Classes

The Switch component consists of these HTML elements:

```html
<div class="e-switch-wrapper">           <!-- Container -->
    <input type="checkbox" class="e-switch-input" />
    <span class="e-switch-inner"></span>  <!-- Track/background -->
    <span class="e-switch-handle"></span> <!-- Draggable circle -->
</div>
```

### CSS Classes Reference

| CSS Class | Purpose | Use For |
|-----------|---------|---------|
| `.e-switch-wrapper` | Container wrapper | Overall switch size, positioning |
| `.e-switch-inner` | Track background | Track color, width, height |
| `.e-switch-handle` | Draggable circle | Handle color, size, border |
| `.e-switch-input` | Hidden checkbox | Not typically styled |
| `.e-switch-active` | Applied when checked | ON state styling |
| `.e-switch-on` | Applied to inner when ON | Track color in ON state |
| `.e-switch-disabled` | Applied when disabled | Disabled state styling |
| `.e-switch-label` | Label text styling | Label appearance |

### State-Based Classes

```css
/* Off state (unchecked) */
.e-switch-wrapper .e-switch-inner {}
.e-switch-wrapper .e-switch-handle {}

/* On state (checked) */
.e-switch-wrapper.e-switch-active .e-switch-inner {}
.e-switch-wrapper .e-switch-handle.e-switch-active {}
.e-switch-wrapper .e-switch-on {}

/* Disabled state */
.e-switch-wrapper.e-switch-disabled {}
.e-switch-wrapper.e-switch-disabled .e-switch-handle {}

/* Hover state */
.e-switch-wrapper:hover .e-switch-handle {}

/* Focus state */
.e-switch-wrapper:focus .e-switch-handle {}
```

## Built-in Themes

### Available Themes

Syncfusion provides pre-designed themes. Switch the theme by changing the CDN CSS URL.

**Material Theme** (Default)
```html
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/22.1.34/material.css" />
```

**Bootstrap 5 Theme**
```html
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/22.1.34/bootstrap5.css" />
```

**Bootstrap 4 Theme**
```html
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/22.1.34/bootstrap.css" />
```

**Fabric Theme** (Microsoft)
```html
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/22.1.34/fabric.css" />
```

**High Contrast Theme**
```html
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/22.1.34/highcontrast.css" />
```

**Fluent Theme**
```html
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/22.1.34/fluent.css" />
```

### Applying Themes

Update the CSS link in `_Layout.cshtml`:

```html
<head>
    <!-- Change one line to switch themes -->
    <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/22.1.34/bootstrap5.css" />
</head>
```

All Syncfusion components on the page automatically use the selected theme.

## Custom CSS Styling

### Basic Color Customization

Override the default colors with your own:

```css
/* Change track color when OFF (default: light gray) */
.e-switch-wrapper .e-switch-inner {
    background-color: #e0e0e0;
}

/* Change track color when ON (default: blue) */
.e-switch-wrapper .e-switch-on {
    background-color: #4CAF50;  /* Green */
}

/* Change handle color when OFF */
.e-switch-wrapper .e-switch-handle {
    background-color: #ffffff;
    border-color: #999999;
}

/* Change handle color when ON */
.e-switch-wrapper .e-switch-handle.e-switch-active {
    background-color: #ffffff;
    border-color: #4CAF50;
}
```

### Complete Custom Styling Example

```css
/* Custom switch styling */
.custom-switch.e-switch-wrapper {
    /* Size customization */
    width: 60px;
    height: 34px;
}

/* Track styles */
.custom-switch .e-switch-inner {
    background-color: #cccccc;  /* OFF track color */
    border-radius: 17px;
    width: 100%;
    height: 100%;
}

.custom-switch .e-switch-on {
    background-color: #2196F3;  /* ON track color (blue) */
}

/* Handle styles */
.custom-switch .e-switch-handle {
    background-color: #ffffff;
    border: 2px solid #999999;
    border-radius: 50%;
    width: 30px;
    height: 30px;
    top: 2px;
}

.custom-switch .e-switch-handle.e-switch-active {
    border-color: #2196F3;
    right: 2px;
}

/* Hover effects */
.custom-switch:hover .e-switch-handle {
    box-shadow: 0 0 8px rgba(33, 150, 243, 0.3);
}

/* Disabled state */
.custom-switch.e-switch-disabled .e-switch-inner {
    background-color: #f5f5f5;
    opacity: 0.5;
}

.custom-switch.e-switch-disabled .e-switch-handle {
    background-color: #eeeeee;
}
```

### Apply Custom Style to Switch

```cshtml
<!-- Reference your custom CSS class -->
<ejs-switch id="customSwitch" cssClass="custom-switch"></ejs-switch>
```

## Size Customization

### Small Switch

```css
.small-switch.e-switch-wrapper {
    width: 45px;
    height: 24px;
}

.small-switch .e-switch-handle {
    width: 20px;
    height: 20px;
    top: 2px;
}

.small-switch .e-switch-handle.e-switch-active {
    right: 2px;
}
```

### Large Switch

```css
.large-switch.e-switch-wrapper {
    width: 75px;
    height: 44px;
}

.large-switch .e-switch-inner {
    font-size: 16px;
}

.large-switch .e-switch-handle {
    width: 40px;
    height: 40px;
    top: 2px;
}

.large-switch .e-switch-handle.e-switch-active {
    right: 2px;
}
```

### Usage

```cshtml
<ejs-switch id="small" cssClass="small-switch"></ejs-switch>
<ejs-switch id="large" cssClass="large-switch"></ejs-switch>
```

## Color Customization

### Brand Color Switches

```css
/* Red switch (destructive action) */
.danger-switch .e-switch-on {
    background-color: #f44336;
}

.danger-switch .e-switch-handle.e-switch-active {
    border-color: #f44336;
}

/* Green switch (positive action) */
.success-switch .e-switch-on {
    background-color: #4CAF50;
}

.success-switch .e-switch-handle.e-switch-active {
    border-color: #4CAF50;
}

/* Orange switch (warning) */
.warning-switch .e-switch-on {
    background-color: #FF9800;
}

.warning-switch .e-switch-handle.e-switch-active {
    border-color: #FF9800;
}

/* Purple switch (premium) */
.premium-switch .e-switch-on {
    background-color: #9C27B0;
}

.premium-switch .e-switch-handle.e-switch-active {
    border-color: #9C27B0;
}
```

### Usage

```cshtml
<!-- Delete feature (red) -->
<ejs-switch id="deleteData" cssClass="danger-switch"></ejs-switch>

<!-- Enable feature (green) -->
<ejs-switch id="enableFeature" cssClass="success-switch"></ejs-switch>

<!-- Premium feature (purple) -->
<ejs-switch id="premiumFeature" cssClass="premium-switch"></ejs-switch>
```

## Theme Studio Integration

### Generate Custom Theme

Syncfusion provides **Theme Studio** for visual customization:

1. **Visit:** https://ej2.syncfusion.com/themestudio/
2. **Choose:** Base theme (Material, Bootstrap, etc.)
3. **Customize:** Colors, fonts, sizes visually
4. **Download:** Generated CSS file
5. **Include:** In your project

### Using Theme Studio CSS

```html
<head>
    <!-- Downloaded custom theme from Theme Studio -->
    <link rel="stylesheet" href="~/css/custom-theme.css" />
</head>
```

### Advantages of Theme Studio
- Visual editor (no CSS knowledge needed)
- Consistent theming across all components
- Export CSS for use in projects
- Easy to maintain and update

## Advanced Styling

### Gradient Background

```css
.gradient-switch .e-switch-on {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

.gradient-switch .e-switch-inner {
    background: linear-gradient(135deg, #f5f5f5 0%, #e0e0e0 100%);
}
```

### Shadow Effects

```css
.shadow-switch.e-switch-wrapper {
    filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.1));
}

.shadow-switch:hover .e-switch-handle {
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}
```

### Smooth Animations

```css
/* Smooth handle animation */
.animated-switch .e-switch-handle {
    transition: all 0.3s ease-in-out;
}

.animated-switch .e-switch-inner {
    transition: background-color 0.3s ease-in-out;
}
```

### Glass Morphism Effect

```css
.glass-switch .e-switch-wrapper {
    background: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.2);
    border-radius: 20px;
}

.glass-switch .e-switch-inner {
    background: rgba(255, 255, 255, 0.2);
}
```

## Responsive Design

### Mobile-Optimized Switch

```css
/* Default (desktop) */
.responsive-switch.e-switch-wrapper {
    width: 60px;
    height: 34px;
}

.responsive-switch .e-switch-handle {
    width: 30px;
    height: 30px;
}

/* Tablets */
@media (max-width: 768px) {
    .responsive-switch.e-switch-wrapper {
        width: 55px;
        height: 32px;
    }
    
    .responsive-switch .e-switch-handle {
        width: 28px;
        height: 28px;
    }
}

/* Mobile phones */
@media (max-width: 480px) {
    .responsive-switch.e-switch-wrapper {
        width: 50px;
        height: 30px;
    }
    
    .responsive-switch .e-switch-handle {
        width: 26px;
        height: 26px;
    }
    
    .responsive-switch .e-switch-label {
        font-size: 14px;
    }
}
```

### Usage

```cshtml
<ejs-switch id="responsive" cssClass="responsive-switch"></ejs-switch>
```

### Container-Based Responsive Design

```css
/* Full-width container */
.container .responsive-switch {
    width: 100%;
}

/* Adjust switch based on container width */
@media (max-width: 600px) {
    .mobile-container .responsive-switch.e-switch-wrapper {
        width: 50px;
    }
}
```

## Complete Styling Example

### HTML/Razor

```cshtml
<div class="settings-panel">
    <div class="setting-row">
        <span class="setting-label">Notifications</span>
        <ejs-switch id="notif" cssClass="modern-switch success-switch"></ejs-switch>
    </div>
    
    <div class="setting-row">
        <span class="setting-label">Dark Mode</span>
        <ejs-switch id="darkMode" cssClass="modern-switch"></ejs-switch>
    </div>
    
    <div class="setting-row">
        <span class="setting-label">Data Collection</span>
        <ejs-switch id="dataCollection" cssClass="modern-switch warning-switch"></ejs-switch>
    </div>
</div>
```

### CSS

```css
.settings-panel {
    max-width: 500px;
    padding: 20px;
    background: #f9f9f9;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.setting-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 16px 0;
    border-bottom: 1px solid #e0e0e0;
}

.setting-row:last-child {
    border-bottom: none;
}

.setting-label {
    font-weight: 500;
    color: #333;
}

/* Modern switch styling */
.modern-switch.e-switch-wrapper {
    width: 60px;
    height: 34px;
    border-radius: 17px;
}

.modern-switch .e-switch-inner {
    border-radius: 17px;
    background-color: #ccc;
}

.modern-switch .e-switch-handle {
    width: 30px;
    height: 30px;
    border-radius: 50%;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
    top: 2px;
}

.modern-switch .e-switch-handle.e-switch-active {
    right: 2px;
}

.modern-switch .e-switch-on {
    background-color: #2196F3;
}

.modern-switch:hover .e-switch-handle {
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
}

/* Success variant (green) */
.success-switch .e-switch-on {
    background-color: #4CAF50;
}

.success-switch .e-switch-handle.e-switch-active {
    border-color: #4CAF50;
}

/* Warning variant (orange) */
.warning-switch .e-switch-on {
    background-color: #FF9800;
}

.warning-switch .e-switch-handle.e-switch-active {
    border-color: #FF9800;
}
```

## Troubleshooting

### Issue: Custom CSS not applying
**Cause:** CSS specificity too low or conflicting styles
**Solution:**
```css
/* Use more specific selector */
.e-switch-wrapper.custom-switch .e-switch-inner {
    background-color: red !important;
}
```

### Issue: Colors look different in different themes
**Cause:** Theme CSS overriding custom styles
**Solution:**
```css
/* Load custom CSS AFTER theme CSS */
<!-- In _Layout.cshtml -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/22.1.34/material.css" />
<link rel="stylesheet" href="~/css/custom-switch-styles.css" /> <!-- Your custom CSS last -->
```

### Issue: Handle doesn't move smoothly
**Cause:** Missing transition CSS
**Solution:**
```css
.e-switch-handle {
    transition: all 0.3s ease;
}

.e-switch-inner {
    transition: background-color 0.3s ease;
}
```

### Issue: Disabled switch still appears clickable
**Cause:** Disabled styles not specific enough
**Solution:**
```css
.e-switch-wrapper.e-switch-disabled {
    opacity: 0.5;
    cursor: not-allowed;
}

.e-switch-wrapper.e-switch-disabled .e-switch-handle {
    cursor: not-allowed;
}
```

## Next Steps

- Handle switch events: [Switch Events Reference](../switch-events.md)
- Configure properties: [Switch Properties](../switch-properties.md)
- Setup guide: [Getting Started](../getting-started.md)
