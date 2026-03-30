# Image Redaction in Image Editor

## Table of Contents
- [Redaction Types](#redaction-types)
- [Drawing Redactions](#drawing-redactions)
- [Managing Redactions](#managing-redactions)
- [Practical Examples](#practical-examples)

## Redaction Types

The Image Editor supports 2 redaction types for hiding sensitive information:

1. **Blur** - Gaussian blur effect (default)
2. **Pixelate** - Pixel mosaic effect

## Drawing Redactions

### Using Toolbar

1. Click **Redact** button in toolbar
2. Select redaction type (Blur or Pixelate)
3. Click and drag on canvas to draw redaction
4. Adjust size and intensity
5. Click tick to apply

### Programmatic Redaction

Use the `drawRedact()` method:

```html
<ejs-imageeditor id="imageEditor"></ejs-imageeditor>

<button onclick="blurArea()">Add Blur Redaction</button>
<button onclick="pixelateArea()">Add Pixelate Redaction</button>

<script>
function blurArea() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    imageEditor.drawRedact(
        'Blur',    // type
        100,       // x coordinate
        100,       // y coordinate
        150,       // width
        80,        // height
        20         // blur value (0-100)
    );
}

function pixelateArea() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    imageEditor.drawRedact(
        'Pixelate',  // type
        200,         // x
        200,         // y
        150,         // width
        80,          // height
        8            // pixel size
    );
}
</script>
```

### DrawRedact Parameters

```javascript
imageEditor.drawRedact(
    type,     // string: 'Blur' or 'Pixelate'
    x,        // number: x-coordinate
    y,        // number: y-coordinate
    width,    // number: redaction width
    height,   // number: redaction height
    value     // number: blur intensity or pixel size
);
```

## Redaction Properties

### Default Values

- **Type:** Blur (if not specified)
- **X:** Center of image if not specified
- **Y:** Center of image if not specified
- **Width:** 100 pixels
- **Height:** 50 pixels
- **Value:** 20 (blur strength or pixel size)

### Blur Redaction

Applies Gaussian blur to hide content:

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Light blur
imageEditor.drawRedact('Blur', 100, 100, 150, 80, 10);

// Medium blur
imageEditor.drawRedact('Blur', 100, 100, 150, 80, 20);

// Heavy blur
imageEditor.drawRedact('Blur', 100, 100, 150, 80, 40);
```

### Pixelate Redaction

Applies pixelation/mosaic effect:

```javascript
// Small pixels (less blur)
imageEditor.drawRedact('Pixelate', 100, 100, 150, 80, 5);

// Medium pixels
imageEditor.drawRedact('Pixelate', 100, 100, 150, 80, 8);

// Large pixels (more blur)
imageEditor.drawRedact('Pixelate', 100, 100, 150, 80, 15);
```

## Managing Redactions

### Select Redaction

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Get all redactions first
var redactions = imageEditor.getRedacts();

// Select specific redaction by ID
if (redactions.length > 0) {
    imageEditor.selectRedact(redactions[0].id);
}
```

### Update Redaction

Modify existing redaction properties:

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Get redactions
var redactions = imageEditor.getRedacts();

if (redactions.length > 0) {
    // Update first redaction
    var updatedSetting = {
        type: 'Pixelate',
        value: 12,
        width: 200,
        height: 100
    };
    
    imageEditor.updateRedact(updatedSetting, true);
}
```

### Delete Redaction

Remove specific redaction:

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Get redactions
var redactions = imageEditor.getRedacts();

if (redactions.length > 0) {
    // Delete first redaction
    imageEditor.deleteRedact(redactions[0].id);
}
```

### Get All Redactions

Retrieve information about all redactions:

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

var redactions = imageEditor.getRedacts();
console.log('Total redactions:', redactions.length);

redactions.forEach(function(redaction, index) {
    console.log('Redaction ' + index + ':', {
        id: redaction.id,
        type: redaction.type,
        x: redaction.x,
        y: redaction.y,
        width: redaction.width,
        height: redaction.height,
        value: redaction.value
    });
});
```

## Practical Examples

### Privacy Protection Tool

```html
<ejs-imageeditor id="imageEditor" width="100%" height="600px"></ejs-imageeditor>

<div class="redaction-controls">
    <h3>Privacy Redaction</h3>
    
    <button onclick="redactFace()">Redact Face</button>
    <button onclick="redactText()">Redact Text</button>
    <button onclick="redactLicensePlate()">Redact License Plate</button>
    <button onclick="clearAllRedactions()">Clear All</button>
    
    <div id="redactionList"></div>
</div>

<script>
function redactFace() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.drawRedact('Blur', 150, 150, 120, 150, 25);
    updateRedactionList();
}

function redactText() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.drawRedact('Pixelate', 200, 300, 200, 40, 10);
    updateRedactionList();
}

function redactLicensePlate() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.drawRedact('Pixelate', 250, 250, 150, 50, 12);
    updateRedactionList();
}

function clearAllRedactions() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var redactions = imageEditor.getRedacts();
    
    redactions.forEach(function(redaction) {
        imageEditor.deleteRedact(redaction.id);
    });
    
    updateRedactionList();
}

