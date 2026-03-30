# Image Operations in Image Editor

## Table of Contents
- [Supported Image Formats](#supported-image-formats)
- [Opening Images](#opening-images)
- [Zooming Capabilities](#zooming-capabilities)
- [Panning Images](#panning-images)

## Supported Image Formats

The Image Editor supports the following image formats:

- **JPEG** (.jpg, .jpeg) - Compressed format, good for photographs
- **PNG** (.png) - Lossless format, supports transparency
- **WebP** (.webp) - Modern format, better compression
- **BMP** (.bmp) - Uncompressed format
- **SVG** (.svg) - Scalable vector format (limited support)

Default supported extensions: `.jpg, .png, .svg, .webp, .bmp`

## Opening Images

### Open Image via Open Button

Users can open images by clicking the **Open** button in the toolbar:

1. Click the Open button
2. File explorer window appears
3. Select an image file (JPEG, PNG, JPG, WEBP, BMP)
4. Image loads into the canvas

### Open Image Programmatically

Use the `open()` method to load images programmatically:

```html
<ejs-imageeditor id="imageEditor"></ejs-imageeditor>

<button onclick="openImage()">Load Image</button>

<script>
function openImage() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.open('sample.png');
}
</script>
```

### Open from Different Sources

#### Local File Path

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
imageEditor.open('images/sample.png');
```

#### Base64 String

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
var base64String = 'data:image/png;base64,iVBORw0KGgoAAAANS...';
imageEditor.open(base64String);
```

#### URL

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
imageEditor.open('https://example.com/images/sample.jpg');
```

#### Blob Storage

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
var blob = new Blob([imageData], { type: 'image/png' });
var url = URL.createObjectURL(blob);
imageEditor.open(url);
```

#### From File Upload

```html
<input type="file" id="fileInput" accept="image/*" onchange="handleFileUpload(event)">

<script>
function handleFileUpload(event) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var file = event.target.files[0];
    
    var reader = new FileReader();
    reader.onload = function(e) {
        imageEditor.open(e.target.result);
    };
    reader.readAsDataURL(file);
}
</script>
```

### Image Loaded Event

Handle the image loaded event:

```html
<ejs-imageeditor id="imageEditor"
    imageLoaded="OnImageLoaded">
</ejs-imageeditor>

<script>
function OnImageLoaded() {
    console.log('Image loaded successfully');
    // Enable editing tools
    document.getElementById('editTools').style.display = 'block';
}
</script>
```

## Zooming Capabilities

The Image Editor provides multiple ways to zoom in and out of images.

### Zoom via Toolbar

Click the **ZoomIn** and **ZoomOut** buttons in the toolbar to adjust the zoom level.

### Zoom via Mouse Wheel

Press **Ctrl** and scroll the mouse wheel to zoom:

- **Ctrl + Mouse Wheel Up** → Zoom in
- **Ctrl + Mouse Wheel Down** → Zoom out

### Zoom via Keyboard

Use keyboard shortcuts to zoom:

- **Ctrl + Plus (+)** → Zoom in
- **Ctrl + Minus (-)** → Zoom out

### Zoom via Pinch (Touch Devices)

On touch-enabled devices, use two-finger pinch gesture:

- **Pinch Outward** → Zoom in
- **Pinch Inward** → Zoom out

### Programmatic Zooming

Use JavaScript to control zoom level:

```html
<ejs-imageeditor id="imageEditor"></ejs-imageeditor>

<button onclick="zoomIn()">Zoom In</button>
<button onclick="zoomOut()">Zoom Out</button>

<script>
function zoomIn() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var currentZoom = imageEditor.zoomSettings.zoomValue || 100;
    var newZoom = Math.min(currentZoom + 10, 1000);
    imageEditor.zoomSettings.zoomValue = newZoom;
}

function zoomOut() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var currentZoom = imageEditor.zoomSettings.zoomValue || 100;
    var newZoom = Math.max(currentZoom - 10, 10);
    imageEditor.zoomSettings.zoomValue = newZoom;
}
</script>
```

### Zoom Settings Configuration

Configure zoom behavior:

```html
<ejs-imageeditor id="imageEditor"
    zoomSettings='new { minZoom = 10, maxZoom = 1000, zoomValue = 100 }'>
</ejs-imageeditor>
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `minZoom` | int | 10 | Minimum zoom percentage |
| `maxZoom` | int | 1000 | Maximum zoom percentage |
| `zoomValue` | int | 100 | Current zoom percentage (100 = actual size) |

### Zoom Levels

- **10-50%** - Zoom out to see full image
- **100%** - Actual image size
- **150-200%** - Zoom in for detail work
- **500%+** - Extreme close-up

## Panning Images

Panning allows users to move/scroll the image canvas when zoomed in.

### Pan via Mouse Drag

Click and drag the image to pan:

1. Zoom in on the image
2. Click and hold on the image
3. Drag to move to desired area
4. Release to stop panning

### When Panning is Enabled

Panning is automatically enabled when:

1. **Image is zoomed in** - When zoom level exceeds 100%
2. **During cropping** - You can pan while selecting crop region

### Programmatic Panning

You can programmatically move the view:

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Pan to specific coordinates
imageEditor.panSettings = {
    offsetX: 100,
    offsetY: 150
};
```

### Panning Behavior During Cropping

When using the crop tool:

1. Select crop region (custom, square, circle, or aspect ratio)
2. Pan the image to show different areas
3. The crop selection rectangle stays in place
4. Adjust crop region by dragging handles
5. Click tick to apply crop

## Practical Examples

### Full-Featured Image Viewer

```html
<ejs-imageeditor id="imageEditor"
    width="100%"
    height="600px"
    toolbar="@new List<string> { 'Open', 'ZoomIn', 'ZoomOut' }"
    zoomSettings='new { minZoom = 10, maxZoom = 1000 }'
    imageLoaded="OnImageLoaded">
</ejs-imageeditor>

<script>
function OnImageLoaded() {
    console.log('Image ready for viewing');
}

// Keyboard support
document.addEventListener('keydown', function(event) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    if (event.ctrlKey && event.key === '+') {
        imageEditor.zoomSettings.zoomValue = 
            Math.min((imageEditor.zoomSettings.zoomValue || 100) + 20, 1000);
    }
    if (event.ctrlKey && event.key === '-') {
        imageEditor.zoomSettings.zoomValue = 
            Math.max((imageEditor.zoomSettings.zoomValue || 100) - 20, 10);
    }
});
</script>
```

### Load and Zoom

```html
<input type="file" id="imageInput" onchange="loadAndZoom(event)">

<script>
function loadAndZoom(event) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var file = event.target.files[0];
    
    var reader = new FileReader();
    reader.onload = function(e) {
        imageEditor.open(e.target.result);
        // Auto-fit: zoom to 100% when image loads
        imageEditor.zoomSettings.zoomValue = 100;
    };
    reader.readAsDataURL(file);
}
</script>
```

## Best Practices

### 1. **Set Reasonable Zoom Limits**

```html
<ejs-imageeditor id="imageEditor"
    zoomSettings='new { minZoom = 25, maxZoom = 500 }'>
</ejs-imageeditor>
```

### 2. **Provide Zoom Feedback**

```html
<div id="zoomLevel">Zoom: 100%</div>

<script>
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Update display
function updateZoomDisplay() {
    var zoom = imageEditor.zoomSettings.zoomValue || 100;
    document.getElementById('zoomLevel').textContent = 'Zoom: ' + zoom + '%';
}

// Update on zoom change
setInterval(updateZoomDisplay, 100);
</script>
```

### 3. **Default Zoom on Image Load**

```javascript
imageEditor.imageLoaded = function() {
    // Fit to window after load
    imageEditor.zoomSettings.zoomValue = 100;
};
```

## Troubleshooting

**Issue:** Zoom buttons not working
- **Solution:** Ensure `ZoomIn` and `ZoomOut` are in toolbar list
- **Solution:** Check zoomSettings configuration

**Issue:** Panning not working
- **Solution:** Zoom level must exceed 100% to enable panning
- **Solution:** Verify image is large enough to require panning

**Issue:** Mouse wheel zoom not responding
- **Solution:** Ensure cursor is over image canvas
- **Solution:** Verify Ctrl key is pressed
- **Solution:** Check browser zoom level (should be 100%)
