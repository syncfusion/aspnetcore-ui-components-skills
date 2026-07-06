# OTP Input API Reference — ASP.NET Core

Complete API reference for the Syncfusion OTP Input component in ASP.NET Core Razor views.

## Table of Contents
- [Component](#component)
- [Properties](#properties)
- [Methods](#methods)
- [Events](#events)
- [Examples](#examples)

---

## Component

**TagHelper Import:**
```html
@addTagHelper *, Syncfusion.EJ2
```

**Basic Usage:**
```html
<ejs-otpinput id="otpinput" length="4" type="number"></ejs-otpinput>
```

---

## Properties

### AutoFocus
**Type:** `bool`  
**Default:** `false`

Specifies whether the OTP input field should automatically receive focus when the component is rendered.

```html
<ejs-otpinput id="otpinput" autoFocus="true"></ejs-otpinput>
```

### CssClass
**Type:** `string`  
**Default:** `''`

Defines one or more CSS classes to customize the appearance. Predefined classes:

| Class | Description |
|-------|-------------|
| `e-success` | Success/positive state styling |
| `e-warning` | Warning state styling |
| `e-error` | Error state styling |

```html
<ejs-otpinput id="otpinput" cssClass="e-success"></ejs-otpinput>
```

### Disabled
**Type:** `bool`  
**Default:** `false`

Specifies whether the OTP Input is disabled. When `true`, user input is not allowed.

```html
<ejs-otpinput id="otpinput" disabled="true"></ejs-otpinput>
```

### EnablePersistence
**Type:** `bool`  
**Default:** `false`

Enable or disable persisting the component's state between page reloads.

```html
<ejs-otpinput id="otpinput" enablePersistence="true"></ejs-otpinput>
```

### EnableRtl
**Type:** `bool`  
**Default:** `false`

Enable or disable rendering the component in right-to-left direction.

```html
<ejs-otpinput id="otpinput" enableRtl="true"></ejs-otpinput>
```

### HtmlAttributes
**Type:** `Dictionary<string, object>`

Specifies additional HTML attributes to be applied as key-value pairs.

```html
<ejs-otpinput id="otpinput" 
              htmlAttributes='new Dictionary<string, object> { 
                  { "title", "One-Time Password" },
                  { "data-testid", "otp-input" }
              }'>
</ejs-otpinput>
```

### Length
**Type:** `int`  
**Default:** `4`

Specifies the number of OTP input fields (i.e., the OTP code length).

```html
<!-- 6-digit OTP -->
<ejs-otpinput id="otpinput" length="6"></ejs-otpinput>
```

### Locale
**Type:** `string`  
**Default:** `''` (uses global culture `'en-US'`)

Overrides the global culture and localization value for this component.

```html
<ejs-otpinput id="otpinput" locale="ar-SA"></ejs-otpinput>
```

### Placeholder
**Type:** `string`  
**Default:** `''`

Specifies hint text shown in each input field until the user enters a value.

- Single character: same character shown in all input fields
- Multiple characters: each character maps to a corresponding input field sequentially

```html
<!-- Single character placeholder -->
<ejs-otpinput id="otpinput" placeholder="x"></ejs-otpinput>

<!-- Per-field placeholder -->
<ejs-otpinput id="otpinput" placeholder="0123"></ejs-otpinput>
```

### Separator
**Type:** `string`  
**Default:** `''`

Specifies the separator character between OTP input fields.

```html
<ejs-otpinput id="otpinput" separator="-"></ejs-otpinput>
```

### StylingMode
**Type:** `string`  
**Default:** `'Outlined'`

Specifies the styling mode of the OTP input. Valid values: `Outlined`, `Filled`, `Underlined`.

```html
<ejs-otpinput id="otpinput" stylingMode="Outlined"></ejs-otpinput>
<ejs-otpinput id="otpinput" stylingMode="Filled"></ejs-otpinput>
<ejs-otpinput id="otpinput" stylingMode="Underlined"></ejs-otpinput>
```

### Type
**Type:** `string`  
**Default:** `'number'`

Specifies the type of characters accepted. Valid values: `number`, `text`, `password`.

```html
<ejs-otpinput id="otpinput" type="number"></ejs-otpinput>
<ejs-otpinput id="otpinput" type="text"></ejs-otpinput>
<ejs-otpinput id="otpinput" type="password"></ejs-otpinput>
```

### Value
**Type:** `string | int`  
**Default:** `''`

Gets or sets the OTP value.

```html
<ejs-otpinput id="otpinput" value="1234"></ejs-otpinput>
```

---

## Methods

Methods can be accessed via JavaScript after the component is initialized.

### Clear()

Clears the OTP input value.

```javascript
const otpComponent = document.getElementById('otpinput').ej2_instances[0];
otpComponent.clear();
```

### Destroy()

Destroys the component and removes it from the DOM.

```javascript
const otpComponent = document.getElementById('otpinput').ej2_instances[0];
otpComponent.destroy();
```

### FocusIn()

Sets focus to the first OTP input field.

```javascript
const otpComponent = document.getElementById('otpinput').ej2_instances[0];
otpComponent.focusIn();
```

### FocusOut()

Removes focus from the OTP input.

```javascript
const otpComponent = document.getElementById('otpinput').ej2_instances[0];
otpComponent.focusOut();
```

### GetPersistData()

Retrieves the properties needed for persistence state.

```javascript
const otpComponent = document.getElementById('otpinput').ej2_instances[0];
const persistData = otpComponent.getPersistData();
console.log(persistData);
```

### GetValue()

Returns the current OTP value.

```javascript
const otpComponent = document.getElementById('otpinput').ej2_instances[0];
const otpValue = otpComponent.getValue();
console.log(otpValue);
```

---

## Events

### Created

Fires once after the OTP Input finishes rendering.

```html
<ejs-otpinput id="otpinput" 
              created="onOtpCreated">
</ejs-otpinput>

<script>
    function onOtpCreated() {
        console.log('OTP Input is ready');
    }
</script>
```

### Focus

Fires when the OTP input group receives focus.

```html
<ejs-otpinput id="otpinput" 
              focus="onOtpFocus">
</ejs-otpinput>

<script>
    function onOtpFocus(args) {
        console.log('OTP Input focused');
    }
</script>
```

### Blur

Fires when the OTP input loses focus.

```html
<ejs-otpinput id="otpinput" 
              blur="onOtpBlur">
</ejs-otpinput>

<script>
    function onOtpBlur(args) {
        console.log('OTP Input blurred');
    }
</script>
```

### Input

Fires each time the value of any individual OTP field changes.

Use `Input` for real-time feedback as the user types.

```html
<ejs-otpinput id="otpinput" 
              input="onOtpInput">
</ejs-otpinput>

<script>
    function onOtpInput(args) {
        console.log('Current OTP value:', args.value);
    }
</script>
```

### ValueChanged

Fires when the OTP is complete (all fields filled) and the input loses focus.

This is the primary event to use for OTP verification/submission logic.

```html
<ejs-otpinput id="otpinput" 
              valueChanged="onOtpValueChanged">
</ejs-otpinput>

<script>
    function onOtpValueChanged(args) {
        console.log('Complete OTP value:', args.value);
        // Submit verification
    }
</script>
```

---

## Examples

### Basic Implementation

**Razor View (CSHTML):**
```html
@{
    ViewBag.Title = "OTP Input";
}

<div class="container mt-5">
    <div class="row justify-content-center">
        <div class="col-md-6">
            <h2>Verify Your Account</h2>

            <form method="post" id="otp-form">
                <div class="form-group mb-3">
                    <label for="otp" class="form-label">Enter Verification Code</label>
                    <ejs-otpinput id="otp" 
                                  length="6"
                                  type="number"
                                  placeholder="0"
                                  autoFocus="true"
                                  input="onOtpInput"
                                  valueChanged="onOtpValueChanged">
                    </ejs-otpinput>
                    <small class="form-text text-muted">
                        Enter the 6-digit code sent to your email
                    </small>
                </div>

                <button type="submit" class="btn btn-primary w-100" id="verify-btn" disabled>
                    Verify Code
                </button>
            </form>
        </div>
    </div>
</div>

<script>
    const otpComponent = document.getElementById('otp').ej2_instances[0];
    const verifyBtn = document.getElementById('verify-btn');

    function onOtpInput(args) {
        console.log('Current value:', args.value);
    }

    function onOtpValueChanged(args) {
        console.log('OTP Complete:', args.value);
        verifyBtn.disabled = false;
    }

    document.getElementById('otp-form').addEventListener('submit', (e) => {
        e.preventDefault();
        const otpValue = otpComponent.getValue();
        console.log('Submitting OTP:', otpValue);
        // Send to server for verification
    });
</script>
```

### Advanced Example with Validation

**Razor View (CSHTML):**
```html
<div class="container mt-5">
    <h2>Advanced OTP Verification</h2>

    <form asp-action="VerifyOtp" method="post" id="advanced-form">
        <div asp-validation-summary="All" class="alert alert-danger"></div>

        <div class="form-group mb-3">
            <label for="otp-advanced">Verification Code</label>
            <ejs-otpinput id="otp-advanced" 
                          length="6"
                          type="number"
                          valueChanged="onOtpComplete"
                          cssClass="e-outline">
            </ejs-otpinput>
        </div>

        <div id="status-message" role="alert" aria-live="polite"></div>

        <button type="submit" class="btn btn-primary" id="submit-btn" disabled>
            Verify
        </button>

        <button type="button" class="btn btn-link" onclick="resendOtp()">
            Resend Code
        </button>
    </form>
</div>

<script>
    const otpComponent = document.getElementById('otp-advanced').ej2_instances[0];
    const statusMessage = document.getElementById('status-message');
    const submitBtn = document.getElementById('submit-btn');

    function onOtpComplete(args) {
        if (args.value && args.value.length === 6) {
            statusMessage.textContent = 'OTP received. Ready to verify.';
            statusMessage.className = 'alert alert-success';
            submitBtn.disabled = false;
        }
    }

    function resendOtp() {
        otpComponent.clear();
        statusMessage.textContent = 'Sending OTP to your phone...';
        statusMessage.className = 'alert alert-info';
        // Call server endpoint to resend OTP
    }

    document.getElementById('advanced-form').addEventListener('submit', async (e) => {
        e.preventDefault();
        const otpValue = otpComponent.getValue();
        
        const response = await fetch('/api/verify-otp', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ otp: otpValue })
        });

        const result = await response.json();
        if (result.success) {
            statusMessage.textContent = 'Verification successful!';
            statusMessage.className = 'alert alert-success';
        } else {
            statusMessage.textContent = 'Invalid OTP. Please try again.';
            statusMessage.className = 'alert alert-danger';
            otpComponent.clear();
        }
    });
</script>
```

---

## See Also

- `otp-input-getting-started.md` — Quick start guide
- `otp-input-configuration.md` — Configuration options
- `otp-input-accessibility.md` — Accessibility features
- `otp-input-events.md` — Event handling patterns