function updateRedactionList() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var redactions = imageEditor.getRedacts();
    
    var listHtml = '<p>Redactions: ' + redactions.length + '</p>';
    document.getElementById('redactionList').innerHTML = listHtml;
}
</script>
```

### Document Confidentiality Redactor

```javascript
// Redact sensitive company information
function redactConfidential() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    // Redact company name (top-left)
    imageEditor.drawRedact('Blur', 20, 20, 200, 30, 20);
    
    // Redact employee ID
    imageEditor.drawRedact('Pixelate', 20, 150, 100, 25, 8);
    
    // Redact salary information
    imageEditor.drawRedact('Blur', 300, 200, 150, 25, 20);
}
```

### Interactive Redaction Management

```html
<div class="redaction-manager">
    <button onclick="listRedactions()">Show Redactions</button>
    <button onclick="editRedaction(0)">Edit 1st Redaction</button>
    <button onclick="removeRedaction(0)">Delete 1st Redaction</button>
    
    <div id="redactionDetails"></div>
</div>

<script>
function listRedactions() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var redactions = imageEditor.getRedacts();
    
    var html = '<h4>Redactions (' + redactions.length + ')</h4>';
    redactions.forEach(function(r, i) {
        html += '<div>' + (i + 1) + '. ' + r.type + ' - ' + r.width + 'x' + r.height + '</div>';
    });
    
    document.getElementById('redactionDetails').innerHTML = html;
}

function editRedaction(index) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var redactions = imageEditor.getRedacts();
    
    if (index < redactions.length) {
        // Update with new blur value
        var newSetting = redactions[index];
        newSetting.value = 30;
        
        imageEditor.updateRedact(newSetting, true);
    }
}

function removeRedaction(index) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var redactions = imageEditor.getRedacts();
    
    if (index < redactions.length) {
        imageEditor.deleteRedact(redactions[index].id);
    }
}
</script>
```

## Best Practices

### 1. **Choose Redaction Type**

- **Blur** - For sensitive text/faces (more natural)
- **Pixelate** - For dramatic privacy effect

### 2. **Cover Completely**

Ensure redaction fully covers sensitive area:

```javascript
// Make redaction larger than necessary
imageEditor.drawRedact('Blur', 140, 140, 160, 180, 25);
// Better to over-cover than under-cover
```

### 3. **Test Before Exporting**

Verify redactions are sufficient before saving:

```javascript
// View redacted image
// Take screenshot/test
// Then export if satisfied
```

### 4. **Keep Redaction Data**

Store redaction positions for later modification:

```javascript
var redactions = imageEditor.getRedacts();
localStorage.setItem('redactions', JSON.stringify(redactions));
```

## Legal/Compliance Use

The redaction feature is useful for:

- GDPR compliance (hiding personal data)
- HIPAA compliance (medical records)
- Legal discovery (confidential documents)
- Privacy protection (public sharing)
- Security purposes (hiding sensitive info)

## Undo/Redo with Redactions

Redactions are tracked in history:

```javascript
imageEditor.drawRedact('Blur', 100, 100, 150, 80, 20);  // Add redaction
imageEditor.undo();                                      // Ctrl+Z removes it
imageEditor.redo();                                      // Ctrl+Y reapplies it
```

## Troubleshooting

**Issue:** Redaction not covering area
- **Solution:** Increase width/height parameters
- **Solution:** Increase blur value for more effect

**Issue:** Redaction looks like blur instead of pixelate
- **Solution:** Verify type parameter is 'Pixelate'
- **Solution:** Increase pixel size value

**Issue:** Can't delete redaction
- **Solution:** Use getRedacts() to get correct ID
- **Solution:** Verify redaction exists before deleting
