# Styling and Appearance

## Table of Contents
- [CSS Customization](#css-customization)
- [Theme Integration](#theme-integration)
- [Input State Styling](#input-state-styling)
- [Placeholder and Label](#placeholder-and-label)
- [Responsive Popup](#responsive-popup)
- [Accessibility and Focus](#accessibility-and-focus)
- [Advanced Styling Examples](#advanced-styling-examples)

## CSS Customization

### Basic CSS Classes

DatePicker generates these CSS classes:

| Class | Applied To | Purpose |
|-------|-----------|---------|
| `.e-datepicker` | Wrapper | Main component container |
| `.e-input` | Input element | Date input field |
| `.e-input-focus` | Input (focused) | Active/focused state |
| `.e-calendar` | Calendar popup | Calendar container |
| `.e-disabled` | Element | Disabled state |
| `.e-error` | Element | Validation error state |
| `.e-readonly` | Element | Readonly state |

### Customizing DatePicker Input

```html
<ejs-datepicker id="customInput"
                 value="@DateTime.Today"
                 format="yyyy-MM-dd"
                 placeholder="Select date">
</ejs-datepicker>

<style>
/* Customize DatePicker input */
#customInput.e-datepicker {
    width: 300px;
}

#customInput.e-input {
    padding: 10px 12px;
    font-size: 16px;
    border-radius: 4px;
    border: 2px solid #e0e0e0;
}

/* On focus */
#customInput.e-input-focus {
    border-color: #2196F3;
    box-shadow: 0 0 0 3px rgba(33, 150, 243, 0.1);
}

/* Hover effect */
#customInput.e-datepicker:hover .e-input {
    border-color: #2196F3;
}
</style>
```

### Calendar Popup Styling

```html
<ejs-datepicker id="customCalendar"
                 format="yyyy-MM-dd">
</ejs-datepicker>

<style>
/* Calendar popup container */
.e-calendar {
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    border-radius: 8px;
}

/* Calendar header */
.e-calendar .e-header {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    border-radius: 8px 8px 0 0;
}

/* Today button */
.e-calendar .e-today {
    background-color: #4CAF50;
    color: white;
}

/* Hovered date */
.e-calendar .e-cell:hover {
    background-color: #E3F2FD;
    border-radius: 4px;
}
</style>
```

## Theme Integration

### Built-In Themes

```html
<!-- Fluent Theme (Modern, Clean) -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/24.1.48/fluent.css" />

<!-- Bootstrap Theme (Bootstrap Framework) -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/24.1.48/bootstrap.css" />

<!-- Material Theme (Material Design) -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/24.1.48/material.css" />

<!-- High Contrast (Accessibility) -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/24.1.48/highcontrast.css" />

<!-- Tailwind Theme (Tailwind CSS) -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/24.1.48/tailwind.css" />
```

### Theme Switching

```csharp
public class ThemeSwitcherModel : PageModel
{
    [BindProperty]
    public string SelectedTheme { get; set; } = "fluent";
    
    public List<SelectListItem> AvailableThemes { get; set; }
    
    public void OnGet()
    {
        AvailableThemes = new()
        {
            new SelectListItem { Text = "Fluent", Value = "fluent" },
            new SelectListItem { Text = "Bootstrap", Value = "bootstrap" },
            new SelectListItem { Text = "Material", Value = "material" },
            new SelectListItem { Text = "High Contrast", Value = "highcontrast" }
        };
    }
}
```

```html
<form method="post">
    <label>Select Theme:</label>
    <select asp-for="SelectedTheme" asp-items="Model.AvailableThemes">
    </select>
    <button type="submit">Apply</button>
</form>

<!-- Dynamic theme loading -->
<div id="themeContainer">
    <ejs-datepicker asp-for="EventDate"
                     placeholder="DatePicker with selected theme">
    </ejs-datepicker>
</div>

<script>
// Load theme dynamically
function loadTheme(themeName) {
    const themeLink = document.getElementById('themeLink') || 
                      document.createElement('link');
    
    themeLink.id = 'themeLink';
    themeLink.rel = 'stylesheet';
    themeLink.href = `https://cdn.syncfusion.com/ej2/24.1.48/${themeName}.css`;
    
    if (!document.getElementById('themeLink')) {
        document.head.appendChild(themeLink);
    } else {
        themeLink.href = `https://cdn.syncfusion.com/ej2/24.1.48/${themeName}.css`;
    }
}

// Load theme on page load
loadTheme('@Model.SelectedTheme');
</script>
```

## Input State Styling

### Readonly State

```html
<ejs-datepicker id="readonlyPicker"
                 value="@DateTime.Today"
                 readonly="true"
                 format="yyyy-MM-dd">
</ejs-datepicker>

<style>
/* Readonly styling */
#readonlyPicker.e-datepicker.e-readonly .e-input {
    background-color: #f5f5f5;
    cursor: not-allowed;
    color: #666;
}

#readonlyPicker.e-datepicker.e-readonly .e-input::placeholder {
    color: #999;
}
</style>
```

### Disabled State

```html
<ejs-datepicker id="disabledPicker"
                 value="@DateTime.Today"
                 enabled="false"
                 format="yyyy-MM-dd">
</ejs-datepicker>

<style>
/* Disabled styling */
#disabledPicker.e-datepicker.e-disabled .e-input {
    background-color: #efefef;
    opacity: 0.6;
    cursor: not-allowed;
}

#disabledPicker.e-datepicker.e-disabled .e-input-group-icon {
    pointer-events: none;
    opacity: 0.4;
}
</style>
```

### Validation Error State

```html
<ejs-datepicker id="validatedPicker"
                 format="yyyy-MM-dd"
                 strict-mode="true">
</ejs-datepicker>
<span id="errorMsg" class="text-danger"></span>

<style>
/* Error state styling */
#validatedPicker.e-datepicker.e-error .e-input {
    border-color: #f44336;
    background-color: #ffebee;
}

#validatedPicker.e-datepicker.e-error .e-input:focus {
    box-shadow: 0 0 0 3px rgba(244, 67, 54, 0.1);
}

