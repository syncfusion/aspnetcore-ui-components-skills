# Range Slider Formatting and Limits — ASP.NET Core

This reference covers formatting labels, setting limits, step values, and custom formatting.

## Table of Contents
- [Min and Max Values](#min-and-max-values)
- [Step Increments](#step-increments)
- [Label Formatting](#label-formatting)
- [Value Limits](#value-limits)
- [Examples](#examples)

---

## Min and Max Values

### Setting Range Boundaries

**Razor View (CSHTML):**
```html
<!-- 0 to 100 (default) -->
<ejs-slider id="basic" 
                 min="0" 
                 max="100" 
                 value="50">
</ejs-slider>

<!-- 0 to 1000 (large range) -->
<ejs-slider id="large" 
                 min="0" 
                 max="1000" 
                 value="500">
</ejs-slider>

<!-- -50 to 50 (negative values) -->
<ejs-slider id="negative" 
                 min="-50" 
                 max="50" 
                 value="0">
</ejs-slider>

<!-- 20 to 80 (restricted range) -->
<ejs-slider id="restricted" 
                 min="20" 
                 max="80" 
                 value="50">
</ejs-slider>
```

### Dynamic Min-Max Updates

**Razor View (CSHTML):**
```html
<div class="container mt-5">
    <h4>Dynamic Range Configuration</h4>

    <div class="mb-3">
        <label class="form-label">Select Price Category:</label>
        <select class="form-select" onchange="updateRange(this.value)">
            <option value="budget">Budget ($0-500)</option>
            <option value="midrange">Mid-Range ($500-2000)</option>
            <option value="premium">Premium ($2000-5000)</option>
        </select>
    </div>

    <ejs-slider id="dynamicRange" 
                     min="0" 
                     max="500" 
                     startValue="100"
                     endValue="400"
                     type="range">
    </ejs-slider>

    <p class="mt-2">
        Selected Range: $<span id="rangeMin">100</span> - $<span id="rangeMax">400</span>
    </p>
</div>

<script>
    function updateRange(category) {
        const slider = document.getElementById('dynamicRange').ej2_instances[0];
        
        const configs = {
            budget: { min: 0, max: 500, start: 100, end: 400 },
            midrange: { min: 500, max: 2000, start: 800, end: 1500 },
            premium: { min: 2000, max: 5000, start: 3000, end: 4500 }
        };
        
        const config = configs[category];
        slider.min = config.min;
        slider.max = config.max;
        slider.startValue = config.start;
        slider.endValue = config.end;
        
        updateDisplay();
    }

    function updateDisplay() {
        const slider = document.getElementById('dynamicRange').ej2_instances[0];
        document.getElementById('rangeMin').textContent = slider.startValue;
        document.getElementById('rangeMax').textContent = slider.endValue;
    }
</script>
```

---

## Step Increments

### Fixed Step Value

Snap slider to increments:

**Razor View (CSHTML):**
```html
<!-- Step of 10 -->
<ejs-slider id="step10" 
                 min="0" 
                 max="100" 
                 value="50"
                 step="10">
</ejs-slider>

<!-- Step of 5 -->
<ejs-slider id="step5" 
                 min="0" 
                 max="100" 
                 value="50"
                 step="5">
</ejs-slider>

<!-- Step of 25 -->
<ejs-slider id="step25" 
                 min="0" 
                 max="100" 
                 value="50"
                 step="25">
</ejs-slider>
```

### Use Cases for Different Steps

**Razor View (CSHTML):**
```html
<!-- Volume (step: 1) -->
<div class="mb-3">
    <label>Volume (1% increments)</label>
    <ejs-slider id="volume" 
                     min="0" 
                     max="100" 
                     value="50"
                     step="1"
                     type="single">
    </ejs-slider>
</div>

<!-- Difficulty (step: 10) -->
<div class="mb-3">
    <label>Difficulty (10% increments)</label>
    <ejs-slider id="difficulty" 
                     min="0" 
                     max="100" 
                     value="50"
                     step="10"
                     type="single">
    </ejs-slider>
</div>

<!-- Year (step: 5) -->
<div class="mb-3">
    <label>Year (5-year increments)</label>
    <ejs-slider id="year" 
                     min="2000" 
                     max="2030" 
                     value="2015"
                     step="5"
                     type="single">
    </ejs-slider>
</div>
```

---

## Label Formatting

### Custom Value Display

**Razor View (CSHTML):**
```html
<div class="container mt-5">
    <h4>Formatted Label Display</h4>

    <!-- Dollar Format -->
    <div class="mb-4">
        <label>Price (with $ format)</label>
        <ejs-slider id="priceFormat" 
                         min="0" 
                         max="1000" 
                         value="500"
                         step="50"
                         input="formatPrice"
                         type="single">
        </ejs-slider>
        <p>Selected: $<span id="priceDisplay">500</span></p>
    </div>

    <!-- Percentage Format -->
    <div class="mb-4">
        <label>Percentage Discount</label>
        <ejs-slider id="discountFormat" 
                         min="0" 
                         max="100" 
                         value="25"
                         step="5"
                         input="formatDiscount"
                         type="single">
        </ejs-slider>
        <p>Discount: <span id="discountDisplay">25</span>%</p>
    </div>

    <!-- Temperature Format -->
    <div class="mb-4">
        <label>Temperature (with unit)</label>
        <ejs-slider id="tempFormat" 
                         min="0" 
                         max="100" 
                         value="20"
                         step="1"
                         input="formatTemp"
                         type="single">
        </ejs-slider>
        <p>Temperature: <span id="tempDisplay">20</span>°C</p>
    </div>
</div>

<script>
    function formatPrice(e) {
        document.getElementById('priceDisplay').textContent = e.value;
    }

    function formatDiscount(e) {
        document.getElementById('discountDisplay').textContent = e.value;
    }

    function formatTemp(e) {
        document.getElementById('tempDisplay').textContent = e.value;
    }
</script>
```

### Tooltip Formatting

**Razor View (CSHTML):**
```html
<ejs-slider id="tooltipFormat" 
                 min="0" 
                 max="100" 
                 value="50"
                 tooltipFormat="getTooltip"
                 type="single">
</ejs-slider>

<script>
    function getTooltip(value) {
        if (value < 33) {
            return 'Low: ' + value;
        } else if (value < 67) {
            return 'Medium: ' + value;
        } else {
            return 'High: ' + value;
        }
    }
</script>
```

---

## Value Limits

### Range Slider Limits

Enforce constraints between start and end values:

**Razor View (CSHTML):**
```html
<div class="container mt-5">
    <h4>Price Range with Limits</h4>

    <ejs-slider id="priceRangeLimited" 
                     min="0" 
                     max="1000" 
                     startValue="200"
                     endValue="800"
                     step="50"
                     change="validatePriceRange"
                     type="range">
    </ejs-slider>

    <p class="mt-2">
        Price: $<span id="limitedMin">200</span> - $<span id="limitedMax">800</span>
    </p>
    <div id="rangeWarning" class="text-warning"></div>
</div>

<script>
    function validatePriceRange(e) {
        const minPrice = e.startValue;
        const maxPrice = e.endValue;
        const warning = document.getElementById('rangeWarning');

        // Minimum range of $100
        if ((maxPrice - minPrice) < 100) {
            warning.textContent = '⚠️ Price range must be at least $100';
            return;
        }

        warning.textContent = '';

        document.getElementById('limitedMin').textContent = minPrice;
        document.getElementById('limitedMax').textContent = maxPrice;
    }
</script>
```

### Value Clamping

Ensure values stay within bounds:

**Razor View (CSHTML):**
```html
<ejs-slider id="clampedSlider" 
                 min="10" 
                 max="90" 
                 value="50"
                 step="5"
                 type="single">
</ejs-slider>

<script>
    // Values will automatically clamp to 10-90 range
    const slider = document.getElementById('clampedSlider').ej2_instances[0];
    
    // Attempting to set value outside range will clamp it
    slider.value = 150;  // Will be set to 90 (max)
    slider.value = 5;    // Will be set to 10 (min)
</script>
```

---

## Examples

### Time Range Selector (with formatting)

**Razor View (CSHTML):**
```html
<div class="card mt-5" style="max-width: 400px; margin: 0 auto;">
    <div class="card-body">
        <h5 class="card-title">Select Time Range</h5>

        <ejs-slider id="timeRange" 
                         min="0" 
                         max="24" 
                         startValue="9"
                         endValue="17"
                         step="1"
                         change="updateTimeDisplay"
                         type="range">
        </ejs-slider>

        <div class="row mt-3">
            <div class="col-6">
                <p class="text-muted small">Start Time</p>
                <p class="h5" id="startTime">9:00 AM</p>
            </div>
            <div class="col-6 text-end">
                <p class="text-muted small">End Time</p>
                <p class="h5" id="endTime">5:00 PM</p>
            </div>
        </div>

        <p class="text-center text-muted mt-2">
            Duration: <strong id="duration">8 hours</strong>
        </p>
    </div>
</div>

<script>
    function formatHour(hour) {
        const period = hour >= 12 ? 'PM' : 'AM';
        const displayHour = hour % 12 || 12;
        return displayHour + ':00 ' + period;
    }

    function updateTimeDisplay(e) {
        const startHour = e.startValue;
        const endHour = e.endValue;

        document.getElementById('startTime').textContent = formatHour(startHour);
        document.getElementById('endTime').textContent = formatHour(endHour);

        const duration = endHour - startHour;
        document.getElementById('duration').textContent = duration + ' hours';
    }
</script>
```

### Discount Price Calculator

**Razor View (CSHTML):**
```html
<div class="container mt-5">
    <div class="card" style="max-width: 400px; margin: 0 auto;">
        <div class="card-body">
            <h5 class="card-title">Price with Discount</h5>

            <p class="text-muted">Original Price: $<strong>100</strong></p>

            <label class="form-label">Discount Amount (%)</label>
            <ejs-slider id="discountAmount" 
                             min="0" 
                             max="50" 
                             value="10"
                             step="5"
                             input="calculateDiscount"
                             type="single">
            </ejs-slider>

            <div class="row mt-4">
                <div class="col-6">
                    <p class="text-muted small">Discount</p>
                    <p class="h5 text-danger">
                        -$<span id="discountValue">10</span>
                    </p>
                </div>
                <div class="col-6 text-end">
                    <p class="text-muted small">Final Price</p>
                    <p class="h5 text-success">
                        $<span id="finalPrice">90</span>
                    </p>
                </div>
            </div>
        </div>
    </div>
</div>

<script>
    function calculateDiscount(e) {
        const originalPrice = 100;
        const discountPercent = e.value;
        const discountAmount = (originalPrice * discountPercent) / 100;
        const finalPrice = originalPrice - discountAmount;

        document.getElementById('discountValue').textContent = discountAmount.toFixed(2);
        document.getElementById('finalPrice').textContent = finalPrice.toFixed(2);
    }
</script>
```

---

## See Also

- `range-slider-getting-started.md` — Quick start guide
- `range-slider-types-and-orientation.md` — Types and orientations
- `range-slider-styling.md` — Custom styling
- `range-slider-api-reference.md` — Complete API reference
