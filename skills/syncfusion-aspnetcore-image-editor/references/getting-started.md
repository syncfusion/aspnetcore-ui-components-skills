# Getting Started with ASP.NET Core Image Editor

## Table of Contents
- [Prerequisites](#prerequisites)
- [Package Installation](#package-installation)
- [Tag Helper Registration](#tag-helper-registration)
- [Add Stylesheet and Scripts](#add-stylesheet-and-scripts)
- [Register Script Manager](#register-script-manager)
- [Add Image Editor Control](#add-image-editor-control)
- [Run the Application](#run-the-application)

## Prerequisites

Before getting started with the Syncfusion ASP.NET Core Image Editor, ensure you have:

- Visual Studio installed (2019 or later recommended)
- .NET 5.0 or higher runtime
- Basic knowledge of ASP.NET Core and Razor Pages
- A web browser that supports ES6+ JavaScript

Refer to [System requirements for ASP.NET Core controls](https://ej2.syncfusion.com/aspnetcore/documentation/system-requirements) for detailed requirements.

## Create ASP.NET Core Web Application

You can create a new ASP.NET Core Razor Pages application using:

- **Microsoft Templates**: Follow [Microsoft's Razor Pages tutorial](https://learn.microsoft.com/en-us/aspnet/core/tutorials/razor-pages/razor-pages-start)
- **Syncfusion Visual Studio Extension**: Use the Syncfusion ASP.NET Core Extension for project creation

## Package Installation

### Install via NuGet Package Manager

1. Open **Tools → NuGet Package Manager → Manage NuGet Packages for Solution**
2. Search for `Syncfusion.EJ2.AspNet.Core`
3. Click **Install**

### Install via Package Manager Console

Run the following command in the Package Manager Console:

```powershell
Install-Package Syncfusion.EJ2.AspNet.Core -Version {{ site.releaseversion }}
```

### Dependencies

The Syncfusion.EJ2.AspNet.Core NuGet package has the following dependencies:

- **Newtonsoft.Json** - For JSON serialization
- **Syncfusion.Licensing** - For license validation

These dependencies are automatically installed with the main package.

> **Note:** Syncfusion ASP.NET Core controls are available on [nuget.org](https://www.nuget.org/packages?q=syncfusion.EJ2). See [NuGet packages topic](https://ej2.syncfusion.com/aspnetcore/documentation/nuget-packages) for more installation methods.

## Tag Helper Registration

Open the `~/Pages/_ViewImports.cshtml` file and add the Syncfusion TagHelper import:

```csharp
@addTagHelper *, Syncfusion.EJ2
```

This registers all Syncfusion components as tag helpers for use in your Razor Pages.

## Add Stylesheet and Scripts

In the `~/Pages/Shared/_Layout.cshtml` file, add the Syncfusion theme and script references inside the `<head>` tag:

```html
<head>
    ...
    <!-- Syncfusion ASP.NET Core controls styles -->
    <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/fluent.css" />
    
    <!-- Syncfusion ASP.NET Core controls scripts -->
    <script src="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/dist/ej2.min.js"></script>
</head>
```

### Theme Selection

Syncfusion provides multiple themes:
- **fluent.css** - Fluent theme (default)
- **bootstrap5.css** - Bootstrap 5 theme
- **tailwind.css** - Tailwind theme
- **material.css** - Material theme

Choose based on your application's design system.

### Script Reference Alternatives

You can add scripts using:

- **CDN** (recommended for production): `https://cdn.syncfusion.com/ej2/{version}/dist/ej2.min.js`
- **NPM Package**: Install `@syncfusion/ej2` and import locally
- **Custom Resource Generator (CRG)**: Generate custom bundles with only needed components

Refer to [Adding Script References](https://ej2.syncfusion.com/aspnetcore/documentation/common/adding-script-references) for detailed options.

## Register Script Manager

At the end of the `<body>` tag in `_Layout.cshtml`, add the Syncfusion script manager:

```html
<body>
    ...
    <!-- Syncfusion ASP.NET Core Script Manager -->
    <ejs-scripts></ejs-scripts>
</body>
```

This initializes the Syncfusion script manager required for component functionality.

## Add Image Editor Control

Now add the Image Editor tag helper to your Razor Page (e.g., `~/Pages/Index.cshtml`):

```html
<ejs-imageeditor id="imageEditor">
</ejs-imageeditor>
```

### Basic Configuration Example

```html
<ejs-imageeditor id="imageEditor" 
    width="100%" 
    height="600px"
    toolbar="@new List<string> { 'Open', 'Crop', 'RotateLeft', 'RotateRight', 'HorizontalFlip', 'VerticalFlip', 'Reset', 'Save' }">
</ejs-imageeditor>
```

### Properties in Tag Helper

```html
<ejs-imageeditor id="imageEditor"
    width="100%"
    height="600px"
    zoomSettings='new { minZoom = 10, maxZoom = 1000 }'
    toolbar="@new List<string> { 'Open', 'Save' }"
    showQuickAccessToolbar="true">
</ejs-imageeditor>
```

## Run the Application

Press **Ctrl+F5** (Windows) or **⌘+F5** (macOS) to run the application.

The Image Editor control will render in your default web browser with the default toolbar and canvas. You should see:

1. A toolbar at the top with default actions (Open, Undo, Redo, Zoom, Crop, etc.)
2. A canvas area in the center for image editing
3. The ability to open images and perform basic operations

### First Steps After Loading

1. Click the **Open** button to select an image file
2. Supported formats: JPEG, PNG, JPG, WEBP, BMP
3. Once loaded, use toolbar buttons to edit the image
4. Click **Save** to export the modified image

## Minimal Working Example

Here's a complete minimal example:

**Index.cshtml:**
```html
@page
@model IndexModel

<div class="container mt-5">
    <h1>Image Editor Demo</h1>
    
    <ejs-imageeditor id="imageEditor" 
        width="100%" 
        height="600px">
    </ejs-imageeditor>
</div>

<script>
    document.addEventListener('DOMContentLoaded', function() {
        var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
        
        // Optional: Set up event handlers
        imageEditor.imageLoaded = function() {
            console.log('Image loaded successfully');
        };
    });
</script>
```

## Troubleshooting

**Issue:** Image Editor not rendering
- **Solution:** Ensure `<ejs-scripts></ejs-scripts>` is at the end of `_Layout.cshtml` body
- **Solution:** Verify CSS and JS CDN links are accessible

**Issue:** Toolbar buttons not working
- **Solution:** Check browser console for JavaScript errors
- **Solution:** Verify script manager is registered

**Issue:** TagHelper not recognized
- **Solution:** Check `_ViewImports.cshtml` has `@addTagHelper *, Syncfusion.EJ2`
- **Solution:** Rebuild solution if Visual Studio cache issues occur

## Next Steps

1. **Customize Toolbar** - See toolbar-customization.md
2. **Handle Image Operations** - See image-operations.md
3. **Add Annotations** - See annotations.md
4. **Configure Save Options** - See export-and-save.md
