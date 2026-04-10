---
name: syncfusion-aspnetcore-timepicker
description: >
  Implements the Syncfusion ASP.NET Core TimePicker component (EJ2). Use this skill when the user needs
  to add a time picker, time selection input, time field, or clock picker in an ASP.NET Core application.
  Trigger this skill for: TimePicker setup, time range configuration, strict mode, time masking, masked time input,
  globalization, RTL support, localization, accessibility, keyboard navigation, CSS customization,
  custom styling, form validation with TimePicker, TimePickerFor model binding, time format configuration,
  time step interval, floating label, clear button, full screen popup, popup behavior, scroll position,
  server timezone, and any scenario involving ejs-timepicker tag helper or Syncfusion EJ2 TimePicker.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Calendars"
---

# Syncfusion ASP.NET Core TimePicker

The **TimePicker** is an intuitive input component that allows users to select a time value from a popup list or type it directly. It supports time ranges, strict mode, masking, globalization, RTL, accessibility, and rich customization options.

## Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet package installation and setup
- Tag helper registration and script/style references
- Basic TimePicker rendering with `<ejs-timepicker>`
- Setting value, min, and max time
- Configuring time format and step interval
- TimePickerFor model binding

### Time Range
📄 **Read:** [references/time-range.md](references/time-range.md)
- Setting min and max time boundaries with `Min` and `Max`
- Behavior when value is out-of-range
- Two-TimePicker range selection (start/end time)
- Enabling/disabling end time based on start time selection
- Business hours selection pattern

### Strict Mode
📄 **Read:** [references/strict-mode.md](references/strict-mode.md)
- Enabling `StrictMode` to enforce valid time entry
- Clamping out-of-range values to min/max
- Default (non-strict) mode with error class indication
- Null behavior for invalid values in non-strict mode

### Globalization
📄 **Read:** [references/globalization.md](references/globalization.md)
- Setting culture with `Locale` property
- Loading CLDR data for non-English cultures
- L10n placeholder localization
- RTL support with `EnableRtl` for Arabic, Hebrew
- German and Arabic culture examples

### Time Masking
📄 **Read:** [references/time-masking.md](references/time-masking.md)
- Enabling masked input with `EnableMask`
- Mask pattern based on format string
- Custom mask placeholder with `MaskPlaceholder` (hour, minute, second)
- Keyboard navigation within masked segments

### Style and Appearance
📄 **Read:** [references/style-appearance.md](references/style-appearance.md)
- CSS customization using `CssClass` property
- Available CSS class targets (wrapper, icon, popup, list items)
- Customizing input, popup button, popup list, hover/active states
- Full-screen popup mode on mobile with `FullScreenMode`
- Wrapper height, font-size, background-color overrides

### Accessibility
📄 **Read:** [references/accessibility.md](references/accessibility.md)
- WCAG 2.2 AA compliance details
- WAI-ARIA attributes: aria-haspopup, aria-selected, aria-disabled, etc.
- Full keyboard interaction table
- Screen reader support
- RTL, color contrast, mobile device support

### How-To Guides
📄 **Read:** [references/how-to.md](references/how-to.md)
- Client-side form validation using FormValidator
- CSS customization with custom class names
- TimePickerFor rendering with model binding and form POST

### API Reference
📄 **Read:** [references/api.md](references/api.md)
- All properties with types, defaults, and descriptions
- All events (Change, Open, Close, Focus, Blur, Cleared, ItemRender, Created, Destroyed)
- CSS class reference table
- MaskPlaceholder sub-properties
- Complete tag helper attribute syntax

---

## Quick Start

**1. Install NuGet package:**
```
Install-Package Syncfusion.EJ2.AspNet.Core
```

**2. Register Tag Helper** in `~/Pages/_ViewImports.cshtml`:
```cshtml
@addTagHelper *, Syncfusion.EJ2
```

**3. Add CSS and script** in `~/Pages/Shared/_Layout.cshtml`:
```html
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/fluent.css" />
<script src="https://cdn.syncfusion.com/ej2/dist/ej2.min.js"></script>
```

**4. Add Script Manager** at end of `<body>`:
```html
<ejs-scripts></ejs-scripts>
```

**5. Render the TimePicker:**
```cshtml
<ejs-timepicker id="timepicker"></ejs-timepicker>
```

---

## Common Patterns

### TimePicker with Value, Min, and Max
```cshtml
@{
    var minVal = new DateTime(2022, 05, 07, 1, 00, 00);
    var maxVal = new DateTime(2022, 05, 07, 11, 00, 00);
    var value  = new DateTime(2022, 05, 07, 4, 00, 00);
}
<ejs-timepicker id="timepicker" value="value" min="minVal" max="maxVal"></ejs-timepicker>
```

### Custom Format and Step
```cshtml
@{
    var value = new DateTime(2022, 05, 07, 4, 00, 00);
}
<ejs-timepicker id="timepicker" format="HH:mm" step="60" value="value"></ejs-timepicker>
```

### Strict Mode
```cshtml
<ejs-timepicker id="timepicker"
    value="@ViewBag.value"
    min="@ViewBag.minVal"
    max="@ViewBag.maxVal"
    strictMode="true">
</ejs-timepicker>
```

### Masked Input
```cshtml
<ejs-timepicker id="timepicker" enableMask="true"></ejs-timepicker>
```

### RTL (Right-to-Left)
```cshtml
<ejs-timepicker id="timepicker" placeholder="Select a time" enableRtl="true"></ejs-timepicker>
```
