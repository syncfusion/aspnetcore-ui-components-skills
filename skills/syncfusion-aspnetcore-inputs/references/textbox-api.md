# TextBox API Reference for ASP.NET Core

Complete API reference for the TextBox TagHelper in ASP.NET Core using Syncfusion.EJ2.AspNet.Core.

## TagHelper Syntax

```html
<ejs-textbox id="textBox" property="value">
</ejs-textbox>
```

---

## Properties

### value

**Type:** `string`  
**Default:** `null`

Gets or sets the content of the TextBox.

```html
<ejs-textbox id="textBox" value="Hello World">
</ejs-textbox>
```

---

### placeholder

**Type:** `string`  
**Default:** `null`

Gets or sets the placeholder text displayed when the TextBox is empty.

```html
<ejs-textbox id="textBox" placeholder="Enter your name">
</ejs-textbox>
```

---

### type

**Type:** `string`  
**Default:** `"text"`

Gets or sets the HTML input type. Common values: `text`, `email`, `password`, `search`, `url`, `tel`, `date`, `time`, `datetime-local`, `number`.

```html
<!-- Text input (default) -->
<ejs-textbox id="username" type="text" placeholder="Username">
</ejs-textbox>

<!-- Email input -->
<ejs-textbox id="email" type="email" placeholder="Enter email">
</ejs-textbox>

<!-- Password input -->
<ejs-textbox id="password" type="password" placeholder="Enter password">
</ejs-textbox>

<!-- URL input -->
<ejs-textbox id="website" type="url" placeholder="https://example.com">
</ejs-textbox>

<!-- Telephone input -->
<ejs-textbox id="phone" type="tel" placeholder="(123) 456-7890">
</ejs-textbox>

<!-- Date input -->
<ejs-textbox id="birthDate" type="date">
</ejs-textbox>

<!-- Search input -->
<ejs-textbox id="search" type="search" placeholder="Search...">
</ejs-textbox>
```

---

### maxlength

**Type:** `int`  
**Default:** `null`

Specifies the maximum number of characters allowed in the TextBox.

```html
<!-- Maximum 50 characters -->
<ejs-textbox id="textBox" maxlength="50" placeholder="Max 50 chars">
</ejs-textbox>
```

---

### minlength

**Type:** `int`  
**Default:** `null`

Specifies the minimum number of characters required in the TextBox.

```html
<!-- Minimum 5 characters -->
<ejs-textbox id="password" type="password" minlength="5" placeholder="Min 5 chars">
</ejs-textbox>
```

---

### enabled

**Type:** `bool`  
**Default:** `true`

Specifies whether the TextBox is enabled for user interaction.

```html
<!-- Enabled (default) -->
<ejs-textbox id="textBox" enabled="true">
</ejs-textbox>

<!-- Disabled -->
<ejs-textbox id="textBox" enabled="false">
</ejs-textbox>
```

**When enabled="false":**
- Appears grayed out
- No user input allowed
- Cannot receive focus
- Read-only appearance

---

### readonly

**Type:** `bool`  
**Default:** `false`

Specifies whether the TextBox is in read-only mode. Users can select and copy but not edit.

```html
<!-- Read-only -->
<ejs-textbox id="textBox" readonly="true" value="This text cannot be edited">
</ejs-textbox>
```

**When readonly="true":**
- Text can be selected and copied
- No editing allowed
- Can receive focus
- Useful for displaying non-editable content

---

### floatLabelType

**Type:** `string`  
**Default:** `null`

Specifies the floating label behavior. Options: `Auto`, `Always`, `Never`.

```html
<!-- Auto floating label (floats on focus or when filled) -->
<ejs-textbox id="username" placeholder="Username" float-label-type="Auto">
</ejs-textbox>

<!-- Always floating label -->
<ejs-textbox id="email" placeholder="Email" float-label-type="Always">
</ejs-textbox>

<!-- No floating label -->
<ejs-textbox id="search" placeholder="Search" float-label-type="Never">
</ejs-textbox>
```

---

### cssClass

**Type:** `string`  
**Default:** `null`

Gets or sets CSS classes for custom styling. Predefined classes: `e-small`, `e-bigger`, `e-error`, `e-warning`, `e-success`.

```html
<!-- Small TextBox -->
<ejs-textbox id="small" css-class="e-small">
</ejs-textbox>

<!-- Large TextBox -->
<ejs-textbox id="large" css-class="e-bigger">
</ejs-textbox>

<!-- Error state -->
<ejs-textbox id="error" css-class="e-error">
</ejs-textbox>

<!-- Success state -->
<ejs-textbox id="success" css-class="e-success">
</ejs-textbox>

<!-- Warning state -->
<ejs-textbox id="warning" css-class="e-warning">
</ejs-textbox>
```

