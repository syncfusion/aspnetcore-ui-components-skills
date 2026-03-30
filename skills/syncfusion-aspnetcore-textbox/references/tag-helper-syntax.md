# Tag Helper Syntax for ASP.NET Core TextBox

## Table of Contents
- [Basic Syntax](#basic-syntax)
- [Common Properties](#common-properties)
- [Data Binding](#data-binding)
- [Event Binding](#event-binding)
- [Tag Helper vs Programmatic](#tag-helper-vs-programmatic)
- [Best Practices](#best-practices)
- [Common Patterns](#common-patterns)

## Basic Syntax

### Minimal TextBox

```cshtml
<ejs-textbox id="basicTextBox"></ejs-textbox>
```

**Result:** Empty input field with default styling

### TextBox with Content

```cshtml
<ejs-textbox id="email" 
              placeholder="Enter email" 
              value="user@example.com">
</ejs-textbox>
```

**Attributes:**
- `id` (required): Unique identifier for JavaScript access
- `placeholder`: Hint text displayed when empty
- `value`: Initial value

### TextBox with Type

```cshtml
<ejs-textbox id="password" 
              type="password" 
              placeholder="Enter password">
</ejs-textbox>

<ejs-textbox id="email" 
              type="email" 
              placeholder="Enter email">
</ejs-textbox>

<ejs-textbox id="phone" 
              type="tel" 
              placeholder="Enter phone number">
</ejs-textbox>
```

**Supported Types:**
- `text` - Default text input
- `email` - Email validation
- `password` - Hidden characters
- `tel` - Phone number
- `number` - Numeric input
- `url` - URL input
- `search` - Search input

## Common Properties

### Text and Display Properties

```cshtml
<!-- Placeholder -->
<ejs-textbox placeholder="Enter your name"></ejs-textbox>

<!-- Initial Value -->
<ejs-textbox value="Default text"></ejs-textbox>

<!-- Read-only (can focus, cannot edit) -->
<ejs-textbox readonly="true" value="Cannot change"></ejs-textbox>

<!-- Disabled (cannot focus or edit) -->
<ejs-textbox disabled="true" value="Disabled"></ejs-textbox>

<!-- Clear Button -->
<ejs-textbox placeholder="Type something..." showClearButton="true"></ejs-textbox>
```

### Label Properties

```cshtml
<!-- Floating Label - Auto (floats on focus or value) -->
<ejs-textbox placeholder="Name" 
              floatLabelType="Auto">
</ejs-textbox>

<!-- Floating Label - Always (always floated) -->
<ejs-textbox placeholder="Email" 
              floatLabelType="Always">
</ejs-textbox>

<!-- Floating Label - Never (no floating) -->
<ejs-textbox placeholder="Phone" 
              floatLabelType="Never">
</ejs-textbox>

<!-- Multi-line (Textarea) -->
<ejs-textbox placeholder="Comments" 
              multiline="true"
              rows="5">
</ejs-textbox>
```

### Styling Properties

```cshtml
<!-- CSS Class for Styling -->
<ejs-textbox cssClass="e-success"></ejs-textbox>
<ejs-textbox cssClass="e-error"></ejs-textbox>
<ejs-textbox cssClass="e-warning"></ejs-textbox>

<!-- Custom CSS Classes -->
<ejs-textbox cssClass="custom-textbox custom-large"></ejs-textbox>

<!-- Width and Height -->
<ejs-textbox placeholder="Full width" 
              width="100%">
</ejs-textbox>

<!-- HTML Attributes -->
<ejs-textbox placeholder="User input" 
              aria-label="Username"
              aria-describedby="userHelp"
              autocomplete="off">
</ejs-textbox>
```

### Input Constraints

```cshtml
<!-- Maximum Length -->
<ejs-textbox placeholder="Max 50 chars" 
              maxLength="50">
</ejs-textbox>

<!-- Minimum Length (client-side hint) -->
<ejs-textbox placeholder="At least 8 chars" 
              minLength="8">
</ejs-textbox>

<!-- Pattern (client-side validation) -->
<ejs-textbox placeholder="Phone: XXX-XXX-XXXX" 
              pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}">
</ejs-textbox>

<!-- Required (HTML5) -->
<ejs-textbox placeholder="Required field" 
              required="true">
</ejs-textbox>
```

### Adornments

```cshtml
<!-- Prepend Template (element before input) -->
<ejs-textbox placeholder="Search" 
              prependTemplate="<span class='icon'>🔍</span>">
</ejs-textbox>

<!-- Append Template (element after input) -->
<ejs-textbox placeholder="Amount" 
              appendTemplate="<span class='currency'>$</span>">
</ejs-textbox>

<!-- Both Prepend and Append -->
<ejs-textbox placeholder="Price" 
              prependTemplate="<span class='label'>$</span>"
              appendTemplate="<span class='cents'>.00</span>">
</ejs-textbox>
```

## Data Binding

### Two-Way Binding with Model Property

**Model:**
```csharp
public class ContactForm
{
    public string Email { get; set; }
    public string Name { get; set; }
    public string Message { get; set; }
}
```

**View:**
```cshtml
@model ContactForm

<form asp-action="Submit">
    <!-- Bind to Model.Email -->
    <ejs-textbox asp-for="Email" 
                  placeholder="Enter email"
                  floatLabelType="Auto">
    </ejs-textbox>
    
    <!-- Bind to Model.Name -->
    <ejs-textbox asp-for="Name" 
                  placeholder="Enter name"
                  floatLabelType="Auto">
    </ejs-textbox>
    
    <!-- Bind to Model.Message -->
    <ejs-textbox asp-for="Message" 
                  placeholder="Enter message"
                  multiline="true"
                  floatLabelType="Auto">
    </ejs-textbox>
    
    <button type="submit" class="btn btn-primary">Submit</button>
</form>
```

**Result:** Form automatically populates from Model and posts back to server

### Binding to ViewBag

```cshtml
@{ 
    ViewBag.UserEmail = "user@example.com";
    ViewBag.UserPhone = "555-123-4567";
}

<!-- Bind to ViewBag -->
<ejs-textbox id="email" 
              value="@ViewBag.UserEmail"
              placeholder="Email">
</ejs-textbox>

<ejs-textbox id="phone" 
              value="@ViewBag.UserPhone"
              placeholder="Phone">
</ejs-textbox>
```

### Binding from List (Collection)

```cshtml
@{
    var settings = new Dictionary<string, object>
    {
        { "id", "apiKey" },
        { "value", "sk_live_abc123" },
        { "readonly", true },
        { "placeholder", "API Key" }
    };
}

<!-- Using Dictionary -->
<ejs-textbox id="@(string)settings["id"]"
              value="@(string)settings["value"]"
              readonly="@(bool)settings["readonly"]"
              placeholder="@(string)settings["placeholder"]">
</ejs-textbox>
```

## Event Binding

### Using created Event

```cshtml
<ejs-textbox id="myTextBox" 
              placeholder="Enter name"
              created="onCreated">
</ejs-textbox>

<script>
    function onCreated(args) {
        console.log("TextBox created:", args);
        // Perform initialization logic
        args.element.addEventListener('change', function(e) {
            console.log("Value changed to:", e.target.value);
        });
    }
</script>
```

### Input Event (on each keystroke)

```cshtml
<ejs-textbox id="myTextBox" 
              placeholder="Type to search...">
</ejs-textbox>

<script>
    document.getElementById('myTextBox').addEventListener('input', function(e) {
        const searchTerm = e.target.value;
        console.log("Search term:", searchTerm);
        // Perform search or validation
    });
</script>
```

### Change Event (on value change)

```cshtml
<ejs-textbox id="myTextBox" 
              placeholder="Enter email">
</ejs-textbox>

<script>
    document.getElementById('myTextBox').addEventListener('change', function(e) {
        const value = e.target.value;
        console.log("Email changed to:", value);
        
        // Validate email format
        const isValid = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value);
        if (!isValid) {
            this.classList.add('e-error');
        } else {
            this.classList.remove('e-error');
        }
    });
</script>
```

### Focus and Blur Events

```cshtml
<ejs-textbox id="myTextBox" 
              placeholder="Click to focus">
</ejs-textbox>

<script>
    const textbox = document.getElementById('myTextBox');
    
    textbox.addEventListener('focus', function(e) {
        console.log("TextBox focused");
        this.classList.add('e-focused');
    });
    
    textbox.addEventListener('blur', function(e) {
        console.log("TextBox blurred");
        this.classList.remove('e-focused');
    });
</script>
```

## Tag Helper vs Programmatic

### Tag Helper Approach (Recommended)

**Advantages:**
- Cleaner, more readable markup
- Built-in ASP.NET Core integration
- Type-safe with `asp-for` binding
- Better IntelliSense support

```cshtml
@model ContactForm

<ejs-textbox asp-for="Email" 
              placeholder="Enter email"
              floatLabelType="Auto">
</ejs-textbox>
```

### Programmatic Approach (HTML Helper)

**Advantages:**
- More control over rendering
- Useful when properties are dynamic
- Can be used in loops or conditionals

```cshtml
@{
    var textBoxConfig = new Dictionary<string, object>
    {
        { "id", "dynamicTextBox" },
        { "placeholder", "Enter text" },
        { "floatLabelType", "Auto" },
        { "cssClass", "e-success" }
    };
}

@Html.EJS().TextBox("emailTextBox")
    .Placeholder("Enter email")
    .FloatLabelType(FloatLabelType.Auto)
    .CssClass("e-success")
    .Render()
```

### Pure HTML with JavaScript

**Advantages:**
- No dependency on Syncfusion tag helpers
- Can be used in any HTML template

```html
<!-- HTML -->
<input id="myTextBox" type="text" placeholder="Enter email">

<script>
    // Initialize with JavaScript
    new ej.inputs.TextBox({
        id: 'myTextBox',
        placeholder: 'Enter email',
        floatLabelType: 'Auto',
        cssClass: 'e-success'
    });
</script>
```

## Best Practices

### 1. Always Use Meaningful IDs

```cshtml
<!-- ✓ Good -->
<ejs-textbox id="emailInput"></ejs-textbox>
<ejs-textbox id="phoneInput"></ejs-textbox>

<!-- ✗ Poor -->
<ejs-textbox id="tb1"></ejs-textbox>
<ejs-textbox id="tb2"></ejs-textbox>
```

### 2. Use Floating Labels for Better UX

```cshtml
<!-- ✓ Good -->
<ejs-textbox placeholder="Email" 
              floatLabelType="Auto">
</ejs-textbox>

<!-- ✗ Less ideal -->
<ejs-textbox placeholder="Email"></ejs-textbox>
```

### 3. Provide Type Hints

```cshtml
<!-- ✓ Good -->
<ejs-textbox id="email" type="email" placeholder="Enter email"></ejs-textbox>
<ejs-textbox id="phone" type="tel" placeholder="Enter phone"></ejs-textbox>

<!-- ✗ Poor -->
<ejs-textbox id="email" placeholder="Enter email"></ejs-textbox>
```

### 4. Use asp-for for Model Binding

```cshtml
@model UserForm

<!-- ✓ Good - automatic validation attributes, error handling -->
<ejs-textbox asp-for="Email"></ejs-textbox>

<!-- ✗ Less ideal - manual binding -->
<ejs-textbox id="email" value="@Model.Email"></ejs-textbox>
```

### 5. Consistent Styling Approach

```cshtml
<!-- ✓ Good - use CSS classes -->
<ejs-textbox cssClass="form-control e-success"></ejs-textbox>

<!-- ✗ Less ideal - inline styles -->
<ejs-textbox style="border: 1px solid green; padding: 8px;"></ejs-textbox>
```

### 6. Handle Accessibility

```cshtml
<!-- ✓ Good -->
<label for="email">Email Address:</label>
<ejs-textbox id="email" 
              type="email"
              aria-label="Email Address"
              aria-describedby="emailHelp">
</ejs-textbox>
<small id="emailHelp">We'll never share your email.</small>

<!-- ✗ Poor -->
<ejs-textbox placeholder="Email" id="email"></ejs-textbox>
```

## Common Patterns

### Pattern 1: Contact Form

```cshtml
@model ContactModel

<form asp-action="SendContact">
    <div class="form-group">
        <ejs-textbox asp-for="Name" 
                      placeholder="Full Name"
                      floatLabelType="Auto"
                      required="true">
        </ejs-textbox>
        <span asp-validation-for="Name"></span>
    </div>

    <div class="form-group">
        <ejs-textbox asp-for="Email" 
                      type="email"
                      placeholder="Email Address"
                      floatLabelType="Auto"
                      required="true">
        </ejs-textbox>
        <span asp-validation-for="Email"></span>
    </div>

    <div class="form-group">
        <ejs-textbox asp-for="Message" 
                      placeholder="Message"
                      floatLabelType="Auto"
                      multiline="true"
                      required="true">
        </ejs-textbox>
        <span asp-validation-for="Message"></span>
    </div>

    <button type="submit" class="btn btn-primary">Send</button>
</form>
```

### Pattern 2: Search Bar with Icon

```cshtml
<div class="search-container">
    <ejs-textbox id="searchBox"
                  placeholder="Search products..."
                  prependTemplate="<span class='e-input-icon-prepend'>🔍</span>"
                  floatLabelType="Never">
    </ejs-textbox>
</div>

<style>
    .search-container {
        max-width: 500px;
        margin: 2rem 0;
    }
    
    .e-input-icon-prepend {
        margin-right: 8px;
        display: inline-flex;
        align-items: center;
    }
</style>

<script>
    document.getElementById('searchBox').addEventListener('input', function(e) {
        const query = e.target.value;
        // Perform search
        console.log("Searching for:", query);
    });
</script>
```

### Pattern 3: Currency Input

```cshtml
<div class="currency-input-group">
    <label>Price</label>
    <ejs-textbox id="priceInput"
                  type="number"
                  placeholder="0.00"
                  prependTemplate="<span class='currency-symbol'>$</span>"
                  floatLabelType="Always"
                  step="0.01"
                  min="0">
    </ejs-textbox>
</div>

<style>
    .currency-input-group {
        width: 300px;
    }
    
    .currency-symbol {
        padding: 0 8px;
        color: #666;
        font-weight: bold;
    }
</style>
```

### Pattern 4: Password with Toggle

```cshtml
<div class="password-input-group">
    <ejs-textbox id="passwordInput"
                  type="password"
                  placeholder="Enter password"
                  floatLabelType="Auto"
                  appendTemplate="<button type='button' class='toggle-btn' onclick='togglePassword()'>👁️</button>">
    </ejs-textbox>
</div>

<style>
    .toggle-btn {
        border: none;
        background: transparent;
        cursor: pointer;
        padding: 0 8px;
        color: #666;
    }
    
    .toggle-btn:hover {
        color: #333;
    }
</style>

<script>
    function togglePassword() {
        const input = document.getElementById('passwordInput');
        const btn = document.querySelector('.toggle-btn');
        
        if (input.type === 'password') {
            input.type = 'text';
            btn.textContent = '🙈';
        } else {
            input.type = 'password';
            btn.textContent = '👁️';
        }
    }
</script>
```

---

**Related References:**
- [validation-states.md](validation-states.md) - Form validation patterns
- [adornments-and-icons.md](adornments-and-icons.md) - Advanced template usage
- [advanced-patterns.md](advanced-patterns.md) - Data binding and events
