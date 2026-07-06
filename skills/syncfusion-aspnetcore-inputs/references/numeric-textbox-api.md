# NumericTextBox API Reference for ASP.NET Core

Complete API reference for the NumericTextBox TagHelper in ASP.NET Core using Syncfusion.EJ2.AspNet.Core.

## TagHelper Syntax

```html
<ejs-numerictextbox id="numericTextBox" property="value">
</ejs-numerictextbox>
```

---

## Properties

### value

**Type:** `decimal`  
**Default:** `null`

Gets or sets the current numeric value of the NumericTextBox.

```html
<ejs-numerictextbox id="numericTextBox" value="10.5">
</ejs-numerictextbox>
```

---

### min

**Type:** `decimal`  
**Default:** `null`

Gets or sets the minimum allowed value. If `strictMode` is enabled, values below this are rejected.

```html
<ejs-numerictextbox id="numericTextBox" value="50" min="0">
</ejs-numerictextbox>
```

---

### max

**Type:** `decimal`  
**Default:** `null`

Gets or sets the maximum allowed value. If `strictMode` is enabled, values above this are rejected.

```html
<ejs-numerictextbox id="numericTextBox" value="50" max="100">
</ejs-numerictextbox>
```

---

### step

**Type:** `decimal`  
**Default:** `1`

Gets or sets the increment step value when using the spinner buttons.

```html
<ejs-numerictextbox id="numericTextBox" value="10" step="5">
</ejs-numerictextbox>
```

---

### decimals

**Type:** `int`  
**Default:** `null`

Specifies the number of decimal places to display. When the control loses focus, the value is rounded to this precision.

```html
<ejs-numerictextbox id="numericTextBox" value="10.5" decimals="2">
</ejs-numerictextbox>
```

---

### format

**Type:** `string`  
**Default:** `null`

Gets or sets the format to display the numeric value. Uses .NET standard numeric format strings.

**Common format specifiers:**
- `n0` - Number format with no decimals (1,234)
- `n2` - Number format with 2 decimals (1,234.56)
- `c0` - Currency with no decimals ($100)
- `c2` - Currency with 2 decimals ($99.99)
- `p` - Percentage format (50%)
- `p2` - Percentage with 2 decimals (50.50%)
- `e2` - Scientific notation (1.23E+03)

```html
<!-- Currency -->
<ejs-numerictextbox id="currency" value="99.99" format="c2">
</ejs-numerictextbox>

<!-- Number with thousand separator -->
<ejs-numerictextbox id="number" value="1234.56" format="n2">
</ejs-numerictextbox>

<!-- Percentage -->
<ejs-numerictextbox id="percentage" value="0.5" format="p">
</ejs-numerictextbox>
```

---

### currency

**Type:** `string`  
**Default:** `null`

Specifies the currency code (ISO 4217) for currency formatting. Use with `format="c"` specifier.

**Common currency codes:**
- `USD` - US Dollar
- `EUR` - Euro
- `GBP` - British Pound
- `JPY` - Japanese Yen
- `CAD` - Canadian Dollar
- `AUD` - Australian Dollar

```html
<ejs-numerictextbox id="numericTextBox" value="99.99" format="c2" currency="EUR">
</ejs-numerictextbox>
```

---

### placeholder

**Type:** `string`  
**Default:** `null`

Gets or sets the placeholder text displayed when the control is empty.

```html
<ejs-numerictextbox id="numericTextBox" placeholder="Enter amount">
</ejs-numerictextbox>
```

---

### enabled

**Type:** `bool`  
**Default:** `true`

Gets or sets whether the NumericTextBox is enabled for user interaction. When `false`, the control appears disabled.

```html
<ejs-numerictextbox id="numericTextBox" value="10" enabled="false">
</ejs-numerictextbox>
```

---

### readonly

**Type:** `bool`  
**Default:** `false`

Gets or sets whether the NumericTextBox is read-only. Users can focus and select, but cannot edit the value.

```html
<ejs-numerictextbox id="numericTextBox" value="10" readonly="true">
</ejs-numerictextbox>
```

---

### strictMode

**Type:** `bool`  
**Default:** `true`

Gets or sets whether to enforce strict validation. When `true`, values outside the min/max range are not allowed. When `false`, out-of-range values are allowed but are corrected on blur.

```html
<!-- Strict validation enforced -->
<ejs-numerictextbox id="strict" value="50" min="0" max="100" strict-mode="true">
</ejs-numerictextbox>

<!-- Allow out-of-range values -->
<ejs-numerictextbox id="nonstrict" value="50" min="0" max="100" strict-mode="false">
</ejs-numerictextbox>
```

---

