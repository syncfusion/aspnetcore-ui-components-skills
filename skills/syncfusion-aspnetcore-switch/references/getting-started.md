# Getting Started with ASP.NET Core Switch

## Table of Contents
- [Prerequisites](#prerequisites)
- [Install NuGet Package](#install-nuget-package)
- [Add TagHelper](#add-taghelper)
- [Add Stylesheet and Script Resources](#add-stylesheet-and-script-resources)
- [Register Script Manager](#register-script-manager)
- [Create Your First Switch](#create-your-first-switch)
- [Add Labels to Switch](#add-labels-to-switch)
- [Verify Installation](#verify-installation)
- [Troubleshooting](#troubleshooting)

## Prerequisites

Before implementing the Switch component, ensure you have:

- **System Requirements:**
  - Visual Studio 2019 or later
  - .NET 5.0 or higher
  - ASP.NET Core web application project (Razor Pages or MVC)
  
- **Existing Project:** Either create a new ASP.NET Core web application or use an existing one

**Create New ASP.NET Core Project:**
```bash
dotnet new webapp -n SwitchDemo
cd SwitchDemo
```

## Install NuGet Package

The Syncfusion Switch component is included in the `Syncfusion.EJ2.AspNet.Core` NuGet package.

### Using NuGet Package Manager
1. Open Visual Studio
2. Tools → NuGet Package Manager → Manage NuGet Packages for Solution
3. Search for `Syncfusion.EJ2.AspNet.Core`
4. Click Install

### Using Package Manager Console
```bash
Install-Package Syncfusion.EJ2.AspNet.Core
```

### Using dotnet CLI
```bash
dotnet add package Syncfusion.EJ2.AspNet.Core
```

**Note:** This package includes dependencies:
- Newtonsoft.Json (for JSON serialization)
- Syncfusion.Licensing (for license validation)

## Add TagHelper

The Syncfusion TagHelper must be registered to use the `<ejs-switch>` tag in your views.

### Step 1: Open _ViewImports.cshtml
Navigate to `~/Pages/_ViewImports.cshtml` (or `~/Views/_ViewImports.cshtml` for MVC)

### Step 2: Add TagHelper Import
Add this line at the top of the file:
```cshtml
@addTagHelper *, Syncfusion.EJ2
```

**Complete Example:**
```cshtml
@using SwitchDemo
@namespace SwitchDemo.Pages
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper *, Syncfusion.EJ2
```

**Verify:** After adding, IntelliSense should suggest `<ejs-switch>` tag

## Add Stylesheet and Script Resources

Add the Syncfusion CSS and JavaScript files to enable styling and functionality.

### Step 1: Open _Layout.cshtml
Navigate to `~/Pages/Shared/_Layout.cshtml` (or `~/Views/Shared/_Layout.cshtml` for MVC)

### Step 2: Add Stylesheet in Head
Add this link inside the `<head>` section:
```html
<head>
    <!-- Other stylesheets -->
    
    <!-- Syncfusion CSS (with Subresource Integrity for CDN security) -->
    <link rel="stylesheet"
          href="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/fluent.css"
          integrity="sha384-<HASH>"
          crossorigin="anonymous" />
</head>
```

### Step 3: Add Script Tag in Body
Add this script before the closing `</body>` tag:
```html
<body>
    <!-- Your content goes here -->
    
    <!-- Syncfusion JavaScript (with Subresource Integrity for CDN security) -->
    <script src="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/dist/ej2.min.js"
            integrity="sha384-<HASH>"
            crossorigin="anonymous"></script>
</body>
```

**Available Themes** (replace `fluent.css` with any of these):
- `bootstrap5.css` - Bootstrap 5 theme
- `material.css` - Material Design theme
- `fabric.css` - Microsoft Fabric theme
- `bootstrap.css` - Bootstrap 4 theme
- `highcontrast.css` - High contrast theme

**Version Note:** Use the current Syncfusion version in CDN URLs (e.g., 22.1.34)

> **CDN Security Note:** Loading assets from an external CDN introduces a supply-chain risk. Apply the following mitigations based on your deployment requirements:
>
> | Mitigation | Description |
> |---|---|
> | **Subresource Integrity (SRI)** | Add `integrity="sha384-<HASH>"` and `crossorigin="anonymous"` attributes to every `<link>` and `<script>` tag. Generate hashes with the [SRI Hash Generator](https://www.srihash.org/) or `openssl dgst -sha384 -binary <file> \| openssl base64 -A`. |
> | **Vendor / Self-hosting** | For production or air-gapped environments, copy the CSS and JS files from the NuGet package's `wwwroot` folder to your own `wwwroot/lib/syncfusion/` and reference them locally instead of the CDN. |
> | **License Key Handling** | Never hard-code your Syncfusion license key in client-side scripts. Register it server-side in `Program.cs` or `Startup.cs` using `Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(Configuration["Syncfusion:LicenseKey"])` and store the key in environment variables or a secrets manager. |

## Register Script Manager

The Syncfusion Script Manager initializes all components on the page.

### Step 1: Add Script Manager Tag
Add this at the end of the `<body>` section in `_Layout.cshtml`:
```html
<body>
    <!-- Your content -->
    
    <!-- Syncfusion Script Manager -->
    <ejs-scripts></ejs-scripts>
</body>
```

**Complete Layout Example (CDN with SRI):**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Syncfusion Switch Demo</title>
    
    <!-- Syncfusion CSS — replace <HASH> with the sha384 hash for your version -->
    <link rel="stylesheet"
          href="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/fluent.css"
          integrity="sha384-<HASH>"
          crossorigin="anonymous" />
</head>
<body>
    <main role="main">
        @RenderBody()
    </main>
    
    <!-- Syncfusion JavaScript — replace <HASH> with the sha384 hash for your version -->
    <script src="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/dist/ej2.min.js"
            integrity="sha384-<HASH>"
            crossorigin="anonymous"></script>
    
    <!-- Syncfusion Script Manager -->
    <ejs-scripts></ejs-scripts>
</body>
</html>
```

**Alternative: Self-hosted (recommended for production)**
```html
<!-- Reference files from your own wwwroot after copying from the NuGet package -->
<link rel="stylesheet" href="~/lib/syncfusion/fluent.css" />
...
<script src="~/lib/syncfusion/ej2.min.js"></script>
```

## Create Your First Switch

Now you're ready to add a Switch component to your page.

### Step 1: Open Index.cshtml
Navigate to `~/Pages/Index.cshtml` (or controller view for MVC)

### Step 2: Add Switch TagHelper
Add this code in the page body:
```cshtml
<div class="demo-section">
    <h1>My First Switch</h1>
    
    <ejs-switch id="basicSwitch"></ejs-switch>
    
    <p id="switchStatus">Status will appear here</p>
</div>
```

### Step 3: Add Interactivity (Optional)
Add this script to see the switch state:
```cshtml
<script>
    // Get switch element after page loads
    document.addEventListener('DOMContentLoaded', function() {
        var switchElement = document.getElementById('basicSwitch').ej2_instances[0];
        
        // Log initial state
        console.log('Switch checked:', switchElement.checked);
    });
</script>
```

### Step 4: Run the Application
```bash
dotnet run
```

Open browser at `https://localhost:5001` and verify the Switch appears.

## Add Labels to Switch

The Switch component supports `onLabel` and `offLabel` to display text in each state.

### Basic Switch with Labels
```cshtml
<ejs-switch id="labeledSwitch" 
            onLabel="ON" 
            offLabel="OFF">
</ejs-switch>
```

**Result:** When switched on, displays "ON"; when off, displays "OFF"

### Realistic Example: Enable Notifications
```cshtml
<div class="notification-preference">
    <label for="notificationSwitch">Enable Notifications:</label>
    <ejs-switch id="notificationSwitch" 
                onLabel="Enabled" 
                offLabel="Disabled">
    </ejs-switch>
</div>

<style>
    .notification-preference {
        display: flex;
        align-items: center;
        gap: 20px;
        margin: 20px 0;
    }
    
    .notification-preference label {
        font-weight: bold;
    }
</style>
```

### Long Custom Labels
```cshtml
<!-- Note: Material theme doesn't support long labels -->
<ejs-switch id="agreeSwitch" 
            onLabel="I Agree" 
            offLabel="I Decline"
            cssClass="e-large">
</ejs-switch>
```

**Label Best Practices:**
- Keep labels short (1-2 words): "ON"/"OFF", "Yes"/"No", "Enabled"/"Disabled"
- Avoid long labels on Material themes (they may not display properly)
- Use adjacent text labels for clarity: `<label>Feature Name</label><ejs-switch></ejs-switch>`

## Verify Installation

Check that everything is working correctly:

### Visual Check
1. Open browser DevTools (F12)
2. Verify no JavaScript errors in Console tab
3. Switch should render with proper styling
4. Clicking switch should toggle appearance

### Code Check
Create a test page to verify all components load:
```cshtml
@{
    ViewData["Title"] = "Syncfusion Setup Verification";
}

<div class="verification-section">
    <h2>Syncfusion Setup Verification</h2>
    
    <div class="test-item">
        <label>Test Switch 1 - Basic</label>
        <ejs-switch id="test1"></ejs-switch>
    </div>
    
    <div class="test-item">
        <label>Test Switch 2 - With Labels</label>
        <ejs-switch id="test2" onLabel="YES" offLabel="NO"></ejs-switch>
    </div>
    
    <div class="test-item">
        <label>Test Switch 3 - Disabled</label>
        <ejs-switch id="test3" disabled="true"></ejs-switch>
    </div>
    
    <div id="verification-status" style="margin-top: 20px; font-weight: bold;"></div>
</div>

<script>
    document.addEventListener('DOMContentLoaded', function() {
        try {
            var switch1 = document.getElementById('test1').ej2_instances[0];
            var switch2 = document.getElementById('test2').ej2_instances[0];
            var switch3 = document.getElementById('test3').ej2_instances[0];
            
            if (switch1 && switch2 && switch3) {
                document.getElementById('verification-status').innerHTML = 
                    '✓ All switches loaded successfully!<br>' +
                    '✓ CSS is applied<br>' +
                    '✓ JavaScript is working';
                document.getElementById('verification-status').style.color = 'green';
            }
        } catch (e) {
            document.getElementById('verification-status').innerHTML = 
                '✗ Error: ' + e.message;
            document.getElementById('verification-status').style.color = 'red';
        }
    });
</script>
```

## Troubleshooting

### Issue: Switch doesn't render
**Cause:** TagHelper not registered or CSS/JS not loaded
**Solution:** 
1. Verify `@addTagHelper *, Syncfusion.EJ2` in _ViewImports.cshtml
2. Check CSS and JS URLs are correct in _Layout.cshtml
3. Ensure `<ejs-scripts></ejs-scripts>` is present

### Issue: Switch renders but styling looks broken
**Cause:** CSS not loaded or wrong theme selected
**Solution:**
1. Open DevTools (F12) → Network tab
2. Check if CSS file loads (should see 200 status)
3. Try different theme: replace `fluent.css` with `bootstrap5.css` or `material.css`

### Issue: IntelliSense doesn't suggest `<ejs-switch>`
**Cause:** TagHelper not recognized by editor
**Solution:**
1. Save _ViewImports.cshtml
2. Close and reopen the view file
3. Clean Solution and Rebuild in Visual Studio
4. Restart Visual Studio if still not working

### Issue: JavaScript error "ej2_instances is undefined"
**Cause:** Script trying to access component before page loaded
**Solution:**
```javascript
// ❌ Wrong
var switchElement = document.getElementById('basicSwitch').ej2_instances[0];

// ✅ Correct
document.addEventListener('DOMContentLoaded', function() {
    var switchElement = document.getElementById('basicSwitch').ej2_instances[0];
});
```

### Issue: CDN resources blocked or not loading
**Cause:** Network issue, incorrect CDN URL, or SRI hash mismatch
**Solution:**
1. Check browser console for 404 errors or `net::ERR_FAILED` / SRI mismatch errors
2. Verify CDN URL has the correct version number
3. Check internet connectivity
4. If using SRI hashes, regenerate them for the exact version you are using:
   ```bash
   # PowerShell
   $bytes = [System.IO.File]::ReadAllBytes("ej2.min.js")
   $hash  = [System.Security.Cryptography.SHA384]::Create().ComputeHash($bytes)
   "sha384-" + [Convert]::ToBase64String($hash)
   ```
5. **For production environments**, self-host assets under `wwwroot/lib/syncfusion/` to remove the external CDN dependency entirely and eliminate the SRI maintenance burden

### Issue: License key exposed in client-side code
**Cause:** License key registered via an inline `<script>` block on the page
**Solution:**
Register the key exclusively on the server side and source it from a secrets store:
```csharp
// Program.cs / Startup.cs — never embed the key in Razor views
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(
    builder.Configuration["Syncfusion:LicenseKey"]); // stored in secrets / env var
```
Store the value in:
- **Development:** `dotnet user-secrets set "Syncfusion:LicenseKey" "<key>"`
- **Production:** an environment variable or a key vault (e.g., Azure Key Vault)

## Next Steps

- Configure properties: [Switch Properties Reference](../switch-properties.md)
- Handle state changes: [Switch Events Reference](../switch-events.md)
- Customize appearance: [Styling and Customization](../styling-customization.md)
