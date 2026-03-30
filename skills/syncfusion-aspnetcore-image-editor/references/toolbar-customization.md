# Toolbar Customization in Image Editor

## Table of Contents
- [Built-in Toolbar Items](#built-in-toolbar-items)
- [Add Custom Toolbar Items](#add-custom-toolbar-items)
- [Show or Hide Toolbar](#show-or-hide-toolbar)
- [Show or Hide Toolbar Items](#show-or-hide-toolbar-items)
- [Default Toolbar Configuration](#default-toolbar-configuration)

## Built-in Toolbar Items

The Image Editor provides 20 built-in toolbar items that can be customized. Here's the complete list:

| Item | Description |
|------|-------------|
| `Open` | Open/load an image file |
| `Undo` | Undo the last action |
| `Redo` | Redo the undone action |
| `ZoomIn` | Zoom in on the image |
| `ZoomOut` | Zoom out from the image |
| `Crop` | Crop/select region of image |
| `RotateLeft` | Rotate image counter-clockwise |
| `RotateRight` | Rotate image clockwise |
| `HorizontalFlip` | Flip image horizontally |
| `VerticalFlip` | Flip image vertically |
| `Straightening` | Straighten image (-45 to +45°) |
| `Annotate` | Add annotations (text, shapes) |
| `Finetune` | Fine-tune image (brightness, contrast, etc.) |
| `Filter` | Apply filters to image |
| `Frame` | Add decorative frames |
| `Resize` | Resize the image |
| `Redact` | Apply redaction (blur, pixelate) |
| `Reset` | Reset all changes to original |
| `Save` | Save/export the edited image |

All items are enabled by default. You control visibility through the `toolbar` property.

## Add Custom Toolbar Items

The `toolbar` property in the Image Editor allows you to add, remove, or rearrange toolbar items to create a personalized editing experience.

### Basic Toolbar Configuration

```html
<ejs-imageeditor id="imageEditor" 
    toolbar="@new List<string> { 
        'Open', 'Undo', 'Redo', 
        'ZoomIn', 'ZoomOut', 
        'Crop', 'RotateLeft', 'RotateRight',
        'Reset', 'Save' 
    }">
</ejs-imageeditor>
```

This example shows only the most essential tools for basic editing.

### Custom Toolbar with Event Handlers

```html
<ejs-imageeditor id="imageEditor" 
    toolbar="@new List<string> { 
        'Open', 'Crop', 'RotateLeft', 'RotateRight',
        'HorizontalFlip', 'VerticalFlip', 
        'Straightening', 'Reset', 'Save' 
    }"
    toolbarUpdating="@new Syncfusion.EJ2.Base.EmitType<Syncfusion.EJ2.ImageEditor.ToolbarEventArgs>(HandleToolbarClick)">
</ejs-imageeditor>
```

**C# Code-Behind:**
```csharp
public class IndexModel : PageModel
{
    public void HandleToolbarClick(ToolbarEventArgs args)
    {
        // Custom logic for toolbar item clicks
        var itemName = args.Item?.Name;
        
        switch(itemName)
        {
            case "save":
                // Handle custom save logic
                break;
            case "reset":
                // Handle custom reset logic
                break;
        }
    }
}
```

### Full-Featured Toolbar

For a complete image editor with all tools:

```html
<ejs-imageeditor id="imageEditor" 
    toolbar="@new List<string> { 
        'Open', 'Undo', 'Redo', 
        'ZoomIn', 'ZoomOut', 
        'Crop', 'RotateLeft', 'RotateRight', 
        'HorizontalFlip', 'VerticalFlip', 
        'Straightening',
        'Annotate', 
        'Finetune', 
        'Filter', 
        'Frame', 
        'Resize', 
        'Redact', 
        'Reset', 
        'Save' 
    }">
</ejs-imageeditor>
```

## Show or Hide Toolbar

### Hide Entire Toolbar

To hide the toolbar completely, set the `toolbar` property to an empty list:

```html
<ejs-imageeditor id="imageEditor" 
    toolbar="@new List<string> { }">
</ejs-imageeditor>
```

This creates an Image Editor with just the canvas, useful for:
- Read-only image viewers
- Programmatic control via JavaScript API
- Custom UI implementations

### Show Toolbar

When the toolbar property contains items, the toolbar is displayed with those items:

```html
<ejs-imageeditor id="imageEditor" 
    toolbar="@new List<string> { 'Open', 'Save' }">
</ejs-imageeditor>
```

## Show or Hide Toolbar Items

### Hide Specific Toolbar Items

Only include the items you want to show in the `toolbar` list. Omitted items are hidden:

```html
<ejs-imageeditor id="imageEditor" 
    toolbar="@new List<string> { 
        'Open', 'Undo', 'Redo',
        'Crop', 'RotateLeft', 'RotateRight',
        'Reset', 'Save'
    }">
</ejs-imageeditor>
```

This hides: ZoomIn, ZoomOut, HorizontalFlip, VerticalFlip, Straightening, Annotate, Finetune, Filter, Frame, Resize, Redact.

### Conditional Toolbar Based on User Role

**C# Code-Behind:**
```csharp
public List<string> GetToolbarForUser(string userRole)
{
    var toolbar = new List<string> { "Open" };
    
    if (userRole == "Editor" || userRole == "Admin")
    {
        toolbar.AddRange(new[] { 
            "Crop", "RotateLeft", "RotateRight",
            "Annotate", "Finetune", "Filter"
        });
    }
    
    if (userRole == "Admin")
    {
        toolbar.AddRange(new[] { "Redact", "Frame", "Resize" });
    }
    
    toolbar.AddRange(new[] { "Reset", "Save" });
    return toolbar;
}
```

**Razor Page:**
```html
<ejs-imageeditor id="imageEditor" 
    toolbar="@GetToolbarForUser(User.FindFirst(\"role\")?.Value)">
</ejs-imageeditor>
```

### Lightweight Toolbar for Mobile

```html
<ejs-imageeditor id="imageEditor" 
    toolbar="@new List<string> { 
        'Open', 'Crop', 'RotateLeft', 'RotateRight',
        'Undo', 'Redo', 'Save' 
    }">
</ejs-imageeditor>
```

This simplified toolbar works well on mobile devices with limited screen space.

## Default Toolbar Configuration

When no toolbar property is specified, all 20 built-in items are shown in this order:

```
Open | Undo Redo | ZoomIn ZoomOut | 
Crop | RotateLeft RotateRight | HorizontalFlip VerticalFlip | Straightening |
Annotate | Finetune | Filter | Frame | Resize | Redact | 
Reset | Save
```

## Best Practices

### 1. **Show Only Necessary Tools**

Don't overwhelm users with all 20 tools. Include only what they need:

```html
<!-- ❌ Too many options -->
<ejs-imageeditor toolbar="@AllToolbarItems">
</ejs-imageeditor>

<!-- ✅ Focused set -->
<ejs-imageeditor toolbar="@new List<string> { 
    'Open', 'Crop', 'Annotate', 'Save' 
}">
</ejs-imageeditor>
```

### 2. **Group Related Operations**

Arrange toolbar items logically:

```html
<ejs-imageeditor toolbar="@new List<string> { 
    // Loading
    'Open',
    // History
    'Undo', 'Redo',
    // Navigation
    'ZoomIn', 'ZoomOut',
    // Transformations
    'Crop', 'RotateLeft', 'RotateRight', 'HorizontalFlip', 'VerticalFlip',
    // Effects
    'Finetune', 'Filter',
    // Annotations
    'Annotate',
    // Export
    'Reset', 'Save'
}">
</ejs-imageeditor>
```

### 3. **Provide Keyboard Shortcuts**

Even with hidden toolbar items, users can still access functions via API or keyboard shortcuts. Document this:

```javascript
// Users can still undo even if Undo button is hidden
imageEditor.undo(); // Programmatically undo
// Or use Ctrl+Z keyboard shortcut
```

### 4. **Test on Different Screen Sizes**

Ensure toolbar items fit on mobile screens. Consider responsive toolbar hiding:

```html
<ejs-imageeditor id="imageEditor" 
    toolbar="@GetResponsiveToolbar(ViewBag.IsMobile)">
</ejs-imageeditor>
```

## JavaScript API for Toolbar

You can programmatically interact with toolbar items:

```javascript
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Programmatic operations
imageEditor.crop();
imageEditor.rotate(90);
imageEditor.filter('Sepia');
imageEditor.reset();
imageEditor.save();
```

## Events Related to Toolbar

### Toolbar Click Event

Handle toolbar item clicks:

```html
<ejs-imageeditor id="imageEditor"
    toolbarUpdating="OnToolbarClick">
</ejs-imageeditor>
```

```javascript
function OnToolbarClick(args) {
    console.log('Toolbar item clicked:', args.Item);
}
```

## Common Toolbar Configurations

### Photo Editing Suite
```csharp
var photoEditingToolbar = new List<string> { 
    "Open", "Undo", "Redo",
    "ZoomIn", "ZoomOut",
    "Crop", "RotateLeft", "RotateRight",
    "Finetune", "Filter", "Frame",
    "Reset", "Save"
};
```

### Document Annotation
```csharp
var documentToolbar = new List<string> { 
    "Open", "Undo", "Redo",
    "Crop", "RotateLeft", "RotateRight",
    "Annotate", "Redact",
    "Reset", "Save"
};
```

### Social Media Content Creator
```csharp
var socialMediaToolbar = new List<string> { 
    "Open", "Crop",
    "RotateLeft", "RotateRight",
    "Annotate", "Filter", "Frame",
    "Undo", "Redo",
    "Save"
};
```

## Troubleshooting

**Issue:** Toolbar items not appearing
- **Solution:** Verify toolbar list contains valid item names (exact case matters)
- **Solution:** Check that CSS is properly loaded

**Issue:** Toolbar taking too much space
- **Solution:** Reduce number of items in toolbar list
- **Solution:** Adjust ImageEditor width or height

**Issue:** Click handlers not triggering
- **Solution:** Ensure toolbarUpdating event is properly wired
- **Solution:** Check browser console for JavaScript errors
