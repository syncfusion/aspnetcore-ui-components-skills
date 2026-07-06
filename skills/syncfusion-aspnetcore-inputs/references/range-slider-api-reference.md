# Range Slider API Reference — ASP.NET Core

Complete reference for Range Slider properties, methods, and events.

## Table of Contents
- [Properties](#properties)
- [Methods](#methods)
- [Events](#events)
- [JavaScript Interop](#javascript-interop)

---

## Properties

### Value Properties

#### `min`
Minimum value of the slider.

```html
<ejs-slider id="slider" min="0" max="100" value="50"></ejs-slider>
```

#### `max`
Maximum value of the slider.

```html
<ejs-slider id="slider" min="0" max="100" value="50"></ejs-slider>
```

#### `value` (Single Slider)
Current value of single slider.

```html
<ejs-slider id="single" min="0" max="100" value="50" type="single"></ejs-slider>
```

#### `startValue` (Range Slider)
Start value of range slider.

```html
<ejs-slider id="range" min="0" max="100" startValue="30" endValue="70" type="range"></ejs-slider>
```

#### `endValue` (Range Slider)
End value of range slider.

```html
<ejs-slider id="range" min="0" max="100" startValue="30" endValue="70" type="range"></ejs-slider>
```

### Configuration Properties

#### `step`
Incremental value for slider movement.

```html
<ejs-slider id="slider" min="0" max="100" value="50" step="5"></ejs-slider>
```

#### `type`
Type of slider: `single` or `range`.

```html
<!-- Single slider -->
<ejs-slider id="single" type="single" min="0" max="100" value="50"></ejs-slider>

<!-- Range slider -->
<ejs-slider id="range" type="range" min="0" max="100" startValue="30" endValue="70"></ejs-slider>
```

#### `orientation`
Slider orientation: `Horizontal` (default) or `Vertical`.

```html
<!-- Horizontal (default) -->
<ejs-slider id="horizontal" orientation="Horizontal" min="0" max="100" value="50"></ejs-slider>

<!-- Vertical -->
<div style="height: 300px;">
    <ejs-slider id="vertical" orientation="Vertical" min="0" max="100" value="50"></ejs-slider>
</div>
```

### State Properties

#### `enabled`
Enables or disables the slider.

```html
<ejs-slider id="slider" min="0" max="100" value="50" enabled="false"></ejs-slider>
```

#### `readOnly`
Makes slider read-only.

```html
<ejs-slider id="slider" min="0" max="100" value="50" readOnly="true"></ejs-slider>
```

### Style Properties

#### `cssClass`
Applies CSS classes to the slider.

```html
<ejs-slider id="slider" min="0" max="100" value="50" cssClass="e-primary"></ejs-slider>
```

### Accessibility Properties

#### `aria-label`
Provides accessible label.

```html
<ejs-slider id="slider" min="0" max="100" value="50" aria-label="Volume control"></ejs-slider>
```

#### `aria-labelledby`
References external label element ID.

```html
<label id="sliderLabel">Volume</label>
<ejs-slider id="slider" min="0" max="100" value="50" aria-labelledby="sliderLabel"></ejs-slider>
```

#### `aria-describedby`
References description element ID.

```html
<ejs-slider id="slider" min="0" max="100" value="50" aria-describedby="help"></ejs-slider>
<p id="help">Adjust the volume level</p>
```

---

## Methods

### `getValue()`
Gets the current value(s).

```javascript
const slider = document.getElementById('slider').ej2_instances[0];
const value = slider.getValue();  // Single slider
console.log(value);
```

### `setValue(value)`
Sets the value(s) programmatically.

```javascript
const slider = document.getElementById('slider').ej2_instances[0];

// Single slider
slider.value = 75;

// Range slider
slider.startValue = 20;
slider.endValue = 80;
```

### `refresh()`
Refreshes the slider after property changes.

```javascript
const slider = document.getElementById('slider').ej2_instances[0];
slider.min = 10;
slider.max = 90;
slider.refresh();
```

### `destroy()`
Destroys the slider component.

```javascript
const slider = document.getElementById('slider').ej2_instances[0];
slider.destroy();
```

---

## Events

### `created`
Fires when the component is initialized.

```html
<ejs-slider id="slider" min="0" max="100" value="50" created="onCreated"></ejs-slider>

<script>
    function onCreated(args) {
        console.log('Slider created');
    }
</script>
```

### `input`
Fires as user drags the slider (real-time).

```html
<ejs-slider id="slider" min="0" max="100" value="50" input="onInput"></ejs-slider>

<script>
    function onInput(args) {
        console.log('Current value:', args.value);
        console.log('Slider value:', args.value);
        // For range: args.startValue, args.endValue
    }
</script>
```

### `change`
Fires when user finishes dragging.

```html
<ejs-slider id="slider" min="0" max="100" value="50" change="onChange"></ejs-slider>

<script>
    function onChange(args) {
        console.log('Final value:', args.value);
    }
</script>
```

### `slideStart`
Fires when user starts dragging.

```html
<ejs-slider id="slider" min="0" max="100" value="50" slideStart="onSlideStart"></ejs-slider>

<script>
    function onSlideStart(args) {
        console.log('Drag started');
    }
</script>
```

### `slideEnd`
Fires when user stops dragging.

```html
<ejs-slider id="slider" min="0" max="100" value="50" slideEnd="onSlideEnd"></ejs-slider>

<script>
    function onSlideEnd(args) {
        console.log('Drag ended');
    }
</script>
```

---

## JavaScript Interop

### Accessing the Component

```javascript
const element = document.getElementById('slider');
const instance = element.ej2_instances[0];
```

### Complete Example

```html
<div class="container mt-5">
    <div class="card" style="max-width: 400px; margin: 0 auto;">
        <div class="card-body">
            <h5 class="card-title">Slider Interop Demo</h5>

            <ejs-slider id="demoSlider" 
                             min="0" 
                             max="100" 
                             value="50"
                             type="single">
            </ejs-slider>

            <div class="row mt-3">
                <div class="col-6">
                    <input class="form-control form-control-sm" 
                           id="valueInput" 
                           type="number" 
                           value="50" 
                           min="0" 
                           max="100">
                </div>
                <div class="col-6">
                    <button class="btn btn-sm btn-primary w-100" onclick="setSliderValue()">
                        Set Value
                    </button>
                </div>
            </div>

            <div class="mt-3 btn-group w-100" role="group">
                <button class="btn btn-outline-primary" onclick="setMin()">Min</button>
                <button class="btn btn-outline-primary" onclick="setMid()">Mid</button>
                <button class="btn btn-outline-primary" onclick="setMax()">Max</button>
            </div>

            <div id="info" class="alert alert-info mt-3 mb-0"></div>
        </div>
    </div>
</div>

<script>
    const slider = document.getElementById('demoSlider').ej2_instances[0];

    function setSliderValue() {
        const value = parseInt(document.getElementById('valueInput').value);
        if (value >= slider.min && value <= slider.max) {
            slider.value = value;
            updateInfo();
        }
    }

    function setMin() {
        slider.value = slider.min;
        updateInfo();
    }

    function setMid() {
        slider.value = Math.floor((slider.min + slider.max) / 2);
        updateInfo();
    }

    function setMax() {
        slider.value = slider.max;
        updateInfo();
    }

    function updateInfo() {
        const info = document.getElementById('info');
        info.innerHTML = `
            <strong>Current Value:</strong> ${slider.value}<br>
            <strong>Range:</strong> ${slider.min} - ${slider.max}
        `;
        document.getElementById('valueInput').value = slider.value;
    }

    // Initialize
    updateInfo();
</script>
```

---

## See Also

- `range-slider-getting-started.md` — Quick start guide
- `range-slider-types-and-orientation.md` — Types and orientations
- `range-slider-events-and-methods.md` — Event handling
- `range-slider-styling.md` — Custom styling
- `range-slider-accessibility.md` — Accessibility features
