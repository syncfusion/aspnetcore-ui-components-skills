# Z-Order Annotation Layering in Image Editor

## Overview

Z-order (stacking order) controls which annotations appear on top when overlapping. This is essential for designing templates like greeting cards, posters, and complex compositions with multiple elements.

## Z-Order Operations

The Image Editor supports 4 z-order operations:

1. **Bring Forward** - Move annotation one layer forward
2. **Send Backward** - Move annotation one layer backward
3. **Bring to Front** - Move annotation to the very front
4. **Send to Back** - Move annotation behind all others

## Managing Z-Order

### Using Toolbar/UI

When an annotation is selected:

1. Right-click on annotation
2. Select z-order option:
   - Bring Forward
   - Send Backward
   - Bring to Front
   - Send to Back

### Programmatic Z-Order Control

```html
<ejs-imageeditor id="imageEditor"></ejs-imageeditor>

<div class="z-order-controls">
    <button onclick="bringForward()">Bring Forward</button>
    <button onclick="sendBackward()">Send Backward</button>
    <button onclick="bringToFront()">Bring to Front</button>
    <button onclick="sendToBack()">Send to Back</button>
</div>

<script>
function bringForward() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.bringForward();
}

function sendBackward() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.sendBackward();
}

function bringToFront() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.bringToFront();
}

function sendToBack() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.sendToBack();
}
</script>
```

## Practical Examples

### Greeting Card Design

```javascript
// Create layered greeting card
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// 1. Background image
imageEditor.open('background.jpg');

// 2. Add decorative frame
imageEditor.drawFrame('Mat', '#FFD700', '', 30);

// 3. Add border shape
imageEditor.drawRectangle(10, 10, 580, 380, 3, '#000000');

// 4. Add main text
imageEditor.drawText(300, 200, 'Happy Birthday!', 'Arial', 48, true, false, '#FF0000');

// 5. Add small decorative elements
imageEditor.drawEllipse(100, 100, 30, 30, 2, '#FF1493');

// Arrange z-order:
// Background → Frame → Border → Text → Decorations
```

### Watermark Placement

```javascript
function addWatermark() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Add watermark text
    imageEditor.drawText(
        200, 300,
        'CONFIDENTIAL',
        'Arial',
        36,
        false, false,
        'rgba(255, 0, 0, 0.3)'
    );
    
    // Send watermark behind image content
    imageEditor.sendToBack();
}
```

### Label Stacking

```javascript
// Create multiple labels with correct stacking order

// 1. Background shape
imageEditor.drawRectangle(100, 100, 200, 100, 2, '#CCCCCC');

// 2. Middle label
imageEditor.drawText(150, 120, 'Level 2', 'Arial', 14, false, false, '#000000');

// 3. Top label (bring to front)
imageEditor.drawText(120, 105, 'Level 1', 'Arial', 16, true, false, '#FF0000');
imageEditor.bringToFront();
```

## Best Practices

### 1. **Plan Layering Before Creation**

```javascript
// Good: Plan layer order
// 1. Background
// 2. Shape/box
// 3. Text
// 4. Decorations/highlights
```

### 2. **Use Send to Back for Backgrounds**

```javascript
// Create background shape
imageEditor.drawRectangle(0, 0, 600, 400, 0, '#F0F0F0');
// Send behind all other content
imageEditor.sendToBack();
```

### 3. **Keep Important Elements on Top**

```javascript
// Add important text
imageEditor.drawText(300, 200, 'IMPORTANT', 'Arial', 36, true, false, '#FF0000');
// Bring to front to ensure visibility
imageEditor.bringToFront();
```

### 4. **Test Final Composition**

Before exporting, verify layering order:

```javascript
// Review all annotations
var annotations = imageEditor.getAnnotations();
console.log('Total elements:', annotations.length);

// Adjust z-order if needed
```

## Template Examples

### Event Poster Design

