# Styling and Customization

## Table of Contents
- [Overview](#overview)
- [CSS Customization Pattern](#css-customization-pattern)
- [Material Theme](#material-theme)
- [Fabric Theme](#fabric-theme)
- [Bootstrap Theme](#bootstrap-theme)
- [Bootstrap4 Theme](#bootstrap4-theme)
- [High Contrast Theme](#high-contrast-theme)
- [Common Customizations](#common-customizations)
- [Advanced Styling](#advanced-styling)

## Overview

The Syncfusion Spinner uses theme-specific CSS selectors that allow for deep customization of appearance. You can modify stroke color, size, animation speed, and other visual properties for each theme.

**Key Customization Approach:**
1. Identify the CSS class for your spinner type
2. Write custom CSS rules targeting that class
3. Override specific properties (stroke, fill, opacity)
4. Test across browsers and devices

## CSS Customization Pattern

### Basic Pattern

Every spinner type uses a specific CSS class hierarchy:

```css
/* General spinner pane container */
.e-spinner-pane { }

/* Inner spinner content */
.e-spinner-inner { }

/* Type-specific class */
.e-spin-{type} { }
```

### Adding Custom Styles

Add your custom CSS in a `<style>` block or external stylesheet AFTER the Syncfusion CSS:

```html
<head>
    <!-- Syncfusion CSS first -->
    <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/material.css" />
    
    <!-- Your custom CSS after -->
    <style>
        /* Custom spinner styles */
        .e-spinner-pane .e-spinner-inner .e-spin-material {
            stroke: #2196F3;  /* Custom color */
        }
    </style>
</head>
```

**Important:** Your custom CSS must come AFTER the Syncfusion CSS to have proper precedence.

## Material Theme

Material theme spinner uses a single animated stroke in the center.

### Default CSS Structure

```css
.e-spinner-pane {
    /* Outer container for the spinner overlay */
}

.e-spinner-inner {
    /* Inner wrapper for spinner SVG */
}

.e-spin-material {
    /* The actual animated SVG element */
    stroke: #2196F3;  /* Default blue */
}
```

### Customization Example: Material Color

```html
<style>
    /* Change Material spinner to green */
    .e-spinner-pane .e-spinner-inner .e-spin-material {
        stroke: #4CAF50;
    }
</style>
```

### Material Customization Options

```css
/* Stroke color */
.e-spinner-pane .e-spinner-inner .e-spin-material {
    stroke: #FF5722;  /* Custom stroke color */
}

/* Stroke width (requires viewBox adjustment) */
.e-spinner-pane .e-spinner-inner .e-spin-material {
    stroke-width: 4;
}

/* Opacity/transparency */
.e-spinner-pane .e-spinner-inner .e-spin-material {
    opacity: 0.8;
}

/* Animation speed (if using custom animation) */
.e-spinner-pane .e-spinner-inner .e-spin-material {
    animation-duration: 1.5s;
}
```

## Fabric Theme

Fabric theme uses nested squares with a specific animation pattern.

### Default CSS Structure

```css
.e-spinner-pane {
    /* Outer container */
}

.e-spinner-inner {
    /* Inner wrapper */
}

.e-spin-fabric {
    /* Fabric-style animated SVG */
    stroke: #0078D4;  /* Default Office blue */
}
```

### Customization Example: Fabric Color

```html
<style>
    /* Change Fabric spinner to purple */
    .e-spinner-pane .e-spinner-inner .e-spin-fabric {
        stroke: #9C27B0;
    }
</style>
```

### Fabric Customization Options

```css
/* Stroke color */
.e-spinner-pane .e-spinner-inner .e-spin-fabric {
    stroke: #0078D4;  /* Office blue */
}

/* Additional Fabric elements */
.e-spinner-pane .e-spinner-inner {
    fill: rgba(0, 120, 212, 0.1);  /* Semi-transparent background */
}

/* Opacity */
.e-spinner-pane .e-spinner-inner .e-spin-fabric {
    opacity: 0.9;
}
```

## Bootstrap Theme

Bootstrap theme uses a filled circular design with rotation.

### Default CSS Structure

```css
.e-spinner-pane {
    /* Outer container */
}

.e-spinner-inner {
    /* Inner wrapper */
}

.e-spin-bootstrap {
    /* Bootstrap-style spinner */
    fill: #007BFF;      /* Default bootstrap blue */
    stroke: #007BFF;
}
```

### Customization Example: Bootstrap Color

```html
<style>
    /* Change Bootstrap spinner to teal */
    .e-spinner-pane .e-spinner-inner .e-spin-bootstrap {
        fill: #17A2B8;
        stroke: #17A2B8;
    }
</style>
```

### Bootstrap Customization Options

```css
/* Stroke and fill color */
.e-spinner-pane .e-spinner-inner .e-spin-bootstrap {
    fill: #28A745;      /* Green */
    stroke: #28A745;
}

/* Opacity */
.e-spinner-pane .e-spinner-inner .e-spin-bootstrap {
    opacity: 0.85;
}

/* Filter effects */
.e-spinner-pane .e-spinner-inner .e-spin-bootstrap {
    filter: drop-shadow(0 2px 4px rgba(0,0,0,0.2));
}
```

## Bootstrap4 Theme

Bootstrap4 theme provides an updated Bootstrap 4+ design with enhanced visuals.

### Default CSS Structure

```css
.e-spinner-pane {
    /* Outer container */
}

.e-spinner-inner {
    /* Inner wrapper */
}

.e-spin-bootstrap4 {
    /* Bootstrap4-style spinner */
    stroke: #007BFF;  /* Bootstrap4 blue */
}
```

### Customization Example: Bootstrap4 Color

```html
<style>
    /* Change Bootstrap4 spinner to danger red */
    .e-spinner-pane .e-spinner-inner .e-spin-bootstrap4 {
        stroke: #DC3545;
    }
</style>
```

### Bootstrap4 Customization Options

```css
/* Stroke color */
.e-spinner-pane .e-spinner-inner .e-spin-bootstrap4 {
    stroke: #6610F2;  /* Indigo */
}

/* Stroke width */
.e-spinner-pane .e-spinner-inner .e-spin-bootstrap4 {
    stroke-width: 5;
}

/* Animation speed */
.e-spinner-pane .e-spinner-inner .e-spin-bootstrap4 {
    animation-duration: 0.8s;
}

/* Opacity */
.e-spinner-pane .e-spinner-inner .e-spin-bootstrap4 {
    opacity: 0.95;
}
```

## High Contrast Theme

High Contrast theme uses bold colors for maximum accessibility and visibility.

### Default CSS Structure

```css
.e-spinner-pane {
    /* Outer container */
    background-color: #000;  /* Dark background */
}

.e-spinner-inner {
    /* Inner wrapper */
}

.e-spin-high-contrast {
    /* High contrast spinner SVG */
}

.e-path-arc {
    /* Animated arc path element */
    stroke: #FFFF00;  /* Yellow for high contrast */
}
```

### Customization Example: High Contrast Colors

```html
<style>
    /* Customize high contrast spinner */
    .e-spinner-pane .e-spinner-inner .e-spin-high-contrast .e-path-arc {
        stroke: #00FF00;  /* Bright green */
    }
</style>
```

### High Contrast Customization Options

```css
/* Arc stroke color */
.e-spinner-pane .e-spinner-inner .e-spin-high-contrast .e-path-arc {
    stroke: #FFFFFF;  /* White */
}

/* Background of spinner pane */
.e-spinner-pane.e-spin-high-contrast {
    background-color: #333;
}

/* Stroke width for better visibility */
.e-spinner-pane .e-spinner-inner .e-spin-high-contrast .e-path-arc {
    stroke-width: 3;
}

/* Opacity - typically full for high contrast */
.e-spinner-pane .e-spinner-inner .e-spin-high-contrast {
    opacity: 1;
}
```

## Common Customizations

### 1. Brand Color Spinner

Replace default colors with your brand color:

```html
<style>
    .e-spinner-pane .e-spinner-inner .e-spin-material {
        stroke: #FF6B35;  /* Brand orange */
    }
    
    .e-spinner-pane .e-spinner-inner .e-spin-fabric {
        stroke: #FF6B35;
    }
</style>
```

### 2. Gradient Spinner (Advanced)

Add gradient fill using SVG or CSS:

```html
<style>
    .e-spinner-pane .e-spinner-inner .e-spin-bootstrap4 {
        stroke: url(#spinnerGradient);
    }
</style>

<svg style="display: none;">
    <defs>
        <linearGradient id="spinnerGradient" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" style="stop-color:#FF6B6B;stop-opacity:1" />
            <stop offset="100%" style="stop-color:#4ECDC4;stop-opacity:1" />
        </linearGradient>
    </defs>
</svg>
```

### 3. Larger Spinner

```html
<style>
    .e-spinner-pane .e-spinner-inner {
        transform: scale(1.5);  /* 150% size */
    }
</style>
```

### 4. Faded Spinner (Low Prominence)

```html
<style>
    .e-spinner-pane .e-spinner-inner .e-spin-bootstrap4 {
        opacity: 0.5;  /* Semi-transparent */
        stroke: #ccc;   /* Light color */
    }
</style>
```

### 5. Dark Mode Spinner

```html
<style>
    /* Dark theme spinner adjustments */
    .e-spinner-pane .e-spinner-inner .e-spin-bootstrap4 {
        stroke: #64B5F6;  /* Light blue for dark background */
    }
    
    .e-spinner-pane {
        background-color: rgba(0, 0, 0, 0.7);  /* Semi-transparent dark overlay */
    }
</style>
```

## Advanced Styling

### Using CSS Variables (Modern Approach)

Define theme colors as CSS variables:

```html
<style>
    :root {
        --spinner-color: #2196F3;
        --spinner-opacity: 0.9;
    }
    
    .e-spinner-pane .e-spinner-inner .e-spin-material {
        stroke: var(--spinner-color);
        opacity: var(--spinner-opacity);
    }
</style>

<script>
    // Change spinner color dynamically
    document.documentElement.style.setProperty('--spinner-color', '#FF5722');
</script>
```

### Responsive Spinner Sizing

```html
<style>
    /* Default size */
    .e-spinner-pane .e-spinner-inner {
        transform: scale(1);
    }
    
    /* Tablet size */
    @media (max-width: 768px) {
        .e-spinner-pane .e-spinner-inner {
            transform: scale(1.2);
        }
    }
    
    /* Mobile size */
    @media (max-width: 480px) {
        .e-spinner-pane .e-spinner-inner {
            transform: scale(0.8);
        }
    }
</style>
```

### Theme-Specific Customization

```html
<style>
    /* Light theme */
    body.light-theme .e-spinner-pane .e-spinner-inner .e-spin-bootstrap4 {
        stroke: #007BFF;
    }
    
    /* Dark theme */
    body.dark-theme .e-spinner-pane .e-spinner-inner .e-spin-bootstrap4 {
        stroke: #64B5F6;
    }
</style>
```

### Animation Speed Adjustment

```html
<style>
    @keyframes e-spin-animation {
        from { transform: rotate(0deg); }
        to { transform: rotate(360deg); }
    }
    
    .e-spinner-pane .e-spinner-inner .e-spin-bootstrap4 {
        animation: e-spin-animation 0.6s linear infinite;  /* Faster */
    }
</style>
```

## Troubleshooting Styles

**Styles not applying:**
- Ensure your CSS comes AFTER Syncfusion CSS
- Use browser DevTools to inspect actual applied styles
- Check CSS specificity (add `!important` if necessary)
- Verify spinner element DOM structure

**Colors look wrong:**
- Check theme CSS is loading correctly
- Inspect actual SVG elements in DOM
- Some SVG elements may use `fill` instead of `stroke`
- Test in different browsers for consistency

**Performance issues:**
- Avoid complex filters on spinner elements
- Use `transform: scale()` instead of `width/height`
- Keep animations simple with CSS keyframes
