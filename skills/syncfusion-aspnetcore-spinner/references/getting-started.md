# Getting Started with ASP.NET Core Spinner

## Table of Contents
- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Project Setup](#project-setup)
- [Adding Resources](#adding-resources)
- [Creating Your First Spinner](#creating-your-first-spinner)
- [Showing and Hiding](#showing-and-hiding)
- [Targeting Elements](#targeting-elements)
- [Complete Example](#complete-example)

## Overview

The Syncfusion Spinner component requires proper setup of stylesheets and scripts before use. This guide walks you through the complete setup process for a new ASP.NET Core project.

## Prerequisites

Before implementing the Spinner, ensure:
- **ASP.NET Core SDK** installed (version 6.0 or later recommended)
- **Visual Studio** or Visual Studio Code with ASP.NET Core extension
- **Razor Pages** project created or access to an existing ASP.NET Core application
- Basic understanding of Razor markup and JavaScript

## Project Setup

### Create ASP.NET Core Web Application

**Using Visual Studio:**
1. Create new project → ASP.NET Core Web App (Model-View-Controller) or Razor Pages
2. Select .NET version (6.0 or later)
3. Configure HTTPS and authentication as needed
4. Create project

**Using Command Line:**
```bash
dotnet new webapp -n MyApp
cd MyApp
dotnet run
```

This creates a project with the basic Razor pages structure needed for Syncfusion components.

## Adding Resources

### Step 1: Add Stylesheet

The stylesheet must be referenced in the `<head>` of your layout file. Open `~/Pages/Shared/_Layout.cshtml`:

```html
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>@ViewData["Title"] - MyApp</title>
    <link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.min.css" />
    <link rel="stylesheet" href="~/css/site.css" />
    
    <!-- Add Syncfusion CSS -->
    <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/fluent.css" />
</head>
```

**Available Themes:**
- `fluent.css` - Microsoft Fluent Design (recommended)
- `bootstrap5.css` - Bootstrap 5 theme
- `material.css` - Material Design theme
- `fabric.css` - Office Fabric theme

### Step 2: Add Scripts

Scripts must be referenced in the `<body>`, typically at the end before the closing tag:

```html
<body>
    <!-- Your page content here -->
    
    <!-- Syncfusion Script Manager (REQUIRED) -->
    <ejs-scripts></ejs-scripts>
    
    <!-- OR manual script reference -->
    <script src="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/dist/ej2.min.js"></script>
    
    <script src="~/lib/bootstrap/dist/js/bootstrap.bundle.min.js"></script>
    <script src="~/js/site.js"></script>
</body>
```

**Note:** You need either `<ejs-scripts>` tag helper OR the manual script include, not both.

### Step 3: Verify Layout

Your complete `_Layout.cshtml` should look like:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>@ViewData["Title"]</title>
    <link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.min.css" />
    
    <!-- Syncfusion CSS -->
    <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/fluent.css" />
</head>
<body>
    <div class="container">
        @RenderBody()
    </div>
    
    <!-- Syncfusion Script Manager -->
    <ejs-scripts></ejs-scripts>
    
    <script src="~/lib/bootstrap/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

## Creating Your First Spinner

### Step 1: Add HTML Container

In your Razor page (`~/Pages/Index.cshtml`), add an element where the spinner will appear:

```html
<div id="container" style="height: 300px; border: 1px solid #ccc; position: relative;">
    <p>Content area where spinner will appear</p>
    <button id="showSpinnerButton">Show Spinner</button>
</div>
```

**Important:** The container should have:
- Defined height or positioned layout
- Content that can be covered by the spinner overlay
- `position: relative` for proper spinner positioning

### Step 2: Create Spinner Instance

Add JavaScript code to create the spinner:

```html
<script>
    // Get the target element
    var spinTarget = document.getElementById('container');
    
    // Create spinner with target configuration
    ej.popups.createSpinner({
        target: spinTarget
    });
</script>
```

**What happens:** 
- Spinner is initialized and ready
- Spinner overlay is created but hidden initially
- Spinner is positioned absolute within the target container

### Step 3: Show and Hide

Add buttons and event handlers to control the spinner:

```html
<button id="showSpinnerButton">Show Spinner</button>
<button id="hideSpinnerButton">Hide Spinner</button>

<script>
    var spinTarget = document.getElementById('container');
    ej.popups.createSpinner({ target: spinTarget });
    
    // Show spinner
    document.getElementById('showSpinnerButton').addEventListener('click', function() {
        ej.popups.showSpinner(spinTarget);
    });
    
    // Hide spinner
    document.getElementById('hideSpinnerButton').addEventListener('click', function() {
        ej.popups.hideSpinner(spinTarget);
    });
</script>
```

## Showing and Hiding

### showSpinner() - Display the Spinner

```javascript
var target = document.getElementById('container');
ej.popups.showSpinner(target);
```

**Behavior:**
- Makes spinner overlay visible
- Overlay becomes opaque and visible
- User cannot interact with target area
- Spinner animation starts automatically

### hideSpinner() - Hide the Spinner

```javascript
ej.popups.hideSpinner(target);
```

**Behavior:**
- Makes spinner overlay invisible
- User can interact with target area again
- Animation stops
- Overlay remains in DOM (ready to show again)

### Automatic Hide with Timeout

Common pattern for simulated loading:

```javascript
ej.popups.showSpinner(target);

setTimeout(function() {
    ej.popups.hideSpinner(target);
}, 3000);  // Hide after 3 seconds
```

## Targeting Elements

### Single Element Target

```javascript
var singleTarget = document.getElementById('specific-element');
ej.popups.createSpinner({ target: singleTarget });
ej.popups.showSpinner(singleTarget);
```

Spinner appears only over that element.

### Full Page Spinner

```javascript
var pageTarget = document.body;  // or document.documentElement
ej.popups.createSpinner({ target: pageTarget });
ej.popups.showSpinner(pageTarget);
```

Spinner appears as full-page overlay.

### Multiple Independent Spinners

```javascript
var target1 = document.getElementById('section1');
var target2 = document.getElementById('section2');

// Create separate spinners
ej.popups.createSpinner({ target: target1 });
ej.popups.createSpinner({ target: target2 });

// Control independently
ej.popups.showSpinner(target1);  // Only target1 shows spinner
```

## Complete Example

Here's a complete working example in an ASP.NET Core Razor page:

**File:** `~/Pages/Index.cshtml`

```html
@page
@model IndexModel

<div class="container mt-5">
    <h1>Syncfusion Spinner Getting Started</h1>
    
    <div id="container" style="
        height: 300px; 
        border: 2px solid #ddd; 
        border-radius: 4px;
        padding: 20px;
        background-color: #f9f9f9;
        position: relative;
        margin: 20px 0;">
        
        <p>This container will show a spinner on demand.</p>
        <p>Click the button below to demonstrate.</p>
    </div>
    
    <div style="margin-top: 20px;">
        <button id="showSpinnerButton" class="btn btn-primary">
            Show Spinner (3 seconds)
        </button>
        <button id="resetButton" class="btn btn-secondary">
            Reset
        </button>
    </div>
</div>

<script>
    // Get target element
    var spinTarget = document.getElementById('container');
    
    // Create spinner
    ej.popups.createSpinner({
        target: spinTarget
    });
    
    // Show spinner on button click
    document.getElementById('showSpinnerButton').addEventListener('click', function() {
        ej.popups.showSpinner(spinTarget);
        
        // Auto-hide after 3 seconds
        setTimeout(function() {
            ej.popups.hideSpinner(spinTarget);
        }, 3000);
    });
    
    // Reset button
    document.getElementById('resetButton').addEventListener('click', function() {
        ej.popups.hideSpinner(spinTarget);
    });
</script>
```

**File:** `~/Pages/Shared/_Layout.cshtml` (layout updates)

```html
<head>
    <!-- ... other content ... -->
    <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/fluent.css" />
</head>
<body>
    @RenderBody()
    
    <ejs-scripts></ejs-scripts>
</body>
```

## Troubleshooting

**Spinner not appearing:**
- Ensure stylesheet is loaded (check Network tab in DevTools)
- Verify `ej.popups` object exists in console
- Check that target element has height/content
- Confirm `createSpinner()` was called before `showSpinner()`

**Spinner position is wrong:**
- Ensure target has `position: relative` or `absolute`
- Parent container should have defined dimensions
- Check for CSS conflicting with positioning

**Script errors about `ej.popups`:**
- Verify `ej2.min.js` script loaded successfully
- Check CDN URL is correct and accessible
- Ensure script tag is placed before your spinner code
