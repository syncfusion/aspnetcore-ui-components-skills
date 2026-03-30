# Accessibility

## Table of Contents
- [WCAG Compliance](#wcag-compliance)
- [Keyboard Navigation](#keyboard-navigation)
- [ARIA Attributes](#aria-attributes)
- [Screen Reader Support](#screen-reader-support)
- [Focus Management](#focus-management)
- [Testing](#testing)

## WCAG Compliance

MultiSelect is built with accessibility standards in mind:

### WCAG 2.2 Level AA Support

The Syncfusion MultiSelect control meets WCAG 2.2 Level AA standards:

- ✅ **Perceivable** - Visual and programmatic identification
- ✅ **Operable** - Keyboard and screen reader operation
- ✅ **Understandable** - Clear labels and error messages
- ✅ **Robust** - Compatible with assistive technologies

### Accessibility Features

| Standard | Support |
|----------|---------|
| WCAG 2.2 Level AA | ✅ Full |
| Section 508 | ✅ Full |
| ADA (Americans with Disabilities Act) | ✅ Full |
| WAI-ARIA | ✅ Full |

## Keyboard Navigation

### Essential Keyboard Shortcuts

| Key | Function |
|-----|----------|
| **Tab** | Move focus to MultiSelect |
| **Shift + Tab** | Move focus backwards |
| **Enter** | Open dropdown / Toggle selection |
| **Space** | Toggle checkbox item selection |
| **Arrow Up** | Move to previous item |
| **Arrow Down** | Move to next item |
| **Home** | Move to first item |
| **End** | Move to last item |
| **Escape** | Close dropdown |
| **Backspace** | Remove last selected chip |
| **Delete** | Remove selected item (when focused) |

### Keyboard Navigation Example

```razor
<ejs-multiselect id="languages" 
    dataSource="ViewBag.LanguageList"
    placeholder="Press Tab to focus, Arrow keys to navigate">
</ejs-multiselect>

<ejs-scriptmanager></ejs-scriptmanager>
```

**Keyboard Flow:**
1. User presses **Tab** → MultiSelect receives focus (visible outline)
2. User presses **Arrow Down** → Dropdown opens, first item highlighted
3. User presses **Arrow Down** → Selection moves to next item
4. User presses **Enter** → Item selected, added as chip
5. User presses **Backspace** → Last chip removed
6. User presses **Escape** → Dropdown closes

### Custom Keyboard Shortcuts

```razor
<ejs-multiselect id="items" 
    dataSource="ViewBag.ItemList"
    created="onMultiSelectCreated">
</ejs-multiselect>

@section Scripts {
    <script>
        function onMultiSelectCreated(args) {
            var multiObj = args.element.ej2_instances[0];
            
            // Custom: Ctrl+A to select all
            document.addEventListener('keydown', function(e) {
                if (e.ctrlKey && e.key === 'a') {
                    multiObj.selectAll();
                    e.preventDefault();
                }
            });
        }
    </script>
}
```

## ARIA Attributes

MultiSelect automatically includes proper ARIA attributes:

### Built-in ARIA Support

```html
<!-- Generated HTML with ARIA -->
<div role="listbox" aria-label="Languages" aria-expanded="true">
    <div class="e-input-group" role="application">
        <input role="textbox" 
               aria-autocomplete="list" 
               aria-haspopup="listbox"
               aria-expanded="true">
    </div>
    <ul role="listbox">
        <li role="option" aria-selected="true">English</li>
        <li role="option" aria-selected="false">Spanish</li>
    </ul>
</div>
```

### ARIA Roles

| Role | Purpose |
|------|---------|
| `listbox` | Container for options |
| `option` | Individual selectable item |
| `textbox` | Input field for search/filter |

### ARIA Attributes

| Attribute | Purpose |
|-----------|---------|
| `aria-label` | Descriptive label for screen readers |
| `aria-labelledby` | Link to label element |
| `aria-expanded` | Indicates if dropdown is open |
| `aria-selected` | Indicates if item is selected |
| `aria-haspopup` | Indicates presence of dropdown |
| `aria-readonly` | Indicates read-only state |
| `aria-disabled` | Indicates disabled state |

## Screen Reader Support

### Proper Labeling

Always associate MultiSelect with a label:

```razor
<label for="languages">Select Languages</label>
<ejs-multiselect id="languages" 
    dataSource="ViewBag.LanguageList"
    aria-label="Select your preferred languages"
    placeholder="Click to select languages">
</ejs-multiselect>
```

**Screen Reader Announces:**
- "Select Languages, listbox, collapsed"
- When opened: "listbox, expanded"
- When item selected: "English, option, selected"

### Accessible Form Structure

```razor
<form>
    <fieldset>
        <legend>Application Preferences</legend>
        
        <div class="form-group">
            <label for="lang">Languages:</label>
            <ejs-multiselect id="lang" 
                dataSource="ViewBag.LanguageList"
                aria-describedby="lang-hint">
            </ejs-multiselect>
            <small id="lang-hint">Select one or more languages you speak</small>
        </div>
        
        <div class="form-group">
            <label for="skills">Skills:</label>
            <ejs-multiselect id="skills" 
                dataSource="ViewBag.SkillsList"
                aria-required="true">
            </ejs-multiselect>
        </div>
    </fieldset>
</form>
```

### Error Messages for Screen Readers

```razor
<ejs-multiselect id="required-field" 
    dataSource="ViewBag.ItemList"
    aria-required="true"
    aria-invalid="false"
    aria-describedby="error-msg">
</ejs-multiselect>

<span id="error-msg" role="alert" class="error-hidden">
    At least one item must be selected
</span>

@section Scripts {
    <script>
        function validateSelection() {
            var multiObj = document.getElementById('required-field').ej2_instances[0];
            var errorMsg = document.getElementById('error-msg');
            
            if (!multiObj.value || multiObj.value.length === 0) {
                multiObj.element.setAttribute('aria-invalid', 'true');
                errorMsg.classList.remove('error-hidden');
                errorMsg.classList.add('error-visible');
            } else {
                multiObj.element.setAttribute('aria-invalid', 'false');
                errorMsg.classList.add('error-hidden');
            }
        }
    </script>
}

<style>
    .error-hidden {
        display: none;
    }
    
    .error-visible {
        display: block;
        color: #d32f2f;
        margin-top: 5px;
    }
</style>
```

## Focus Management

### Clear Focus Indicators

Ensure focus is visually apparent:

```css
/* MultiSelect focus */
.e-multiselect.e-input-group:focus-within {
    outline: 3px solid #2196F3;
    outline-offset: 2px;
}

/* List item focus */
.e-multiselect .e-list-item:focus {
    outline: 2px solid #2196F3;
    outline-offset: -2px;
}

/* Chip focus */
.e-multiselect .e-chip:focus {
    outline: 2px solid #2196F3;
    outline-offset: 2px;
}
```

### Focus Trap (Modal Behavior)

For accessibility in dialogs:

```razor
<div role="dialog" aria-modal="true" aria-labelledby="dialog-title">
    <h2 id="dialog-title">Select Options</h2>
    
    <ejs-multiselect id="options" 
        dataSource="ViewBag.OptionList"
        created="onDialogMultiSelectCreated">
    </ejs-multiselect>
    
    <button id="close-btn">Close</button>
</div>

@section Scripts {
    <script>
        function onDialogMultiSelectCreated(args) {
            // Focus trap: keep focus within dialog
            var multiSelect = args.element;
            var closeBtn = document.getElementById('close-btn');
            
            multiSelect.addEventListener('keydown', function(e) {
                if (e.key === 'Tab' && e.shiftKey) {
                    closeBtn.focus();
                    e.preventDefault();
                }
            });
        }
    </script>
}
```

### Initial Focus

Set proper initial focus:

```csharp
public IActionResult Form()
{
    ViewBag.FirstFieldId = "languages"; // Set focus here
    return View();
}
```

```razor
@section Scripts {
    <script>
        window.addEventListener('load', function() {
            var firstField = document.getElementById('@ViewBag.FirstFieldId');
            if (firstField) {
                firstField.focus();
            }
        });
    </script>
}
```

## Testing

### Automated Accessibility Testing

Use tools to verify accessibility:

```razor
<ejs-multiselect id="test-multi" 
    dataSource="ViewBag.TestData">
</ejs-multiselect>

@section Scripts {
    <script>
        // Using axe DevTools
        if (typeof axe !== 'undefined') {
            axe.run('test-multi', function(results) {
                console.log('Accessibility Issues: ' + results.violations.length);
                results.violations.forEach(function(violation) {
                    console.error(violation);
                });
            });
        }
    </script>
}
```

### Manual Testing Checklist

- [ ] **Keyboard Navigation**
  - [ ] Tab enters the control
  - [ ] Arrow keys navigate items
  - [ ] Enter/Space selects items
  - [ ] Escape closes dropdown
  - [ ] Backspace removes chips

- [ ] **Screen Reader**
  - [ ] Label is announced
  - [ ] State (open/closed) is announced
  - [ ] Selected items are announced
  - [ ] Error messages are announced

- [ ] **Visual**
  - [ ] Focus indicator is visible (2px outline minimum)
  - [ ] Color is not the only differentiator
  - [ ] Text contrast ratio ≥ 4.5:1
  - [ ] Interactive elements are 44x44px minimum

- [ ] **Forms**
  - [ ] MultiSelect is associated with label
  - [ ] Required state is indicated
  - [ ] Validation errors are linked to field
  - [ ] Instructions are provided

### Accessibility Audit Example

```razor
<div id="accessibility-report"></div>

@section Scripts {
    <script>
        async function auditAccessibility() {
            var multiObj = document.getElementById('languages').ej2_instances[0];
            var issues = [];
            
            // Check label
            if (!document.querySelector('label[for="languages"]')) {
                issues.push('MultiSelect lacks associated label');
            }
            
            // Check focus indicator
            var styles = window.getComputedStyle(multiObj.element.parentElement);
            if (styles.outline === 'none') {
                issues.push('No focus indicator defined');
            }
            
            // Check ARIA attributes
            if (!multiObj.element.hasAttribute('aria-label') && 
                !document.querySelector('label[for="languages"]')) {
                issues.push('No accessible name');
            }
            
            // Display report
            var report = document.getElementById('accessibility-report');
            if (issues.length === 0) {
                report.innerHTML = '<p style="color: green;">✓ Accessibility check passed</p>';
            } else {
                report.innerHTML = '<p style="color: red;">Issues found:</p><ul>' +
                    issues.map(i => '<li>' + i + '</li>').join('') + '</ul>';
            }
        }
        
        auditAccessibility();
    </script>
}
```

## Complete Accessible Example

```razor
@addTagHelper *, Syncfusion.EJ2

<form method="post" id="settings-form">
    <fieldset>
        <legend>User Preferences</legend>
        
        <div class="form-group">
            <label for="languages">
                Preferred Languages <span aria-label="required">*</span>
            </label>
            <ejs-multiselect id="languages"
                dataSource="ViewBag.LanguageList"
                placeholder="Select languages"
                aria-required="true"
                aria-describedby="languages-help"
                change="validateForm">
            </ejs-multiselect>
            <small id="languages-help">
                Select one or more languages
            </small>
            <span id="languages-error" role="alert" class="error-message"></span>
        </div>
        
        <div class="form-group">
            <label for="skills">
                Technical Skills <span aria-label="required">*</span>
            </label>
            <ejs-multiselect id="skills"
                dataSource="ViewBag.SkillsList"
                placeholder="Select your skills"
                aria-required="true"
                allowFiltering="true"
                change="validateForm">
            </ejs-multiselect>
            <span id="skills-error" role="alert" class="error-message"></span>
        </div>
        
        <button type="submit" class="btn btn-primary">Submit</button>
        <button type="reset" class="btn btn-secondary">Reset</button>
    </fieldset>
</form>

@section Scripts {
    <script>
        function validateForm() {
            var languagesMulti = document.getElementById('languages').ej2_instances[0];
            var skillsMulti = document.getElementById('skills').ej2_instances[0];
            var isValid = true;
            
            // Validate languages
            if (!languagesMulti.value || languagesMulti.value.length === 0) {
                var error = document.getElementById('languages-error');
                error.textContent = 'Please select at least one language';
                error.style.display = 'block';
                languagesMulti.element.setAttribute('aria-invalid', 'true');
                isValid = false;
            } else {
                document.getElementById('languages-error').style.display = 'none';
                languagesMulti.element.setAttribute('aria-invalid', 'false');
            }
            
            // Validate skills
            if (!skillsMulti.value || skillsMulti.value.length === 0) {
                var error = document.getElementById('skills-error');
                error.textContent = 'Please select at least one skill';
                error.style.display = 'block';
                skillsMulti.element.setAttribute('aria-invalid', 'true');
                isValid = false;
            } else {
                document.getElementById('skills-error').style.display = 'none';
                skillsMulti.element.setAttribute('aria-invalid', 'false');
            }
            
            return isValid;
        }
        
        // Initial focus on first field
        window.addEventListener('load', function() {
            document.getElementById('languages').focus();
        });
    </script>
}

<style>
    .form-group {
        margin-bottom: 20px;
    }
    
    label {
        display: block;
        margin-bottom: 5px;
        font-weight: bold;
    }
    
    [aria-required="true"]::after {
        content: "*";
        color: red;
        margin-left: 3px;
    }
    
    .error-message {
        display: none;
        color: #d32f2f;
        font-size: 12px;
        margin-top: 5px;
    }
    
    .e-multiselect:focus-within {
        outline: 3px solid #2196F3;
        outline-offset: 2px;
    }
</style>
```

## Best Practices Summary

### ✅ DO

- ✅ Always provide associated `<label>` elements
- ✅ Use semantic HTML with proper heading hierarchy
- ✅ Ensure keyboard navigation works for all features
- ✅ Include aria-label or aria-labelledby
- ✅ Test with screen readers (NVDA, JAWS)
- ✅ Maintain 4.5:1 color contrast ratio
- ✅ Provide clear focus indicators
- ✅ Handle errors with `role="alert"`
- ✅ Use proper ARIA attributes

### ❌ DON'T

- ❌ Rely on color alone to convey information
- ❌ Use placeholder text as a label substitute
- ❌ Disable outline/focus indicators
- ❌ Use tiny (< 44x44px) interactive elements
- ❌ Forget to validate server-side
- ❌ Hide error messages from screen readers
- ❌ Create custom keyboard shortcuts without documentation

## Next Steps

✅ **Learn More:**
- [Getting Started](getting-started.md) - Build your first accessible MultiSelect
- [Advanced Features](advanced-features.md) - Use accessible patterns
- Test with: [NVDA](https://www.nvaccess.org/), [JAWS](https://www.freedomscientific.com/), [axe DevTools](https://www.deque.com/axe/devtools/)
