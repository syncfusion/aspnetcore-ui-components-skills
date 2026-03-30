---
name: syncfusion-aspnetcore-image-editor
description: Comprehensive guide for implementing Syncfusion ASP.NET Core Image Editor. Use this when adding image editing capabilities, creating custom image tools, configuring annotations, applying filters, or handling image transformations. Covers relevant references for specific Image Editor features, customization patterns, and event handling implementations.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing the Syncfusion ASP.NET Core Image Editor

The Syncfusion ASP.NET Core Image Editor is a powerful control that enables users to edit, annotate, filter, and transform images directly in web applications. It provides a comprehensive set of features for image manipulation with a customizable toolbar and extensive API support.

## When to Use This Skill

Use this skill when you need to:

- **Add image editing capabilities** to your ASP.NET Core application
- **Configure image operations** like cropping, resizing, rotation, and transformation
- **Customize the toolbar** with specific tools and actions
- **Implement annotations** (text, shapes, freehand drawings) on images
- **Apply filters and fine-tuning** effects to enhance images
- **Handle image I/O** (open, save, export in multiple formats)
- **Manage image state** with undo/redo functionality
- **Integrate with dialogs** for modal editing workflows
- **Implement image restrictions** for file validation and security
- **Add accessibility features** and multi-language support

## Component Overview

The Image Editor control combines:

- **Visual editing tools** - Toolbar with 20+ built-in actions
- **Image manipulation** - Crop, resize, rotate, flip, straighten
- **Annotation system** - Text, shapes, paths, freehand drawings
- **Effects & filters** - 8 filter types, 7 fine-tuning options
- **Advanced features** - Frames, redaction, z-order management
- **Export capabilities** - PNG, JPEG, SVG, WEBP, BMP formats
- **Event system** - Comprehensive events for all operations
- **Keyboard support** - Shortcuts and accessibility compliance

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

- ASP.NET Core project setup
- NuGet package installation (Syncfusion.EJ2.AspNet.Core)
- Tag helper registration in _ViewImports.cshtml
- Stylesheet and script resource setup
- Basic component initialization
- Rendering in Razor pages

### Toolbar Configuration

📄 **Read:** [references/toolbar-customization.md](references/toolbar-customization.md)

- Built-in toolbar items (20 items: Open, Undo, Redo, Zoom, Crop, Transform, Annotate, Finetune, Filter, Frame, Resize, Redact, Reset, Save)
- Adding custom toolbar items
- Showing/hiding entire toolbar
- Hiding specific toolbar items
- Toolbar configuration best practices

### Image Loading & Viewing

📄 **Read:** [references/image-operations.md](references/image-operations.md)

- Opening images (JPEG, PNG, JPG, WEBP, BMP)
- Loading from file system
- Supported image formats
- Zooming options (toolbar, pinch, mouse wheel, keyboard shortcuts)
- Panning and navigation
- Canvas management

### Selection & Cropping

📄 **Read:** [references/selection-and-cropping.md](references/selection-and-cropping.md)

- Custom selection
- Preset shapes (square, circle)
- Aspect ratio selections (2:3, 3:2, 3:4, 4:3, 4:5, 5:4, 5:7, 7:5, 9:16, 16:9)
- Select method parameters and usage
- Crop functionality

### Image Transformation

📄 **Read:** [references/transform.md](references/transform.md)

- Rotation (multiples of 90°, any angle)
- Horizontal and vertical flipping
- Straightening (-45 to +45 degrees)
- Transform annotations alongside image

### Annotations & Markup

📄 **Read:** [references/annotations.md](references/annotations.md)

- Text annotations with font customization
- Shape annotations (rectangle, ellipse, arrow, path, line)
- Freehand drawing
- Annotation positioning, resizing, and rotation
- Color and style customization
- Quick access toolbar for selected annotations
- DrawText method comprehensive guide

### Filters & Visual Effects

📄 **Read:** [references/filters.md](references/filters.md)

- Filter types (default, cold, warm, chrome, sepia, invert, grayscale, black & white)
- Applying filters via applyImageFilter method
- Filter events and callbacks

### Fine-Tuning Adjustments

📄 **Read:** [references/fine-tuning.md](references/fine-tuning.md)

- Brightness, contrast, saturation adjustments
- Hue, exposure, blur, opacity control
- finetuneImage method and parameters
- Fine-tune value changing events
- Adjustment sliders and real-time preview

### Decorative Frames

📄 **Read:** [references/frames.md](references/frames.md)

- Frame types (mat, bevel, line, hook, inset)
- drawFrame method and parameters
- Frame customization (color, gradient, size, style)
- Frame line styles and border radius
- Frame changing events

### Image Redaction

📄 **Read:** [references/redact.md](references/redact.md)

- Redaction types (blur, pixelate)
- Drawing redactions with drawRedact method
- Selecting, updating, deleting redactions
- Getting redaction collection
- Privacy and data protection use cases

### Annotation Layering

📄 **Read:** [references/z-order.md](references/z-order.md)

- Z-order operations (bring forward, send backward, bring to front, send to back)
- Managing annotation layers
- Template design patterns

### Export & Save

📄 **Read:** [references/export-and-save.md](references/export-and-save.md)

- Supported export formats (PNG, JPEG, SVG, WEBP, BMP)
- Opening images (local, base64, URL, blob storage)
- toBlob method for saving
- JPEG quality settings
- getImageData for base64 conversion
- Download functionality
- Keyboard shortcuts (Ctrl+S)

