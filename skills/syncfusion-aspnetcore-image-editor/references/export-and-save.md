# Export and Save Images

## Table of Contents
- [Supported Export Formats](#supported-export-formats)
- [Open Images](#open-images)
- [Save Methods](#save-methods)
- [Image Quality](#image-quality)
- [Practical Examples](#practical-examples)

## Supported Export Formats

The Image Editor supports exporting to 5 formats:

| Format | Extension | Use Case | Quality |
|--------|-----------|----------|---------|
| PNG | .png | Web, lossless | Lossless |
| JPEG | .jpg | Photos, web | Adjustable |
| WebP | .webp | Modern web | Balanced |
| SVG | .svg | Scalable graphics | Vector |
| BMP | .bmp | Windows, legacy | Uncompressed |

## Open Images

### Open from Local File

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
imageEditor.open('sample.png');
```

### Open from Base64

```javascript
var base64Image = 'data:image/png;base64,iVBORw0KGgoAAAANS...';
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
imageEditor.open(base64Image);
```

### Open from URL

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
imageEditor.open('https://example.com/image.jpg');
```

### Open from Blob

```html
<input type="file" id="imageFile" onchange="openBlob(event)">

<script>
function openBlob(event) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var file = event.target.files[0];
    
    var reader = new FileReader();
    reader.onload = function(e) {
        imageEditor.open(e.target.result); // Base64 from blob
    };
    reader.readAsDataURL(file);
}
</script>
```

## Save Methods

### Save via Toolbar

1. Click **Save** button in toolbar
2. Choose export format (PNG, JPEG, SVG, WEBP, BMP)
3. For JPEG, adjust quality slider
4. Click Download

### Save Programmatically

Use the `toBlob()` method:

```html
<ejs-imageeditor id="imageEditor"></ejs-imageeditor>

<button onclick="saveImage()">Save Image</button>

<script>
function saveImage() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    imageEditor.toBlob(function(blob) {
        downloadBlob(blob, 'edited-image.png');
    });
}

function downloadBlob(blob, filename) {
    var url = URL.createObjectURL(blob);
    var link = document.createElement('a');
    link.href = url;
    link.download = filename;
    link.click();
    URL.revokeObjectURL(url);
}
</script>
```

### Save with Format Selection

```javascript
function saveAsFormat(format) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Format: 'png', 'jpeg', 'webp', 'svg', 'bmp'
    imageEditor.toBlob(function(blob) {
        downloadBlob(blob, 'edited-image.' + format.toLowerCase());
    }, format);
}
```

## Image Quality

### JPEG Quality Control

```html
<input type="range" min="0" max="100" value="80" 
    onchange="saveWithQuality(this.value)">

<script>
function saveWithQuality(quality) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    imageEditor.toBlob(function(blob) {
        downloadBlob(blob, 'image-q' + quality + '.jpg');
    }, 'jpeg', quality / 100);
}
</script>
```

### Quality Guidelines

- **100 (highest)** - Best quality, largest file
- **80-90** - Excellent quality, good file size
- **60-75** - Good quality for web
- **30-50** - Lower quality, smaller file
- **0 (lowest)** - Minimal quality, smallest file

## Get Image Data

### Get as Base64

```javascript
function getImageData() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    imageEditor.toBlob(function(blob) {
        var reader = new FileReader();
        reader.onload = function(e) {
            var base64 = e.target.result;
            console.log('Base64:', base64);
            // Send to server
        };
        reader.readAsDataURL(blob);
    });
}
```

### Upload to Server

```html
<button onclick="uploadImage()">Upload to Server</button>

<script>
function uploadImage() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    imageEditor.toBlob(function(blob) {
        var formData = new FormData();
        formData.append('image', blob, 'edited-image.png');
        
        fetch('/api/upload', {
            method: 'POST',
            body: formData
        })
        .then(response => response.json())
        .then(data => console.log('Upload successful:', data))
        .catch(error => console.error('Upload failed:', error));
    });
}
</script>
```

## Keyboard Shortcuts

- **Ctrl+S** - Save in current format (same as loaded)

When image is loaded as PNG, Ctrl+S saves as PNG.
When image is loaded as JPEG, Ctrl+S saves as JPEG.

## Practical Examples

### Multi-Format Exporter

```html
<ejs-imageeditor id="imageEditor" width="100%" height="500px"></ejs-imageeditor>

<div class="export-options">
    <h3>Export As</h3>
    <button onclick="exportFormat('png')">📥 PNG (Lossless)</button>
    <button onclick="exportFormat('jpeg')">📥 JPEG (Compressed)</button>
    <button onclick="exportFormat('webp')">📥 WebP (Modern)</button>
    <button onclick="exportFormat('svg')">📥 SVG (Vector)</button>
</div>

<script>
function exportFormat(format) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var extension = format.toLowerCase();
    
    imageEditor.toBlob(function(blob) {
        var url = URL.createObjectURL(blob);
        var link = document.createElement('a');
        link.href = url;
        link.download = 'image.' + extension;
        link.click();
        URL.revokeObjectURL(url);
    }, format === 'jpeg' ? 'JPEG' : format.toUpperCase());
}
</script>
```

### Batch Export to Multiple Formats

```javascript
function batchExport() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var formats = ['png', 'jpeg', 'webp'];
    
    formats.forEach(function(format) {
        setTimeout(function() {
            imageEditor.toBlob(function(blob) {
                downloadBlob(blob, 'image.' + format);
            }, format.toUpperCase());
        }, 1000); // 1 second delay between exports
    });
}
```

### Save with Metadata

```javascript
function saveWithMetadata() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    imageEditor.toBlob(function(blob) {
        var metadata = {
            editedAt: new Date().toISOString(),
            format: 'png',
            size: blob.size,
            type: blob.type
        };
        
        // Save both image and metadata
        var formData = new FormData();
        formData.append('image', blob);
        formData.append('metadata', JSON.stringify(metadata));
        
        fetch('/api/save-with-metadata', {
            method: 'POST',
            body: formData
        });
    });
}
```

### Progressive Quality Saving

```html
<label>Quality Preset:</label>
<select onchange="savePreset(this.value)">
    <option value="">-- Select Preset --</option>
    <option value="high">High Quality (90%)</option>
    <option value="medium">Medium Quality (75%)</option>
    <option value="low">Low Quality (60%)</option>
    <option value="web">Web Optimized (70%)</option>
    <option value="mobile">Mobile Optimized (50%)</option>
</select>

<script>
const presets = {
    'high': { quality: 0.9, name: 'image-hq.jpg' },
    'medium': { quality: 0.75, name: 'image-md.jpg' },
    'low': { quality: 0.6, name: 'image-lq.jpg' },
    'web': { quality: 0.7, name: 'image-web.jpg' },
    'mobile': { quality: 0.5, name: 'image-mobile.jpg' }
};

function savePreset(preset) {
    if (!preset) return;
    
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var config = presets[preset];
    
    imageEditor.toBlob(function(blob) {
        downloadBlob(blob, config.name);
    }, 'JPEG', config.quality);
}
</script>
```

## Format Recommendations

### Choose PNG When:
- Lossless quality required
- Transparency needed
- Screenshots or diagrams
- No file size concerns

### Choose JPEG When:
- File size important
- Photographs
- Quality loss acceptable
- Maximum compatibility

### Choose WebP When:
- Modern browser support acceptable
- Smaller file sizes important
- Better compression than JPEG

### Choose SVG When:
- Scalable graphics needed
- Icons or logos
- Vector graphics
- Size-independent

### Choose BMP When:
- Legacy system support
- Uncompressed quality
- Windows compatibility
- Rarely needed today

## Save to Cloud Storage

### AWS S3 Example

```javascript
function uploadToS3() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    imageEditor.toBlob(function(blob) {
        fetch('/api/get-s3-url', { method: 'POST' })
            .then(r => r.json())
            .then(data => {
                var xhr = new XMLHttpRequest();
                xhr.open('PUT', data.uploadUrl);
                xhr.setRequestHeader('Content-Type', 'image/png');
                xhr.send(blob);
            });
    });
}
```

### Database Storage

```javascript
function saveToDatabase() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    imageEditor.toBlob(function(blob) {
        var reader = new FileReader();
        reader.onload = function(e) {
            var base64 = e.target.result;
            
            fetch('/api/save-image', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({
                    imageData: base64,
                    filename: 'edited-' + Date.now() + '.png'
                })
            });
        };
        reader.readAsDataURL(blob);
    });
}
```

## Troubleshooting

**Issue:** Downloaded file is corrupted
- **Solution:** Verify blob creation completed
- **Solution:** Check browser download settings

**Issue:** Quality loss on JPEG export
- **Solution:** Increase quality slider (0-100)
- **Solution:** Use PNG for lossless

**Issue:** File size too large
- **Solution:** Use JPEG instead of PNG
- **Solution:** Reduce JPEG quality setting
- **Solution:** Try WebP format

**Issue:** Browser compatibility
- **Solution:** Provide multiple format options
- **Solution:** Fallback to PNG for all browsers
