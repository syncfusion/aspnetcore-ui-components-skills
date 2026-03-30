# Image Upload Restrictions and Validation

## Table of Contents
- [UploadSettings Configuration](#uploadsettings-configuration)
- [File Extension Restrictions](#file-extension-restrictions)
- [File Size Restrictions](#file-size-restrictions)
- [Default Behavior](#default-behavior)
- [Practical Examples](#practical-examples)

## UploadSettings Configuration

The `UploadSettings` property controls file validation when uploading images.

### Default Allowed Extensions

If `UploadSettings` is not defined, Image Editor allows by default:

- `.jpg` - JPEG format
- `.png` - PNG format
- `.svg` - SVG format
- `.webp` - WebP format
- `.bmp` - BMP format

No file size restrictions by default.

## File Extension Restrictions

### Specify Allowed Extensions

```html
<ejs-imageeditor id="imageEditor"
    uploadSettings='new { AllowedExtensions = ".png, .svg" }'>
</ejs-imageeditor>
```

This allows only PNG and SVG files.

### Extension Format

Extensions must be specified as comma-separated string with dots:

```html
<!-- ✅ Correct format -->
<ejs-imageeditor uploadSettings='new { AllowedExtensions = ".jpg, .png" }'>
</ejs-imageeditor>

<!-- ❌ Incorrect format -->
<ejs-imageeditor uploadSettings='new { AllowedExtensions = "jpg, png" }'>
</ejs-imageeditor>
```

### Common Extension Restrictions

```html
<!-- Photo editing (JPEG preferred) -->
<ejs-imageeditor 
    uploadSettings='new { AllowedExtensions = ".jpg, .jpeg, .png" }'>
</ejs-imageeditor>

<!-- Web graphics only -->
<ejs-imageeditor 
    uploadSettings='new { AllowedExtensions = ".png, .webp, .svg" }'>
</ejs-imageeditor>

<!-- All formats -->
<ejs-imageeditor 
    uploadSettings='new { AllowedExtensions = ".jpg, .jpeg, .png, .bmp, .svg, .webp" }'>
</ejs-imageeditor>
```

## File Size Restrictions

### Minimum File Size

Set minimum file size in bytes:

```html
<ejs-imageeditor id="imageEditor"
    uploadSettings='new { MinFileSize = 10240 }'>
</ejs-imageeditor>
```

This requires minimum 10 KB (10240 bytes).

### Maximum File Size

Set maximum file size in bytes:

```html
<ejs-imageeditor id="imageEditor"
    uploadSettings='new { MaxFileSize = 5242880 }'>
</ejs-imageeditor>
```

This allows maximum 5 MB (5242880 bytes).

### Combined Restrictions

```html
<ejs-imageeditor id="imageEditor"
    uploadSettings='new { 
        MinFileSize = 1024,          <!-- 1 KB minimum -->
        MaxFileSize = 2097152        <!-- 2 MB maximum -->
    }'>
</ejs-imageeditor>
```

## Complete UploadSettings Example

```csharp
// C# Model
public class ImageEditorSettings
{
    public string AllowedExtensions { get; set; }
    public int MinFileSize { get; set; }
    public int MaxFileSize { get; set; }
}

// In Controller
var settings = new ImageEditorSettings
{
    AllowedExtensions = ".jpg, .png",
    MinFileSize = 5120,      // 5 KB
    MaxFileSize = 10485760   // 10 MB
};
```

```html
<!-- In Razor Page -->
<ejs-imageeditor id="imageEditor"
    uploadSettings='new { 
        AllowedExtensions = ".jpg, .png",
        MinFileSize = 5120,
        MaxFileSize = 10485760
    }'>
</ejs-imageeditor>
```

## Error Messages

When file doesn't meet restrictions, users see alert messages:

- **File too small** - "File size is below the minimum size"
- **File too large** - "File size exceeds the maximum size"
- **Invalid format** - "File type is not supported"

## Default Behavior

When `UploadSettings` is not defined:

| Setting | Default | Behavior |
|---------|---------|----------|
| **AllowedExtensions** | Not set | All 5 formats allowed |
| **MinFileSize** | Not set | No minimum |
| **MaxFileSize** | Not set | No maximum |

## Practical Examples

### Strict Photo Upload

```html
<ejs-imageeditor id="photoEditor"
    uploadSettings='new { 
        AllowedExtensions = ".jpg, .jpeg",
        MinFileSize = 100000,    <!-- 100 KB -->
        MaxFileSize = 5242880    <!-- 5 MB -->
    }'>
</ejs-imageeditor>

<!-- Users can only upload JPEG files between 100 KB and 5 MB -->
```

### Web Graphic Editor

```html
<ejs-imageeditor id="webGraphicsEditor"
    uploadSettings='new { 
        AllowedExtensions = ".png, .webp, .svg",
        MaxFileSize = 1048576    <!-- 1 MB -->
    }'>
</ejs-imageeditor>
```

### Flexible Document Scanning

```html
<ejs-imageeditor id="scannerEditor"
    uploadSettings='new { 
        AllowedExtensions = ".png, .jpg, .bmp",
        MinFileSize = 10240,     <!-- 10 KB minimum -->
        MaxFileSize = 10485760   <!-- 10 MB maximum -->
    }'>
</ejs-imageeditor>
```

### Mobile-Optimized

```html
<ejs-imageeditor id="mobileEditor"
    uploadSettings='new { 
        AllowedExtensions = ".jpg, .webp",
        MaxFileSize = 2097152    <!-- 2 MB for mobile networks -->
    }'>
</ejs-imageeditor>
```

## Base64 Upload Validation

When uploading images as Base64 strings:

- **Extension validation:** Bypassed (no file extension)
- **Size restrictions:** Still apply (check blob size)

```javascript
function uploadBase64(base64String) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Base64 string uploaded - extension not validated
    // But size restrictions still apply if configured
    imageEditor.open(base64String);
}
```

## Handling Upload Errors

### Display User-Friendly Messages

```html
<div id="uploadError" style="display:none; color: red;"></div>

<input type="file" id="imageInput" onchange="handleUpload(event)">

<script>
function handleUpload(event) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var file = event.target.files[0];
    
    var settings = imageEditor.uploadSettings;
    var errorDiv = document.getElementById('uploadError');
    
    // Check extension
    var extension = '.' + file.name.split('.').pop().toLowerCase();
    if (settings.AllowedExtensions && 
        !settings.AllowedExtensions.includes(extension)) {
        errorDiv.textContent = 'File type not allowed. Use: ' + 
            settings.AllowedExtensions;
        errorDiv.style.display = 'block';
        return;
    }
    
    // Check size
    if (settings.MinFileSize && file.size < settings.MinFileSize) {
        errorDiv.textContent = 'File too small. Minimum: ' + 
            (settings.MinFileSize / 1024) + ' KB';
        errorDiv.style.display = 'block';
        return;
    }
    
    if (settings.MaxFileSize && file.size > settings.MaxFileSize) {
        errorDiv.textContent = 'File too large. Maximum: ' + 
            (settings.MaxFileSize / (1024 * 1024)) + ' MB';
        errorDiv.style.display = 'block';
        return;
    }
    
    // All validations passed
    errorDiv.style.display = 'none';
    
    var reader = new FileReader();
    reader.onload = function(e) {
        imageEditor.open(e.target.result);
    };
    reader.readAsDataURL(file);
}
</script>
```

## Size Constants

Common file size conversions for reference:

```javascript
const SIZES = {
    '1 KB': 1024,
    '10 KB': 10 * 1024,
    '100 KB': 100 * 1024,
    '1 MB': 1024 * 1024,
    '2 MB': 2 * 1024 * 1024,
    '5 MB': 5 * 1024 * 1024,
    '10 MB': 10 * 1024 * 1024,
    '50 MB': 50 * 1024 * 1024,
    '100 MB': 100 * 1024 * 1024
};

// Usage:
uploadSettings='new { MaxFileSize = 5242880 }'  // 5 MB
uploadSettings='new { MaxFileSize = 1048576 }'  // 1 MB
```

## Best Practices

### 1. **Set Realistic Limits**

```html
<!-- ✅ Reasonable limits -->
<ejs-imageeditor uploadSettings='new {
    AllowedExtensions = ".jpg, .png",
    MaxFileSize = 5242880  }'> <!-- 5 MB -->
</ejs-imageeditor>

<!-- ❌ Too restrictive -->
<ejs-imageeditor uploadSettings='new {
    MaxFileSize = 102400  }'> <!-- 100 KB - too small -->
</ejs-imageeditor>
```

### 2. **Inform Users**

```html
<div class="upload-info">
    <p>Accepted formats: JPG, PNG</p>
    <p>Maximum file size: 5 MB</p>
</div>

<input type="file" accept=".jpg, .jpeg, .png">
```

### 3. **Match HTML Input to Server Rules**

```html
<!-- HTML file input matches server validation -->
<input type="file" accept=".jpg, .jpeg, .png" 
    id="imageUpload" onchange="uploadImage(event)">

<!-- C# Server: AllowedExtensions = ".jpg, .jpeg, .png" -->
```

### 4. **Provide Alternative Upload Methods**

```javascript
// If direct upload fails, allow Base64 paste
function pasteBase64() {
    var base64 = prompt('Paste Base64 image:');
    if (base64) {
        var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
        imageEditor.open(base64);
    }
}
```

## Troubleshooting

**Issue:** All files rejected
- **Solution:** Check AllowedExtensions format (.jpg, .png with dots)
- **Solution:** Verify file sizes are within limits

**Issue:** Specific file format rejected
- **Solution:** Verify extension in AllowedExtensions list
- **Solution:** Check case sensitivity of extensions

**Issue:** Users can't upload large files
- **Solution:** Increase MaxFileSize setting
- **Solution:** Compress images client-side before upload

**Issue:** Very small files rejected
- **Solution:** Lower MinFileSize or remove it
- **Solution:** Verify actual file size vs. minsize setting
