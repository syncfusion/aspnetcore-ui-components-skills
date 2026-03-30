# Decorative Frames in Image Editor

## Overview

The frame feature allows adding decorative borders around images. Frames enhance the visual appeal and can create professional presentations, greeting cards, or posters.

## Frame Types

The Image Editor supports 5 frame types:

1. **Mat** - Flat colored border frame
2. **Bevel** - 3D beveled edge frame
3. **Line** - Simple line-based frame
4. **Hook** - Artistic hook-style frame
5. **Inset** - Inset border frame

## Drawing Frames

### Using Toolbar

1. Click **Frame** button in toolbar
2. Select desired frame type
3. Configure options:
   - Color
   - Size
   - Gradient (if applicable)
   - Style (for line frames)
4. Click tick to apply

### Programmatic Frame Application

Use the `drawFrame()` method:

```html
<ejs-imageeditor id="imageEditor"></ejs-imageeditor>

<button onclick="addMatFrame()">Add Mat Frame</button>
<button onclick="addBevelFrame()">Add Bevel Frame</button>
<button onclick="addLineFrame()">Add Line Frame</button>

<script>
function addMatFrame() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.drawFrame(
        'Mat',           // frameType
        '#FF0000',       // color (red)
        '',              // gradientColor
        20               // size (thickness in pixels)
    );
}

function addBevelFrame() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.drawFrame('Bevel', '#0000FF', '', 15);
}

function addLineFrame() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.drawFrame('Line', '#000000', '', 10);
}
</script>
```

### DrawFrame Method Parameters

```javascript
imageEditor.drawFrame(
    frameType,        // string: Mat | Bevel | Line | Hook | Inset
    color,            // string: hex or named color
    gradientColor,    // string: gradient color (optional)
    size,             // number: frame thickness
    inset,            // number: inset value (for Line/Hook/Inset types)
    offset,           // number: offset value (for Line/Inset types)
    borderRadius,     // number: border radius (for Line type)
    frameLineStyle,   // string: line style (for Line type)
    lineCount         // number: line count (for Line type)
);
```

## Frame Customization

### Mat Frame

Solid colored flat border:

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Simple red mat frame
imageEditor.drawFrame('Mat', '#FF0000', '', 30);

// Gold mat frame
imageEditor.drawFrame('Mat', '#FFD700', '', 25);

// Custom color with gradient
imageEditor.drawFrame('Mat', '#FF69B4', '#FFB6C1', 20);
```

### Bevel Frame

3D beveled edge effect:

```javascript
// Silver bevel
imageEditor.drawFrame('Bevel', '#C0C0C0', '#FFFFFF', 15);

// Dark wood effect
imageEditor.drawFrame('Bevel', '#8B4513', '#D2B48C', 20);
```

### Line Frame

Simple line-based frame with customization:

```javascript
imageEditor.drawFrame(
    'Line',           // type
    '#000000',        // color (black)
    '',               // no gradient
    3,                // size (thickness)
    5,                // inset
    2,                // offset
    10,               // borderRadius
    'Solid',          // frameLineStyle
    2                 // lineCount
);
```

**Line Styles:**
- `Solid` - Solid line
- `Dotted` - Dotted line
- `Dashed` - Dashed line

### Hook Frame

Artistic hook-style decorative frame:

```javascript
imageEditor.drawFrame(
    'Hook',           // type
    '#8B7355',        // color (brown)
    '',
    12,               // size
    8,                // inset
    3                 // offset
);
```

### Inset Frame

Inset border frame:

```javascript
imageEditor.drawFrame(
    'Inset',          // type
    '#696969',        // color (gray)
    '',
    15,               // size
    6,                // inset
    2                 // offset
);
```

## Frame Events

### Frame Changing Event

Handle frame application:

```html
<ejs-imageeditor id="imageEditor"
    frameChanging="OnFrameChanging">
</ejs-imageeditor>

<script>
function OnFrameChanging(args) {
    console.log('Frame type:', args.CurrentFrameSetting.Type);
    console.log('Frame color:', args.CurrentFrameSetting.Color);
    console.log('Frame size:', args.CurrentFrameSetting.Size);
    
    // You can cancel if needed
    // args.Cancel = true;
}
</script>
```

### Event Arguments

`FrameChangeEventArgs` provides:

- **previousFrameSetting** - Previous frame configuration
- **currentFrameSetting** - New frame configuration
- **cancel** - Boolean to cancel operation

### Frame Settings Properties

- **Type** - Frame type (Mat, Bevel, Line, Hook, Inset)
- **Color** - Frame color
- **GradientColor** - Gradient color (if applicable)
- **Size** - Frame thickness
- **Inset** - Inset value
- **Offset** - Offset value
- **BorderRadius** - Border radius (for Line frames)
- **FrameLineStyle** - Line style (for Line frames)
- **LineCount** - Line count (for Line frames)

## Practical Examples

### Greeting Card Creator

```html
<ejs-imageeditor id="imageEditor" width="100%" height="500px"></ejs-imageeditor>

