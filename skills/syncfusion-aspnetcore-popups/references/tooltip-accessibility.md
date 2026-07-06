# Accessibility — Syncfusion ASP.NET Core Tooltip

## Table of Contents
- [Overview](#overview)
- [WCAG 2.2 Compliance](#wcag-22-compliance)
- [ARIA Attributes](#aria-attributes)
- [Keyboard Navigation](#keyboard-navigation)
- [Screen Reader Support](#screen-reader-support)
- [Focus Management](#focus-management)
- [Best Practices](#best-practices)

---

## Overview

The Syncfusion Tooltip component is built with accessibility in mind and complies with **WCAG 2.2** and **Section 508** standards. This ensures tooltips are usable by people with disabilities using assistive technologies like screen readers, keyboard navigation, and voice control.

---

## WCAG 2.2 Compliance

The tooltip component meets WCAG 2.2 Level AA standards:

- **Perceivable:** Content is visible and distinguishable
- **Operable:** All functionality is keyboard-accessible
- **Understandable:** Content is clear and comprehensible
- **Robust:** Compatible with assistive technologies

---

## ARIA Attributes

The tooltip component automatically manages ARIA attributes:

| Attribute | Value | Purpose |
|-----------|-------|---------|
| `role` | `"tooltip"` | Identifies the element as a tooltip |
| `aria-describedby` | `{tooltipId}` | Links target element to its tooltip |
| `aria-hidden` | `"true"` / `"false"` | Indicates tooltip visibility to screen readers |
| `aria-live` | `"polite"` | Announces dynamic content updates |
| `aria-label` | `{content}` | Provides tooltip text as fallback |

```csharp
@* The component automatically adds ARIA attributes *@
<ejs-tooltip content="Save your work" position="TopCenter">
    <button class="e-btn" aria-label="Save">Save</button>
</ejs-tooltip>

@* Resulting HTML includes: *@
@* <div role="tooltip" aria-describedby="..." aria-hidden="...">
       Save your work
    </div> *@
```

---

## Keyboard Navigation

All tooltip interactions are keyboard-accessible:

| Key | Action |
|-----|--------|
| `Tab` | Navigate to focusable target elements |
| `Enter` | Open tooltip on Focus mode (`opensOn="Focus"`) |
| `Escape` | Close tooltip (if `opensOn="Custom"` or sticky mode) |
| `Space` | Open/close on Click mode (`opensOn="Click"`) |

```csharp
@* Focus mode - opens with Tab/Enter *@
<ejs-tooltip content="Focus this input" 
    opensOn="Focus" 
    position="TopCenter">
    <input type="text" class="e-input" placeholder="Tab to focus" />
</ejs-tooltip>

@* Click mode - opens with Enter/Space *@
<ejs-tooltip content="Click to show" 
    opensOn="Click" 
    position="TopCenter">
    <button class="e-btn">Click Me</button>
</ejs-tooltip>

@* Custom mode with keyboard control *@
<ejs-tooltip id="tooltip" 
    content="Keyboard controlled" 
    opensOn="Custom" 
    position="TopCenter">
    <button class="e-btn" 
        onkeydown="handleKeyboard(event)">
        Press K to toggle
    </button>
</ejs-tooltip>

<script>
    function handleKeyboard(event) {
        if (event.key === 'k' || event.key === 'K') {
            var tooltipObj = document.getElementById('tooltip').ej2_instances[0];
            if (event.target.getAttribute('data-open') === 'true') {
                tooltipObj.close();
                event.target.setAttribute('data-open', 'false');
            } else {
                tooltipObj.open(event.target);
                event.target.setAttribute('data-open', 'true');
            }
        }
    }
</script>
```

---

## Screen Reader Support

Screen readers announce tooltip content when elements receive focus or when the tooltip opens:

```csharp
@* Screen readers announce the tooltip content *@
<ejs-tooltip 
    content="Required field - enter your email address" 
    opensOn="Focus"
    position="TopCenter">
    <input type="email" 
        class="e-input" 
        placeholder="Email" 
        aria-required="true"
        aria-describedby="email-tooltip" />
</ejs-tooltip>

@* For custom content, ensure alt text is provided *@
<ejs-tooltip opensOn="Hover" position="TopCenter">
    <button class="e-btn" aria-label="Help - Click for more information">
        Help <i class="e-icons e-help" aria-hidden="true"></i>
    </button>
    <e-content-template>
        <div>
            <p>This action will delete the selected items permanently.</p>
            <p>You cannot undo this action.</p>
        </div>
    </e-content-template>
</ejs-tooltip>
```

---

## Focus Management

Proper focus management ensures keyboard users can navigate efficiently:

```csharp
@* Multi-target tooltip with proper focus management *@
<ejs-tooltip target=".form-input" position="RightCenter" opensOn="Focus">
    <form>
        <div>
            <label for="name">Name:</label>
            <input id="name" 
                class="e-input form-input" 
                type="text" 
                title="Enter your full name" />
        </div>
        
        <div>
            <label for="email">Email:</label>
            <input id="email" 
                class="e-input form-input" 
                type="email" 
                title="Enter a valid email address" />
        </div>
        
        <div>
            <label for="password">Password:</label>
            <input id="password" 
                class="e-input form-input" 
                type="password" 
                title="At least 8 characters" />
        </div>
    </form>
</ejs-tooltip>
```

---

## Best Practices

### 1. **Provide Descriptive Content**
Ensure tooltips contain meaningful, concise information:

```csharp
<!-- ✅ Good: Clear, actionable -->
<ejs-tooltip content="Save changes to the document">
    <button class="e-btn">Save</button>
</ejs-tooltip>

<!-- ❌ Poor: Vague -->
<ejs-tooltip content="Click here">
    <button class="e-btn">Save</button>
</ejs-tooltip>
```

### 2. **Don't Duplicate Labels**
Don't repeat button text in the tooltip:

```csharp
<!-- ✅ Good: Tooltip provides additional info -->
<ejs-tooltip content="Save changes and continue editing">
    <button class="e-btn">Save</button>
</ejs-tooltip>

<!-- ❌ Poor: Redundant information -->
<ejs-tooltip content="Save">
    <button class="e-btn">Save</button>
</ejs-tooltip>
```

### 3. **Use Appropriate Trigger Modes**
- Use `Hover` for non-essential information
- Use `Focus` for form field help
- Use `Click` for important or complex information

```csharp
<ejs-tooltip content="Enter your username" opensOn="Focus">
    <input class="e-input" placeholder="Username" />
</ejs-tooltip>
```

### 4. **Don't Hide Critical Information**
Never place critical information only in tooltips. Make it available in the page itself.

```csharp
<!-- ✅ Good: Critical info visible, tooltip provides extra detail -->
<p>Password must be at least 8 characters.
    <ejs-tooltip content="Include uppercase, lowercase, and numbers for stronger security">
        <span class="e-info" tabindex="0">More info</span>
    </ejs-tooltip>
</p>

<!-- ❌ Poor: Critical info only in tooltip -->
<ejs-tooltip content="Password must be at least 8 characters">
    <input type="password" class="e-input" />
</ejs-tooltip>
```

### 5. **Test with Assistive Technologies**
- Test with screen readers (NVDA, JAWS, VoiceOver)
- Test keyboard navigation without a mouse
- Use browser accessibility inspector tools
