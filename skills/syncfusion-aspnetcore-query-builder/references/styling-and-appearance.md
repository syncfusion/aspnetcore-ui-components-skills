# Styling and Appearance in QueryBuilder for ASP.NET MVC

## Table of Contents
- [Overview](#overview)
- [CSS Structure](#css-structure)
- [Theme Application](#theme-application)
- [RTL Support](#rtl-support)
- [Responsive Design](#responsive-design)
- [Custom CSS Classes](#custom-css-classes)
- [Dark Mode](#dark-mode)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

Styling QueryBuilder involves:
- **Themes:** Built-in themes (Material, Bootstrap, Fluent)
- **Custom CSS:** Override default styles
- **RTL:** Support for right-to-left languages
- **Responsive:** Adapt to different screen sizes
- **Dark Mode:** Alternative color schemes

## CSS Structure

QueryBuilder uses a hierarchical CSS structure for styling.

### Main CSS Classes

```css
.e-querybuilder {
    /* Main container */
}

.e-query-builder {
    /* Root element */
}

.e-group-header {
    /* Group header section */
}

.e-rule {
    /* Individual rule row */
}

.e-rule-field {
    /* Field selector */
}

.e-rule-operator {
    /* Operator selector */
}

.e-rule-value {
    /* Value input */
}

.e-group-footer {
    /* Group action buttons */
}

.e-btn-add {
    /* Add button */
}

.e-btn-delete {
    /* Delete button */
}
```

## Theme Application

### Option 1: CDN-Based Themes

```html
<head>
    <!-- Material Theme (default) -->
    <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/23.1.36/material.css" />
</head>
```

### Available Themes

| Theme | CDN Link | Best For |
|-------|----------|----------|
| **Fluent** | `fluent.css` | Modern Microsoft design |
| **Material** | `material.css` | Material Design (Google) |
| **Bootstrap** | `bootstrap.css` | Bootstrap 3/4 compatibility |
| **Bootstrap4** | `bootstrap4.css` | Bootstrap 4 specific |
| **Fabric** | `fabric.css` | Microsoft Fabric design |
| **Tailwind** | `tailwind.css` | Tailwind CSS framework |

### Switching Themes Dynamically

```html
<button onclick="switchTheme('material')">Material</button>
<button onclick="switchTheme('bootstrap')">Bootstrap</button>
<button onclick="switchTheme('fabric')">Fabric</button>

<script>
function switchTheme(themeName) {
    // Remove current theme
    var currentTheme = document.querySelector('link[rel="stylesheet"][href*="ej2"]');
    if (currentTheme) {
        currentTheme.remove();
    }
    
    // Add new theme
    var themeLink = document.createElement('link');
    themeLink.rel = 'stylesheet';
    themeLink.href = `https://cdn.syncfusion.com/ej2/23.1.36/${themeName}.css`;
    document.head.appendChild(themeLink);
}
</script>
```

## RTL Support

Enable Right-to-Left (RTL) text direction for Arabic, Hebrew, Persian, Urdu languages.

### Example: Enable RTL

```html
<div dir="rtl">
    @Html.EJS().QueryBuilder("querybuilder")
        .Columns(columns)
        .DataSource(data)
        .EnableRtl(true)
        .Render()
</div>

<style>
/* RTL-specific adjustments */
[dir="rtl"] .e-querybuilder {
    text-align: right;
    direction: rtl;
}

[dir="rtl"] .e-rule {
    flex-direction: row-reverse;
}

[dir="rtl"] .e-btn-delete {
    margin-left: 0;
    margin-right: 10px;
}
</style>
```

### RTL with Localization

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(data)
    .EnableRtl(true)
    .Locale("ar-SA") // Arabic locale
    .Render()
```

## Responsive Design

Adapt QueryBuilder layout for different screen sizes.

### Example: Responsive Layout

```html
<div id="querybuilder-container">
    @Html.EJS().QueryBuilder("querybuilder")
        .Columns(columns)
        .DataSource(data)
        .Width("100%")
        .Render()
</div>

<style>
/* Desktop (1200px+) */
@media (min-width: 1200px) {
    #querybuilder-container {
        max-width: 1000px;
        margin: 0 auto;
    }
    
    .e-rule {
        display: flex;
        gap: 10px;
    }
}

/* Tablet (768px - 1199px) */
@media (min-width: 768px) and (max-width: 1199px) {
    .e-rule {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 10px;
    }
    
    .e-rule-actions {
        grid-column: 1 / -1;
        text-align: right;
    }
}

/* Mobile (< 768px) */
@media (max-width: 767px) {
    #querybuilder-container {
        padding: 0 10px;
    }
    
    .e-rule {
        display: block;
    }
    
    .e-rule-field,
    .e-rule-operator,
    .e-rule-value,
    .e-rule-actions {
        width: 100%;
        margin-bottom: 10px;
    }
    
    .e-group-header select {
        width: 100%;
    }
}
```

## Custom CSS Classes

Override default styles with custom CSS.

### Example: Custom Styling

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(data)
    .CssClass("custom-querybuilder")
    .Render()

<style>
/* Custom theme */
.custom-querybuilder {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    padding: 20px;
    border-radius: 8px;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.custom-querybuilder .e-group-header {
    background: rgba(255, 255, 255, 0.1);
    border-bottom: 2px solid rgba(255, 255, 255, 0.2);
    padding: 15px;
    border-radius: 4px 4px 0 0;
    color: white;
}

.custom-querybuilder .e-rule {
    background: white;
    padding: 15px;
    margin-bottom: 10px;
    border-radius: 4px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    transition: all 0.3s ease;
}

.custom-querybuilder .e-rule:hover {
    box-shadow: 0 4px 16px rgba(0, 0, 0, 0.15);
}

.custom-querybuilder .e-rule-field select,
.custom-querybuilder .e-rule-operator select,
.custom-querybuilder .e-rule-value input {
    border: 2px solid #ddd;
    border-radius: 4px;
    padding: 10px;
    font-size: 14px;
}

.custom-querybuilder .e-rule-field select:focus,
.custom-querybuilder .e-rule-operator select:focus,
.custom-querybuilder .e-rule-value input:focus {
    border-color: #667eea;
    outline: none;
    box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
}

.custom-querybuilder .e-btn-add {
    background: #667eea;
    color: white;
    border: none;
    padding: 10px 20px;
    border-radius: 4px;
    cursor: pointer;
    font-weight: 600;
    transition: background 0.3s;
}

.custom-querybuilder .e-btn-add:hover {
    background: #5568d3;
}

.custom-querybuilder .e-btn-delete {
    background: #f56565;
    color: white;
    border: none;
    padding: 8px 12px;
    border-radius: 4px;
    cursor: pointer;
}

.custom-querybuilder .e-btn-delete:hover {
    background: #e53e3e;
}
</style>
```

## Dark Mode

Implement dark mode styling for better UX in low-light environments.

### Example: Dark Mode Theme

```html
<button onclick="toggleDarkMode()">🌙 Dark Mode</button>

<div id="querybuilder-container">
    @Html.EJS().QueryBuilder("querybuilder")
        .Columns(columns)
        .DataSource(data)
        .Render()
</div>

<style>
/* Light mode (default) */
#querybuilder-container {
    --bg-primary: #ffffff;
    --bg-secondary: #f5f5f5;
    --text-primary: #333333;
    --text-secondary: #666666;
    --border-color: #dddddd;
    --accent-color: #007bff;
}

#querybuilder-container .e-querybuilder {
    background: var(--bg-primary);
    color: var(--text-primary);
}

#querybuilder-container .e-rule {
    background: var(--bg-secondary);
    border: 1px solid var(--border-color);
}

#querybuilder-container .e-rule input,
#querybuilder-container .e-rule select {
    background: var(--bg-primary);
    color: var(--text-primary);
    border: 1px solid var(--border-color);
}

/* Dark mode */
#querybuilder-container.dark-mode {
    --bg-primary: #1e1e1e;
    --bg-secondary: #2d2d2d;
    --text-primary: #e0e0e0;
    --text-secondary: #b0b0b0;
    --border-color: #404040;
    --accent-color: #4a9eff;
}

#querybuilder-container.dark-mode .e-querybuilder {
    background: var(--bg-primary);
    color: var(--text-primary);
}

#querybuilder-container.dark-mode .e-rule {
    background: var(--bg-secondary);
    border: 1px solid var(--border-color);
}

