# Quick Access Toolbar for Annotations

## Overview

The quick access toolbar (contextual toolbar) appears when an annotation (shape, text) is selected, providing convenient shortcuts for common operations without navigating the main toolbar.

## Default Quick Access Items

When an annotation is selected, the quick access toolbar shows:

- **Duplicate** - Create a copy of the annotation
- **Delete** - Remove the annotation
- **Edit** (text only) - Edit text content
- **Color** - Change stroke/text color
- **Stroke Width** - Adjust line thickness
- **Font Options** (text only) - Font family, size, style

## Showing/Hiding Quick Access Toolbar

### Global Control

```html
<ejs-imageeditor id="imageEditor"
    showQuickAccessToolbar="true">
</ejs-imageeditor>
```

Set to `false` to disable the toolbar entirely.

## Customizing Quick Access Toolbar

### Handle QuickAccessToolbarOpening Event

```html
<ejs-imageeditor id="imageEditor"
    quickAccessToolbarOpen="OnQuickAccessToolbarOpen">
</ejs-imageeditor>

<script>
function OnQuickAccessToolbarOpen(args) {
    // args.Item: The annotation being edited
    // args.ToolbarItems: Array of toolbar items
    
    // Add custom item
    var customItem = {
        text: 'Custom Action',
        tooltipText: 'My Custom Tool'
    };
    
    args.ToolbarItems.push(customItem);
    
    console.log('Quick access toolbar opened');
}
</script>
```

### Add Custom Toolbar Items

```javascript
function OnQuickAccessToolbarOpen(args) {
    // Keep default items
    // args.ToolbarItems = default items
    
    // Add new items
    args.ToolbarItems.push({
        text: 'Mirror Horizontally',
        tooltipText: 'Flip annotation left-right',
        prefixIcon: 'e-icons e-flip-horizontal'
    });
    
    args.ToolbarItems.push({
        text: 'Rotate 90',
        tooltipText: 'Rotate annotation 90 degrees',
        prefixIcon: 'e-icons e-rotate-right'
    });
}
```

### Remove Toolbar Items

```javascript
function OnQuickAccessToolbarOpen(args) {
    // Remove Delete option
    args.ToolbarItems = args.ToolbarItems.filter(function(item) {
        return item.text !== 'Delete';
    });
}
```

### Conditional Toolbar Items

```javascript
function OnQuickAccessToolbarOpen(args) {
    // Different items based on annotation type
    var annotationType = args.Item.Type;
    
    if (annotationType === 'Text') {
        // Add text-specific options
        args.ToolbarItems.push({
            text: 'Change Case',
            tooltipText: 'Toggle uppercase/lowercase'
        });
    } else if (annotationType === 'Arrow') {
        // Add shape-specific options
        args.ToolbarItems.push({
            text: 'Swap Direction',
            tooltipText: 'Reverse arrow direction'
        });
    }
}
```

## Practical Examples

### Enhanced Annotation Toolbar

```html
<ejs-imageeditor id="imageEditor"
    quickAccessToolbarOpen="EnhanceToolbar">
</ejs-imageeditor>

<script>
function EnhanceToolbar(args) {
    // Add layer management
    args.ToolbarItems.push({
        text: 'Bring Forward',
        tooltipText: 'Move forward in layers',
        prefixIcon: 'e-icons e-arrow-up'
    });
    
    args.ToolbarItems.push({
        text: 'Send Backward',
        tooltipText: 'Move backward in layers',
        prefixIcon: 'e-icons e-arrow-down'
    });
    
    // Add animation option
    args.ToolbarItems.push({
        text: 'Animate',
        tooltipText: 'Add animation effect',
        prefixIcon: 'e-icons e-play'
    });
}
</script>
```

### Text Annotation Toolbar

```javascript
function OnTextAnnotationToolbar(args) {
    // Only add for text annotations
    if (args.Item.Type !== 'Text') return;
    
    // Add text formatting
    args.ToolbarItems.push({
        text: 'Bold',
        tooltipText: 'Make text bold',
        prefixIcon: 'e-icons e-bold'
    });
    
    args.ToolbarItems.push({
        text: 'Italic',
        tooltipText: 'Make text italic',
        prefixIcon: 'e-icons e-italic'
    });
    
    args.ToolbarItems.push({
        text: 'Underline',
        tooltipText: 'Underline text',
        prefixIcon: 'e-icons e-underline'
    });
}
```

### Shape Annotation Toolbar

```javascript
function OnShapeAnnotationToolbar(args) {
    // Shape-specific toolbar
    if (!['Rectangle', 'Ellipse', 'Arrow', 'Line'].includes(args.Item.Type)) return;
    
    // Add fill option
    args.ToolbarItems.push({
        text: 'Fill Shape',
        tooltipText: 'Add fill color to shape',
        prefixIcon: 'e-icons e-paint-bucket'
    });
    
    // Add shadow option
    args.ToolbarItems.push({
        text: 'Add Shadow',
        tooltipText: 'Add shadow effect',
        prefixIcon: 'e-icons e-shadow'
    });
}
```