### Undo/Redo History

📄 **Read:** [references/undo-redo.md](references/undo-redo.md)

- Undo/redo functionality (16-step history limit)
- Undo and redo methods
- Keyboard shortcuts (Ctrl+Z, Ctrl+Y)
- History management

### Quick Access Toolbar

📄 **Read:** [references/quick-access-toolbar.md](references/quick-access-toolbar.md)

- Contextual quick access toolbar for annotations
- Customizing toolbar items
- QuickAccessToolbarOpening event
- Adding custom actions
- showQuickAccessToolbar property

### Dialog Integration

📄 **Read:** [references/dialog-integration.md](references/dialog-integration.md)

- Rendering Image Editor in a modal dialog
- Dialog lifecycle management
- clearImage method for fresh state
- Dialog patterns and best practices

### Image Upload Restrictions

📄 **Read:** [references/image-restrictions.md](references/image-restrictions.md)

- File extension validation (AllowedExtensions)
- File size restrictions (MinFileSize, MaxFileSize)
- UploadSettings configuration
- Default supported formats
- Error handling and user feedback

### Localization & i18n

📄 **Read:** [references/localization.md](references/localization.md)

- Locale property configuration
- Translation object setup
- Supported cultures (Arabic, Deutsch, French, etc.)
- Complete list of 60+ UI strings
- Custom locale implementation

### Reset & Utility Methods

📄 **Read:** [references/reset-and-utilities.md](references/reset-and-utilities.md)

- Reset method for reverting all changes
- Difference between reset and clearImage
- Utility methods summary
- Common helper patterns

## Quick Start Example

Here's a minimal example of setting up the Image Editor in ASP.NET Core:

**CSHTML (Razor Page):**
```html
<ejs-imageeditor id="imageEditor"
    toolbar="@new List<string> { 
        'Open', 'Crop', 'RotateLeft', 'RotateRight', 
        'HorizontalFlip', 'VerticalFlip', 'Annotate', 
        'Finetune', 'Filter', 'Reset', 'Save' 
    }">
</ejs-imageeditor>
```

**JavaScript:**
```javascript
// Initialize the image editor
var imageEditor = document.getElementById('imageEditor').ej2_instances[0];

// Open an image
imageEditor.open('sample.png');

// Listen to save event
imageEditor.save = function() {
    console.log('Image saved');
};
```

## Common Patterns

### Pattern: Basic Image Editing Workflow

1. Initialize Image Editor with toolbar
2. User opens image via Open button
3. User applies operations (crop, filter, annotation, etc.)
4. User clicks Save to export
5. Application handles blob/download

### Pattern: Dialog-Based Editing

1. Create Image Editor inside a Dialog component
2. On dialog open, load image via `open()` method
3. User performs edits
4. On dialog close, clear image with `clearImage()` method
5. This prevents residual state on re-open

### Pattern: Custom Toolbar with Events

1. Define custom toolbar items in the `toolbar` property
2. Attach click handlers to toolbar items
3. Call Image Editor methods (crop, filter, etc.) based on user action
4. Listen to corresponding events for state changes

### Pattern: Annotation Management

1. Listen to annotation events
2. Use `getAnnotations()` to retrieve all annotations
3. Customize via Quick Access toolbar or programmatically
4. Export image with annotations included

## Key Props & Configuration

| Property | Type | Purpose |
|----------|------|---------|
| `toolbar` | List<string> | Configures visible toolbar items |
| `width` | string | Canvas width |
| `height` | string | Canvas height |
| `showQuickAccessToolbar` | bool | Show/hide quick access toolbar |
| `uploadSettings` | UploadSettings | File restrictions and validation |
| `locale` | string | Language/culture code |

## Events

- `ImageLoading` - Image is loading
- `ImageLoaded` - Image loaded successfully
- `ImageFiltering` - Filter applied
- `FinetuneValueChanging` - Fine-tune adjustment
- `FrameChanging` - Frame applied
- `Resizing` - Image resized
- `QuickAccessToolbarOpening` - Quick access toolbar opened

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| Ctrl+Z | Undo |
| Ctrl+Y | Redo |
| Ctrl+S | Save (same format) |
| Ctrl+Plus | Zoom in |
| Ctrl+Minus | Zoom out |

## Common Use Cases

1. **Photo Editing App** - Full-featured image editor with all tools
2. **E-commerce Product Images** - Crop, resize, and export for different platforms
3. **Social Media Content** - Add text, stickers, filters before sharing
4. **Document Scanning** - Crop scanned images, adjust rotation
5. **Privacy/Redaction Tool** - Blur sensitive information before sharing
6. **Template Design** - Use annotations and layering for greeting cards, posters
7. **Image Annotation** - Add markup (shapes, text) for collaboration

## Next Steps

1. **Start with Getting Started** - Set up the basic component
2. **Customize the Toolbar** - Show only needed tools
3. **Implement Image Operations** - Load, crop, resize images
4. **Add Annotations** - Enable user markup capabilities
5. **Configure Save/Export** - Choose output formats
6. **Add Restrictions** - Validate uploaded files
7. **Test Events** - Handle user interactions

---

**For detailed implementation of any feature, refer to the specific reference documents listed in the Documentation and Navigation Guide section above.**