### showSpinButton

**Type:** `bool`  
**Default:** `true`

Gets or sets whether to display the spinner (up/down) buttons.

```html
<!-- With spinner buttons -->
<ejs-numerictextbox id="withSpinner" value="10" show-spin-button="true">
</ejs-numerictextbox>

<!-- Without spinner buttons -->
<ejs-numerictextbox id="noSpinner" value="10" show-spin-button="false">
</ejs-numerictextbox>
```

---

### allowMouseWheel

**Type:** `bool`  
**Default:** `true`

Gets or sets whether the mouse wheel can be used to increment/decrement the value.

```html
<ejs-numerictextbox id="numericTextBox" value="10" allow-mouse-wheel="true">
</ejs-numerictextbox>
```

---

### cssClass

**Type:** `string`  
**Default:** `null`

Gets or sets CSS classes for custom styling. Predefined classes: `e-small`, `e-bigger`, `e-error`, `e-warning`, `e-success`.

```html
<ejs-numerictextbox id="small" value="10" css-class="e-small">
</ejs-numerictextbox>

<ejs-numerictextbox id="error" value="10" css-class="e-error">
</ejs-numerictextbox>

<ejs-numerictextbox id="success" value="10" css-class="e-success">
</ejs-numerictextbox>
```

---

### htmlAttributes

**Type:** `IDictionary<string, object>`  
**Default:** `null`

Gets or sets additional HTML attributes for the input element (like `aria-label`, `data-*` attributes).

```html
<ejs-numerictextbox id="numericTextBox" value="10" 
    aria-label="Enter quantity"
    data-testid="quantity-input">
</ejs-numerictextbox>
```

---

## Events

Events are handled through the `<e-events>` child element:

### change

Fires when the value changes (on blur or when using spinner buttons).

```html
<ejs-numerictextbox id="numericTextBox" value="10">
    <e-events change="onChange"></e-events>
</ejs-numerictextbox>

<script>
function onChange(args) {
    console.log('New value:', args.value);
}
</script>
```

### focus

Fires when the NumericTextBox receives focus.

```html
<ejs-numerictextbox id="numericTextBox" value="10">
    <e-events focus="onFocus"></e-events>
</ejs-numerictextbox>

<script>
function onFocus(args) {
    console.log('NumericTextBox focused');
}
</script>
```

### blur

Fires when the NumericTextBox loses focus.

```html
<ejs-numerictextbox id="numericTextBox" value="10">
    <e-events blur="onBlur"></e-events>
</ejs-numerictextbox>

<script>
function onBlur(args) {
    console.log('NumericTextBox blurred, value:', args.value);
}
</script>
```

---

## Complete Example

```html
<!-- Views/Home/Index.cshtml -->
<div style="padding: 20px; max-width: 600px;">
    <h2>Complete NumericTextBox Example</h2>

    <div style="margin-bottom: 20px;">
        <label for="productPrice">Product Price (USD):</label>
        <ejs-numerictextbox id="productPrice" 
            value="99.99" 
            format="c2" 
            currency="USD"
            min="0" 
            max="10000"
            decimals="2"
            strict-mode="true"
            placeholder="Enter price">
            <e-events change="onPriceChange" blur="onPriceBlur"></e-events>
        </ejs-numerictextbox>
        <p id="priceDisplay"></p>
    </div>

    <div style="margin-bottom: 20px;">
        <label for="quantity">Quantity:</label>
        <ejs-numerictextbox id="quantity" 
            value="1" 
            min="1" 
            max="100"
            step="1"
            strict-mode="true">
            <e-events change="onQuantityChange"></e-events>
        </ejs-numerictextbox>
    </div>

    <button onclick="submitForm()">Submit Order</button>
</div>

@section scripts {
    <ejs-scripts></ejs-scripts>
    <script>
        function onPriceChange(args) {
            console.log('Price changed to:', args.value);
            document.getElementById('priceDisplay').textContent = 'Price: ' + args.value;
        }

        function onPriceBlur(args) {
            console.log('Price finalized:', args.value);
        }

        function onQuantityChange(args) {
            console.log('Quantity changed to:', args.value);
        }

        function submitForm() {
            const price = ej.base.getValue('#productPrice').ej2_instances[0].value;
            const quantity = ej.base.getValue('#quantity').ej2_instances[0].value;
            console.log('Submitting: Price =', price, ', Quantity =', quantity);
        }
    </script>
}
```

---

## Related Topics
- [Getting Started](numeric-textbox-getting-started.md)
- [Formats & Validation](numeric-textbox-formats-and-validation.md)
- [Adornments & Styling](numeric-textbox-adornments-and-styling.md)
