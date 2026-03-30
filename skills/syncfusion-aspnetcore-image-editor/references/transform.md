# Image Transformation in Image Editor

## Table of Contents
- [Rotate Image](#rotate-image)
- [Flip Image](#flip-image)
- [Straighten Image](#straighten-image)
- [Transform Events](#transform-events)

## Rotate Image

The Image Editor allows rotating both the image and its annotations by specific degrees clockwise or counter-clockwise.

### Using Toolbar

1. Click **RotateLeft** button to rotate 90° counter-clockwise
2. Click **RotateRight** button to rotate 90° clockwise
3. Each click rotates by 90° in the selected direction

### Programmatic Rotation

Use the `rotate()` method to rotate the image:

```html
<ejs-imageeditor id="imageEditor"></ejs-imageeditor>

<button onclick="rotateClockwise()">Rotate 90°</button>
<button onclick="rotateCounterClockwise()">Rotate -90°</button>

<script>
function rotateClockwise() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.rotate(90);
}

function rotateCounterClockwise() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.rotate(-90);
}
</script>
```

### Rotation Parameters

The `rotate()` method accepts an angle in degrees:

```javascript
// Clockwise rotations
imageEditor.rotate(90);    // 90° clockwise
imageEditor.rotate(180);   // 180° (flip)
imageEditor.rotate(270);   // 270° clockwise (90° counter-clockwise)
imageEditor.rotate(-90);   // 90° counter-clockwise

// Any angle (not recommended)
imageEditor.rotate(45);    // 45° clockwise
imageEditor.rotate(-30);   // 30° counter-clockwise
```

### Recommended Rotations

**Best Practice:** Use multiples of 90° for proper alignment:

```javascript
// ✅ Recommended
imageEditor.rotate(90);    // Common, straightforward
imageEditor.rotate(180);   // Flip upside down
imageEditor.rotate(-90);   // Counter-clockwise

// ⚠️ Works but not recommended
imageEditor.rotate(45);    // Creates partial pixels, reduces quality
```

### Rotation Effects on Annotations

When you rotate an image, all annotations rotate with it:

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Before rotation: annotations at their positions
// Rotate 90° clockwise
imageEditor.rotate(90);
// After: image rotated AND all annotations rotated together
```

### Cumulative Rotation

Each call to `rotate()` adds to the current rotation:

```javascript
imageEditor.rotate(90);   // Total rotation: 90°
imageEditor.rotate(90);   // Total rotation: 180° (not 90°)
imageEditor.rotate(-180); // Total rotation: 0° (back to original)
```

## Flip Image

The Image Editor provides methods to flip images horizontally or vertically.

### Using Toolbar

1. Click **HorizontalFlip** to create mirror effect (left-right)
2. Click **VerticalFlip** to flip vertically (top-bottom)
3. Click again to toggle back

### Programmatic Flipping

Use the `flip()` method:

```html
<ejs-imageeditor id="imageEditor"></ejs-imageeditor>

<button onclick="flipHorizontal()">Flip Horizontal</button>
<button onclick="flipVertical()">Flip Vertical</button>

<script>
function flipHorizontal() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.flip('Horizontal');
}

function flipVertical() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.flip('Vertical');
}
</script>
```

### Flip Parameters

The `flip()` method accepts a direction parameter:

```javascript
// Horizontal flip (mirror left-right)
imageEditor.flip('Horizontal');

// Vertical flip (mirror top-bottom)
imageEditor.flip('Vertical');
```

### Flip Effects

#### Horizontal Flip

```
Original:          Flipped Horizontal:
  →                  ←
  Face right   →    Face left
```

#### Vertical Flip

```
Original:          Flipped Vertical:
  ↑                  ↓
  Top    →      Bottom
  Bottom    ↓    Top
```

### Combining Flip Operations

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Flip both directions (equivalent to 180° rotation)
imageEditor.flip('Horizontal');
imageEditor.flip('Vertical');

// Result: Image rotated 180°, annotations also transformed
```

### Flip with Annotations

Annotations are flipped along with the image:

```javascript
// Before flip: text annotation reads "HELLO"
imageEditor.flip('Horizontal');
// After flip: text annotation is mirrored horizontally
```

## Straighten Image

The straightening feature allows users to adjust image rotation by small degrees within -45 to +45 range.

### Using Toolbar

1. Click **Crop** button
2. Look for **Straightening** slider
3. Drag slider to adjust angle
4. Click tick to apply

### Straightening Range

- **Minimum:** -45 degrees (45° counter-clockwise)
- **Maximum:** +45 degrees (45° clockwise)
- **Default:** 0 degrees (no rotation)

### Programmatic Straightening

Use the `straightenImage()` method:

```html
<ejs-imageeditor id="imageEditor"></ejs-imageeditor>

<input type="range" min="-45" max="45" value="0" 
    onchange="straighten(this.value)">
<span id="angleDisplay">0°</span>

<script>
function straighten(angle) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.straightenImage(angle);
    document.getElementById('angleDisplay').textContent = angle + '°';
}
</script>
```

### Straightening Examples

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Straighten by rotating 2° clockwise
imageEditor.straightenImage(2);

// Straighten by rotating 5° counter-clockwise
imageEditor.straightenImage(-5);

// Reset straightening
imageEditor.straightenImage(0);
```

### Use Cases for Straightening

- **Horizon fixing:** Straighten tilted horizon in landscape photos
- **Document scanning:** Align scanned documents
- **Architectural photos:** Correct lens distortion
- **Portrait correction:** Minor angle adjustments

## Transform Events

### Rotation Event

Track when rotation occurs:

```html
<ejs-imageeditor id="imageEditor"
    rotating="OnRotating"
    rotated="OnRotated">
</ejs-imageeditor>

<script>
function OnRotating(args) {
    console.log('Starting rotation...');
}

function OnRotated(args) {
    console.log('Rotation complete');
    console.log('Final angle: ' + args.Degree);
}
</script>
```

## Practical Examples

### Photo Straightener Tool

```html
<ejs-imageeditor id="imageEditor" width="100%" height="500px">
</ejs-imageeditor>

<div class="straighten-controls">
    <button onclick="straightenMinor(-5)">-5°</button>
    <input type="range" min="-45" max="45" value="0" 
        onchange="straighten(this.value)" id="straightenSlider">
    <button onclick="straightenMinor(5)">+5°</button>
    <button onclick="applyStraighten()">Apply</button>
    <button onclick="resetStraighten()">Reset</button>
</div>

<script>
var currentAngle = 0;

function straighten(angle) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.straightenImage(angle);
    currentAngle = angle;
}

function straightenMinor(delta) {
    var newAngle = Math.max(-45, Math.min(45, currentAngle + delta));
    document.getElementById('straightenSlider').value = newAngle;
    straighten(newAngle);
}

function applyStraighten() {
    console.log('Straightening applied at ' + currentAngle + '°');
}

function resetStraighten() {
    document.getElementById('straightenSlider').value = 0;
    straighten(0);
    currentAngle = 0;
}
</script>
```

### Document Scanner

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Typical workflow for document scanning
1. Scan document (usually slightly tilted)
2. Open in Image Editor
3. Detect skew angle
4. Straighten using detected angle
5. Crop to document boundaries
6. Save

// Example
var detectedSkew = 2.5;
imageEditor.straightenImage(detectedSkew);
```

### Image Orientation Corrector

```html
<ejs-imageeditor id="imageEditor"></ejs-imageeditor>

<div class="orientation-buttons">
    <button onclick="rotateLeft()">Rotate Left (90°)</button>
    <button onclick="rotateRight()">Rotate Right (90°)</button>
    <button onclick="flipH()">Flip Horizontal</button>
    <button onclick="flipV()">Flip Vertical</button>
</div>

<script>
function rotateLeft() {
    document.getElementById('imageEditor').ej2_instances[0].rotate(-90);
}

function rotateRight() {
    document.getElementById('imageEditor').ej2_instances[0].rotate(90);
}

function flipH() {
    document.getElementById('imageEditor').ej2_instances[0].flip('Horizontal');
}

function flipV() {
    document.getElementById('imageEditor').ej2_instances[0].flip('Vertical');
}
</script>
```

## Best Practices

### 1. **Use 90° Rotations for Quality**

```javascript
// ✅ Maintains quality
imageEditor.rotate(90);
imageEditor.rotate(180);

// ⚠️ May reduce quality
imageEditor.rotate(45);
```

### 2. **Offer Both Toolbar and API**

Provide users with both:
- Toolbar buttons for quick common operations
- Programmatic API for advanced workflows

### 3. **Show Current State**

```javascript
function displayTransformStatus() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    console.log('Current transformations applied');
}
```

### 4. **Undo/Redo Transforms**

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// User rotates
imageEditor.rotate(90);

// User changes mind - undo
imageEditor.undo(); // Back to original orientation
```

## Troubleshooting

**Issue:** Rotation seems stuck
- **Solution:** Check undo/redo history
- **Solution:** Verify rotate angle is in valid range

**Issue:** Straightening not applying
- **Solution:** Angle must be between -45 and +45
- **Solution:** Check that straightenImage method is called after image loads

**Issue:** Annotations not rotating with image
- **Solution:** Ensure annotations are created before rotation
- **Solution:** Annotations should automatically rotate with image
