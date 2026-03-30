# Selection and Cropping in Image Editor

## Table of Contents
- [Selection Types](#selection-types)
- [Custom Selection](#custom-selection)
- [Square Selection](#square-selection)
- [Circle Selection](#circle-selection)
- [Aspect Ratio Selection](#aspect-ratio-selection)
- [Crop Functionality](#crop-functionality)
- [Select Method Parameters](#select-method-parameters)
- [Practical Examples](#practical-examples)

## Selection Types

The Image Editor provides multiple selection options for cropping images:

1. **Custom** - Freeform rectangular selection
2. **Square** - Fixed square shape
3. **Circle** - Circular selection
4. **Aspect Ratios** - Predefined proportions

## Custom Selection

### Using Toolbar

1. Click the **Crop** button in the toolbar
2. Click **Custom** in the contextual menu
3. Click and drag on the image to create selection
4. Adjust by dragging corners/edges
5. Click tick to apply

### Programmatic Custom Selection

```html
<ejs-imageeditor id="imageEditor"></ejs-imageeditor>

<button onclick="selectCustom()">Custom Selection</button>

<script>
function selectCustom() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Parameters: type, startX, startY, width, height
    imageEditor.select('custom', 100, 100, 300, 200);
}
</script>
```

## Square Selection

### Using Toolbar

1. Click the **Crop** button
2. Click **Square** option
3. Click on image to place square
4. Adjust size by dragging corners
5. Click tick to apply

### Programmatic Square Selection

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Create 300x300 square starting at position 100, 100
imageEditor.select('square', 100, 100, 300, 300);
```

### Square Selection Example

```html
<ejs-imageeditor id="imageEditor" width="100%" height="600px"></ejs-imageeditor>

<div class="controls">
    <button onclick="square200()">200x200 Square</button>
    <button onclick="square400()">400x400 Square</button>
    <button onclick="square600()">600x600 Square</button>
</div>

<script>
function square200() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.select('square', 50, 50, 200, 200);
}

function square400() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.select('square', 50, 50, 400, 400);
}

function square600() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.select('square', 50, 50, 600, 600);
}
</script>
```

## Circle Selection

### Using Toolbar

1. Click the **Crop** button
2. Click **Circle** option
3. Click on image to place circle
4. Adjust diameter by dragging handles
5. Click tick to apply

### Programmatic Circle Selection

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Create circular selection
// Parameters: type, startX, startY, width, height
imageEditor.select('circle', 150, 150, 300, 300);
```

**Note:** For circles, width and height should be equal (width = height = diameter)

## Aspect Ratio Selection

The Image Editor supports 10 predefined aspect ratios for different use cases:

| Ratio | Use Case |
|-------|----------|
| `2:3` | Portrait photos, documents |
| `3:2` | Landscape photos |
| `3:4` | Vertical banner |
| `4:3` | Classic photo ratio |
| `4:5` | Portrait instagram |
| `5:4` | Landscape classic |
| `5:7` | Vertical poster |
| `7:5` | Horizontal poster |
| `9:16` | Vertical video |
| `16:9` | Widescreen, landscape video |

### Using Toolbar

1. Click the **Crop** button
2. Select **Aspect Ratio** option
3. Choose desired ratio (2:3, 3:2, etc.)
4. Click on image to place selection
5. Adjust position by dragging
6. Click tick to apply

### Programmatic Aspect Ratio Selection

```html
<ejs-imageeditor id="imageEditor"></ejs-imageeditor>

<div class="ratio-buttons">
    <button onclick="selectRatio('2:3')">2:3 (Portrait)</button>
    <button onclick="selectRatio('16:9')">16:9 (Widescreen)</button>
    <button onclick="selectRatio('1:1')">1:1 (Square)</button>
    <button onclick="selectRatio('4:3')">4:3 (Photo)</button>
</div>

<script>
function selectRatio(ratio) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Aspect ratio format: startX, startY, ratio string
    var parts = ratio.split(':');
    var ratioValue = parseInt(parts[0]) / parseInt(parts[1]);
    
    imageEditor.select(ratio, 50, 50);
}
</script>
```

## Select Method Parameters

The `select()` method takes these parameters:

```javascript
imageEditor.select(type, startX, startY, width, height);
```

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | string | Selection type: 'custom', 'square', 'circle', or ratio like '16:9' |
| `startX` | number | X-coordinate of selection top-left corner |
| `startY` | number | Y-coordinate of selection top-left corner |
| `width` | number | Width of selection (not needed for aspect ratios) |
| `height` | number | Height of selection (not needed for aspect ratios) |

### Type Examples

```javascript
// Custom rectangle
imageEditor.select('custom', 100, 100, 300, 200);

// Square
imageEditor.select('square', 50, 50, 200, 200);

// Circle (width = height)
imageEditor.select('circle', 100, 100, 300, 300);

// Aspect ratio
imageEditor.select('16:9', 100, 100);
imageEditor.select('2:3', 50, 50);
imageEditor.select('1:1', 100, 100);
```

## Crop Functionality

### Apply Crop

Once a selection is made, apply the crop:

1. **Via Toolbar:** Click the tick/check mark in contextual toolbar
2. **Via Canvas:** Click on the canvas outside selection
3. **Programmatically:** Use crop method

### Programmatic Crop

```html
<ejs-imageeditor id="imageEditor"></ejs-imageeditor>

<button onclick="selectAndCrop()">Select and Crop</button>

<script>
function selectAndCrop() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Create custom selection
    imageEditor.select('custom', 100, 100, 300, 200);
    
    // Apply crop (in real implementation, this happens after user adjusts)
    imageEditor.crop();
}
</script>
```

### Cancel Crop

Press **Escape** key or click the cross/X button to cancel selection without cropping.

## Practical Examples

### Instagram Profile Picture (1:1 Square)

```html
<ejs-imageeditor id="imageEditor"></ejs-imageeditor>

<button onclick="instagramProfileCrop()">Crop for Instagram Profile</button>

<script>
function instagramProfileCrop() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Select 1:1 square ratio
    imageEditor.select('1:1', 100, 100);
    
    // Crop button shows tick to apply
}
</script>
```

### Landscape Widescreen (16:9)

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Select 16:9 widescreen ratio
imageEditor.select('16:9', 100, 100);
```

### Custom Aspect Ratio Selector

```html
<select id="aspectRatioSelect" onchange="applyRatio()">
    <option value="2:3">Portrait (2:3)</option>
    <option value="3:2">Landscape (3:2)</option>
    <option value="4:3">Classic Photo (4:3)</option>
    <option value="16:9">Widescreen (16:9)</option>
    <option value="1:1">Square (1:1)</option>
    <option value="custom">Custom</option>
</select>

<script>
function applyRatio() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var ratio = document.getElementById('aspectRatioSelect').value;
    
    if (ratio === 'custom') {
        imageEditor.select('custom', 50, 50, 200, 200);
    } else {
        imageEditor.select(ratio, 50, 50);
    }
}
</script>
```

### Crop to Document Size

```javascript
// Crop to A4 dimensions (210x297mm at 72 DPI)
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// A4 ratio: 210/297 ≈ 0.707 ≈ 7:10
imageEditor.select('7:10', 100, 100);
```

### Multiple Crop Options

```html
<ejs-imageeditor id="imageEditor" width="100%" height="500px"></ejs-imageeditor>

<div class="crop-options">
    <button onclick="crop('custom')">Custom</button>
    <button onclick="crop('1:1')">Square</button>
    <button onclick="crop('4:3')">4:3</button>
    <button onclick="crop('16:9')">16:9</button>
    <button onclick="crop('9:16')">9:16 Portrait</button>
</div>

<script>
function crop(type) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    if (type === 'custom') {
        imageEditor.select('custom', 50, 50, 300, 200);
    } else if (type === '1:1') {
        imageEditor.select('square', 50, 50, 300, 300);
    } else {
        imageEditor.select(type, 50, 50);
    }
}
</script>
```

## Best Practices

### 1. **Provide Aspect Ratio Presets**

```csharp
public List<string> GetCommonAspectRatios()
{
    return new List<string> { "1:1", "4:3", "16:9", "2:3" };
}
```

### 2. **Show Selection Preview**

```javascript
imageEditor.select('16:9', 100, 100);
// Selection appears immediately for user to adjust
```

### 3. **Keyboard Support for Selection**

Document that users can:
- Use arrow keys to adjust selection
- Press Escape to cancel
- Press Enter/Space to apply

### 4. **Store Crop Coordinates**

```javascript
function saveCropData() {
    // Store crop coordinates for undo/redo
    var cropData = {
        type: 'custom',
        startX: 100,
        startY: 100,
        width: 300,
        height: 200
    };
    
    // Save to database or session
}
```

## Troubleshooting

**Issue:** Selection not appearing
- **Solution:** Verify image is loaded first
- **Solution:** Check coordinates are within image bounds

**Issue:** Crop not applying
- **Solution:** Click tick button in contextual toolbar
- **Solution:** Ensure selection is visible (not off-canvas)

**Issue:** Aspect ratio seems off
- **Solution:** Width/height parameters are ignored for ratios (use startX, startY only)
- **Solution:** Verify ratio format (should be "width:height")
