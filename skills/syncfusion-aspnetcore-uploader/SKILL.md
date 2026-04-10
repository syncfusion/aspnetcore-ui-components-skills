---
name: syncfusion-aspnetcore-uploader
description: Complete guide to implementing the Syncfusion Uploader component in ASP.NET Core applications. Use this when working with file uploads, asynchronous processing, validation, drag-and-drop support, or enterprise-grade file handling for professional web forms.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "File Upload Controls"
  platform: "ASP.NET Core"
  framework: "Syncfusion.EJ2.AspNetCore"
---

# Implementing Uploader in ASP.NET Core

The Uploader is a powerful file upload component for ASP.NET Core that handles file selection, validation, asynchronous uploads, drag-and-drop support, and progress tracking. This skill guides you through implementing Uploader in ASP.NET Core applications using Syncfusion's TagHelper syntax.

## When to Use This Skill

Use this skill when you need to:
- Create file upload forms for images, documents, and media
- Implement asynchronous file uploads with progress feedback
- Validate file types and sizes before upload
- Support drag-and-drop file selection
- Handle multiple file uploads simultaneously
- Process file removal and cleanup
- Build enterprise-grade file management interfaces
- Implement chunk uploads for large files
- Integrate file uploads into HTML forms
- Support paste-to-upload and directory uploads
- Ensure proper file handling and security

## Key Features

| Feature | Use Case |
|---------|----------|
| **Asynchronous Upload** | Non-blocking uploads with SaveUrl/RemoveUrl handlers |
| **Multiple File Upload** | Select and upload many files at once |
| **Single File Upload** | Restrict to one file per selection |
| **File Validation** | Validate extensions, size, count, duplicates |
| **Drag and Drop** | Drop files directly onto uploader area |
| **Custom Drop Area** | Define external elements as drop zones |
| **Auto Upload** | Upload files automatically or manually |
| **Progress Tracking** | Display upload progress with progress bar |
| **File Removal** | Remove uploaded files with RemoveUrl handler |
| **Chunk Upload** | Break large files into chunks for reliable upload |
| **Paste Upload** | Upload images from clipboard |
| **Directory Upload** | Upload entire folders and directory structure |
| **Form Integration** | Submit files as part of form submission |
| **Templates** | Customize file list and button appearance |
| **Localization** | Support multiple languages and cultures |
| **Error Handling** | Display upload failures with custom messages |

## Documentation Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Prerequisites and system requirements
- Installing Syncfusion.EJ2.AspNetCore NuGet package
- Configuring TagHelpers in _ViewImports.cshtml
- Adding stylesheets and script resources via CDN
- Registering the script manager
- Creating your first Uploader
- Basic configuration and properties

### Asynchronous Upload
📄 **Read:** [references/async-upload.md](references/async-upload.md)
- Configuring SaveUrl and RemoveUrl endpoints
- Single vs multiple file upload modes
- Auto-upload settings and manual upload triggers
- Success and failure event handlers
- Progress tracking and upload lifecycle
- Handling upload cancellation

### File Validation
📄 **Read:** [references/file-validation.md](references/file-validation.md)
- File type validation with allowed-extensions
- File size constraints (min-file-size, max-file-size)
- Maximum file count restrictions
- Duplicate file detection and prevention
- Custom validation patterns and error messages
- Validation before and after upload

### Drag and Drop Support
📄 **Read:** [references/drag-drop-support.md](references/drag-drop-support.md)
- Default drag-and-drop behavior
- Configuring custom drop areas
- Multiple drop zones on single page
- Drag-drop events and event handling
- Paste-to-upload from clipboard
- Directory drag-drop support

### Templates and Styling
📄 **Read:** [references/templates-and-styling.md](references/templates-and-styling.md)
- Template property for custom UI
- Button customization and arrangement
- File list item rendering
- CSS class customization
- Theme integration (Fluent, Material, Bootstrap)
- Responsive design patterns

### Advanced Patterns
📄 **Read:** [references/advanced-patterns.md](references/advanced-patterns.md)
- Chunk upload for large files
- Form submission with file uploads
- Directory upload handling
- File removal and cleanup strategies
- Advanced error handling
- Security considerations and CORS

## Quick Start Example

### Basic Uploader with Async Upload (TagHelper)

```html
<!-- Views/Home/Index.cshtml -->
<div class="form-group">
    <ejs-uploader id="uploader">
        <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
        <e-upload-files></e-upload-files>
    </ejs-uploader>
</div>

<div id="uploadedFiles"></div>
```

### Configuration in HomeController.cs

```csharp
using Microsoft.AspNetCore.Mvc;
using System.IO;

public class HomeController : Controller
{
    private readonly IWebHostEnvironment _webHostEnvironment;

    public HomeController(IWebHostEnvironment webHostEnvironment)
    {
        _webHostEnvironment = webHostEnvironment;
    }

    [HttpPost]
    public IActionResult Save(IFormFile[] uploader)
    {
        if (uploader != null && uploader.Length > 0)
        {
            string uploadPath = Path.Combine(_webHostEnvironment.WebRootPath, "Uploads");
            if (!Directory.Exists(uploadPath))
                Directory.CreateDirectory(uploadPath);

            foreach (IFormFile file in uploader)
            {
                string filePath = Path.Combine(uploadPath, file.FileName);
                using (FileStream fs = System.IO.File.Create(filePath))
                {
                    file.CopyTo(fs);
                    fs.Flush();
                }
            }
        }
        return Ok();
    }

    [HttpPost]
    public IActionResult Remove(string[] files)
    {
        string uploadPath = Path.Combine(_webHostEnvironment.WebRootPath, "Uploads");
        
        foreach (string file in files)
        {
            string filePath = Path.Combine(uploadPath, file);
            if (System.IO.File.Exists(filePath))
                System.IO.File.Delete(filePath);
        }
        return Ok();
    }
}
```