```javascript
function createEventPoster() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Layer 1: Background image
    imageEditor.open('event-background.jpg');
    
    // Layer 2: Semi-transparent overlay
    imageEditor.drawRectangle(0, 0, 600, 800, 0, 'rgba(0, 0, 0, 0.3)');
    
    // Layer 3: Event title (on top)
    imageEditor.drawText(300, 100, 'ANNUAL GALA', 'Arial', 48, true, false, '#FFFFFF');
    imageEditor.bringToFront();
    
    // Layer 4: Date and time
    imageEditor.drawText(300, 200, 'June 15, 2026 - 7:00 PM', 'Arial', 24, false, false, '#FFFF00');
    imageEditor.bringForward();
    
    // Layer 5: Location
    imageEditor.drawText(300, 250, 'Grand Ballroom', 'Arial', 20, false, false, '#FFFFFF');
}
```

### Business Card Design

```javascript
function createBusinessCard() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Background (solid color or gradient via frame)
    imageEditor.drawFrame('Mat', '#003366', '', 0); // Blue background
    
    // Company name
    imageEditor.drawText(50, 50, 'TECH SOLUTIONS INC', 'Arial', 24, true, false, '#FFFFFF');
    imageEditor.bringToFront();
    
    // Logo placeholder
    imageEditor.drawRectangle(450, 40, 100, 100, 2, '#FFFFFF');
    
    // Name
    imageEditor.drawText(50, 150, 'John Smith', 'Arial', 18, false, false, '#000000');
    
    // Title
    imageEditor.drawText(50, 180, 'Senior Developer', 'Arial', 14, false, true, '#666666');
    
    // Contact info
    imageEditor.drawText(50, 240, 'john@techsolutions.com', 'Arial', 12, false, false, '#000000');
    
    // Bottom accent line
    imageEditor.drawLine(50, 320, 550, 320, 3, '#003366');
    imageEditor.sendToBack();
}
```

## Keyboard Shortcuts

Most image editors support:

- **Page Up** - Bring Forward
- **Page Down** - Send Backward
- **Ctrl+Page Up** - Bring to Front
- **Ctrl+Page Down** - Send to Back

Check if your implementation supports these.

## Undo/Redo with Z-Order

Z-order changes are tracked in history:

```javascript
imageEditor.drawText(100, 100, 'Text 1', 'Arial', 20, false, false, '#000000');
imageEditor.drawText(200, 200, 'Text 2', 'Arial', 20, false, false, '#FF0000');

imageEditor.bringToFront();   // User changes z-order
imageEditor.undo();           // Ctrl+Z reverts change
imageEditor.redo();           // Ctrl+Y reapplies change
```

## Common Issues

**Issue:** Can't see annotation behind others
- **Solution:** Select annotation and use "Bring Forward"
- **Solution:** Send blocking annotations to back

**Issue:** Z-order doesn't persist
- **Solution:** Save annotation data before exporting
- **Solution:** Document z-order in exported metadata

**Issue:** Confusion about layer terminology
- **Solution:** "Forward" = toward viewer (on top)
- **Solution:** "Backward" = away from viewer (behind)

## Workflow Tips

### Typical Design Workflow

1. Create background element → Send to Back
2. Add base content
3. Add foreground elements → Bring to Front
4. Fine-tune z-order as needed
5. Test composition
6. Export when satisfied

### Layer Management Strategy

```javascript
// Document layer structure
const layers = [
    { position: 0, name: 'Background', type: 'rectangle' },
    { position: 1, name: 'Watermark', type: 'text' },
    { position: 2, name: 'Title', type: 'text' },
    { position: 3, name: 'Decorations', type: 'shapes' }
];

// Implement using z-order commands
// Background stays at bottom
// Title stays on top
```

## Advanced Z-Order Control

### Auto-Arrange Layers

```javascript
function arrangeLayersAuto() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Send all rectangles to back
    imageEditor.sendToBack();
    
    // Keep text on top
    imageEditor.bringToFront();
}
```

### Get Z-Order Information

```javascript
function getLayerOrder() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var annotations = imageEditor.getAnnotations();
    
    annotations.forEach(function(ann, index) {
        console.log('Layer ' + index + ':', ann.Type, ann.Text);
    });
}
```