<div class="frame-presets">
    <h3>Frame Presets</h3>
    <button onclick="applyFrame('elegant')">Elegant Gold</button>
    <button onclick="applyFrame('festive')">Festive Red</button>
    <button onclick="applyFrame('modern')">Modern Black</button>
    <button onclick="applyFrame('wooden')">Wooden</button>
</div>

<script>
const framePresets = {
    'elegant': { type: 'Mat', color: '#FFD700', size: 25 },
    'festive': { type: 'Mat', color: '#FF0000', size: 20 },
    'modern': { type: 'Line', color: '#000000', size: 5 },
    'wooden': { type: 'Bevel', color: '#8B4513', size: 20 }
};

function applyFrame(presetName) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var preset = framePresets[presetName];
    
    imageEditor.drawFrame(
        preset.type,
        preset.color,
        '',
        preset.size
    );
}
</script>
```

### Professional Photo Frames

```javascript
// Gallery-style frame
function galleryFrame() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.drawFrame('Mat', '#FFFFFF', '', 30);
}

// Poster frame
function posterFrame() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.drawFrame('Bevel', '#000000', '#FFFFFF', 40);
}

// Vintage frame
function vintageFrame() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.drawFrame('Mat', '#8B8680', '', 35);
}
```

### Custom Frame Builder

```html
<div class="frame-builder">
    <label>Frame Type:</label>
    <select id="frameType" onchange="updateFrame()">
        <option value="Mat">Mat</option>
        <option value="Bevel">Bevel</option>
        <option value="Line">Line</option>
        <option value="Hook">Hook</option>
        <option value="Inset">Inset</option>
    </select>
    
    <label>Color:</label>
    <input type="color" id="frameColor" value="#FF0000" onchange="updateFrame()">
    
    <label>Size:</label>
    <input type="range" id="frameSize" min="5" max="50" value="20" onchange="updateFrame()">
    <span id="sizeDisplay">20px</span>
    
    <button onclick="applyCustomFrame()">Apply Frame</button>
</div>

<script>
function updateFrame() {
    var sizeDisplay = document.getElementById('frameSize').value;
    document.getElementById('sizeDisplay').textContent = sizeDisplay + 'px';
}

function applyCustomFrame() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var frameType = document.getElementById('frameType').value;
    var color = document.getElementById('frameColor').value;
    var size = document.getElementById('frameSize').value;
    
    imageEditor.drawFrame(frameType, color, '', parseInt(size));
}
</script>
```

## Best Practices

### 1. **Match Frame to Content**

- Professional photos → Elegant frames
- Casual shots → Simple line frames
- Artistic images → Hook/Bevel frames

### 2. **Color Harmony**

Choose frame colors that complement the image:

```javascript
// For sunset image: warm frame
imageEditor.drawFrame('Mat', '#DAA520', '', 25); // Goldenrod

// For winter scene: cool frame
imageEditor.drawFrame('Mat', '#4682B4', '', 25); // Steel blue
```

### 3. **Size Proportion**

Frame size should be proportional to image:

- Small images (< 300px) → 10-15px frame
- Medium images (300-600px) → 15-30px frame
- Large images (> 600px) → 30-50px frame

### 4. **Test Before Exporting**

Always preview frame before final export:

```javascript
imageEditor.drawFrame('Mat', '#FF0000', '', 20);
// View result, then save if satisfied
```

## Undo/Redo with Frames

Frames are tracked in undo/redo history:

```javascript
imageEditor.drawFrame('Mat', '#FF0000', '', 20);  // User adds frame
imageEditor.undo();                               // Ctrl+Z removes it
imageEditor.redo();                               // Ctrl+Y reapplies it
```

## Troubleshooting

**Issue:** Frame not appearing
- **Solution:** Verify image is loaded before applying frame
- **Solution:** Check frame size is visible (minimum 5px)

**Issue:** Frame color looks wrong
- **Solution:** Use hex format (#FF0000) instead of RGB
- **Solution:** Check browser color support

**Issue:** Frame extends beyond canvas
- **Solution:** Reduce frame size
- **Solution:** Increase image editor height/width
