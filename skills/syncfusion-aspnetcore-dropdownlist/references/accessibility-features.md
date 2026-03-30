# Accessibility and Features in DropdownList

## Table of Contents
- [WCAG Compliance](#wcag-compliance)
- [Keyboard Navigation](#keyboard-navigation)
- [ARIA Attributes](#aria-attributes)
- [Screen Reader Support](#screen-reader-support)
- [Focus Management](#focus-management)
- [Disabled State](#disabled-state)
- [RTL Support](#rtl-support)
- [Testing Accessibility](#testing-accessibility)

## WCAG Compliance

Syncfusion DropdownList meets WCAG 2.1 Level AA standards.

### Compliance Features Built-In

✅ **Level A Compliance:**
- Keyboard accessible
- Proper semantic markup
- Clear focus indicators
- Alternative text for visual content

✅ **Level AA Compliance:**
- Color contrast ≥4.5:1 for text
- Focus visible with ≥3px outline
- Error identification and recovery
- Sufficient time for user interactions

### Implementing Accessible Dropdowns

```csharp
@Html.EJ2().DropDownList()
    .Id("ServiceType")
    .DataSource(ViewBag.Services)
    .Fields(f => f.Text("ServiceName").Value("ServiceId"))
    .Placeholder("Select a service type")
    .AriaLabel("Service Type Selection")  // Screen reader label
    .AriaDescribedBy("service-help")      // Help text
    .Render()

<!-- Help text for screen readers -->
<span id="service-help" style="display: none;">
    Select the type of service you need. Use arrow keys to navigate.
</span>
```

## Keyboard Navigation

Users can navigate dropdowns entirely with keyboard.

### Standard Keyboard Shortcuts

| Key | Action |
|-----|--------|
| **Arrow Down** | Open popup, move to next item |
| **Arrow Up** | Move to previous item |
| **Space** | Open/close dropdown, select focused item |
| **Enter** | Select focused item |
| **Tab** | Move to next form field, close popup |
| **Shift + Tab** | Move to previous form field, close popup |
| **Escape** | Close popup, return focus to input |
| **Alt + Down** | Open dropdown |
| **Alt + Up** | Close dropdown |

### Example: Keyboard Navigation

```csharp
@Html.EJ2().DropDownList()
    .Id("Categories")
    .DataSource(ViewBag.Categories)
    .Fields(f => f.Text("Name").Value("Id"))
    .Placeholder("Use arrow keys to navigate")
    .Render()

<!-- User can:
1. Tab to dropdown
2. Press Down arrow to open
3. Press Arrow keys to navigate
4. Press Enter to select
5. Press Escape to close
-->
```

### Keyboard with Filtering

When filtering enabled, keyboard still works:

```csharp
@Html.EJ2().DropDownList()
    .Id("Products")
    .DataSource(ViewBag.Products)
    .AllowFiltering(true)
    .Render()

<!-- User workflow:
1. Tab to dropdown
2. Type to filter (keyboard input goes to search box)
3. Arrow keys navigate filtered results
4. Enter to select
-->
```

## ARIA Attributes

Add semantic meaning for assistive technologies.

### ARIA Roles and Properties

```csharp
@Html.EJ2().DropDownList()
    .Id("Priority")
    .DataSource(ViewBag.Priorities)
    .AriaLabel("Task Priority")                    // Label for control
    .AriaDescribedBy("priority-instructions")     // Helper text
    .AriaLive("polite")                           // Announce changes
    .Role("combobox")                             // ARIA role
    .Render()

<!-- Corresponding elements -->
<label for="Priority">Task Priority</label>
<span id="priority-instructions">
    Select priority level for this task. 
    Press Down arrow to see options.
</span>
```

### ARIA Attributes in Implementation

```html
<!-- Rendered dropdown structure with ARIA -->
<input 
    id="Priority"
    role="combobox"
    aria-label="Task Priority"
    aria-described-by="priority-instructions"
    aria-expanded="false"
    aria-owns="Priority_popup"
    aria-live="polite"
/>

<!-- Associated popup list -->
<ul id="Priority_popup" 
    role="listbox"
    aria-label="Priority options"
    style="display: none;">
    <li role="option" aria-selected="false">Low</li>
    <li role="option" aria-selected="true">Medium</li>
    <li role="option" aria-selected="false">High</li>
</ul>
```

## Screen Reader Support

Optimize for assistive technology users.

### Labels for Screen Readers

```csharp
<!-- ALWAYS use associated label for screen readers -->
@Html.EJ2().DropDownList()
    .Id("Department")
    .DataSource(ViewBag.Departments)
    .Render()

<!-- Associated label - REQUIRED for accessibility -->
<label for="Department">Select your department:</label>

<!-- Or use aria-label if visual label not visible -->
@Html.EJ2().DropDownList()
    .Id("Status")
    .DataSource(ViewBag.Statuses)
    .AriaLabel("Task Status")
    .Render()
```

### Announcing Selected Values

```csharp
@Html.EJ2().DropDownList()
    .Id("Countries")
    .DataSource(ViewBag.Countries)
    .Fields(f => f.Text("CountryName").Value("CountryCode"))
    .Change("announceSelection")
    .Render()

<script>
function announceSelection(args) {
    // Screen reader announces the change
    var announcement = document.createElement('div');
    announcement.setAttribute('role', 'status');
    announcement.setAttribute('aria-live', 'polite');
    announcement.setAttribute('aria-atomic', 'true');
    announcement.textContent = 'Selected: ' + args.text;
    document.body.appendChild(announcement);
    
    // Remove after 3 seconds
    setTimeout(() => announcement.remove(), 3000);
}
</script>
```

### Grouped List Support

When grouping items, screen readers understand structure:

```csharp
@Html.EJ2().DropDownList()
    .Id("Products")
    .DataSource(ViewBag.Products)
    .Fields(f => f
        .Text("ProductName")
        .Value("ProductId")
        .GroupBy("Category")
    )
    .GroupBy("Category")
    .Render()

<!-- Screen readers announce:
"Electronics group, 3 items
  - Laptop
  - Mouse
  - Keyboard
Furniture group, 2 items
  - Desk
  - Chair
-->
```

## Focus Management

Ensure proper focus flow for keyboard users.

### Initial Focus

```csharp
@Html.EJ2().DropDownList()
    .Id("Priority")
    .DataSource(ViewBag.Priorities)
    .AutoFocus(true)  // Set focus on page load
    .Render()

<!-- Or using JavaScript -->
<script>
window.addEventListener('load', function() {
    document.getElementById('Priority').focus();
});
</script>
```

### Focus Visible Styling

```csharp
@Html.EJ2().DropDownList()
    .Id("Category")
    .DataSource(ViewBag.Categories)
    .CssClass("accessible-dropdown")
    .Render()

<style>
/* Ensure focus indicator visible */
.accessible-dropdown .e-input:focus {
    outline: 3px solid #0078d4;
    outline-offset: 2px;
}

/* High contrast focus outline */
@media (prefers-contrast: more) {
    .accessible-dropdown .e-input:focus {
        outline: 3px solid #000;
    }
}
</style>
```

### Focus Trap in Popup

```javascript
// Handle focus when popup opens/closes
function onPopupOpening(args) {
    // Set focus to first item when popup opens
    setTimeout(() => {
        var firstItem = document.querySelector('.e-list-item');
        if (firstItem) firstItem.focus();
    }, 100);
}

function onPopupClosing(args) {
    // Return focus to input when popup closes
    document.getElementById('Dropdown').focus();
}
```

## Disabled State

Properly handle disabled dropdowns.

### Disabling the Dropdown

```csharp
@Html.EJ2().DropDownList()
    .Id("Status")
    .DataSource(ViewBag.Statuses)
    .Enabled(false)  // Disable the control
    .Render()

<!-- With conditional logic -->
@{
    bool isEditable = Model.AllowEdit;
}

@Html.EJ2().DropDownList()
    .Id("Category")
    .DataSource(ViewBag.Categories)
    .Enabled(isEditable)
    .Render()
```

### Disabled State Styling

```csharp
@Html.EJ2().DropDownList()
    .Id("Options")
    .DataSource(ViewBag.Options)
    .Enabled(false)
    .CssClass("disabled-dropdown")
    .Render()

<style>
.disabled-dropdown .e-input {
    background-color: #f5f5f5;
    color: #999;
    cursor: not-allowed;
    opacity: 0.6;
}

.disabled-dropdown .e-input:focus {
    outline: none;
    border-color: #ccc;
}
</style>
```

### Explaining Disabled State

```csharp
<!-- Provide reason why dropdown is disabled -->
@if (!Model.AllowEdit)
{
    <p class="disabled-reason">
        <i class="e-icons e-info"></i>
        This field is disabled because the record is archived.
    </p>
}

@Html.EJ2().DropDownList()
    .Id("Status")
    .DataSource(ViewBag.Statuses)
    .Enabled(Model.AllowEdit)
    .AriaDescribedBy("disabled-reason")
    .Render()
```

## RTL Support

Support right-to-left languages.

### Enabling RTL

```csharp
@Html.EJ2().DropDownList()
    .Id("Languages")
    .DataSource(ViewBag.Languages)
    .EnableRtl(true)  // Enable RTL layout
    .Render()

<!-- Or via HTML attribute -->
<div dir="rtl">
    @Html.EJ2().DropDownList()
        .Id("ArabicOptions")
        .DataSource(ViewBag.Options)
        .Render()
</div>
```

### RTL with Arabic/Hebrew

```csharp
@Html.EJ2().DropDownList()
    .Id("Countries")
    .DataSource(ViewBag.CountriesArabic)
    .Fields(f => f.Text("CountryNameAr").Value("CountryId"))
    .EnableRtl(true)
    .Placeholder("اختر دولة")  // Arabic placeholder
    .Render()
```

### RTL Styling

```css
[dir="rtl"] .e-dropdownlist .e-input {
    text-align: right;
    direction: rtl;
}

[dir="rtl"] .e-list-item {
    text-align: right;
    direction: rtl;
}
```

## Testing Accessibility

### Automated Testing Tools

1. **Axe DevTools** - Browser extension for accessibility audits
2. **WAVE** - Web accessibility evaluation tool
3. **Screen Readers:**
   - NVDA (Windows, free)
   - JAWS (Windows, paid)
   - VoiceOver (macOS, built-in)
   - TalkBack (Android, built-in)

### Manual Keyboard Testing

```
Test Script:
1. Press Tab to focus dropdown
2. Press Down arrow - dropdown opens
3. Press Arrow keys multiple times - items highlight
4. Press Enter - item selected
5. Press Tab - focus moves to next control
6. Press Shift+Tab - focus moves back to dropdown
7. Repeat with filtering enabled
8. Test with grouping enabled
```

### Screen Reader Testing

```
Test with NVDA/JAWS:
1. Navigate to dropdown using arrow keys
2. Verify label announced
3. Open dropdown (Space or Down arrow)
4. Verify list structure announced
5. Navigate items
6. Select item (Enter)
7. Verify selected value announced
8. Test grouped items - verify groups announced
```

### Keyboard-Only Testing

```
Complete workflow using only keyboard:
1. Tab to dropdown
2. Interact completely via keyboard
3. No mouse interaction
4. Verify all features work
```

## Accessibility Checklist

- [ ] Dropdown has associated `<label>`
- [ ] AriaLabel set for screen readers
- [ ] Keyboard navigation works completely
- [ ] Focus indicators visible (≥3px)
- [ ] Color contrast ≥4.5:1
- [ ] No color used as only differentiator
- [ ] Error messages clear and linked
- [ ] Disabled state properly indicated
- [ ] RTL support if needed
- [ ] Tested with keyboard only
- [ ] Tested with screen reader
- [ ] Works without JavaScript (fallback)

## Accessible Form Example

```csharp
<form>
    <fieldset>
        <legend>Service Request</legend>
        
        <!-- Service Type -->
        <div class="form-group">
            <label for="ServiceType">
                Service Type <span aria-label="required">*</span>
            </label>
            @Html.EJ2().DropDownList()
                .Id("ServiceType")
                .DataSource(ViewBag.Services)
                .Fields(f => f.Text("Name").Value("Id"))
                .Required(true)
                .AriaDescribedBy("service-help")
                .Render()
            <span id="service-help" class="form-hint">
                Select the service you need assistance with
            </span>
        </div>
        
        <!-- Priority -->
        <div class="form-group">
            <label for="Priority">Priority Level</label>
            @Html.EJ2().DropDownList()
                .Id("Priority")
                .DataSource(ViewBag.Priorities)
                .Render()
        </div>
        
        <button type="submit" class="btn-primary">Submit Request</button>
    </fieldset>
</form>
```

---

## Next Steps

- Review [Advanced Scenarios](advanced-scenarios.md) for complex patterns
- Check [Templates and Styling](templates-styling.md) for accessible styling
- Learn [Data Binding](data-binding.md) for dynamic content
