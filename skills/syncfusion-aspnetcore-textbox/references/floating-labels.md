# Floating Labels for ASP.NET Core TextBox

## Table of Contents
- [Floating Label Types](#floating-label-types)
- [FloatLabelType Property](#floatlabeltype-property)
- [Styling Floating Labels](#styling-floating-labels)
- [Accessibility with Floating Labels](#accessibility-with-floating-labels)
- [Animation Patterns](#animation-patterns)
- [Edge Cases](#edge-cases)
- [Best Practices](#best-practices)

## Floating Label Types

TextBox supports three floating label behaviors:

| Type | Behavior | Use Case |
|------|----------|----------|
| **Auto** | Float on focus or when value exists | Default for most forms |
| **Always** | Always floated above input | When space is constrained |
| **Never** | Never float (traditional placeholder) | Specific UI designs |

## FloatLabelType Property

### Auto (Default)

Label floats when input is focused or has a value:

```cshtml
<!-- Label floats on focus or value -->
<ejs-textbox id="nameField" 
              placeholder="Enter your name"
              floatLabelType="Auto"
              value="">
</ejs-textbox>

<!-- When user focuses or types, placeholder moves to float above -->
```

**Behavior:**
1. Initially: Placeholder text inside input
2. On focus: Placeholder floats above, input is empty
3. On blur without value: Placeholder returns to input

**Result:** Clean, space-efficient form

### Always

Label always floats above input, regardless of state:

```cshtml
<!-- Label always appears above input -->
<ejs-textbox id="emailField" 
              placeholder="Email address"
              floatLabelType="Always"
              value="">
</ejs-textbox>

<!-- Placeholder always appears as floating label above input -->
```

**Behavior:**
- Placeholder always floats above input
- Input remains empty until user types
- Useful for dense forms

**Result:** Clear, explicit labels always visible

### Never

Traditional behavior - placeholder inside input:

```cshtml
<!-- Placeholder stays in input (no floating) -->
<ejs-textbox id="searchField" 
              placeholder="Search..."
              floatLabelType="Never">
</ejs-textbox>

<!-- Placeholder only visible when input is empty -->
```

**Behavior:**
- Placeholder inside input field
- Placeholder disappears when typing
- No label element required

**Result:** Minimal, compact design

## Styling Floating Labels

### Default Floating Label Styles

```cshtml
<!-- Basic floating label -->
<ejs-textbox id="email" 
              type="email"
              placeholder="Email Address"
              floatLabelType="Auto">
</ejs-textbox>

<!-- CSS that Syncfusion applies automatically: -->
<style>
    /* Floating label container */
    .e-input-group.e-float-label {
        position: relative;
    }

    /* Placeholder styled as floating label */
    .e-input-group.e-float-label > input {
        padding-top: 12px;
    }

    /* Floating label appearance */
    .e-input-group.e-float-label .e-float-label {
        position: absolute;
        top: 4px;
        font-size: 12px;
        color: #999;
        pointer-events: none;
        transition: all 0.3s ease;
    }

    /* Focused state */
    .e-input-group.e-float-label input:focus ~ .e-float-label {
        color: #007bff;
    }

    /* Filled state */
    .e-input-group.e-float-label input:not(:placeholder-shown) ~ .e-float-label {
        top: 4px;
        font-size: 12px;
    }
</style>
```

### Custom Floating Label Styles

```cshtml
<!-- Styled floating labels -->
<ejs-textbox id="customEmail" 
              type="email"
              placeholder="Email"
              floatLabelType="Auto"
              cssClass="custom-float-label">
</ejs-textbox>

<style>
    .custom-float-label {
        /* Custom styling for floating labels */
    }

    .custom-float-label.e-input-group.e-float-label .e-float-label {
        font-weight: 600;
        color: #495057;
        font-size: 13px;
    }

    .custom-float-label.e-input-group.e-float-label input:focus ~ .e-float-label {
        color: #6c63ff;
        font-weight: 700;
    }

    .custom-float-label.e-input-group.e-float-label input {
        border-bottom: 2px solid #e9ecef;
        transition: border-color 0.3s ease;
    }

    .custom-float-label.e-input-group.e-float-label input:focus {
        border-bottom-color: #6c63ff;
    }
</style>
```

### Color-Based Floating Labels

```cshtml
<div class="floating-label-examples">
    <!-- Primary color -->
    <ejs-textbox id="primaryLabel" 
                  placeholder="Primary Label"
                  floatLabelType="Auto"
                  cssClass="label-primary">
    </ejs-textbox>

    <!-- Success color -->
    <ejs-textbox id="successLabel" 
                  placeholder="Success Label"
                  floatLabelType="Auto"
                  cssClass="label-success">
    </ejs-textbox>

    <!-- Danger color -->
    <ejs-textbox id="dangerLabel" 
                  placeholder="Danger Label"
                  floatLabelType="Auto"
                  cssClass="label-danger">
    </ejs-textbox>
</div>

<style>
    .label-primary.e-input-group.e-float-label input:focus ~ .e-float-label {
        color: #007bff;
    }

    .label-success.e-input-group.e-float-label input:focus ~ .e-float-label {
        color: #28a745;
    }

    .label-danger.e-input-group.e-float-label input:focus ~ .e-float-label {
        color: #dc3545;
    }
</style>
```

## Accessibility with Floating Labels

### Proper Label Structure

```cshtml
<!-- ✓ Good: Label properly associated -->
<label for="email">Email Address</label>
<ejs-textbox id="email" 
              type="email"
              placeholder="your@email.com"
              floatLabelType="Auto">
</ejs-textbox>

<!-- ✗ Poor: No explicit label -->
<ejs-textbox id="email" 
              placeholder="your@email.com"
              floatLabelType="Auto">
</ejs-textbox>
```

### ARIA Attributes

```cshtml
<!-- With ARIA labels for accessibility -->
<ejs-textbox id="username" 
              placeholder="Username"
              floatLabelType="Auto"
              aria-label="Username"
              aria-describedby="usernameHelp">
</ejs-textbox>
<small id="usernameHelp">3-20 characters, letters and numbers only</small>

<!-- With aria-required for required fields -->
<ejs-textbox id="password" 
              type="password"
              placeholder="Password"
              floatLabelType="Auto"
              required="true"
              aria-required="true"
              aria-label="Password (required)">
</ejs-textbox>
```

### Screen Reader Support

```cshtml
<!-- Clear label structure for screen readers -->
<div class="form-group">
    <label for="email">
        Email Address
        <span class="required" aria-label="required">*</span>
    </label>
    <ejs-textbox id="email" 
                  type="email"
                  placeholder="name@example.com"
                  floatLabelType="Auto"
                  required="true"
                  aria-required="true"
                  aria-describedby="emailHint">
    </ejs-textbox>
    <small id="emailHint">We'll never share your email</small>
</div>

<style>
    .required {
        color: #dc3545;
        margin-left: 4px;
    }
</style>
```

## Animation Patterns

### Smooth Label Animation

```cshtml
<div class="animated-form">
    <ejs-textbox id="animEmail" 
                  type="email"
                  placeholder="Enter email"
                  floatLabelType="Auto"
                  cssClass="smooth-animation">
    </ejs-textbox>
    
    <ejs-textbox id="animPassword" 
                  type="password"
                  placeholder="Enter password"
                  floatLabelType="Auto"
                  cssClass="smooth-animation">
    </ejs-textbox>
</div>

<style>
    .smooth-animation.e-input-group.e-float-label .e-float-label {
        transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        transform: translateY(0);
    }

    .smooth-animation.e-input-group.e-float-label input:focus ~ .e-float-label,
    .smooth-animation.e-input-group.e-float-label input:not(:placeholder-shown) ~ .e-float-label {
        transform: translateY(-20px);
        font-size: 12px;
    }
</style>
```

### Floating Label with Icon

```cshtml
<!-- Floating label with prepended icon -->
<div class="icon-float-form">
    <ejs-textbox id="iconEmail" 
                  type="email"
                  placeholder="Email"
                  floatLabelType="Auto"
                  prependTemplate="<span class='email-icon'>✉️</span>"
                  cssClass="with-icon">
    </ejs-textbox>
</div>

<style>
    .with-icon {
        position: relative;
    }

    .email-icon {
        display: inline-flex;
        align-items: center;
        padding: 0 8px;
        color: #007bff;
    }

    .with-icon.e-input-group.e-float-label .e-float-label {
        left: 40px;
        transition: all 0.3s ease;
    }

    .with-icon.e-input-group.e-float-label input:focus ~ .e-float-label {
        left: 40px;
        color: #0056b3;
    }
</style>
```

## Edge Cases

### Empty vs Placeholder-Only

```cshtml
<!-- If value is set, label floats -->
<ejs-textbox id="prefilled" 
              placeholder="Country"
              floatLabelType="Auto"
              value="United States">
</ejs-textbox>

<!-- If value is empty, label doesn't float until focus -->
<ejs-textbox id="empty" 
              placeholder="Country"
              floatLabelType="Auto"
              value="">
</ejs-textbox>
```

**Expected Behavior:**
- First: Label floats immediately (has value)
- Second: Label inside input (no value)

### Readonly with Floating Label

```cshtml
<!-- Readonly field with floating label -->
<ejs-textbox id="readonlyField" 
              placeholder="Account ID"
              floatLabelType="Auto"
              readonly="true"
              value="ACC-12345-67890">
</ejs-textbox>

<!-- Label should float with readonly input -->
```

### Disabled with Floating Label

```cshtml
<!-- Disabled field with floating label -->
<ejs-textbox id="disabledField" 
              placeholder="Disabled Field"
              floatLabelType="Auto"
              disabled="true"
              value="This is disabled">
</ejs-textbox>

<!-- Label floats but input is disabled -->
```

### Multiline with Floating Label

```cshtml
<!-- Multiline (textarea) with floating label -->
<ejs-textbox id="commentField" 
              placeholder="Write your comment here..."
              floatLabelType="Auto"
              multiline="true"
              rows="4">
</ejs-textbox>

<!-- Label should float on focus or value -->
```

### Floating Label with Validation

```cshtml
<!-- Floating label with validation state -->
<ejs-textbox id="validField" 
              type="email"
              placeholder="Email"
              floatLabelType="Auto"
              cssClass="e-success"
              value="valid@example.com">
</ejs-textbox>

<ejs-textbox id="invalidField" 
              type="email"
              placeholder="Email"
              floatLabelType="Auto"
              cssClass="e-error"
              value="invalid-email">
</ejs-textbox>

<!-- Floating labels work with validation states -->
```

## Best Practices

### 1. Consistency in Float Type

```cshtml
<!-- ✓ Good: Consistent throughout form -->
<ejs-textbox placeholder="Name" floatLabelType="Auto"></ejs-textbox>
<ejs-textbox placeholder="Email" floatLabelType="Auto"></ejs-textbox>
<ejs-textbox placeholder="Phone" floatLabelType="Auto"></ejs-textbox>

<!-- ✗ Poor: Inconsistent types -->
<ejs-textbox placeholder="Name" floatLabelType="Auto"></ejs-textbox>
<ejs-textbox placeholder="Email" floatLabelType="Always"></ejs-textbox>
<ejs-textbox placeholder="Phone" floatLabelType="Never"></ejs-textbox>
```

### 2. Meaningful Placeholders

```cshtml
<!-- ✓ Good: Clear, descriptive placeholder -->
<ejs-textbox placeholder="First and Last Name" 
              floatLabelType="Auto"></ejs-textbox>

<!-- ✗ Poor: Vague placeholder -->
<ejs-textbox placeholder="Name" 
              floatLabelType="Auto"></ejs-textbox>
```

### 3. Combine with Explicit Labels

```cshtml
<!-- ✓ Good: Both floating label and explicit label -->
<label for="email">Email Address <span class="required">*</span></label>
<ejs-textbox id="email" 
              placeholder="your@example.com"
              floatLabelType="Auto"
              required="true">
</ejs-textbox>

<!-- Helps accessibility and clarity -->
```

### 4. Sufficient Spacing

```cshtml
<div class="form-group">
    <ejs-textbox placeholder="First Name" 
                  floatLabelType="Auto">
    </ejs-textbox>
</div>

<div class="form-group">
    <ejs-textbox placeholder="Last Name" 
                  floatLabelType="Auto">
    </ejs-textbox>
</div>

<style>
    .form-group {
        margin-bottom: 1.5rem; /* Space for floating label */
    }
</style>
```

### 5. Test with Real Content

```cshtml
<!-- Test with various content lengths -->
<ejs-textbox placeholder="Short" 
              floatLabelType="Auto"
              value="X">
</ejs-textbox>

<ejs-textbox placeholder="This is a very long placeholder text for testing" 
              floatLabelType="Auto"
              value="This is a much longer value that the user has entered into the field">
</ejs-textbox>

<!-- Ensure layout doesn't break with varied content -->
```

### 6. Handle Dynamic Content

```cshtml
@model UserForm

<ejs-textbox asp-for="Email" 
              placeholder="Email Address"
              floatLabelType="Auto">
</ejs-textbox>

<!-- With model binding, floating label adjusts based on value -->
```

---

**Related References:**
- [tag-helper-syntax.md](tag-helper-syntax.md) - TextBox properties
- [validation-states.md](validation-states.md) - Validation with floating labels
- [advanced-patterns.md](advanced-patterns.md) - Complex floating label scenarios