#querybuilder-container.dark-mode .e-rule input,
#querybuilder-container.dark-mode .e-rule select {
    background: var(--bg-primary);
    color: var(--text-primary);
    border: 1px solid var(--border-color);
}

#querybuilder-container.dark-mode .e-rule input::placeholder {
    color: var(--text-secondary);
}
</style>

<script>
function toggleDarkMode() {
    var container = document.getElementById("querybuilder-container");
    container.classList.toggle("dark-mode");
    
    // Persist preference
    localStorage.setItem("darkMode", container.classList.contains("dark-mode"));
}

// Apply saved preference on load
window.addEventListener("DOMContentLoaded", function() {
    if (localStorage.getItem("darkMode") === "true") {
        document.getElementById("querybuilder-container").classList.add("dark-mode");
    }
});
</script>
```

## Common Patterns

### Pattern 1: Custom Theme with CSS Variables

```css
:root {
    --primary-color: #007bff;
    --primary-light: #e7f3ff;
    --primary-dark: #0056b3;
    --success-color: #28a745;
    --danger-color: #dc3545;
    --warning-color: #ffc107;
}

.e-querybuilder {
    --qb-header-bg: var(--primary-color);
    --qb-header-text: white;
    --qb-rule-bg: white;
    --qb-rule-border: 1px solid #ddd;
    --qb-button-bg: var(--primary-color);
    --qb-button-text: white;
}