---

### htmlAttributes

**Type:** `IDictionary<string, object>`  
**Default:** `null`

Gets or sets additional HTML attributes for the input element.

```html
<ejs-textbox id="textBox" 
    aria-label="Search box"
    aria-describedby="search-help"
    data-testid="search-input"
    autofocus="true">
</ejs-textbox>
<small id="search-help">Enter search terms</small>
```

---

## Events

Events are handled through the `<e-events>` child element:

### change

Fires when the TextBox value changes.

```html
<ejs-textbox id="textBox">
    <e-events change="onChange"></e-events>
</ejs-textbox>

<script>
function onChange(args) {
    console.log('TextBox value changed');
    console.log('New value:', args.value);
}
</script>
```

### focus

Fires when the TextBox receives focus.

```html
<ejs-textbox id="textBox">
    <e-events focus="onFocus"></e-events>
</ejs-textbox>

<script>
function onFocus(args) {
    console.log('TextBox focused');
}
</script>
```

### blur

Fires when the TextBox loses focus.

```html
<ejs-textbox id="textBox">
    <e-events blur="onBlur"></e-events>
</ejs-textbox>

<script>
function onBlur(args) {
    console.log('TextBox blurred');
    console.log('Current value:', args.value);
}
</script>
```

### input

Fires when the user types or pastes content.

```html
<ejs-textbox id="textBox">
    <e-events input="onInput"></e-events>
</ejs-textbox>

<script>
function onInput(args) {
    console.log('User input detected');
    console.log('Current value:', args.value);
}
</script>
```

---

## Complete Example

```html
<!-- Views/Home/Index.cshtml -->
<div style="padding: 20px; max-width: 500px;">
    <h2>Contact Form</h2>

    <form id="contactForm">
        <div style="margin-bottom: 20px;">
            <label for="fullName">Full Name <span style="color: red;">*</span>:</label>
            <ejs-textbox id="fullName"
                placeholder="Enter your full name"
                minlength="2"
                maxlength="100"
                float-label-type="Auto"
                required="true">
                <e-events blur="validateName"></e-events>
            </ejs-textbox>
            <small id="nameError" style="color: red;"></small>
        </div>

        <div style="margin-bottom: 20px;">
            <label for="email">Email <span style="color: red;">*</span>:</label>
            <ejs-textbox id="email"
                type="email"
                placeholder="Enter your email"
                float-label-type="Auto"
                required="true">
                <e-events blur="validateEmail"></e-events>
            </ejs-textbox>
            <small id="emailError" style="color: red;"></small>
        </div>

        <div style="margin-bottom: 20px;">
            <label for="phone">Phone Number:</label>
            <ejs-textbox id="phone"
                type="tel"
                placeholder="(123) 456-7890"
                pattern="[0-9\-\(\)]{10,}"
                float-label-type="Auto">
            </ejs-textbox>
        </div>

        <div style="margin-bottom: 20px;">
            <label for="website">Website:</label>
            <ejs-textbox id="website"
                type="url"
                placeholder="https://example.com"
                float-label-type="Auto">
            </ejs-textbox>
        </div>

        <button type="submit" style="padding: 10px 20px;">Submit</button>
    </form>
</div>

@section scripts {
    <ejs-scripts></ejs-scripts>
    <script>
        function validateName(args) {
            const name = (args.value || '').trim();
            const errorDiv = document.getElementById('nameError');
            
            if (name.length === 0) {
                errorDiv.textContent = 'Name is required';
            } else if (name.length < 2) {
                errorDiv.textContent = 'Name must be at least 2 characters';
            } else {
                errorDiv.textContent = '';
            }
        }

        function validateEmail(args) {
            const email = (args.value || '').trim();
            const errorDiv = document.getElementById('emailError');
            const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            
            if (email.length === 0) {
                errorDiv.textContent = 'Email is required';
            } else if (!emailRegex.test(email)) {
                errorDiv.textContent = 'Invalid email format';
            } else {
                errorDiv.textContent = '';
            }
        }

        document.getElementById('contactForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const fullName = document.getElementById('fullName').ej2_instances[0].value;
            const email = document.getElementById('email').ej2_instances[0].value;
            const phone = document.getElementById('phone').ej2_instances[0].value;
            const website = document.getElementById('website').ej2_instances[0].value;
            
            console.log('Form submitted:', { fullName, email, phone, website });
        });
    </script>
}
```

---

## Related Topics
- [Getting Started](textbox-getting-started.md)
- [Events](textbox-events.md)
- [Form Support](textbox-form-support.md)
- [Style and Customization](textbox-style-and-customization.md)
