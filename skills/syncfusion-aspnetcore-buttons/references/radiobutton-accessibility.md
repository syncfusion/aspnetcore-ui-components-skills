# Accessibility – ASP.NET Core RadioButton

## Semantic HTML

Always use proper `<input type="radio">` elements with labels:

**✓ CORRECT:**
```cshtml
<fieldset>
    <legend>Choose Option</legend>
    <input type="radio" id="opt1" name="choice" value="1" />
    <label for="opt1">Option 1</label>
    
    <input type="radio" id="opt2" name="choice" value="2" />
    <label for="opt2">Option 2</label>
</fieldset>
```

**✗ INCORRECT:**
```cshtml
<div onclick="selectOption(1)">Option 1</div>
<div onclick="selectOption(2)">Option 2</div>
```

---

## Fieldset and Legend

Group radio buttons with `<fieldset>` and provide context with `<legend>`:

```cshtml
<fieldset>
    <legend>Shipping Method</legend>
    
    <div>
        <input type="radio" id="standard" name="shipping" value="standard" />
        <label for="standard">Standard (5-7 business days)</label>
    </div>
    
    <div>
        <input type="radio" id="express" name="shipping" value="express" />
        <label for="express">Express (2-3 business days)</label>
    </div>
    
    <div>
        <input type="radio" id="overnight" name="shipping" value="overnight" />
        <label for="overnight">Overnight</label>
    </div>
</fieldset>
```

---

## WAI-ARIA Attributes

### aria-label

```cshtml
<div role="group" aria-label="Sort Direction">
    <input type="radio" id="asc" name="sort" value="asc" />
    <label for="asc">Ascending</label>
    
    <input type="radio" id="desc" name="sort" value="desc" />
    <label for="desc">Descending</label>
</div>
```

### aria-disabled

```cshtml
<input 
    type="radio" 
    id="old" 
    name="option" 
    value="old" 
    disabled 
    aria-disabled="true" />
<label for="old">Disabled Option</label>
```

---

## Keyboard Navigation

Radio button groups support:
- **Tab** — Navigate to next focused element
- **Arrow Keys** — Cycle through radio buttons (native browser behavior)
- **Space** — Toggle selected radio button

---

## Focus Management

```cshtml
<div role="group" aria-label="View Options">
    <input 
        type="radio" 
        id="list" 
        name="view" 
        value="list" 
        checked 
        tabindex="0" />
    <label for="list">List View</label>
    
    <input type="radio" id="grid" name="view" value="grid" tabindex="-1" />
    <label for="grid">Grid View</label>
    
    <input type="radio" id="table" name="view" value="table" tabindex="-1" />
    <label for="table">Table View</label>
</div>

<style>
    input[type="radio"]:focus {
        outline: 2px solid #0066cc;
        outline-offset: 2px;
    }
</style>
```

---

## Complete Accessible Example

```cshtml
<fieldset>
    <legend>Payment Method</legend>
    
    <div class="radio-group">
        <input type="radio" id="credit" name="payment" value="credit" checked />
        <label for="credit">
            <span class="radio-label">Credit Card</span>
            <span class="radio-description">Visa, Mastercard, or Amex</span>
        </label>
    </div>
    
    <div class="radio-group">
        <input type="radio" id="paypal" name="payment" value="paypal" />
        <label for="paypal">
            <span class="radio-label">PayPal</span>
            <span class="radio-description">Pay securely with your PayPal account</span>
        </label>
    </div>
    
    <div class="radio-group">
        <input type="radio" id="bank" name="payment" value="bank" />
        <label for="bank">
            <span class="radio-label">Bank Transfer</span>
            <span class="radio-description">Direct bank account transfer</span>
        </label>
    </div>
</fieldset>

<style>
    fieldset {
        border: 1px solid #ddd;
        border-radius: 4px;
        padding: 16px;
        margin: 16px 0;
    }
    
    legend {
        padding: 0 8px;
        font-weight: bold;
    }
    
    .radio-group {
        margin: 12px 0;
    }
    
    .radio-label {
        font-weight: 500;
    }
    
    .radio-description {
        display: block;
        font-size: 12px;
        color: #666;
    }
    
    input[type="radio"]:focus {
        outline: 2px solid #0066cc;
        outline-offset: 2px;
    }
</style>
```

---

## See Also

- [RadioButton Getting Started](radiobutton-getting-started.md)
- [RadioButton Features and State](radiobutton-features-and-state.md)
- [RadioButton Label and Size](radiobutton-label-and-size.md)