.text-danger {
    color: #f44336;
    font-size: 12px;
    margin-top: 4px;
    display: block;
}
</style>
```

## Placeholder and Label

### Placeholder Customization

```html
<form class="form-example">
    <div class="form-group">
        <label for="startDate">Start Date</label>
        <ejs-datepicker id="startDate"
                         placeholder="mm/dd/yyyy"
                         format="MM/dd/yyyy">
        </ejs-datepicker>
    </div>
    
    <div class="form-group">
        <label for="endDate">End Date</label>
        <ejs-datepicker id="endDate"
                         placeholder="Select end date..."
                         format="MM/dd/yyyy">
        </ejs-datepicker>
    </div>
</form>

<style>
.form-group {
    margin-bottom: 20px;
}

label {
    display: block;
    margin-bottom: 5px;
    font-weight: 600;
    color: #333;
}

/* Placeholder styling */
.e-datepicker .e-input::placeholder {
    color: #999;
    font-style: italic;
}
</style>
```

### Label with Required Indicator

```html
<div class="form-group">
    <label for="requiredDate">
        Event Date <span class="required-indicator">*</span>
    </label>
    <ejs-datepicker id="requiredDate"
                     format="yyyy-MM-dd"
                     placeholder="Required field">
    </ejs-datepicker>
</div>

<style>
.required-indicator {
    color: #f44336;
    font-weight: bold;
    margin-left: 2px;
}

.form-group .e-datepicker {
    border: 2px solid #e0e0e0;
    border-radius: 4px;
    padding: 2px;
}

.form-group .e-datepicker:hover {
    border-color: #2196F3;
}
</style>
```

## Responsive Popup

### Mobile-Friendly Popup

```html
<ejs-datepicker id="responsivePicker"
                 format="yyyy-MM-dd"
                 placeholder="Click to open calendar">
</ejs-datepicker>

<style>
/* Responsive calendar positioning */
@media (max-width: 768px) {
    .e-calendar {
        width: 100% !important;
        max-width: calc(100% - 20px);
        position: fixed;
        bottom: 0;
        left: 10px;
        right: 10px;
        border-radius: 12px 12px 0 0;
        z-index: 1000;
    }
    
    .e-calendar .e-header {
        padding: 16px;
    }
    
    .e-calendar .e-cell {
        padding: 12px;
        font-size: 16px;
    }
}

@media (min-width: 769px) {
    .e-calendar {
        box-shadow: 0 4px 20px rgba(0, 0, 0, 0.12);
        border-radius: 8px;
    }
}
</style>
```

### Popup Position Control

```html
<script>
// Position popup below input
document.getElementById('datepicker').ej2_instances[0].popupSettings = {
    position: { X: 'left', Y: 'bottom' }
};

// Position popup above input (if space limited)
document.getElementById('datepicker').ej2_instances[0].popupSettings = {
    position: { X: 'left', Y: 'top' }
};

// Center popup on screen
document.getElementById('datepicker').ej2_instances[0].popupSettings = {
    position: { X: 'center', Y: 'center' }
};
</script>
```

## Accessibility and Focus

### Focus Styling

```html
<ejs-datepicker id="focusablePicker"
                 format="yyyy-MM-dd"
                 placeholder="Tab to focus">
</ejs-datepicker>

<style>
/* Clear focus indicator */
#focusablePicker.e-input:focus {
    outline: 2px solid #2196F3;
    outline-offset: 2px;
}

/* Focus visible for keyboard navigation */
#focusablePicker.e-input:focus-visible {
    outline: 3px dashed #2196F3;
    outline-offset: 2px;
}

