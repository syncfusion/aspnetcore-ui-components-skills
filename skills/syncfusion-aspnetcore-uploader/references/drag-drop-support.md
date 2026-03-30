# Drag and Drop Support in ASP.NET Core Uploader

## Table of Contents
- [Overview](#overview)
- [Default Drag-Drop](#default-drag-drop)
- [Custom Drop Area](#custom-drop-area)
- [Multiple Drop Zones](#multiple-drop-zones)
- [Drop Events](#drop-events)
- [Paste Upload](#paste-upload)
- [Directory Upload](#directory-upload)

## Overview

Drag-and-drop functionality allows users to select files without clicking browse button. By default, the entire Uploader acts as a drop area. You can customize drop areas using the `drop-area` property.

**Key Features:**
- Default drop area is the Uploader itself
- Customize any element as drop area
- Visual feedback during drag-over
- Multiple drop zones possible
- Paste from clipboard

## Default Drag-Drop

By default, the Uploader enables drag-and-drop:

```html
<ejs-uploader id="uploader">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>
```

**Result:**
1. User drags file from file explorer
2. Hovers over Uploader
3. Visual feedback shows drop area active
4. Drops file into Uploader
5. File uploads automatically (if auto-upload enabled)

### Disable Drag-Drop

To disable drag-and-drop and allow only browse button:

```html
<ejs-uploader id="uploader"
    allow-drag-and-drop="false">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>
```

## Custom Drop Area

Define any HTML element as drop area using `drop-area` property:

### Single Custom Drop Area

```html
<div id="dropZone" style="border: 2px dashed #ccc; padding: 20px; min-height: 200px; text-align: center;">
    <p>Drop files here</p>
</div>

<ejs-uploader id="uploader"
    drop-area="#dropZone">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>
```

**Result:**
- Uploader itself is no longer a drop area
- Only the #dropZone element accepts drops
- Visual feedback on #dropZone during drag-over

### Styled Drop Area

```html
<style>
#dropZone {
    border: 2px dashed #2196F3;
    border-radius: 8px;
    padding: 40px;
    text-align: center;
    background-color: #f5f5f5;
    cursor: pointer;
    transition: all 0.3s ease;
}

#dropZone.drag-over {
    background-color: #e3f2fd;
    border-color: #1976D2;
    box-shadow: 0 0 10px rgba(33, 150, 243, 0.3);
}

#dropZone p {
    margin: 0;
    color: #666;
    font-size: 16px;
}
</style>

<div id="dropZone">
    <svg style="width: 48px; height: 48px; color: #2196F3; margin-bottom: 10px;" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"></path>
        <polyline points="17 8 12 3 7 8"></polyline>
        <line x1="12" y1="3" x2="12" y2="15"></line>
    </svg>
    <p>Drag files here or click to browse</p>
</div>

<ejs-uploader id="uploader"
    drop-area="#dropZone">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>
```

## Multiple Drop Zones

Create multiple areas that accept drops:

```html
<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-bottom: 20px;">
    <div id="dropZone1" style="border: 2px dashed #2196F3; padding: 20px; min-height: 150px;">
        <p>Images Drop Zone</p>
    </div>
    <div id="dropZone2" style="border: 2px dashed #4CAF50; padding: 20px; min-height: 150px;">
        <p>Documents Drop Zone</p>
    </div>
</div>

<!-- Images Uploader -->
<ejs-uploader id="imageUploader"
    drop-area="#dropZone1"
    allowed-extensions=".jpg,.png,.gif">
    <e-async-settings save-url="Home/SaveImage" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>

<!-- Documents Uploader -->
<ejs-uploader id="docUploader"
    drop-area="#dropZone2"
    allowed-extensions=".pdf,.doc,.docx">
    <e-async-settings save-url="Home/SaveDocument" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>
```

## Drop Events

Handle drag and drop events:

### Drag-Over Event

Triggered when files are dragged over drop area:

```html
<ejs-uploader id="uploader"
    drag-over="onDragOver"
    drag-leave="onDragLeave"
    drop="onDrop">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>

<script>
function onDragOver(args) {
    // args.event - native drag event
    console.log('Drag over: ' + args.files.length + ' files');
    
    // Highlight drop area
    document.getElementById('uploader').style.backgroundColor = '#f0f0f0';
}

function onDragLeave(args) {
    console.log('Drag left');
    
    // Remove highlight
    document.getElementById('uploader').style.backgroundColor = 'white';
}

function onDrop(args) {
    console.log('Dropped: ' + args.files.length + ' files');
}
</script>
```

### File Type Filter on Drop

```html
<ejs-uploader id="uploader"
    drop="onDrop">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>

<script>
function onDrop(args) {
    // Filter only images
    var imageFiles = args.files.filter(function(file) {
        return file.type.startsWith('image/');
    });
    
    if (imageFiles.length === 0) {
        args.cancel = true;
        alert('Only image files are allowed');
    }
}
</script>
```

## Paste Upload

Upload images from clipboard:

### Paste Image to Upload

```html
<div id="pasteArea" style="border: 2px dashed #2196F3; padding: 20px; min-height: 100px;">
    Click here or paste an image (Ctrl+V)
</div>

<script>
document.getElementById('pasteArea').addEventListener('paste', function(event) {
    event.preventDefault();
    
    var items = event.clipboardData.items;
    
    for (var i = 0; i < items.length; i++) {
        if (items[i].type.startsWith('image/')) {
            var file = items[i].getAsFile();
            
            // Create FormData and upload
            var formData = new FormData();
            formData.append('uploader', file);
            
            fetch('Home/Save', {
                method: 'POST',
                body: formData
            })
            .then(response => response.json())
            .then(data => {
                console.log('Pasted image uploaded');
                alert('Image pasted and uploaded successfully');
            })
            .catch(error => console.error('Error:', error));
        }
    }
});
</script>
```

### Paste with Uploader Integration

```html
<ejs-uploader id="uploader"
    selected="onFileSelected">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>

<script>
document.addEventListener('paste', function(event) {
    var items = event.clipboardData.items;
    
    for (var i = 0; i < items.length; i++) {
        if (items[i].type.startsWith('image/')) {
            var file = items[i].getAsFile();
            
            // Get uploader instance
            var uploader = document.getElementById('uploader').ej2_instances[0];
            
            // Add to uploader and upload
            // Note: Direct paste handling varies by browser
            console.log('Image pasted: ' + file.name);
        }
    }
});
</script>
```

## Directory Upload

Upload entire folders:

### Enable Directory Upload

```html
<ejs-uploader id="uploader"
    directory-upload="true"
    multiple="true">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>
```

**Result:** Users can drag entire folders, and all files upload maintaining directory structure.

### Handle Directory Structure on Server

```csharp
[HttpPost]
public IActionResult Save(IFormFile[] uploader)
{
    string baseUploadPath = Path.Combine(_webHostEnvironment.WebRootPath, "Uploads");
    if (!Directory.Exists(baseUploadPath))
        Directory.CreateDirectory(baseUploadPath);

    if (uploader != null)
    {
        foreach (IFormFile file in uploader)
        {
            if (file.Length > 0)
            {
                // Get relative path from upload form data
                string relativePath = file.Name; // May contain folder structure
                
                string filePath = Path.Combine(baseUploadPath, relativePath);
                
                // Create directory if it doesn't exist
                string directoryPath = Path.GetDirectoryName(filePath);
                if (!Directory.Exists(directoryPath))
                    Directory.CreateDirectory(directoryPath);

                using (FileStream fs = System.IO.File.Create(filePath))
                {
                    file.CopyTo(fs);
                    fs.Flush();
                }
            }
        }
    }
    return Ok();
}
```

## Edge Cases

### Large File Drag-Drop

Handle large files efficiently:

```html
<ejs-uploader id="uploader"
    max-file-size="1073741824"
    drop="onDrop">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove" chunk-size="524288"></e-async-settings>
</ejs-uploader>

<script>
function onDrop(args) {
    args.files.forEach(function(file) {
        if (file.size > 1073741824) {
            args.cancel = true;
            alert('File too large. Maximum 1GB allowed');
        }
    });
}
</script>
```

### Prevent Default Drop Behavior

```javascript
document.addEventListener('dragover', function(e) {
    e.preventDefault();
    e.stopPropagation();
});

document.addEventListener('drop', function(e) {
    e.preventDefault();
    e.stopPropagation();
});
```

## Summary

| Feature | Code |
|---------|------|
| Default drop | No additional config needed |
| Custom drop area | `drop-area="#elementId"` |
| Disable drag-drop | `allow-drag-and-drop="false"` |
| Drag-over event | `drag-over="handler"` |
| Drop event | `drop="handler"` |
| Directory upload | `directory-upload="true"` |
| Paste upload | Handle clipboard events |
