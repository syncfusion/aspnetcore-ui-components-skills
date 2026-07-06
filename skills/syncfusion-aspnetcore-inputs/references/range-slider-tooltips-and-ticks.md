# Range Slider Tooltips and Ticks — ASP.NET Core

This reference covers tooltip display options, tick marks, labels, and positioning.

## Table of Contents
- [Tooltip Overview](#tooltip-overview)
- [Tooltip Placement](#tooltip-placement)
- [Ticks Configuration](#ticks-configuration)
- [Labels](#labels)
- [Examples](#examples)

---

## Tooltip Overview

Tooltips display the current value as users interact with the slider.

### Show/Hide Tooltips

**Razor View (CSHTML):**
```html
<!-- Default (tooltips shown) -->
<ejs-slider id="default" 
                 min="0" 
                 max="100" 
                 value="50">
</ejs-slider>

<!-- Tooltips hidden -->
<ejs-slider id="noTooltip" 
                 min="0" 
                 max="100" 
                 value="50"
                 showButtons="false">
</ejs-slider>
```

### Tooltip Formats

**Razor View (CSHTML):**
```html
<!-- Currency format -->
<ejs-slider id="currency" 
                 min="0" 
                 max="1000" 
                 value="500"
                 tooltipFormat="getCurrencyTooltip">
</ejs-slider>

<!-- Percentage format -->
<ejs-slider id="percentage" 
                 min="0" 
                 max="100" 
                 value="50"
                 tooltipFormat="getPercentageTooltip">
</ejs-slider>

<!-- Custom text -->
<ejs-slider id="custom" 
                 min="0" 
                 max="100" 
                 value="50"
                 tooltipFormat="getCustomTooltip">
</ejs-slider>

<script>
    function getCurrencyTooltip(value) {
        return '$' + value;
    }

    function getPercentageTooltip(value) {
        return value + '%';
    }

    function getCustomTooltip(value) {
        if (value < 33) return 'Low: ' + value;
        if (value < 67) return 'Medium: ' + value;
        return 'High: ' + value;
    }
</script>
```

---

## Tooltip Placement

### Before (Default)

Tooltip appears above/before the handle.

**Razor View (CSHTML):**
```html
<ejs-slider id="before" 
                 min="0" 
                 max="100" 
                 value="50"
                 tooltipPlacement="Before">
</ejs-slider>
```

### After

Tooltip appears below/after the handle.

**Razor View (CSHTML):**
```html
<ejs-slider id="after" 
                 min="0" 
                 max="100" 
                 value="50"
                 tooltipPlacement="After">
</ejs-slider>
```

### Example: Placement Comparison

**Razor View (CSHTML):**
```html
<div class="container mt-5">
    <h4>Tooltip Placement Options</h4>

    <div class="row mt-4">
        <div class="col-md-6 mb-4">
            <label>Tooltip Before (Top)</label>
            <ejs-slider id="tooltipBefore" 
                             min="0" 
                             max="100" 
                             value="50"
                             tooltipPlacement="Before">
            </ejs-slider>
        </div>

        <div class="col-md-6 mb-4">
            <label>Tooltip After (Bottom)</label>
            <ejs-slider id="tooltipAfter" 
                             min="0" 
                             max="100" 
                             value="50"
                             tooltipPlacement="After">
            </ejs-slider>
        </div>
    </div>
</div>
```

---

## Ticks Configuration

### Show Ticks

Display tick marks along the slider track.

**Razor View (CSHTML):**
```html
<ejs-slider id="withTicks" 
                 min="0" 
                 max="100" 
                 value="50"
                 showTicks="true">
</ejs-slider>
```

### Tick Positioning

**Razor View (CSHTML):**
```html
<!-- Ticks Before (above) -->
<ejs-slider id="ticksBefore" 
                 min="0" 
                 max="100" 
                 value="50"
                 showTicks="true"
                 tickPosition="Before">
</ejs-slider>

<!-- Ticks After (below) -->
<ejs-slider id="ticksAfter" 
                 min="0" 
                 max="100" 
                 value="50"
                 showTicks="true"
                 tickPosition="After">
</ejs-slider>

<!-- Ticks Both -->
<ejs-slider id="ticksBoth" 
                 min="0" 
                 max="100" 
                 value="50"
                 showTicks="true"
                 tickPosition="Both">
</ejs-slider>
```

### Tick Frequency

**Razor View (CSHTML):**
```html
<!-- Large ticks every 20 units -->
<ejs-slider id="largeTicks" 
                 min="0" 
                 max="100" 
                 value="50"
                 showTicks="true"
                 largeStep="20"
                 smallStep="1">
</ejs-slider>

<!-- Large ticks every 25 units, small every 5 -->
<ejs-slider id="mixedTicks" 
                 min="0" 
                 max="100" 
                 value="50"
                 showTicks="true"
                 largeStep="25"
                 smallStep="5">
</ejs-slider>
```

---

## Labels

### Tick Labels

Display text labels at tick positions.

**Razor View (CSHTML):**
```html
<ejs-slider id="labeled" 
                 min="0" 
                 max="100" 
                 value="50"
                 showTicks="true"
                 labels="getLabels"
                 largeStep="25">
</ejs-slider>

<script>
    function getLabels() {
        return ['Very Low', 'Low', 'Medium', 'High', 'Very High'];
    }
</script>
```

### Custom Label Formatting

**Razor View (CSHTML):**
```html
<ejs-slider id="customLabels" 
                 min="0" 
                 max="100" 
                 value="50"
                 showTicks="true"
                 labels="formatLabels"
                 largeStep="20">
</ejs-slider>

<script>
    function formatLabels(value) {
        if (value === 0) return 'Min';
        if (value === 100) return 'Max';
        return value + '%';
    }
</script>
```

---

## Examples

### Rating Scale with Ticks and Labels

**Razor View (CSHTML):**
```html
<div class="container mt-5">
    <div class="card" style="max-width: 500px; margin: 0 auto;">
        <div class="card-body">
            <h5 class="card-title">Rate Your Experience</h5>

            <ejs-slider id="ratingSlider" 
                             min="1" 
                             max="5" 
                             value="3"
                             step="1"
                             showTicks="true"
                             largeStep="1"
                             labels="getRatingLabels"
                             change="updateRatingDisplay"
                             type="single">
            </ejs-slider>

            <div class="text-center mt-4">
                <h4 id="ratingDisplay">★★★☆☆ Good</h4>
            </div>

            <p class="text-muted text-center small">
                1 = Poor, 5 = Excellent
            </p>
        </div>
    </div>
</div>

<script>
    function getRatingLabels(value) {
        const labels = {
            1: '★☆☆☆☆ Poor',
            2: '★★☆☆☆ Fair',
            3: '★★★☆☆ Good',
            4: '★★★★☆ Very Good',
            5: '★★★★★ Excellent'
        };
        return labels[value] || '';
    }

    function updateRatingDisplay(e) {
        const rating = e.value;
        document.getElementById('ratingDisplay').textContent = getRatingLabels(rating);
    }
</script>

<style>
    .e-tick {
        height: 6px;
    }

    .e-tick-major {
        height: 10px;
        background-color: #333;
    }

    .e-tick-label {
        font-size: 12px;
        color: #666;
    }
</style>
```

### Price Range with Dollar Labels

**Razor View (CSHTML):**
```html
<div class="container mt-5">
    <div class="card">
        <div class="card-body">
            <h5 class="card-title">Price Range</h5>

            <ejs-slider id="priceWithLabels" 
                             min="0" 
                             max="1000" 
                             startValue="250"
                             endValue="750"
                             step="50"
                             showTicks="true"
                             largeStep="250"
                             labels="formatPriceLabels"
                             change="updatePriceDisplay"
                             type="range">
            </ejs-slider>

            <div class="row mt-4">
                <div class="col-6">
                    <p class="text-muted small">Min Price</p>
                    <p class="h4">$<span id="minPrice">250</span></p>
                </div>
                <div class="col-6 text-end">
                    <p class="text-muted small">Max Price</p>
                    <p class="h4">$<span id="maxPrice">750</span></p>
                </div>
            </div>
        </div>
    </div>
</div>

<script>
    function formatPriceLabels(value) {
        return '$' + value;
    }

    function updatePriceDisplay(e) {
        document.getElementById('minPrice').textContent = e.startValue;
        document.getElementById('maxPrice').textContent = e.endValue;
    }
</script>

<style>
    .e-tick-label {
        font-weight: 500;
        color: #2c3e50;
    }

    .e-tick-major {
        background-color: #3498db;
    }
</style>
```

### Difficulty Level with Rich Ticks

**Razor View (CSHTML):**
```html
<div class="container mt-5">
    <h4>Select Difficulty Level</h4>

    <ejs-slider id="difficultySlider" 
                     min="1" 
                     max="5" 
                     value="3"
                     step="1"
                     showTicks="true"
                     largeStep="1"
                     change="updateDifficulty"
                     type="single">
    </ejs-slider>

    <div class="mt-4 p-3" id="difficultyInfo" style="background: #f8f9fa; border-radius: 4px;">
        <h5>Medium Difficulty</h5>
        <p class="mb-0 text-muted">
            Suitable for intermediate players with some experience.
        </p>
    </div>
</div>

<script>
    const difficulties = [
        { name: 'Tutorial', desc: 'Perfect for beginners to learn the basics.' },
        { name: 'Easy', desc: 'Relaxed pace, suitable for casual players.' },
        { name: 'Medium', desc: 'Balanced challenge for intermediate players.' },
        { name: 'Hard', desc: 'Challenging for experienced players.' },
        { name: 'Extreme', desc: 'Maximum challenge for hardcore enthusiasts.' }
    ];

    function updateDifficulty(e) {
        const difficulty = difficulties[e.value - 1];
        const infoDiv = document.getElementById('difficultyInfo');
        infoDiv.innerHTML = `
            <h5>${difficulty.name}</h5>
            <p class="mb-0 text-muted">${difficulty.desc}</p>
        `;
    }
</script>

<style>
    .e-handle {
        width: 16px;
        height: 16px;
        border-radius: 50%;
    }

    .e-tick-major {
        height: 12px;
        background-color: #3498db;
    }

    .e-tick {
        height: 4px;
        background-color: #bdc3c7;
    }

    .e-tick-label {
        font-size: 13px;
        font-weight: 500;
        color: #2c3e50;
    }
</style>
```

### Time Duration with Ticks

**Razor View (CSHTML):**
```html
<div class="container mt-5">
    <h4>Video Duration Selection</h4>

    <ejs-slider id="durationSlider" 
                     min="0" 
                     max="120" 
                     value="30"
                     step="5"
                     showTicks="true"
                     largeStep="30"
                     labels="formatDuration"
                     change="updateDuration"
                     type="single">
    </ejs-slider>

    <div class="mt-4 text-center">
        <h5 id="durationDisplay">30 minutes</h5>
        <p class="text-muted">Video Length</p>
    </div>
</div>

<script>
    function formatDuration(value) {
        return value + ' min';
    }

    function updateDuration(e) {
        const minutes = e.value;
        let text = minutes + ' minute';
        if (minutes !== 1) text += 's';
        document.getElementById('durationDisplay').textContent = text;
    }
</script>

<style>
    .e-tick-label {
        font-size: 12px;
        color: #666;
    }

    .e-tick-major {
        height: 12px;
        background-color: #27ae60;
    }

    .e-tick {
        height: 6px;
        background-color: #ecf0f1;
    }
</style>
```

---

## See Also

- `range-slider-getting-started.md` — Quick start guide
- `range-slider-types-and-orientation.md` — Types and orientations
- `range-slider-events-and-methods.md` — Event handling
- `range-slider-styling.md` — Custom styling
- `range-slider-api-reference.md` — Complete API reference
