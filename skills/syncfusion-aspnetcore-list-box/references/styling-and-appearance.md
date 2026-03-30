# Styling and Appearance

## Table of Contents
- [Overview](#overview)
- [CSS Class Customization](#css-class-customization)
- [Built-in Themes](#built-in-themes)
- [Theme Studio Integration](#theme-studio-integration)
- [Horizontal ListBox Layout](#horizontal-listbox-layout)
- [Custom CSS Rules](#custom-css-rules)
- [Advanced Styling Patterns](#advanced-styling-patterns)

## Overview

The Syncfusion ListBox provides multiple styling approaches:

- **Built-in Themes** - Use pre-designed themes via CDN
- **CSS Classes** - Override default styles with custom CSS
- **Theme Studio** - Generate custom themes graphically
- **cssClass Property** - Apply custom classes to the component
- **Templates** - Completely customize item rendering (see Features)

## CSS Class Customization

### Target ListBox Elements

The ListBox generates specific CSS classes you can override:

```css
/* ListBox wrapper container */
.e-listbox-wrapper { }

/* List items container */
.e-list-parent { }

/* Individual list item */
.e-list-parent .e-list-item { }

/* List item on hover */
.e-list-parent .e-list-item:hover { }

/* Selected list item */
.e-list-parent .e-list-item.e-selected { }

/* Toolbar wrapper (for dual list) */
.e-listboxtool-wrapper { }

/* Toolbar buttons */
.e-listboxtool-wrapper .e-listbox-tool .e-btn { }

/* Toolbar icons */
.e-listboxtool-wrapper .e-listbox-tool .e-btn .e-btn-icon { }
```

### Example: Custom Item Styling

```cshtml
<!-- CSHTML -->
<ejs-listbox id="customListBox" 
             dataSource="@Model.Items"
             cssClass="custom-listbox">
</ejs-listbox>

<!-- CSS in site.css -->
<style>
    .custom-listbox .e-list-item {
        padding: 12px 16px;
        font-size: 14px;
        border-bottom: 1px solid #e0e0e0;
        transition: background-color 0.2s;
    }
    
    .custom-listbox .e-list-item:hover {
        background-color: #f5f5f5;
        cursor: pointer;
    }
    
    .custom-listbox .e-list-item.e-selected {
        background-color: #2196F3;
        color: white;
        border-left: 4px solid #1976D2;
    }
    
    .custom-listbox .e-listbox-wrapper {
        border: 1px solid #d0d0d0;
        border-radius: 4px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
</style>
```

## Built-in Themes

### Apply Theme via CDN

Themes are loaded via stylesheet. Change the CSS file in `_Layout.cshtml`:

```cshtml
<!-- Material Theme (default) -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/25.1.35/material.css" />

<!-- OR Bootstrap 5 Theme -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/25.1.35/bootstrap5.css" />

<!-- OR Tailwind Theme -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/25.1.35/tailwind.css" />

<!-- OR High Contrast (Accessibility) -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/25.1.35/highcontrast.css" />
```

### Available Themes

| Theme | File | Best For |
|-------|------|----------|
| **Material** | `material.css` | Modern, Material Design look |
| **Bootstrap4** | `bootstrap4.css` | Bootstrap 4 consistency |
| **Bootstrap5** | `bootstrap5.css` | Bootstrap 5 consistency |
| **Fluent** | `fluent.css` | Microsoft Fluent Design |
| **Tailwind** | `tailwind.css` | Tailwind CSS aesthetic |
| **HighContrast** | `highcontrast.css` | Accessibility-focused |
| **Fabric** | `fabric.css` | Office Fabric UI |

### Theme Switching (Runtime)

```html
<div class="theme-selector">
    <button onclick="switchTheme('material')">Material</button>
    <button onclick="switchTheme('bootstrap5')">Bootstrap 5</button>
    <button onclick="switchTheme('tailwind')">Tailwind</button>
</div>

<script>
    function switchTheme(theme) {
        // Get current stylesheet
        var link = document.querySelector('link[href*="cdn.syncfusion.com"]');
        
        // Replace with new theme
        link.href = `https://cdn.syncfusion.com/ej2/25.1.35/${theme}.css`;
    }
</script>
```

## Theme Studio Integration

### Generate Custom Theme

1. Visit [Syncfusion Theme Studio](https://ej2.syncfusion.com/themestudio/?theme=material)
2. Customize colors, fonts, sizes
3. Export as CSS file
4. Include in your project

### Use Exported Theme

```cshtml
<!-- In _Layout.cshtml -->
<head>
    <!-- Your custom theme from Theme Studio -->
    <link rel="stylesheet" href="~/css/custom-theme.css" />
</head>
```

**Order matters:** Place custom theme AFTER base theme:

```cshtml
<head>
    <!-- Base theme (required) -->
    <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/25.1.35/material.css" />
    
    <!-- Your overrides on top -->
    <link rel="stylesheet" href="~/css/custom-theme.css" />
    <link rel="stylesheet" href="~/css/site.css" />
</head>
```

## Horizontal ListBox Layout

Display items horizontally instead of vertically.

### Implementation

```csharp
// Page Model
public List<Color> AvailableColors { get; set; }

public void OnGet()
{
    AvailableColors = new List<Color>
    {
        new Color { Id = 1, Name = "Red" },
        new Color { Id = 2, Name = "Green" },
        new Color { Id = 3, Name = "Blue" },
        new Color { Id = 4, Name = "Yellow" }
    };
}
```

```cshtml
<!-- Horizontal ListBox with custom CSS -->
<ejs-listbox id="colorListBox" 
             dataSource="@Model.AvailableColors"
             fields="@(new Syncfusion.EJ2.DropDowns.ListBoxFieldSettings { Text = "Name", Value = "Id" })"
             cssClass="horizontal-listbox">
</ejs-listbox>

<style>
    .horizontal-listbox {
        width: 100%;
    }
    
    /* Make list items display inline */
    .horizontal-listbox .e-list-parent {
        display: flex;
        flex-wrap: wrap;
        gap: 8px;
    }
    
    .horizontal-listbox .e-list-item {
        display: inline-block;
        flex: 0 1 auto;
        padding: 8px 16px;
        border: 1px solid #d0d0d0;
        border-radius: 4px;
        cursor: pointer;
        text-align: center;
        min-width: 80px;
        transition: all 0.2s;
    }
    
    .horizontal-listbox .e-list-item:hover {
        background-color: #f0f0f0;
        border-color: #999;
    }
    
    .horizontal-listbox .e-list-item.e-selected {
        background-color: #2196F3;
        color: white;
        border-color: #1976D2;
    }
</style>
```

**Result:** Items display in a horizontal row instead of vertical list.

### Horizontal Grid Layout

For product selection or color swatches:

```css
.horizontal-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
    gap: 12px;
}

.horizontal-grid .e-list-item {
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: 60px;
    border: 2px solid transparent;
}

.horizontal-grid .e-list-item.e-selected {
    border: 2px solid #2196F3;
    background-color: #E3F2FD;
}
```

## Custom CSS Rules

### Spacing and Padding

```css
.custom-listbox {
    margin-bottom: 20px;
}

.custom-listbox .e-list-item {
    padding: 12px 20px;      /* More spacious items */
    margin: 2px 0;
    line-height: 1.6;
}
```

### Typography

```css
.custom-listbox .e-list-item {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-size: 15px;
    font-weight: 500;
    letter-spacing: 0.3px;
}

.custom-listbox .e-list-item:hover {
    font-weight: 600;
}
```

### Colors and Backgrounds

```css
.custom-listbox .e-listbox-wrapper {
    background-color: #fafafa;
    border: 2px solid #e0e0e0;
}

.custom-listbox .e-list-item {
    background-color: white;
    color: #333;
}

.custom-listbox .e-list-item:hover {
    background-color: #f9f9f9;
    color: #000;
}

.custom-listbox .e-list-item.e-selected {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
}
```

### Borders and Shadows

```css
.custom-listbox .e-listbox-wrapper {
    border-radius: 8px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    border: none;
}

.custom-listbox .e-list-item {
    border-left: 3px solid transparent;
    transition: all 0.3s ease;
}

.custom-listbox .e-list-item.e-selected {
    border-left-color: #2196F3;
    box-shadow: inset 0 0 8px rgba(33, 150, 243, 0.1);
}
```

## Advanced Styling Patterns

### Pattern 1: Status-Based Item Coloring

```csharp
public class Task
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Status { get; set; }  // "completed", "pending", "urgent"
}
```

```cshtml
<ejs-listbox id="taskListBox" 
             dataSource="@Model.Tasks"
             fields="@(new Syncfusion.EJ2.DropDowns.ListBoxFieldSettings { Text = "Title", Value = "Id" })"
             change="onTaskSelect"
             itemTemplate="<span class='task-${Status}'>${Title}</span>">
</ejs-listbox>

<style>
    .task-completed {
        color: #4caf50;
        text-decoration: line-through;
    }
    
    .task-pending {
        color: #ff9800;
        font-weight: 500;
    }
    
    .task-urgent {
        color: #f44336;
        font-weight: bold;
    }
</style>
```

### Pattern 2: Icon + Text with Custom Styling

```cshtml
<ejs-listbox id="menuListBox" 
             dataSource="@Model.MenuItems"
             itemTemplate="@Html.Raw("<span class='menu-icon ${IconClass}'></span><span>${Title}</span>")">
</ejs-listbox>

<style>
    #menuListBox .e-list-item {
        display: flex;
        align-items: center;
        gap: 12px;
        padding: 10px;
    }
    
    .menu-icon {
        display: inline-block;
        width: 20px;
        height: 20px;
        background-size: contain;
    }
    
    #menuListBox .e-list-item:hover {
        background-color: #ecf0f1;
        border-left: 3px solid #3498db;
    }
</style>
```

### Pattern 3: Responsive Styling

```css
/* Desktop: Multi-column layout */
@media (min-width: 768px) {
    .responsive-listbox {
        max-width: 500px;
    }
    
    .responsive-listbox .e-list-parent {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
    }
}

/* Tablet: Single column, larger items */
@media (min-width: 481px) and (max-width: 767px) {
    .responsive-listbox .e-list-item {
        padding: 14px;
        font-size: 15px;
    }
}

/* Mobile: Touch-friendly sizing */
@media (max-width: 480px) {
    .responsive-listbox .e-list-item {
        padding: 16px;
        font-size: 16px;
        min-height: 50px;
    }
}
```

## Next Steps

- **Selection** → Apply styling based on selection in [references/selection-modes.md](selection-modes.md)
- **Features** → Use templates for advanced customization in [references/features.md](features.md)
- **Data Binding** → Bind style-related data in [references/data-binding.md](data-binding.md)
- **Common Tasks** → Practical styling examples in [references/howto-common-tasks.md](howto-common-tasks.md)
