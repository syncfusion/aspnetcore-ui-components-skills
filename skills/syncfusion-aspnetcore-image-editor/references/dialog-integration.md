# Rendering Image Editor in a Dialog

## Overview

Rendering the Image Editor in a modal dialog provides a focused editing experience without navigating away from the current page. This is useful for pop-up editors, modal workflows, and side-panel editing.

## Basic Dialog Implementation

```html
<!-- Dialog Component -->
<ejs-dialog id="imageEditorDialog" 
    header="Image Editor"
    showCloseIcon="true"
    width="800px"
    height="600px"
    visible="false">
    
    <!-- Image Editor inside Dialog -->
    <ejs-imageeditor id="imageEditor"></ejs-imageeditor>
</ejs-dialog>

<!-- Button to open dialog -->
<button onclick="openImageEditor()">Open Image Editor</button>

<script>
function openImageEditor() {
    var dialog = document.getElementById('imageEditorDialog').ej2_instances[0];
    dialog.show();
}
</script>
```

## Dialog Lifecycle

### Open Dialog

```javascript
function openDialog() {
    var dialog = document.getElementById('imageEditorDialog').ej2_instances[0];
    
    // Show dialog
    dialog.show();
    
    // Load image
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.open('sample.jpg');
}
```

### Close Dialog

```javascript
function closeDialog() {
    var dialog = document.getElementById('imageEditorDialog').ej2_instances[0];
    
    // Close dialog
    dialog.hide();
}
```

### Clear Image on Close

When closing the dialog, clear the image to ensure a fresh state on re-open:

```html
<ejs-dialog id="imageEditorDialog" 
    header="Image Editor"
    showCloseIcon="true"
    beforeClose="OnBeforeClose">
    
    <ejs-imageeditor id="imageEditor"></ejs-imageeditor>
</ejs-dialog>

<script>
function OnBeforeClose(args) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Clear image to prevent residual state
    imageEditor.clearImage();
}
</script>
```

## Complete Dialog Example

```html
<!-- Page content -->
<div class="main-content">
    <h1>My Application</h1>
    <button onclick="openImageEditor()">Edit Image</button>
</div>

<!-- Image Editor Dialog -->
<ejs-dialog id="imageEditorDialog" 
    header="Image Editor"
    showCloseIcon="true"
    width="900px"
    height="700px"
    visible="false"
    beforeClose="OnDialogBeforeClose">
    
    <div class="dialog-content">
        <ejs-imageeditor id="imageEditor" 
            width="100%"
            height="600px"
            toolbar="@new List<string> {
                'Open', 'Crop', 'RotateLeft', 'RotateRight',
                'Filter', 'Annotate', 'Reset', 'Save'
            }">
        </ejs-imageeditor>
    </div>
    
    <!-- Dialog buttons -->
    <div class="dialog-buttons">
        <button onclick="saveAndClose()">Save & Close</button>
        <button onclick="closeDialog()">Close</button>
    </div>
</ejs-dialog>

<script>
function openImageEditor() {
    var dialog = document.getElementById('imageEditorDialog').ej2_instances[0];
    dialog.show();
    
    // Load default image
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.open('default.jpg');
}

function closeDialog() {
    var dialog = document.getElementById('imageEditorDialog').ej2_instances[0];
    dialog.hide();
}

function saveAndClose() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Save image
    imageEditor.toBlob(function(blob) {
        // Process blob (upload, display, etc.)
        console.log('Image saved:', blob);
        
        // Close dialog
        closeDialog();
    });
}

function OnDialogBeforeClose(args) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.clearImage();
}
</script>
```

## ClearImage Method

The `clearImage()` method clears the current image and resets the editor to empty state. This is important when using Image Editor in dialogs.

### Why Clear Image?

```javascript
// ❌ Problem: Image remains in memory
function BadPractice() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    // User opens dialog, edits image A
    // User closes dialog without clearing
    // User opens dialog again - Image A still visible!
}

// ✅ Solution: Clear image on close
function GoodPractice() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.open('imageA.jpg');
    // ... user edits ...
    // Before closing dialog:
    imageEditor.clearImage();  // Clear for next use
}
```

## Modal Image Upload

