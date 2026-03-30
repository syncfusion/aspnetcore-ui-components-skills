# Getting Started with ASP.NET Core TextBox

## Table of Contents
- [Prerequisites](#prerequisites)
- [Project Setup](#project-setup)
- [Install NuGet Package](#install-nuget-package)
- [Configure TagHelper](#configure-taghelper)
- [Add Stylesheets and Scripts](#add-stylesheets-and-scripts)
- [Register Script Manager](#register-script-manager)
- [Create Your First TextBox](#create-your-first-textbox)
- [Running the Application](#running-the-application)
- [Troubleshooting](#troubleshooting)

## Prerequisites

Before implementing TextBox in ASP.NET Core, ensure you have:

- **ASP.NET Core Runtime**: 6.0 or higher
- **Visual Studio**: 2019 (16.8) or later, or Visual Studio Code
- **Node.js**: Optional (for custom build processes)

**System Requirements Check:**

```bash
dotnet --version
# Output should be: 6.0.x or higher
```

## Project Setup

### Create a New ASP.NET Core Project

**Using Visual Studio:**
1. File → New → Project
2. Select "ASP.NET Core Web App"
3. Choose target framework: .NET 6.0 or higher
4. Click "Create"

**Using .NET CLI:**

```bash
dotnet new webapp -n TextBoxApp
cd TextBoxApp
```

**Result:**
```
TextBoxApp/
├── Pages/
│   ├── Index.cshtml
│   ├── Index.cshtml.cs
│   └── _Layout.cshtml
├── Program.cs
├── appsettings.json
└── textboxapp.csproj
```

## Install NuGet Package

### Install Syncfusion.EJ2.AspNet.Core

Open NuGet Package Manager and search for `Syncfusion.EJ2.AspNet.Core`:

**Option 1: NuGet Package Manager GUI**
1. Tools → NuGet Package Manager → Manage NuGet Packages for Solution
2. Search for "Syncfusion.EJ2.AspNet.Core"
3. Click "Install"

**Option 2: Package Manager Console**

```powershell
Install-Package Syncfusion.EJ2.AspNet.Core -Version 24.1.48
```

**Option 3: .NET CLI**

```bash
dotnet add package Syncfusion.EJ2.AspNet.Core --version 24.1.48
```

**Verification:**

Check `TextBoxApp.csproj` contains:

```xml
<ItemGroup>
    <PackageReference Include="Syncfusion.EJ2.AspNet.Core" Version="24.1.48" />
</ItemGroup>
```

## Configure TagHelper

### Step 1: Add TagHelper Reference

Open `Pages/_ViewImports.cshtml` and add the Syncfusion TagHelper:

```cshtml
@* Pages/_ViewImports.cshtml *@
@using TextBoxApp
@using TextBoxApp.Pages
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper *, Syncfusion.EJ2

@* Optional: Add other TagHelpers as needed *@
```

**What This Does:**
- Enables Syncfusion tag helpers in all Razor pages
- Allows using `<ejs-textbox>` and other Syncfusion components
- Makes IntelliSense available in Visual Studio

### Step 2: Verify TagHelper

Create a test file to verify TagHelper is working:

```cshtml
@* Pages/TestTextBox.cshtml *@
@page
@{
    ViewData["Title"] = "TextBox Test";
}

<h2>TextBox TagHelper Test</h2>
<ejs-textbox id="testTextBox" placeholder="Type here..."></ejs-textbox>
```

## Add Stylesheets and Scripts

### Add CDN References to _Layout.cshtml

Open `Pages/Shared/_Layout.cshtml` and add Syncfusion CSS and JavaScript:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>@ViewData["Title"] - TextBox Demo</title>
    
    <!-- Bootstrap CSS (optional) -->
    <link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.min.css" />
    
    <!-- Syncfusion CSS - Must be included -->
    <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/24.1.48/fluent.css" />
    
    <link rel="stylesheet" href="~/css/site.css" asp-append-version="true" />
</head>
<body>
    <nav class="navbar navbar-expand-sm navbar-toggleable-sm navbar-light bg-white border-bottom box-shadow mb-3">
        <div class="container">
            <a class="navbar-brand" asp-area="" asp-page="/Index">TextBox Demo</a>
        </div>
    </nav>
    
    <div class="container">
        <main role="main" class="pb-3">
            @RenderBody()
        </main>
    </div>

    <!-- JavaScript Files -->
    <script src="~/lib/bootstrap/dist/js/bootstrap.bundle.min.js"></script>
    
    <!-- Syncfusion JavaScript - Must be included -->
    <script src="https://cdn.syncfusion.com/ej2/24.1.48/dist/ej2.min.js"></script>
    
    <script src="~/js/site.js" asp-append-version="true"></script>
    
    @await RenderSectionAsync("Scripts", required: false)
</body>
</html>
```

**Available Themes:**
- `fluent.css` - Modern, clean design (recommended)
- `bootstrap5.css` - Bootstrap 5 compatible
- `tailwind.css` - Tailwind CSS compatible
- `material.css` - Material Design theme

**CDN URL Pattern:**
```
https://cdn.syncfusion.com/ej2/{VERSION}/{THEME}.css
https://cdn.syncfusion.com/ej2/{VERSION}/dist/ej2.min.js
```

## Register Script Manager

### Add Script Manager

Add the Syncfusion script manager at the **end of `<body>`** in `_Layout.cshtml`:

```html
<body>
    @RenderBody()
    
    <script src="https://cdn.syncfusion.com/ej2/24.1.48/dist/ej2.min.js"></script>
    
    <!-- Syncfusion Script Manager - Initializes Syncfusion controls -->
    <ejs-scripts></ejs-scripts>
</body>
```

**Important:** The `<ejs-scripts>` tag must be placed **after** the Syncfusion JavaScript file and **after** all content that uses Syncfusion components.

## Create Your First TextBox

### Basic TextBox Example

Create `Pages/TextBoxDemo.cshtml`:

```cshtml
@page
@{
    ViewData["Title"] = "TextBox Demo";
}

<div class="container mt-5">
    <h1>Syncfusion TextBox Examples</h1>
    
    <!-- Example 1: Basic TextBox -->
    <div class="form-group mb-4">
        <h3>Example 1: Basic TextBox</h3>
        <ejs-textbox id="basicTextBox" 
                      placeholder="Enter your name">
        </ejs-textbox>
    </div>
    
    <!-- Example 2: TextBox with Floating Label -->
    <div class="form-group mb-4">
        <h3>Example 2: Floating Label</h3>
        <ejs-textbox id="floatingTextBox" 
                      placeholder="Enter your email" 
                      floatLabelType="Auto"
                      type="email">
        </ejs-textbox>
    </div>
    
    <!-- Example 3: Disabled TextBox -->
    <div class="form-group mb-4">
        <h3>Example 3: Disabled TextBox</h3>
        <ejs-textbox id="disabledTextBox" 
                      placeholder="This is disabled" 
                      disabled="true"
                      value="Cannot edit">
        </ejs-textbox>
    </div>
    
    <!-- Example 4: Readonly TextBox -->
    <div class="form-group mb-4">
        <h3>Example 4: Readonly TextBox</h3>
        <ejs-textbox id="readonlyTextBox" 
                      readonly="true"
                      value="Read-only content">
        </ejs-textbox>
    </div>
    
    <!-- Example 5: TextBox with Validation -->
    <div class="form-group mb-4">
        <h3>Example 5: Validation States</h3>
        
        <p>Success State:</p>
        <ejs-textbox id="successTextBox" 
                      placeholder="Valid input" 
                      value="john@example.com"
                      cssClass="e-success"
                      floatLabelType="Auto">
        </ejs-textbox>
        <small class="text-success d-block mt-2">✓ Email is valid</small>
        
        <p class="mt-4">Error State:</p>
        <ejs-textbox id="errorTextBox" 
                      placeholder="Invalid input" 
                      value="invalid-email"
                      cssClass="e-error"
                      floatLabelType="Auto">
        </ejs-textbox>
        <small class="text-danger d-block mt-2">✗ Invalid email format</small>
        
        <p class="mt-4">Warning State:</p>
        <ejs-textbox id="warningTextBox" 
                      placeholder="Warning" 
                      value="maybe-email"
                      cssClass="e-warning"
                      floatLabelType="Auto">
        </ejs-textbox>
        <small class="text-warning d-block mt-2">⚠ Check format</small>
    </div>
    
    <!-- Example 6: Multiline TextBox (Textarea) -->
    <div class="form-group mb-4">
        <h3>Example 6: Multiline TextBox</h3>
        <ejs-textbox id="multilineTextBox" 
                      placeholder="Enter your message..." 
                      multiline="true"
                      floatLabelType="Auto">
        </ejs-textbox>
    </div>
</div>

<style>
    .form-group {
        margin-bottom: 2rem;
        padding: 1rem;
        background-color: #f8f9fa;
        border-radius: 4px;
    }
    
    h3 {
        color: #333;
        margin-bottom: 1rem;
        font-size: 1.25rem;
    }
    
    .text-success { color: #28a745; }
    .text-danger { color: #dc3545; }
    .text-warning { color: #ffc107; }
</style>
```

## Running the Application

### Run via Visual Studio

1. Press **F5** or select Debug → Start Debugging
2. Application opens in default browser at `https://localhost:5001`
3. Navigate to `/TextBoxDemo` route

### Run via .NET CLI

```bash
cd TextBoxApp
dotnet run
```

**Output:**
```
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 GET https://localhost:5001/textboxdemo - text/html
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished HTTP/2 GET https://localhost:5001/textboxdemo - text/html
```

Visit `https://localhost:5001/textboxdemo` in your browser.

### Expected Result

You should see:
- 6 different TextBox examples
- Floating labels that animate on focus
- Validation states with colored borders
- Disabled and readonly inputs
- Multiline textarea

## Troubleshooting

### Issue 1: TagHelper Not Recognized

**Error:** "The name 'ejs-textbox' does not exist in the current context"

**Solution:**
1. Verify `_ViewImports.cshtml` contains: `@addTagHelper *, Syncfusion.EJ2`
2. Restart Visual Studio
3. Clean solution and rebuild: `dotnet clean && dotnet build`

### Issue 2: Styles Not Applying

**Error:** TextBox appears unstyled (plain input)

**Solution:**
1. Check Syncfusion CSS is loaded in `_Layout.cshtml`
2. Open browser DevTools (F12) → Network tab
3. Verify CSS file loads (status 200)
4. Try different CDN URL: `https://cdn.syncfusion.com/ej2/24.1.48/fluent.css`

### Issue 3: JavaScript Not Initializing

**Error:** TextBox functionality doesn't work (no floating label animation)

**Solution:**
1. Verify `<ejs-scripts></ejs-scripts>` is at end of `<body>`
2. Verify JavaScript file loads: `https://cdn.syncfusion.com/ej2/24.1.48/dist/ej2.min.js`
3. Check browser console for errors (F12)
4. Ensure ej2.min.js loads **before** `<ejs-scripts>`

### Issue 4: NuGet Package Not Installing

**Error:** "Could not find a part of the path"

**Solution:**
```bash
# Clear NuGet cache
dotnet nuget locals all --clear

# Restore packages
dotnet restore

# Try installing again
dotnet add package Syncfusion.EJ2.AspNet.Core --version 24.1.48
```

### Issue 5: License Warning

**Warning:** "License key not registered"

**Solution:**
1. Register your Syncfusion Community or Commercial license
2. Add to `Program.cs`:

```csharp
using Syncfusion.Licensing;

var builder = WebApplication.CreateBuilder(args);
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("your-license-key");

// ... rest of Program.cs
```

## Next Steps

1. ✅ TextBox is now ready to use
2. Review [tag-helper-syntax.md](tag-helper-syntax.md) for available properties
3. Explore [validation-states.md](validation-states.md) for form validation
4. Check [adornments-and-icons.md](adornments-and-icons.md) for icons and buttons
5. See [advanced-patterns.md](advanced-patterns.md) for data binding and events