/* High contrast mode */
@media (prefers-contrast: more) {
    #focusablePicker.e-input:focus {
        outline: 3px solid #000;
        background-color: #fff;
    }
}
</style>
```

### ARIA Labels and Accessibility

```html
<!-- Accessible DatePicker with ARIA labels -->
<div class="date-input-group">
    <label for="accessibleDate" id="dateLabel">
        Appointment Date <span aria-label="required">*</span>
    </label>
    
    <ejs-datepicker id="accessibleDate"
                     aria-labelledby="dateLabel"
                     aria-describedby="dateHelp"
                     format="yyyy-MM-dd"
                     placeholder="yyyy-MM-dd">
    </ejs-datepicker>
    
    <div id="dateHelp" class="help-text">
        Format: YYYY-MM-DD. Select from calendar or type manually.
    </div>
    
    <div id="dateError" class="error-message" role="alert" aria-live="polite">
    </div>
</div>

<style>
.date-input-group {
    margin-bottom: 20px;
}

label {
    display: block;
    margin-bottom: 8px;
    font-weight: 600;
}

.help-text {
    font-size: 12px;
    color: #666;
    margin-top: 4px;
}

.error-message {
    color: #f44336;
    font-size: 12px;
    margin-top: 4px;
}
</style>
```

### Keyboard Navigation Styling

```css
/* Highlight keyboard focus vs mouse focus */
.e-datepicker .e-input:focus-visible {
    outline: 2px solid #2196F3;
    outline-offset: 2px;
}

/* Calendar cell keyboard focus */
.e-calendar .e-cell:focus-visible {
    outline: 2px solid #2196F3;
    outline-offset: -2px;
}

/* High contrast mode */
@media (prefers-color-scheme: dark) {
    .e-datepicker .e-input {
        background-color: #1e1e1e;
        color: #e0e0e0;
        border-color: #404040;
    }
    
    .e-datepicker .e-input:focus {
        border-color: #64b5f6;
    }
}
```

## Advanced Styling Examples

### Example 1: Material Design DatePicker

```html
<ejs-datepicker id="materialDatePicker"
                 format="yyyy-MM-dd"
                 placeholder="Material Design">
</ejs-datepicker>

<style>
/* Material Design appearance */
#materialDatePicker.e-datepicker {
    margin: 16px 0;
}

#materialDatePicker.e-input {
    padding: 12px 0;
    border-bottom: 2px solid #bdbdbd;
    border-radius: 0;
    font-size: 14px;
    transition: all 0.3s ease;
}

#materialDatePicker.e-input:focus {
    border-bottom-color: #2196F3;
    box-shadow: 0 2px 4px rgba(33, 150, 243, 0.3);
}

#materialDatePicker.e-input::placeholder {
    color: #bdbdbd;
}

/* Floating label effect */
#materialDatePicker.e-input-focus::placeholder {
    color: transparent;
}
</style>
```

### Example 2: Glassmorphism DatePicker

```html
<ejs-datepicker id="glassDatePicker"
                 format="yyyy-MM-dd"
                 placeholder="Modern glass effect">
</ejs-datepicker>

<style>
#glassDatePicker.e-datepicker .e-input {
    background: rgba(255, 255, 255, 0.25);
    border: 1px solid rgba(255, 255, 255, 0.18);
    backdrop-filter: blur(10px);
    border-radius: 8px;
    padding: 12px 16px;
    color: #333;
}

#glassDatePicker.e-datepicker .e-input:focus {
    background: rgba(255, 255, 255, 0.35);
    border-color: rgba(255, 255, 255, 0.3);
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
}

.e-calendar {
    background: rgba(255, 255, 255, 0.9);
    backdrop-filter: blur(10px);
    border-radius: 12px;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.15);
}
</style>
```

### Example 3: Dark Mode DatePicker

```html
<ejs-datepicker id="darkDatePicker"
                 format="yyyy-MM-dd"
                 placeholder="Dark mode">
</ejs-datepicker>

<style>
/* Dark mode container */
body.dark-mode {
    background-color: #121212;
    color: #fff;
}

body.dark-mode #darkDatePicker.e-input {
    background-color: #1e1e1e;
    color: #fff;
    border-color: #404040;
}

body.dark-mode #darkDatePicker.e-input:focus {
    border-color: #64b5f6;
    background-color: #252525;
}

body.dark-mode .e-calendar {
    background-color: #1e1e1e;
    color: #fff;
}

body.dark-mode .e-calendar .e-header {
    background-color: #2196F3;
}

body.dark-mode .e-calendar .e-cell:hover {
    background-color: #304d7a;
}
</style>
```

### Example 4: Compact DatePicker

```html
<ejs-datepicker id="compactPicker"
                 format="yyyy-MM-dd"
                 placeholder="Compact">
</ejs-datepicker>

<style>
/* Compact sizing */
#compactPicker.e-datepicker {
    height: 32px;
}

#compactPicker.e-input {
    padding: 4px 8px;
    font-size: 12px;
    height: 100%;
}

#compactPicker.e-datepicker .e-input-group-icon {
    font-size: 12px;
}

.e-calendar {
    width: 250px;
}

.e-calendar .e-cell {
    padding: 6px;
    font-size: 12px;
}
</style>
```

---

**Previous:** [Globalization and Localization](../globalization-localization.md)  
**Last Reference:** See [SKILL.md](../SKILL.md) for complete navigation
