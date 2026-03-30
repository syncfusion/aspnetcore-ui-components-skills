---
name: syncfusion-aspnetcore-dialog
description: Comprehensive guide to implement and customize the Syncfusion Dialog component using Tag Helper syntax in ASP.NET Core applications. Covers modal dialogs, positioning, animations, templates, accessibility, and event handling.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  platform: "ASP.NET Core"
  category: "Popups"
---

# Implementing Dialog — ASP.NET Core

This skill documents how to implement, configure, and customize the Syncfusion Dialog in ASP.NET Core applications using the `<ejs-dialog>` Tag Helper API. It covers setup, modality, positioning, animations, templates, events, accessibility, and styling.

## Quick Links
- [Getting Started](references/getting-started.md)
- [Basic Features](references/basic-features.md)
- [Positioning & Sizing](references/positioning-and-sizing.md)
- [Animations & Transitions](references/animations-and-transitions.md)
- [Templates](references/templates.md)
- [Accessibility & Localization](references/accessibility-and-localization.md)
- [Events & Interactions](references/events-and-interactions.md)
- [Styling & Themes](references/styling-and-themes.md)

## When To Use
- You need a versatile modal or non-modal dialog for confirmations, alerts, or content display.
- You want server-side configuration via Tag Helper syntax in ASP.NET Core.
- You need animations, custom templates, or advanced positioning control.
- You require accessibility compliance and multi-language support.

## Quick Start (Tag Helper)

In your Razor view:

```csharp
@using Syncfusion.EJ2

<ejs-dialog id="dialog" header="Welcome" is-modal="true" width="500px">
    <e-content-template>
        <p>This is a basic dialog with default settings.</p>
    </e-content-template>
</ejs-dialog>

<script>
    var dialogObj = document.getElementById('dialog').ej2_instances[0];
    document.getElementById('openBtn').onclick = function() {
        dialogObj.show();
    };
</script>
```

Ensure you include the script manager and styles in your layout (`_Layout.cshtml`):

```csharp
<!-- In <head> -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/fluent.css" />

<!-- At end of <body> -->
<script src="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/dist/ej2.min.js"></script>
<ejs-scripts></ejs-scripts>
```

## Common Patterns

### Modal Confirmation Dialog
```csharp
<ejs-dialog id="confirmDialog" header="Confirm Action" is-modal="true" width="400px">
    <e-dialog-buttons>
        <e-dialog-button content="Yes" is-primary="true" on-click="confirmAction"></e-dialog-button>
        <e-dialog-button content="No" on-click="closeDialog"></e-dialog-button>
    </e-dialog-buttons>
    <e-content-template>
        <p>Are you sure you want to proceed?</p>
    </e-content-template>
</ejs-dialog>
```

### Draggable Dialog with Header
```csharp
<ejs-dialog id="draggableDialog" 
    header="Drag Me" 
    allow-dragging="true" 
    width="500px" 
    show-close-icon="true">
    <e-content-template>
        <p>Grab the header to drag this dialog.</p>
    </e-content-template>
</ejs-dialog>
```

### Centered Dialog
```csharp
<ejs-dialog id="centeredDialog" 
    header="Centered Position" 
    width="500px"
    position-x="center" 
    position-y="center">
    <e-content-template>
        <p>This dialog is centered on the page.</p>
    </e-content-template>
</ejs-dialog>
```

## Key Properties

| Property | Type | Purpose | Example |
|----------|------|---------|---------|
| `header` | string | Dialog title bar | `header="Confirm Delete"` |
| `is-modal` | bool | Shows overlay behind dialog | `is-modal="true"` |
| `width` | string | Dialog width (px, %) | `width="500px"` |
| `height` | string | Dialog height (px, %) | `height="300px"` |
| `allow-dragging` | bool | Enable drag to reposition | `allow-dragging="true"` |
| `show-close-icon` | bool | Display close button | `show-close-icon="true"` |
| `position-x` | string | Horizontal position (left, center, right, offset) | `position-x="center"` |
| `position-y` | string | Vertical position (top, center, bottom, offset) | `position-y="center"` |
| `animation` | object | Animation settings | `animation-effect="Zoom"` |
| `is-hidden` | bool | Initially hidden state | `is-hidden="true"` |

## Next Steps
Read `getting-started.md` for full installation steps, NuGet package setup, and configuration details.