### Minimal Toolbar

```javascript
function MinimalToolbar(args) {
    // Keep only essential items
    args.ToolbarItems = args.ToolbarItems.filter(function(item) {
        return ['Delete', 'Duplicate'].includes(item.text);
    });
}
```

## Quick Access Item Properties

Each toolbar item can have:

```javascript
{
    text: 'Action Name',                    // Display text
    tooltipText: 'Hover description',      // Tooltip
    prefixIcon: 'e-icons e-icon-name',    // Icon class
    width: '80px',                          // Item width
    align: 'Left',                          // Alignment
}
```

## Handling Toolbar Clicks

### Track Toolbar Interactions

```html
<ejs-imageeditor id="imageEditor"
    quickAccessToolbarItemClick="OnToolbarItemClick">
</ejs-imageeditor>

<script>
function OnToolbarItemClick(args) {
    var itemName = args.Item?.Text || 'Unknown';
    
    console.log('Toolbar item clicked:', itemName);
    
    if (itemName === 'Duplicate') {
        console.log('User duplicated annotation');
    } else if (itemName === 'Delete') {
        console.log('User deleted annotation');
    }
}
</script>
```

## Best Practices

### 1. **Keep Toolbar Uncluttered**

```javascript
// ❌ Too many items (overwhelms users)
function OverloadedToolbar(args) {
    args.ToolbarItems.push(item1);
    args.ToolbarItems.push(item2);
    args.ToolbarItems.push(item3);
    args.ToolbarItems.push(item4);
    args.ToolbarItems.push(item5);
    // ... 10 more items
}

// ✅ Focused items (essential only)
function FocusedToolbar(args) {
    args.ToolbarItems.push(duplicateItem);
    args.ToolbarItems.push(deleteItem);
    args.ToolbarItems.push(layerItem);
}
```

### 2. **Use Appropriate Icons**

```javascript
{
    text: 'Delete',
    prefixIcon: 'e-icons e-delete',      // Clear icon meaning
    tooltipText: 'Remove this annotation'
}
```

### 3. **Group Related Items**

```javascript
function OrganizedToolbar(args) {
    // Edit items
    args.ToolbarItems.push(editItem);
    args.ToolbarItems.push(deleteItem);
    
    // Separator (if supported)
    
    // Layer items
    args.ToolbarItems.push(bringForwardItem);
    args.ToolbarItems.push(sendBackwardItem);
}
```

### 4. **Test with Different Annotation Types**

```javascript
function DynamicToolbar(args) {
    switch(args.Item.Type) {
        case 'Text':
            // Add text-specific items
            break;
        case 'Rectangle':
        case 'Ellipse':
            // Add shape-specific items
            break;
        case 'Arrow':
            // Add arrow-specific items
            break;
    }
}
```

## Common Toolbar Patterns

### Complete Editing Toolbar

```javascript
function CompleteToolbar(args) {
    // Selection
    args.ToolbarItems.push({
        text: 'Duplicate',
        tooltipText: 'Duplicate this element'
    });
    args.ToolbarItems.push({
        text: 'Delete',
        tooltipText: 'Delete this element'
    });
    
    // Organization
    args.ToolbarItems.push({
        text: 'Bring Forward',
        tooltipText: 'Move in front'
    });
    args.ToolbarItems.push({
        text: 'Send Backward',
        tooltipText: 'Move behind'
    });
    
    // Styling (for shapes)
    if (args.Item.Type !== 'Text') {
        args.ToolbarItems.push({
            text: 'Line Width',
            tooltipText: 'Adjust line thickness'
        });
    }
}
```

### Collaboration Toolbar

```javascript
function CollaborationToolbar(args) {
    // Add comment
    args.ToolbarItems.push({
        text: 'Add Comment',
        tooltipText: 'Add comment to this element'
    });
    
    // Link sharing
    args.ToolbarItems.push({
        text: 'Share',
        tooltipText: 'Share this annotation'
    });
    
    // Edit history
    args.ToolbarItems.push({
        text: 'History',
        tooltipText: 'View edit history'
    });
}
```

## Troubleshooting

**Issue:** Quick access toolbar not appearing
- **Solution:** Set `showQuickAccessToolbar="true"`
- **Solution:** Click annotation to select it first

**Issue:** Custom items not showing
- **Solution:** Ensure items are added to `args.ToolbarItems` array
- **Solution:** Check event handler is properly wired

**Issue:** Toolbar items not responding to clicks
- **Solution:** Implement `quickAccessToolbarItemClick` event
- **Solution:** Add proper item text values

**Issue:** Toolbar too wide for canvas
- **Solution:** Reduce number of items
- **Solution:** Use icons without text
- **Solution:** Implement dropdown menu for additional items
