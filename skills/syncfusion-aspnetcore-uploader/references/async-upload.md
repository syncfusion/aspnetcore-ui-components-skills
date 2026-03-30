# Asynchronous Upload in ASP.NET Core Uploader

## Table of Contents
- [Overview](#overview)
- [Configure SaveUrl and RemoveUrl](#configure-saveurl-and-removeurl)
- [Multiple File Upload](#multiple-file-upload)
- [Single File Upload](#single-file-upload)
- [Auto-Upload Settings](#auto-upload-settings)
- [Event Handlers](#event-handlers)
- [Progress Tracking](#progress-tracking)
- [Cancellation](#cancellation)

## Overview

Asynchronous upload allows files to upload without blocking the page. The Uploader sends files to server endpoints defined by `save-url` and `remove-url`, handling the entire upload lifecycle with events at each stage.

**Key Points:**
- Upload happens in background without page reload
- Multiple files upload simultaneously or sequentially
- Progress updates in real-time
- User can continue interacting with page
- Supports upload cancellation

## Configure SaveUrl and RemoveUrl

The async settings define where to send uploaded files:

```html
<ejs-uploader id="uploader">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>
```

### SaveUrl Action Handler

Receives uploaded files and saves them to disk:

```csharp
[HttpPost]
public IActionResult Save(IFormFile[] uploader)
{
    string uploadPath = Path.Combine(_webHostEnvironment.WebRootPath, "Uploads");
    if (!Directory.Exists(uploadPath))
        Directory.CreateDirectory(uploadPath);

    if (uploader != null)
    {
        foreach (IFormFile file in uploader)
        {
            if (file.Length > 0)
            {
                string filePath = Path.Combine(uploadPath, Path.GetFileName(file.FileName));
                
                // Prevent directory traversal attacks
                if (!Path.GetDirectoryName(filePath).Equals(uploadPath))
                {
                    return BadRequest("Invalid file path");
                }

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

### RemoveUrl Action Handler

Deletes uploaded files from server:

```csharp
[HttpPost]
public IActionResult Remove(string[] files)
{
    string uploadPath = Path.Combine(_webHostEnvironment.WebRootPath, "Uploads");
    
    if (files != null)
    {
        foreach (string file in files)
        {
            string filePath = Path.Combine(uploadPath, Path.GetFileName(file));
            
            if (System.IO.File.Exists(filePath))
            {
                try
                {
                    System.IO.File.Delete(filePath);
                }
                catch (Exception ex)
                {
                    return StatusCode(500, $"Error deleting file: {ex.Message}");
                }
            }
        }
    }
    return Ok();
}
```

## Multiple File Upload

Allow users to select and upload many files at once:

```html
<ejs-uploader id="multiUploader"
    multiple="true"
    auto-upload="true">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>
```

**Controller remains same as above.** The `IFormFile[]` parameter automatically handles multiple files sent in single request.

### Multiple Upload with Progress

```html
<ejs-uploader id="multiUploader"
    multiple="true"
    auto-upload="false"
    uploading="onUploading"
    success="onSuccess">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>

<script>
function onUploading(args) {
    let fileName = args.file.name;
    let progress = Math.round((args.percentComplete) * 100);
    console.log(fileName + ' progress: ' + progress + '%');
}

function onSuccess(args) {
    console.log(args.file.name + ' uploaded successfully');
}
</script>
```

## Single File Upload

Restrict upload to one file per selection:

```html
<ejs-uploader id="singleUploader"
    multiple="false"
    auto-upload="true">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>
```

**Result:** Selecting a new file replaces the previous selection. Only one file displays in file list.

## Auto-Upload Settings

### Auto-Upload Enabled (Default)

```html
<ejs-uploader id="autoUploader"
    auto-upload="true">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>
```

**Behavior:** Files upload immediately when selected (no manual trigger needed).

### Manual Upload

```html
<button id="uploadBtn">Upload Files</button>

<ejs-uploader id="manualUploader"
    auto-upload="false">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>

<script>
// Get uploader instance
var uploaderObj = document.getElementById("manualUploader").ej2_instances[0];

// Manual upload button
document.getElementById("uploadBtn").onclick = function() {
    uploaderObj.upload(uploaderObj.getFilesData());
};
</script>
```

## Event Handlers

### Upload Event Lifecycle

```
Selected → Uploading → Progress → Success/Failure → Change
```

### Success Event

Triggered when file uploads successfully:

```html
<ejs-uploader id="uploader"
    success="onSuccess">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>

<script>
function onSuccess(args) {
    // args.file - file object
    // args.operation - 'upload' or 'remove'
    // args.response - server response
    
    if (args.operation === 'upload') {
        console.log(args.file.name + ' uploaded');
        console.log('Server response: ' + args.response);
    }
    if (args.operation === 'remove') {
        console.log(args.file.name + ' removed');
    }
}
</script>
```

### Failure Event

Triggered when upload fails:

```html
<ejs-uploader id="uploader"
    failure="onFailure">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>

<script>
function onFailure(args) {
    console.log('Upload failed: ' + args.statusCode);
    console.log('Error: ' + args.response);
    
    // Display user-friendly message
    alert('Failed to upload ' + args.file.name);
}
</script>
```

### Selected Event

Triggered when files are selected:

```html
<ejs-uploader id="uploader"
    selected="onSelected">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>

<script>
function onSelected(args) {
    // args.filesData - array of selected files
    
    args.filesData.forEach(function(file) {
        console.log('Selected: ' + file.name + ' (' + file.size + ' bytes)');
    });
}
</script>
```

## Progress Tracking

### Uploading Event

Fires continuously during upload with progress percentage:

```html
<ejs-uploader id="uploader"
    uploading="onUploading">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>

<script>
function onUploading(args) {
    let percentComplete = Math.round(args.percentComplete * 100);
    console.log(args.file.name + ': ' + percentComplete + '% uploaded');
    
    // Update custom progress bar
    document.getElementById('progressBar').style.width = percentComplete + '%';
}
</script>
```

### Custom Progress Display

```html
<div style="margin-bottom: 10px;">
    <div style="height: 5px; width: 100%; background: #e0e0e0; border-radius: 5px;">
        <div id="progressBar" style="height: 100%; background: #4caf50; border-radius: 5px; width: 0%; transition: width 0.3s;"></div>
    </div>
    <span id="progressText">0%</span>
</div>

<ejs-uploader id="uploader"
    uploading="updateProgress">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>

<script>
function updateProgress(args) {
    let percent = Math.round(args.percentComplete * 100);
    document.getElementById('progressBar').style.width = percent + '%';
    document.getElementById('progressText').textContent = percent + '%';
}
</script>
```

## Cancellation

### Cancel Upload

Stop upload in progress:

```html
<ejs-uploader id="uploader"
    uploading="onUploading">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>

<script>
function onUploading(args) {
    // Cancel upload if file size exceeds 10MB
    if (args.file.size > 10485760) {
        args.cancel = true;
        alert('File too large. Maximum 10MB allowed.');
    }
}

// Or cancel specific file
var uploaderObj = document.getElementById("uploader").ej2_instances[0];
function cancelUpload() {
    uploaderObj.cancel();
}
</script>
```

### Remove During Upload

Remove files that are uploading:

```html
<button id="removeBtn">Remove File</button>

<ejs-uploader id="uploader">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>

<script>
// Get uploader instance
var uploaderObj = document.getElementById("uploader").ej2_instances[0];

// Remove specific file
document.getElementById("removeBtn").onclick = function() {
    var fileList = uploaderObj.getFilesData();
    if (fileList.length > 0) {
        uploaderObj.remove(fileList[0]);
    }
};
</script>
```

## Edge Cases

### Network Timeout

Handle when upload takes too long:

```html
<ejs-uploader id="uploader"
    failure="onFailure">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>

<script>
function onFailure(args) {
    if (args.statusCode === 0) {
        console.log('Network timeout or connection lost');
    }
}
</script>
```

### Duplicate File Upload

Same file uploaded twice in same session:

```html
<ejs-uploader id="uploader"
    selected="onSelected">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>

<script>
var uploadedFiles = {};

function onSelected(args) {
    args.filesData.forEach(function(file) {
        if (uploadedFiles[file.name]) {
            args.cancel = true;
            alert(file.name + ' already uploaded');
        }
    });
}
</script>
```

## Summary

| Task | Code |
|------|------|
| Enable auto-upload | `auto-upload="true"` |
| Disable auto-upload | `auto-upload="false"` |
| Multiple files | `multiple="true"` |
| Single file | `multiple="false"` |
| Track progress | `uploading="handler"` |
| Handle success | `success="handler"` |
| Handle failure | `failure="handler"` |
| Cancel upload | `uploaderObj.cancel()` |
