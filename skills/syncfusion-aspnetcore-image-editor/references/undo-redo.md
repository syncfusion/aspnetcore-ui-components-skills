# Undo and Redo History Management

## Overview

The Image Editor maintains a history of all editing actions (up to 16 steps) allowing users to undo and redo operations.

## History Limit

- **Maximum steps:** 16 actions
- **After 16th step:** Oldest action is removed from history
- **Includes:** All operations (crop, filter, annotations, transforms, etc.)

## Undo Operations

### Using Toolbar

Click the **Undo** button in the toolbar to revert the last action.

### Using Keyboard

Press **Ctrl+Z** to undo the last action.

### Programmatic Undo

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
imageEditor.undo();
```

### Multiple Undos

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Undo last action
imageEditor.undo();

// Undo again
imageEditor.undo();

// Can undo up to 16 times (if that many actions exist)
```

## Redo Operations

### Using Toolbar

Click the **Redo** button in the toolbar to repeat the last undone action.

### Using Keyboard

Press **Ctrl+Y** to redo the last undone action.

### Programmatic Redo

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
imageEditor.redo();
```

### Multiple Redos

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// User undoes 3 actions
imageEditor.undo();
imageEditor.undo();
imageEditor.undo();

// Now redo them back
imageEditor.redo();
imageEditor.redo();
imageEditor.redo();
```

## Actions in Undo/Redo History

All of these actions are tracked:

- Image open/load
- Crop operations
- Rotation (rotate, flip, straighten)
- Annotations (text, shapes, freehand)
- Filters (all 8 filter types)
- Fine-tuning adjustments
- Frames
- Redaction
- Z-order changes (bring forward, send backward)
- Resize
- Any other modifications

## Practical Examples

### Basic Undo/Redo

```html
<ejs-imageeditor id="imageEditor" width="100%" height="500px">
</ejs-imageeditor>

<div class="history-controls">
    <button onclick="undo()" id="undoBtn">↶ Undo</button>
    <button onclick="redo()" id="redoBtn">↷ Redo</button>
    <span id="historyDisplay">No actions</span>
</div>

<script>
var actionCount = 0;

function undo() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.undo();
    actionCount--;
    updateDisplay();
}

function redo() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.redo();
    actionCount++;
    updateDisplay();
}

function updateDisplay() {
    document.getElementById('historyDisplay').textContent = 'Actions: ' + actionCount;
}
</script>
```

### History State Display

```javascript
function showHistoryStatus() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Note: No direct API to get history length, track manually
    console.log('Undo available: Check if undo() works');
    console.log('Redo available: Check if redo() works');
    
    // Try undo - if nothing happens, none available
    imageEditor.undo();
}
```

### Workflow with Undo

```javascript
// User workflow example
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// 1. Load image
imageEditor.open('photo.jpg');

// 2. Crop image
imageEditor.select('custom', 100, 100, 300, 200);
imageEditor.crop();

// 3. Apply filter
imageEditor.applyImageFilter('Warm');

// 4. Add annotation
imageEditor.drawText(150, 150, 'Sample', 'Arial', 20, false, false, '#FF0000');

// 5. User regrets annotation - undo it
imageEditor.undo();
// Image back to state before text annotation

// 6. User realizes filter was good - redo
imageEditor.redo();
// Text annotation back
```

### Batch Undo Example

```javascript
function undoLast(count) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    for (var i = 0; i < count; i++) {
        imageEditor.undo();
    }
}

function undoToStart() {
    // Undo up to 16 times to get back to original
    undoLast(16);
}
```

### Undo with Visual Feedback

```html
<ejs-imageeditor id="imageEditor"></ejs-imageeditor>

<div class="undo-controls">
    <button onclick="performUndoWithFeedback()">Undo</button>
    <div id="feedback"></div>
</div>

<script>
var lastAction = '';

function performUndoWithFeedback() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Store current state
    var beforeUndo = 'Current state';
    
    // Perform undo
    imageEditor.undo();
    
    // Show feedback
    var feedback = document.getElementById('feedback');
    feedback.textContent = 'Undid: ' + lastAction;
    feedback.style.display = 'block';
    
    // Hide after 2 seconds
    setTimeout(function() {
        feedback.style.display = 'none';
    }, 2000);
}
</script>
```

## Undo/Redo Reset

### When Does History Clear?

History clears when:

1. **New image loaded** - Opening different image resets history
2. **Close editor** - Closing component clears history
3. **Page reload** - Refreshing page clears history

### Preserve History Across Images

```javascript
var historyStack = [];

function saveHistory() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    imageEditor.toBlob(function(blob) {
        historyStack.push({
            image: blob,
            timestamp: new Date()
        });
    });
}

function restoreHistory() {
    if (historyStack.length > 0) {
        var previous = historyStack.pop();
        var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
        
        var reader = new FileReader();
        reader.onload = function(e) {
            imageEditor.open(e.target.result);
        };
        reader.readAsDataURL(previous.image);
    }
}
```

## History Best Practices

### 1. **Keep Track of Actions**

```javascript
var actionLog = [];

function logAction(actionName) {
    actionLog.push({
        name: actionName,
        timestamp: new Date()
    });
}

// Then track
imageEditor.applyImageFilter('Warm');
logAction('Apply Warm Filter');
```

### 2. **Provide Clear Undo Feedback**

```html
<button onclick="undo()">
    <span id="undoLabel">Undo</span>
</button>

<script>
function undo() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var before = document.getElementById('undoLabel').textContent;
    
    imageEditor.undo();
    
    document.getElementById('undoLabel').textContent = 'Undo';
    // Or use service to track last action name
}
</script>
```

### 3. **Warn Before Discard**

```javascript
function closeEditor() {
    if (confirm('Undo history will be lost. Continue?')) {
        // Close/navigate
        window.close();
    }
}
```

### 4. **Test History Limits**

```javascript
function testHistoryLimit() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Perform 20 actions
    for (var i = 0; i < 20; i++) {
        imageEditor.applyImageFilter(i % 2 === 0 ? 'Cold' : 'Warm');
    }
    
    // Only last 16 can be undone
}
```

## Troubleshooting

**Issue:** Undo not working
- **Solution:** Verify keyboard shortcut (Ctrl+Z)
- **Solution:** Check that action was actually performed

**Issue:** Redo doesn't work after undo
- **Solution:** Redo only works immediately after undo
- **Solution:** Any new action clears redo history

**Issue:** Reaching undo limit (16 steps)
- **Solution:** Oldest action automatically removed
- **Solution:** Save progress if need more than 16 steps

**Issue:** History lost when opening new image
- **Solution:** Save current image first
- **Solution:** Use custom history manager if needed across images

## History Comparison

| Feature | Supported |
|---------|-----------|
| Undo | ✅ Yes (up to 16) |
| Redo | ✅ Yes (after undo) |
| History Limit | ✅ 16 steps |
| Undo Toolbar | ✅ Yes |
| Redo Toolbar | ✅ Yes |
| Keyboard Shortcuts | ✅ Ctrl+Z, Ctrl+Y |
| History Clear | On new image/close |
| Persistent History | ❌ No (local to session) |
