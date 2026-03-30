# Filters in Image Editor

## Table of Contents
- [Available Filters](#available-filters)
- [Apply Filters](#apply-filters)
- [Filter Effects](#filter-effects)
- [Filter Events](#filter-events)
- [Best Practices](#best-practices)

## Available Filters

The Image Editor provides 8 pre-defined filters to enhance images:

| Filter | Effect |
|--------|--------|
| `Default` | Original image (no filter) |
| `Cold` | Blue/cool tone, reduces warmth |
| `Warm` | Orange/warm tone, increases warmth |
| `Chrome` | High contrast metallic effect |
| `Sepia` | Vintage brownish tone |
| `Invert` | Inverts all colors (negative) |
| `Grayscale` | Converts to black & white |
| `BlackAndWhite` | High contrast black & white |

## Apply Filters

### Using Toolbar

1. Click **Filter** button in toolbar
2. Select desired filter from dropdown
3. Filter applies immediately to image
4. Click tick to confirm

### Programmatic Filter Application

Use the `applyImageFilter()` method:

```html
<ejs-imageeditor id="imageEditor"></ejs-imageeditor>

<button onclick="applyWarmFilter()">Warm Filter</button>
<button onclick="applySepiaFilter()">Sepia Filter</button>
<button onclick="applyGrayscaleFilter()">Grayscale</button>
<button onclick="removeFilter()">Remove Filter</button>

<script>
function applyWarmFilter() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.applyImageFilter('Warm');
}

function applySepiaFilter() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.applyImageFilter('Sepia');
}

function applyGrayscaleFilter() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.applyImageFilter('Grayscale');
}

function removeFilter() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.applyImageFilter('Default');
}
</script>
```

## Filter Effects

### Cold

Creates a cool, blue-toned effect. Useful for:
- Winter/snow themes
- Cool mood photography
- Reducing warm colors

```javascript
imageEditor.applyImageFilter('Cold');
```

### Warm

Adds warm, orange tones. Useful for:
- Sunset/sunrise enhancement
- Warm mood photography
- Golden hour effect

```javascript
imageEditor.applyImageFilter('Warm');
```

### Chrome

High contrast metallic effect. Useful for:
- Modern/artistic style
- Enhanced sharpness
- Bold look

```javascript
imageEditor.applyImageFilter('Chrome');
```

### Sepia

Vintage brown tones. Useful for:
- Vintage photo effect
- Aged photograph look
- Nostalgic styling

```javascript
imageEditor.applyImageFilter('Sepia');
```

### Invert

Creates color-inverted effect. Useful for:
- Negative effect
- Special artistic use
- Accessibility (dark mode images)

```javascript
imageEditor.applyImageFilter('Invert');
```

### Grayscale

Converts to black and white (color removed). Useful for:
- Professional/formal look
- Document processing
- Testing color sensitivity

```javascript
imageEditor.applyImageFilter('Grayscale');
```

### BlackAndWhite

High contrast black and white. Useful for:
- Dramatic effect
- Document scanning
- Bold artistic style

```javascript
imageEditor.applyImageFilter('BlackAndWhite');
```

### Default

Removes all filters, returns to original:

```javascript
imageEditor.applyImageFilter('Default');
```

## Filter Events

### Image Filtering Event

Handle filter application:

```html
<ejs-imageeditor id="imageEditor"
    imageFiltering="OnImageFiltering">
</ejs-imageeditor>

<script>
function OnImageFiltering(args) {
    console.log('Applying filter:', args.Filter);
    
    // You can cancel the filter if needed
    // args.Cancel = true;
}
</script>
```

### Event Arguments

The `ImageFilterEventArgs` provides:

- **filter** - The filter type being applied (Cold, Warm, Chrome, Sepia, Invert, Grayscale, BlackAndWhite)
- **cancel** - Boolean to cancel the operation

### Filter Change Logging

```javascript
function OnImageFiltering(args) {
    var filterName = args.Filter;
    var timestamp = new Date().toLocaleTimeString();
    
    console.log(filterName + ' filter applied at ' + timestamp);
    
    // Log to server
    fetch('/api/log-filter', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ filter: filterName, time: timestamp })
    });
}
```

## Best Practices

### 1. **Provide Filter Preview**

Show filter name before applying:

```html
<div class="filter-selector">
    <button onclick="previewFilter('Cold')">❄️ Cold</button>
    <button onclick="previewFilter('Warm')">🔥 Warm</button>
    <button onclick="previewFilter('Sepia')">🟤 Sepia</button>
    <button onclick="previewFilter('Grayscale')">⚪ Grayscale</button>
    <button onclick="previewFilter('Default')">✓ Original</button>
</div>

<script>
function previewFilter(filterName) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.applyImageFilter(filterName);
}
</script>
```

### 2. **Allow Filter Combinations**

```javascript
// Apply warm, then add some chrome effect
imageEditor.applyImageFilter('Warm');
// Note: Filters override each other, not combine
// Use fine-tuning instead for more control
```

### 3. **Save Filter Preference**

```javascript
function applyAndRemember(filterName) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.applyImageFilter(filterName);
    
    // Remember user preference
    localStorage.setItem('lastUsedFilter', filterName);
}

// On page load
var lastFilter = localStorage.getItem('lastUsedFilter') || 'Default';
```

### 4. **Filter + Fine-tuning Workflow**

For more control, combine filters with fine-tuning:

```javascript
// Apply base filter
imageEditor.applyImageFilter('Warm');

// Then fine-tune for exact effect
imageEditor.finetuneImage('Brightness', 10);
imageEditor.finetuneImage('Contrast', 20);
```

## Practical Examples

### Photo Enhancement Suite

```html
<ejs-imageeditor id="imageEditor" width="100%" height="500px">
</ejs-imageeditor>

<div class="enhancement-controls">
    <h3>Quick Filters</h3>
    <div class="filter-buttons">
        <button onclick="applyFilter('Default')">Original</button>
        <button onclick="applyFilter('Cold')">Cool</button>
        <button onclick="applyFilter('Warm')">Warm</button>
        <button onclick="applyFilter('Sepia')">Vintage</button>
        <button onclick="applyFilter('Chrome')">Bold</button>
        <button onclick="applyFilter('Grayscale')">B&W</button>
    </div>
</div>

<script>
function applyFilter(filterName) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.applyImageFilter(filterName);
}
</script>
```

### Artistic Photo Editor

```html
<ejs-imageeditor id="imageEditor"></ejs-imageeditor>

<div class="artistic-filters">
    <button onclick="artisticEffect()">Create Artistic Version</button>
    <button onclick="vintageEffect()">Vintage Look</button>
    <button onclick="blackWhiteEffect()">High Contrast</button>
</div>

<script>
function artisticEffect() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.applyImageFilter('Chrome');
}

function vintageEffect() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.applyImageFilter('Sepia');
}

function blackWhiteEffect() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.applyImageFilter('BlackAndWhite');
}
</script>
```

### Filter Before/After Comparison

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
var currentFilter = 'Default';

function showBefore() {
    imageEditor.applyImageFilter('Default');
}

function showAfter() {
    imageEditor.applyImageFilter(currentFilter);
}

function setFilter(filterName) {
    currentFilter = filterName;
    imageEditor.applyImageFilter(filterName);
}
```

## Undo/Redo with Filters

Filters are included in undo/redo history:

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

imageEditor.applyImageFilter('Warm');    // User applies warm filter
imageEditor.undo();                      // User presses Ctrl+Z
// Back to original (no filter)

imageEditor.redo();                      // User presses Ctrl+Y
// Warm filter reapplied
```

## Troubleshooting

**Issue:** Filter not applying
- **Solution:** Verify image is loaded first
- **Solution:** Check filter name spelling (case-sensitive)

**Issue:** Filter looks different than expected
- **Solution:** Different images respond differently to filters
- **Solution:** Use fine-tuning for precise control

**Issue:** Can't remove filter
- **Solution:** Use `applyImageFilter('Default')` to reset

## Difference: Filters vs Fine-tuning

| Feature | Filters | Fine-tuning |
|---------|---------|------------|
| **Types** | 8 preset effects | 7 adjustment types |
| **Control** | All-or-nothing | Slider control |
| **Examples** | Sepia, Warm, Chrome | Brightness, Contrast, Hue |
| **Use** | Quick effects | Precise adjustments |
| **Combination** | One at a time | Can combine multiple |

See `fine-tuning.md` for detailed fine-tuning options.
