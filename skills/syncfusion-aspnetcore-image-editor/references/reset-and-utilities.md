# Reset and Utility Methods

## Table of Contents
- [reset() Method](#reset-method)
- [clearImage() Method](#clearimage-method)
- [Comparison: reset() vs clearImage()](#comparison-reset-vs-clearimage)
- [Utility Methods](#utility-methods)
- [Practical Patterns](#practical-patterns)

## reset() Method

Revert all changes to the current image and restore the original.

### Basic Usage

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Reset all changes
imageEditor.reset();
```

### What reset() Does

- ✅ Removes all annotations (text, shapes, lines, etc.)
- ✅ Removes all filters and effects
- ✅ Resets fine-tune adjustments (brightness, contrast, etc.)
- ✅ Removes frames and redactions
- ✅ Restores original zoom level
- ✅ Restores to original rotation/flip state
- ✅ Clears undo/redo history

### Example: Reset Button

```html
<button onclick="resetImage()" class="btn btn-secondary">Reset Image</button>

<script>
function resetImage() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Confirm before reset
    if (confirm('Are you sure? All changes will be lost.')) {
        imageEditor.reset();
        console.log('Image reset to original');
    }
}
</script>
```

### Reset on New Image Open

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

imageEditor.imageLoading = function(args) {
    // Auto-reset when new image loads
    imageEditor.reset();
};

// Or open new image:
function openNewImage() {
    imageEditor.reset();  // Clear previous state
    imageEditor.open(imageUrl);
}
```

## clearImage() Method

Completely clear the Image Editor - remove the current image.

### Basic Usage

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Clear the editor
imageEditor.clearImage();
```

### What clearImage() Does

- ✅ Removes the currently displayed image
- ✅ Clears all editor state
- ✅ Resets to empty/blank editor
- ✅ Closes any open dialogs
- ✅ Prevents residual state in dialogs

### Example: Clear Button

```html
<button onclick="clearEditor()" class="btn btn-danger">Clear Image</button>

<script>
function clearEditor() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.clearImage();
    console.log('Image cleared');
}
</script>
```

## Comparison: reset() vs clearImage()

| Aspect | reset() | clearImage() |
|--------|---------|-------------|
| **Image Removal** | ❌ Image stays | ✅ Image removed |
| **Annotation Removal** | ✅ Removed | ✅ Removed |
| **Editor State** | Resets to original | Completely empty |
| **Use Case** | Undo all edits | Prepare for new image |
| **When to Use** | User wants original version | Dialog close, new image load |

### Decision Tree

```
Need to remove everything? → YES → Use clearImage()
                          → NO ↓

Just revert edits?        → YES → Use reset()
                          → NO ↓

Keep current image?       → YES → No action needed
                          → NO ↓
```

## Utility Methods

### Get Image Data

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Get current image as Blob
imageEditor.toBlob(function(blob) {
    console.log('Image blob:', blob);
});

// Get as Base64 string
imageEditor.getImageData(function(data) {
    console.log('Base64:', data);
});
```

### Check Current Image

```javascript
// Get current image source
function getCurrentImage() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Access internal image element
    var canvasElement = imageEditor.baseElement.querySelector('canvas');
    if (canvasElement) {
        console.log('Image loaded');
        return true;
    }
    return false;
}
```

### Get Dimensions

```javascript
function getImageDimensions() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Get canvas dimensions
    var canvas = imageEditor.baseElement.querySelector('canvas');
    if (canvas) {
        return {
            width: canvas.width,
            height: canvas.height
        };
    }
    return null;
}
```

### Get Undo/Redo State

```javascript
function getHistoryState() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    return {
        canUndo: imageEditor.canUndo,      // Boolean
        canRedo: imageEditor.canRedo,      // Boolean
        historyCount: imageEditor.historyCount  // Number
    };
}
```

## Practical Patterns

### Pattern 1: Dialog Cleanup

Always clear image when closing dialog:

```html
<!-- Dialog Container -->
<div id="editorDialog" class="modal">
    <div class="modal-content">
        <span class="close" onclick="closeEditorDialog()">&times;</span>
        
        <ejs-imageeditor id="dialogImageEditor"></ejs-imageeditor>
        
        <button onclick="saveImage()" class="btn btn-primary">Save</button>
        <button onclick="closeEditorDialog()" class="btn btn-secondary">Close</button>
    </div>
</div>

<script>
function closeEditorDialog() {
    var imageEditor = document.getElementById('dialogImageEditor').ej2_instances[0];
    
    // Clear image before closing - prevents residual state
    imageEditor.clearImage();
    
    // Close dialog
    document.getElementById('editorDialog').style.display = 'none';
}
</script>
```

### Pattern 2: Edit Workflow

```javascript
function startEditSession(imageUrl) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Clear any previous session
    imageEditor.clearImage();
    
    // Load new image
    imageEditor.open(imageUrl);
}

function discardChanges() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Reset to original loaded image
    imageEditor.reset();
}

function deleteCurrentImage() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Remove completely
    imageEditor.clearImage();
}
```

### Pattern 3: Multi-Image Editor

```javascript
var currentImageIndex = 0;
var images = [
    'image1.jpg',
    'image2.jpg',
    'image3.jpg'
];

function switchImage(index) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Clear previous image
    imageEditor.clearImage();
    
    // Load new image
    imageEditor.open(images[index]);
    currentImageIndex = index;
}

function previousImage() {
    if (currentImageIndex > 0) {
        switchImage(currentImageIndex - 1);
    }
}

function nextImage() {
    if (currentImageIndex < images.length - 1) {
        switchImage(currentImageIndex + 1);
    }
}
```

### Pattern 4: Save with Reset Option

```javascript
function saveWithReset() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Save current state
    imageEditor.toBlob(function(blob) {
        // Upload or process blob
        uploadImage(blob);
    });
    
    // Reset after save
    imageEditor.reset();
}

function saveAndClear() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Save
    imageEditor.toBlob(function(blob) {
        uploadImage(blob);
    });
    
    // Clear for new image
    imageEditor.clearImage();
}
```

### Pattern 5: Reset with History Tracking

```html
<div class="editor-controls">
    <button onclick="undoLastAction()" class="btn">Undo</button>
    <button onclick="redoLastAction()" class="btn">Redo</button>
    <button onclick="resetToOriginal()" class="btn btn-warning">Reset</button>
    <button onclick="clearForNew()" class="btn btn-danger">Clear</button>
</div>

<script>
function undoLastAction() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    if (imageEditor.canUndo) {
        imageEditor.undo();
        updateHistoryUI();
    }
}

function redoLastAction() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    if (imageEditor.canRedo) {
        imageEditor.redo();
        updateHistoryUI();
    }
}

function resetToOriginal() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    if (confirm('Reset all changes?')) {
        imageEditor.reset();
        updateHistoryUI();
    }
}

function clearForNew() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    if (confirm('Clear current image?')) {
        imageEditor.clearImage();
        updateHistoryUI();
    }
}

function updateHistoryUI() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Update button states
    document.querySelector('[onclick="undoLastAction()"]').disabled = 
        !imageEditor.canUndo;
    document.querySelector('[onclick="redoLastAction()"]').disabled = 
        !imageEditor.canRedo;
}
</script>
```

## Common Patterns Summary

### When to Use reset()
- ✅ User clicks "Reset" button
- ✅ Revert all edits before save
- ✅ Start fresh with same image
- ✅ Undo all changes at once

### When to Use clearImage()
- ✅ Dialog is closing
- ✅ Switching to different image
- ✅ Clearing editor for new upload
- ✅ Leaving edit mode
- ✅ Preventing residual state in multi-use editors

### Combined Usage

```javascript
function prepareForNewImage() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Best practice: Always clear first
    imageEditor.clearImage();
    
    // Then load new image
    imageEditor.open(newImageUrl);
}
```

## Best Practices

### 1. **Always Clear Dialogs**

```javascript
// ✅ Correct - clears before close
function closeDialog() {
    imageEditor.clearImage();
    dialog.close();
}

// ❌ Incorrect - doesn't clear
function closeDialog() {
    dialog.close();  // Previous image may still be there
}
```

### 2. **Use reset() for User Actions**

```javascript
// ✅ Good UX - user expects to keep image
button.text = "Reset Image";
imageEditor.reset();

// ❌ Unexpected - removes image when user expects reset
imageEditor.clearImage();
```

### 3. **Confirm Destructive Actions**

```javascript
function resetImage() {
    if (confirm('Discard all changes?')) {
        imageEditor.reset();
    }
}

function clearImage() {
    if (confirm('Remove current image?')) {
        imageEditor.clearImage();
    }
}
```

### 4. **Handle Empty State**

```javascript
function onImageCleared() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Detect if editor is empty
    var canvas = imageEditor.baseElement.querySelector('canvas');
    if (!canvas || canvas.toDataURL() === '') {
        showUploadPrompt();
        disableEditButtons();
    }
}
```

## Troubleshooting

**Issue:** Previous image still visible after clear
- **Solution:** Ensure clearImage() is called
- **Solution:** Check for dialog caching - always clear on close

**Issue:** Reset doesn't work as expected
- **Solution:** No image loaded yet - load image first
- **Solution:** May need to refresh UI components after reset

**Issue:** History not working after reset
- **Solution:** Reset() clears history by design - this is expected
- **Solution:** Use undo() instead if you need history preserved

**Issue:** Dialog shows previous image
- **Solution:** ALWAYS call clearImage() in dialog close handler
- **Solution:** Don't assume dialog.close() clears the editor
