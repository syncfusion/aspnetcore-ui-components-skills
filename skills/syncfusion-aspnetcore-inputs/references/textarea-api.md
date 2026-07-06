# TextArea API Reference for ASP.NET Core

Complete API reference for the TextArea TagHelper in ASP.NET Core using Syncfusion.EJ2.AspNet.Core.

## TagHelper Syntax

```html
<ejs-textarea id="textarea" property="value">
</ejs-textarea>
```

---

## Properties

### value

**Type:** `string`  
**Default:** `null`

Gets or sets the content of the TextArea.

```html
<ejs-textarea id="textarea" value="This is some text">
</ejs-textarea>
```

---

### placeholder

**Type:** `string`  
**Default:** `null`

Gets or sets the placeholder text displayed when the TextArea is empty.

```html
<ejs-textarea id="textarea" placeholder="Enter your comments">
</ejs-textarea>
```

---

### rows

**Type:** `int`  
**Default:** `null`

Gets or sets the visible height of the TextArea, measured in lines.

```html
<!-- 5 rows visible -->
<ejs-textarea id="textarea" rows="5">
</ejs-textarea>

<!-- 10 rows visible -->
<ejs-textarea id="textarea" rows="10">
</ejs-textarea>
```

---

### cols

**Type:** `int`  
**Default:** `null`

Gets or sets the visible width of the TextArea, measured in average character widths.

```html
<!-- 50 columns wide -->
<ejs-textarea id="textarea" cols="50">
</ejs-textarea>

<!-- 80 columns wide -->
<ejs-textarea id="textarea" cols="80">
</ejs-textarea>
```

---

### maxlength

**Type:** `int`  
**Default:** `null`

Specifies the maximum number of characters allowed in the TextArea.

```html
<!-- Maximum 500 characters -->
<ejs-textarea id="textarea" maxlength="500">
</ejs-textarea>

<!-- Maximum 1000 characters -->
<ejs-textarea id="textarea" maxlength="1000">
</ejs-textarea>
```

**Behavior:**
- Input is prevented when limit is reached
- Paste content is truncated to fit
- Existing content can be edited freely
- Use with a character counter for better UX

---

### enabled

**Type:** `bool`  
**Default:** `true`

Specifies whether the TextArea is enabled for user interaction.

```html
<!-- Enabled (default) -->
<ejs-textarea id="textarea" enabled="true">
</ejs-textarea>

<!-- Disabled -->
<ejs-textarea id="textarea" enabled="false">
</ejs-textarea>
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

Specifies whether the TextArea is in read-only mode. Users can select and copy but not edit.

```html
<!-- Read-only -->
<ejs-textarea id="textarea" readonly="true" value="This text cannot be edited">
</ejs-textarea>
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
<ejs-textarea id="textarea" placeholder="Comments" float-label-type="Auto">
</ejs-textarea>

<!-- Always floating label -->
<ejs-textarea id="textarea" placeholder="Comments" float-label-type="Always">
</ejs-textarea>

<!-- No floating label -->
<ejs-textarea id="textarea" placeholder="Comments" float-label-type="Never">
</ejs-textarea>
```

---

### cssClass

**Type:** `string`  
**Default:** `null`

Gets or sets CSS classes for custom styling. Predefined classes: `e-small`, `e-bigger`, `e-error`, `e-warning`, `e-success`.

```html
<!-- Small TextArea -->
<ejs-textarea id="small" css-class="e-small">
</ejs-textarea>

<!-- Large TextArea -->
<ejs-textarea id="large" css-class="e-bigger">
</ejs-textarea>

<!-- Error state -->
<ejs-textarea id="error" css-class="e-error">
</ejs-textarea>

<!-- Success state -->
<ejs-textarea id="success" css-class="e-success">
</ejs-textarea>
```

---

### htmlAttributes

**Type:** `IDictionary<string, object>`  
**Default:** `null`

Gets or sets additional HTML attributes for the textarea element.

```html
<ejs-textarea id="textarea" 
    aria-label="User feedback"
    aria-describedby="feedback-help"
    data-testid="feedback-textarea">
</ejs-textarea>
<small id="feedback-help">Please provide detailed feedback</small>
```

---

## Events

Events are handled through the `<e-events>` child element:

### change

Fires when the TextArea content changes.

```html
<ejs-textarea id="textarea">
    <e-events change="onChange"></e-events>
</ejs-textarea>

<script>
function onChange(args) {
    console.log('TextArea value changed');
    console.log('New value:', args.value);
}
</script>
```

### focus

Fires when the TextArea receives focus.

```html
<ejs-textarea id="textarea">
    <e-events focus="onFocus"></e-events>
</ejs-textarea>

<script>
function onFocus(args) {
    console.log('TextArea focused');
}
</script>
```

### blur

Fires when the TextArea loses focus.

```html
<ejs-textarea id="textarea">
    <e-events blur="onBlur"></e-events>
</ejs-textarea>

<script>
function onBlur(args) {
    console.log('TextArea blurred');
    console.log('Current value:', args.value);
}
</script>
```

### input

Fires when the user types or pastes content.

```html
<ejs-textarea id="textarea">
    <e-events input="onInput"></e-events>
</ejs-textarea>

<script>
function onInput(args) {
    console.log('User input detected');
    console.log('Current length:', args.value.length);
}
</script>
```

---

## Complete Example

```html
<!-- Views/Home/Index.cshtml -->
<div style="padding: 20px; max-width: 700px;">
    <h2>Customer Feedback Form</h2>

    <form id="feedbackForm">
        <div style="margin-bottom: 20px;">
            <label for="name">Name:</label>
            <input type="text" id="name" style="width: 100%; padding: 8px;" />
        </div>

        <div style="margin-bottom: 20px;">
            <label for="email">Email:</label>
            <input type="email" id="email" style="width: 100%; padding: 8px;" />
        </div>

        <div style="margin-bottom: 20px;">
            <label for="feedback">Your Feedback (max 500 characters):</label>
            <ejs-textarea id="feedback"
                placeholder="Please share your thoughts..."
                rows="6"
                maxlength="500"
                float-label-type="Auto">
                <e-events change="onFeedbackChange" blur="onFeedbackBlur"></e-events>
            </ejs-textarea>
            <small id="charCount" style="color: #666;">0 / 500 characters</small>
        </div>

        <button type="submit" style="padding: 10px 20px;">Submit Feedback</button>
    </form>
</div>

@section scripts {
    <ejs-scripts></ejs-scripts>
    <script>
        function onFeedbackChange(args) {
            const charCount = (args.value || '').length;
            document.getElementById('charCount').textContent = charCount + ' / 500 characters';
        }

        function onFeedbackBlur(args) {
            console.log('Feedback submitted:', args.value);
        }

        document.getElementById('feedbackForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const name = document.getElementById('name').value;
            const email = document.getElementById('email').value;
            const feedback = document.getElementById('feedback').ej2_instances[0].value;
            
            console.log('Form submitted:', { name, email, feedback });
        });
    </script>
}
```

---

## Related Topics
- [Getting Started](textarea-getting-started.md)
- [Events](textarea-events.md)
- [Max Length](textarea-max-length.md)
- [Floating Label](textarea-floating-label.md)
- [Form Support](textarea-form-support.md)