.e-querybuilder .e-btn-add {
    background: var(--qb-button-bg);
    color: var(--qb-button-text);
}

.e-querybuilder .e-btn-delete {
    background: var(--danger-color);
    color: white;
}
```

### Pattern 2: Minimal Clean Design

```css
.e-querybuilder {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
    background: #fafbfc;
    border: 1px solid #e1e4e8;
    border-radius: 6px;
    padding: 16px;
}

.e-querybuilder .e-rule {
    background: white;
    border: 1px solid #e1e4e8;
    border-radius: 4px;
    padding: 12px;
    margin-bottom: 8px;
}

.e-querybuilder .e-rule input,
.e-querybuilder .e-rule select {
    border: 1px solid #e1e4e8;
    border-radius: 4px;
    padding: 8px 12px;
    font-size: 13px;
}

.e-querybuilder .e-btn-add,
.e-querybuilder .e-btn-delete {
    background: none;
    border: 1px solid #d1d5da;
    color: #24292e;
    padding: 6px 12px;
    border-radius: 4px;
    font-size: 12px;
    font-weight: 500;
    cursor: pointer;
}

.e-querybuilder .e-btn-add:hover {
    background: #fafbfc;
    border-color: #d1d5da;
}

.e-querybuilder .e-btn-delete:hover {
    background: #ffeef0;
    border-color: #d1d5da;
    color: #cb2431;
}
```

## Troubleshooting

### Issue: "Theme CSS not loading"
**Cause:** Wrong CDN URL or version mismatch  
**Solution:**
1. Verify CDN URL matches EJ2 script version
2. Check network tab for 404 errors
3. Use absolute URLs, not relative paths

### Issue: "RTL text not displaying correctly"
**Cause:** `EnableRtl` not set or HTML `dir` attribute missing  
**Solution:**
1. Set `EnableRtl(true)` in QueryBuilder
2. Add `dir="rtl"` to parent container
3. Verify language locale is set (e.g., "ar-SA")

### Issue: "Custom CSS not applied"
**Cause:** CSS specificity or selector incorrect  
**Solution:**
1. Use browser DevTools to inspect elements
2. Increase CSS specificity if needed
3. Check CSS file is loaded after theme CSS
4. Use `!important` as last resort

### Issue: "Responsive layout breaks on mobile"
**Cause:** Viewport meta tag missing or breakpoints too wide  
**Solution:**
1. Add `<meta name="viewport" content="width=device-width, initial-scale=1.0">`
2. Adjust breakpoints for device sizes
3. Test on actual devices, not just browser emulation

---

## Next Steps

- Review [Accessibility and Localization](accessibility-and-localization.md) for i18n support
- Explore [Templates and Customization](templates-and-customization.md) for UI customization
- Check [Advanced Features](advanced-features.md) for import/export options