### Configuration in _ViewImports.cshtml (one-time setup)

```html
@addTagHelper *, Syncfusion.EJ2
```

### In _Layout.cshtml (one-time setup)

> **W012 – Runtime remote-code dependency risk:** Loading scripts and styles from an external CDN means a compromised or tampered CDN file would execute in your users' browsers. Mitigate this with one of the two approaches below.
>
> **Preferred – Self-host the assets** (eliminates the runtime remote dependency entirely):
> 1. Copy `ej2.min.js` and the theme CSS from the NuGet package or a one-time CDN download into `wwwroot/lib/syncfusion/`.
> 2. Reference them with relative paths — no `integrity` attribute needed because the files are served from your own origin.
>
> **Alternative – CDN with Subresource Integrity (SRI):** If you must use the CDN, pin the file with an `integrity` hash so the browser rejects any tampered version. Compute the hash with `openssl dgst -sha384 -binary <file> | openssl base64 -A` and update it on every version upgrade.

#### Option A — Self-hosted (recommended)

```html
<head>
    <!-- Served from your own wwwroot — no external runtime dependency -->
    <link rel="stylesheet" href="~/lib/syncfusion/fluent.css" />
</head>
<body>
    @RenderBody()

    <script src="~/lib/syncfusion/ej2.min.js"></script>
    <ejs-scripts></ejs-scripts>
</body>
```

#### Option B — CDN with SRI pinning

```html
<head>
    <!-- integrity hash must match the exact file; regenerate on every version upgrade -->
    <link rel="stylesheet"
          href="https://cdn.syncfusion.com/ej2/24.1.48/fluent.css"
          integrity="sha384-REPLACE_WITH_ACTUAL_HASH_FOR_fluent.css"
          crossorigin="anonymous" />
</head>
<body>
    @RenderBody()

    <script src="https://cdn.syncfusion.com/ej2/24.1.48/dist/ej2.min.js"
            integrity="sha384-REPLACE_WITH_ACTUAL_HASH_FOR_ej2.min.js"
            crossorigin="anonymous"></script>
    <ejs-scripts></ejs-scripts>
</body>
```

## Common Patterns

### Pattern 1: Multiple File Upload with Progress

```html
<ejs-uploader id="multiUploader"
    allowed-extensions=".jpg,.png,.pdf,.doc,.docx"
    max-file-size="5242880"
    auto-upload="false"
    multiple="true"
    uploading="onUploading"
    success="onSuccess"
    failure="onFailure">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>

<script>
function onUploading(args) {
    console.log('Uploading: ' + args.file.name);
    console.log('Progress: ' + (args.percentComplete * 100) + '%');
}

function onSuccess(args) {
    if (args.operation === 'upload') {
        console.log(args.file.name + ' uploaded successfully');
    }
}

function onFailure(args) {
    console.log('Upload failed: ' + args.response);
}
</script>
```

### Pattern 2: Single File Upload with Validation

```html
<ejs-uploader id="singleUploader"
    allowed-extensions=".jpg,.png,.jpeg"
    max-file-size="2097152"
    multiple="false"
    selected="onFileSelected"
    auto-upload="true">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>

<script>
function onFileSelected(args) {
    if (args.filesData.length > 1) {
        args.filesData.splice(1);
        alert('Only one file can be selected');
    }
}
</script>
```

### Pattern 3: Drag-Drop with Custom Area

```html
<div id="dropArea" style="border: 2px dashed #ccc; padding: 20px; min-height: 200px;">
    Drop files here
</div>

<ejs-uploader id="uploader"
    drop-area="#dropArea"
    auto-upload="true">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>
```

## Key Properties

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| **allowed-extensions** | string | "" | File types to allow (e.g., ".jpg,.png") |
| **max-file-size** | long | 28.4MB | Maximum file size in bytes |
| **min-file-size** | long | 0 | Minimum file size in bytes |
| **multiple** | bool | true | Allow multiple file selection |
| **auto-upload** | bool | true | Upload files automatically after selection |
| **save-url** | string | "" | URL handler for saving files |
| **remove-url** | string | "" | URL handler for removing files |
| **drop-area** | string | "" | CSS selector for custom drop area |

## Common Use Cases

### Use Case 1: Profile Picture Upload
- Single file, image only, auto-upload
- Reference: [references/advanced-patterns.md](references/advanced-patterns.md)

### Use Case 2: Document Management
- Multiple files, validation, chunk upload for large files
- Reference: [references/async-upload.md](references/async-upload.md)

### Use Case 3: Bulk Import
- Directory upload, form submission, error handling
- Reference: [references/advanced-patterns.md](references/advanced-patterns.md)

### Use Case 4: Media Gallery
- Multiple uploads, custom templates, drag-drop
- Reference: [references/templates-and-styling.md](references/templates-and-styling.md)

---

**Next Step:** Read [references/getting-started.md](references/getting-started.md) to begin implementing Uploader in your ASP.NET Core application.
