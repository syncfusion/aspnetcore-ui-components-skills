# Fine-Tuning Image Adjustments

## Table of Contents
- [Available Fine-tune Options](#available-fine-tune-options)
- [Brightness, Contrast, Saturation](#brightness-contrast-saturation)
- [Hue, Exposure, Blur, Opacity](#hue-exposure-blur-opacity)
- [Fine-tune Events](#fine-tune-events)
- [Practical Examples](#practical-examples)

## Available Fine-tune Options

The Image Editor provides 7 precise adjustment controls via sliders:

| Option | Range | Default | Purpose |
|--------|-------|---------|---------|
| `Brightness` | 0-100 | 50 | Image lightness |
| `Contrast` | 0-100 | 50 | Color depth difference |
| `Saturation` | 0-100 | 50 | Color intensity |
| `Hue` | 0-360 | 0 | Color shift (degrees) |
| `Exposure` | 0-100 | 50 | Overall exposure level |
| `Blur` | 0-100 | 0 | Blur amount |
| `Opacity` | 0-100 | 100 | Transparency level |

## Brightness, Contrast, Saturation

### Brightness Adjustment

Controls overall image lightness/darkness.

#### Using Toolbar

1. Click **Finetune** button
2. Select **Brightness**
3. Adjust slider (0-100, default 50)
4. Click tick to apply

#### Programmatic Control

```html
<ejs-imageeditor id="imageEditor"></ejs-imageeditor>

<input type="range" min="0" max="100" value="50" 
    onchange="adjustBrightness(this.value)">
<span id="brightnessValue">50%</span>

<script>
function adjustBrightness(value) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.finetuneImage('Brightness', value);
    document.getElementById('brightnessValue').textContent = value + '%';
}
</script>
```

### Contrast Adjustment

Controls difference between light and dark areas.

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Increase contrast (more extreme difference)
imageEditor.finetuneImage('Contrast', 75);

// Decrease contrast (more uniform)
imageEditor.finetuneImage('Contrast', 25);

// Reset to default
imageEditor.finetuneImage('Contrast', 50);
```

### Saturation Adjustment

Controls color intensity/vibrancy.

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Highly saturated colors
imageEditor.finetuneImage('Saturation', 100);

// Muted colors
imageEditor.finetuneImage('Saturation', 25);

// Grayscale (no saturation)
imageEditor.finetuneImage('Saturation', 0);
```

### Combined Example

```html
<ejs-imageeditor id="imageEditor" width="100%" height="500px"></ejs-imageeditor>

<div class="adjustment-controls">
    <div>
        <label>Brightness:</label>
        <input type="range" min="0" max="100" value="50"
            onchange="setAdjustment('Brightness', this.value)">
        <span id="brightnessLabel">50</span>
    </div>
    
    <div>
        <label>Contrast:</label>
        <input type="range" min="0" max="100" value="50"
            onchange="setAdjustment('Contrast', this.value)">
        <span id="contrastLabel">50</span>
    </div>
    
    <div>
        <label>Saturation:</label>
        <input type="range" min="0" max="100" value="50"
            onchange="setAdjustment('Saturation', this.value)">
        <span id="saturationLabel">50</span>
    </div>
</div>

<script>
function setAdjustment(type, value) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.finetuneImage(type, value);
    
    document.getElementById(type.toLowerCase() + 'Label').textContent = value;
}
</script>
```

## Hue, Exposure, Blur, Opacity

### Hue Adjustment

Shifts colors around the color wheel (0-360°).

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Shift colors 90 degrees (green becomes cyan)
imageEditor.finetuneImage('Hue', 90);

// Shift 180 degrees (complementary colors)
imageEditor.finetuneImage('Hue', 180);

// No shift
imageEditor.finetuneImage('Hue', 0);
```

**Use Cases:**
- Change overall color tone
- Create color variations
- Correct color cast

### Exposure Adjustment

Controls overall light exposure level.

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Increase exposure (brighter, may wash out)
imageEditor.finetuneImage('Exposure', 80);

// Normal exposure
imageEditor.finetuneImage('Exposure', 50);

// Decrease exposure (darker, more detail)
imageEditor.finetuneImage('Exposure', 30);
```

**Use Cases:**
- Correct underexposed photos
- Fix overexposed images
- Adjust overall brightness with preservation

### Blur Adjustment

Applies blur effect to entire image.

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Slight blur
imageEditor.finetuneImage('Blur', 10);

// Moderate blur
imageEditor.finetuneImage('Blur', 30);

// Heavy blur
imageEditor.finetuneImage('Blur', 60);

// No blur
imageEditor.finetuneImage('Blur', 0);
```

**Use Cases:**
- Reduce noise
- Create dreamy effect
- Privacy (blur faces)

### Opacity Adjustment

Controls overall image transparency.

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Fully opaque (normal)
imageEditor.finetuneImage('Opacity', 100);

// 75% visible
imageEditor.finetuneImage('Opacity', 75);

// 50% transparent
imageEditor.finetuneImage('Opacity', 50);

// Nearly invisible
imageEditor.finetuneImage('Opacity', 10);
```

**Use Cases:**
- Create watermark effect
- Overlay on other content
- Fade effects

## Fine-tune Events

### Fine-tune Value Changing Event

Handle fine-tune adjustments:

```html
<ejs-imageeditor id="imageEditor"
    finetuneValueChanging="OnFinetuneValueChanging">
</ejs-imageeditor>

<div id="adjustmentLog"></div>

<script>
function OnFinetuneValueChanging(args) {
    var log = document.getElementById('adjustmentLog');
    log.innerHTML += '<p>' + args.Finetune + ': ' + args.Value + '</p>';
    
    console.log('Fine-tune:', args.Finetune, 'Value:', args.Value);
}
</script>
```

### Event Arguments

- **finetune** - Type of adjustment (Brightness, Contrast, etc.)
- **value** - Current value (0-100 or 0-360 for Hue)
- **cancel** - Boolean to cancel operation

## Practical Examples

### Photo Enhancement Tool

```html
<ejs-imageeditor id="imageEditor" width="100%" height="600px">
</ejs-imageeditor>

<div class="enhancement-panel">
    <h3>Image Enhancement</h3>
    
    <label>Brightness: <span id="bVal">50</span>%</label>
    <input type="range" min="0" max="100" value="50"
        oninput="enhance('Brightness', this.value)">
    
    <label>Contrast: <span id="cVal">50</span>%</label>
    <input type="range" min="0" max="100" value="50"
        oninput="enhance('Contrast', this.value)">
    
    <label>Saturation: <span id="sVal">50</span>%</label>
    <input type="range" min="0" max="100" value="50"
        oninput="enhance('Saturation', this.value)">
    
    <button onclick="resetEnhancements()">Reset All</button>
</div>

<script>
function enhance(type, value) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.finetuneImage(type, value);
    document.getElementById(type.charAt(0).toLowerCase() + 'Val').textContent = value;
}

function resetEnhancements() {
    document.querySelectorAll('input[type="range"]').forEach(function(slider) {
        slider.value = 50;
        enhance(slider.dataset.type || 'Brightness', 50);
    });
}
</script>
```

### Professional Photo Editor

```javascript
// Common enhancement presets
const presets = {
    'Vivid': { Brightness: 55, Contrast: 70, Saturation: 80 },
    'Soft': { Brightness: 60, Contrast: 30, Saturation: 40 },
    'Cool': { Brightness: 50, Contrast: 50, Saturation: 60, Hue: 180 },
    'Warm': { Brightness: 55, Contrast: 45, Saturation: 70, Hue: 30 },
    'BlackAndWhite': { Brightness: 50, Contrast: 70, Saturation: 0 }
};

function applyPreset(presetName) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var preset = presets[presetName];
    
    Object.keys(preset).forEach(function(type) {
        imageEditor.finetuneImage(type, preset[type]);
    });
}
```

### Color Temperature Adjustment

```javascript
function makeWarmer(amount) {
    // Amount: 1-10 (10 = very warm)
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var hueShift = amount * 10; // 0-100 degrees
    
    imageEditor.finetuneImage('Hue', hueShift);
    imageEditor.finetuneImage('Brightness', 50 + (amount * 2));
}

function makeCooler(amount) {
    // Amount: 1-10 (10 = very cool)
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var hueShift = 360 - (amount * 10); // Reverse hue
    
    imageEditor.finetuneImage('Hue', hueShift);
    imageEditor.finetuneImage('Saturation', 50 - (amount * 2));
}
```

### Exposure Recovery

```javascript
function fixUnderexposed(level) {
    // level: 1-10
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    imageEditor.finetuneImage('Exposure', 50 + (level * 5));
    imageEditor.finetuneImage('Brightness', 50 + (level * 2));
    imageEditor.finetuneImage('Contrast', 50 + (level * 3));
}

function fixOverexposed(level) {
    // level: 1-10
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    
    imageEditor.finetuneImage('Exposure', 50 - (level * 3));
    imageEditor.finetuneImage('Brightness', 50 - (level * 2));
    imageEditor.finetuneImage('Contrast', 50 + (level * 2));
}
```

## Best Practices

### 1. **Start with Default Values**

Always reset to defaults (50) before applying adjustments:

```javascript
function resetAllAdjustments() {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    var adjustments = ['Brightness', 'Contrast', 'Saturation', 'Hue', 'Exposure', 'Blur', 'Opacity'];
    
    adjustments.forEach(function(adj) {
        imageEditor.finetuneImage(adj, 50);
    });
}
```

### 2. **Provide Presets**

Help users with predefined starting points:

```javascript
const presets = {
    'Portrait': { Brightness: 55, Saturation: 70, Blur: 5 },
    'Landscape': { Contrast: 60, Saturation: 80, Exposure: 55 }
};
```

### 3. **Show Visual Feedback**

Display current values as users adjust:

```javascript
function adjustWithFeedback(type, value) {
    updateDisplay(type, value);
    applyAdjustment(type, value);
    logChange(type, value);
}
```

### 4. **Combine Strategically**

Some combinations work better than others:

- **Exposure + Brightness** - Avoid double adjustment
- **Contrast + Saturation** - Works well together
- **Hue + Saturation** - Good for color grading

## Troubleshooting

**Issue:** Adjustments not applying
- **Solution:** Verify value is in correct range
- **Solution:** Check that image is loaded first

**Issue:** Slider not responsive
- **Solution:** Use oninput instead of onchange for real-time updates

**Issue:** Value range confusion
- **Solution:** All range 0-100 except Hue (0-360)
- **Solution:** Default value 50 means "no adjustment"