```html
<button onclick="uploadImage()">Upload Image</button>

<ejs-dialog id="uploadDialog"
    header="Upload and Edit"
    width="600px">
    
    <input type="file" id="imageUpload" accept="image/*" 
        onchange="loadUploadedImage(event)">
    
    <ejs-imageeditor id="imageEditor"></ejs-imageeditor>
    
    <button onclick="saveAndExit()">Save</button>
</ejs-dialog>

<script>
function uploadImage() {
    var dialog = document.getElementById('uploadDialog').ej2_instances[0];
    dialog.show();
}

function loadUploadedImage(event) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var file = event.target.files[0];
    
    var reader = new FileReader();
    reader.onload = function(e) {
        imageEditor.open(e.target.result);
    };
    reader.readAsDataURL(file);
}

function saveAndExit() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    imageEditor.toBlob(function(blob) {
        // Save blob
        saveToServer(blob);
        
        // Clear and close
        imageEditor.clearImage();
        document.getElementById('uploadDialog').ej2_instances[0].hide();
    });
}
</script>
```

## Multi-Page Image Editor

```html
<ejs-dialog id="multiPageDialog"
    header="Edit Multiple Images"
    width="800px"
    height="700px">
    
    <div class="image-list">
        <h3>Images to Edit:</h3>
        <ul id="imageList"></ul>
    </div>
    
    <div class="editor-panel">
        <ejs-imageeditor id="pageEditor"></ejs-imageeditor>
        <button onclick="previousImage()">← Previous</button>
        <button onclick="nextImage()">Next →</button>
    </div>
</ejs-dialog>

<script>
var images = ['image1.jpg', 'image2.jpg', 'image3.jpg'];
var currentIndex = 0;

function showImage(index) {
    var imageEditor = document.getElementById('pageEditor').ej2_instances[0];
    imageEditor.clearImage();
    imageEditor.open(images[index]);
    currentIndex = index;
}

function previousImage() {
    if (currentIndex > 0) {
        showImage(currentIndex - 1);
    }
}

function nextImage() {
    if (currentIndex < images.length - 1) {
        showImage(currentIndex + 1);
    }
}
</script>
```

## Responsive Dialog

```html
<ejs-dialog id="imageEditorDialog"
    header="Image Editor"
    width="@(ViewBag.IsMobile ? '100%' : '800px')"
    height="@(ViewBag.IsMobile ? '100%' : '600px')"
    allowDragging="@(!ViewBag.IsMobile)"
    isModal="true">
    
    <ejs-imageeditor id="imageEditor"></ejs-imageeditor>
</ejs-dialog>
```

## Best Practices

### 1. **Always Clear Image on Close**

```javascript
function OnDialogClose(args) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.clearImage();  // Essential!
}
```

### 2. **Set Appropriate Dialog Size**

```html
<!-- Desktop: Large dialog -->
<ejs-dialog id="imageEditorDialog" width="900px" height="700px">

<!-- Mobile: Full screen -->
<ejs-dialog id="imageEditorDialog" width="100%" height="100%">
```

### 3. **Provide Clear Save/Cancel**

```html
<div class="dialog-footer">
    <button onclick="save()">Save Changes</button>
    <button onclick="cancel()">Cancel</button>
</div>
```

### 4. **Handle Dialog State**

```javascript
var editorState = {
    originalImage: null,
    isModified: false
};

function trackChanges() {
    editorState.isModified = true;
}

function onDialogClose() {
    if (editorState.isModified && !confirm('Discard changes?')) {
        return false;  // Prevent close
    }
}
```

## Troubleshooting

**Issue:** Previous image still visible when opening dialog
- **Solution:** Call `clearImage()` when closing dialog
- **Solution:** Clear in `beforeClose` event handler

**Issue:** Dialog overlaps with Image Editor
- **Solution:** Ensure Image Editor width/height are set to 100%
- **Solution:** Adjust dialog padding/margins

**Issue:** Image Editor buttons not responding in dialog
- **Solution:** Verify dialog allows pointer events
- **Solution:** Check z-index settings

**Issue:** Save not working in dialog
- **Solution:** Ensure `toBlob()` completes before closing dialog
- **Solution:** Use async/await for proper sequencing
