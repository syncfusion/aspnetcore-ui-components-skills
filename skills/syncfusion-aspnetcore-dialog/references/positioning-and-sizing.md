# Positioning & Sizing — Dialog (ASP.NET Core)

## Table of Contents
- [Position Basics](#position-basics)
- [Horizontal Positioning](#horizontal-positioning)
- [Vertical Positioning](#vertical-positioning)
- [Center Dialog](#center-dialog)
- [Custom Coordinates](#custom-coordinates)
- [Responsive Sizing](#responsive-sizing)
- [Viewport Constraints](#viewport-constraints)

## Position Basics

The Dialog's position is controlled by `position-x` and `position-y` attributes. These accept:
- **Keywords:** `left`, `center`, `right` (horizontal) and `top`, `center`, `bottom` (vertical)
- **Pixel values:** Any numeric offset (e.g., `"100px"`, `"50px"`)
- **Combined:** Keywords with offsets (e.g., `"center + 20px"`)

### Position Element

By default, Dialog positions relative to the body or target container:

```csharp
<div id="dialogTarget" style="width: 100%; height: 500px; border: 1px solid #ddd; position: relative;">
    <!-- Dialog will position relative to this element -->
    <ejs-dialog id="targetDialog" 
        header="Positioned in Container" 
        position-x="center" 
        position-y="center"
        width="500px">
        <e-content-template>
            <p>This dialog is centered within its target element.</p>
        </e-content-template>
    </ejs-dialog>
</div>
```

## Horizontal Positioning

### Left Alignment

Position dialog on the left side:

```csharp
<ejs-dialog id="leftDialog" 
    header="Left Aligned" 
    position-x="left" 
    position-y="top" 
    width="400px">
    <e-content-template>
        <p>Dialog aligned to the left side of the page.</p>
    </e-content-template>
</ejs-dialog>
```

### Right Alignment

Position dialog on the right side:

```csharp
<ejs-dialog id="rightDialog" 
    header="Right Aligned" 
    position-x="right" 
    position-y="top" 
    width="400px">
    <e-content-template>
        <p>Dialog aligned to the right side of the page.</p>
    </e-content-template>
</ejs-dialog>
```

### Center Horizontally

```csharp
<ejs-dialog id="centerHorizDialog" 
    header="Centered Horizontally" 
    position-x="center" 
    position-y="top" 
    width="500px">
    <e-content-template>
        <p>Dialog is centered horizontally with top alignment.</p>
    </e-content-template>
</ejs-dialog>
```

## Vertical Positioning

### Top Alignment

```csharp
<ejs-dialog id="topDialog" 
    header="Top Aligned" 
    position-x="center" 
    position-y="top" 
    width="500px">
    <e-content-template>
        <p>Dialog aligned to the top of the page.</p>
    </e-content-template>
</ejs-dialog>
```

### Bottom Alignment

```csharp
<ejs-dialog id="bottomDialog" 
    header="Bottom Aligned" 
    position-x="center" 
    position-y="bottom" 
    width="500px">
    <e-content-template>
        <p>Dialog aligned to the bottom of the page.</p>
    </e-content-template>
</ejs-dialog>
```

### Center Vertically

```csharp
<ejs-dialog id="centerVertDialog" 
    header="Centered Vertically" 
    position-x="left" 
    position-y="center" 
    width="400px">
    <e-content-template>
        <p>Dialog is centered vertically on the left side.</p>
    </e-content-template>
</ejs-dialog>
```

## Center Dialog

### Complete Center Position

Center the dialog both horizontally and vertically on the page:

```csharp
<ejs-dialog id="centeredDialog" 
    header="Perfectly Centered" 
    position-x="center" 
    position-y="center" 
    width="500px" 
    height="300px"
    is-modal="true">
    <e-content-template>
        <p style="text-align: center;">
            This dialog is centered in both axes on the page.
        </p>
    </e-content-template>
</ejs-dialog>

<button class="e-btn" onclick="showCenteredDialog()">Show Centered</button>

<script>
    function showCenteredDialog() {
        var dialog = document.getElementById('centeredDialog').ej2_instances[0];
        dialog.show();
    }
</script>
```

### Center on Scroll

Keep dialog centered when page scrolls:

```csharp
<ejs-dialog id="scrollCenterDialog" 
    header="Stays Centered" 
    position-x="center" 
    position-y="center" 
    width="500px"
    is-modal="true">
    <e-content-template>
        <p>This dialog remains centered even when the page scrolls.</p>
    </e-content-template>
</ejs-dialog>

<script>
    window.addEventListener('scroll', function() {
        var dialog = document.getElementById('scrollCenterDialog').ej2_instances[0];
        dialog.setProperties({positionX: 'center', positionY: 'center'});
    });
</script>
```

## Custom Coordinates

### Pixel Offset Positioning

Set absolute positions using pixel values:

```csharp
<ejs-dialog id="pixelDialog" 
    header="Custom Position" 
    position-x="100px" 
    position-y="150px" 
    width="400px">
    <e-content-template>
        <p>This dialog is positioned 100px from left and 150px from top.</p>
    </e-content-template>
</ejs-dialog>
```

### Offset from Keyword Positions

Combine keywords with pixel offsets:

```csharp
<ejs-dialog id="offsetDialog" 
    header="Offset Position" 
    position-x="center - 50px" 
    position-y="center + 20px" 
    width="500px">
    <e-content-template>
        <p>Dialog is centered minus 50px horizontally, plus 20px vertically.</p>
    </e-content-template>
</ejs-dialog>
```

### Dynamic Positioning via JavaScript

Change position at runtime:

```csharp
<ejs-dialog id="dynamicPositionDialog" 
    header="Dynamic Position" 
    width="400px">
    <e-content-template>
        <p>Position can be changed dynamically.</p>
    </e-content-template>
</ejs-dialog>

<button class="e-btn" onclick="repositionDialog('top-left')">Top Left</button>
<button class="e-btn" onclick="repositionDialog('center')">Center</button>
<button class="e-btn" onclick="repositionDialog('bottom-right')">Bottom Right</button>

<script>
    function repositionDialog(position) {
        var dialog = document.getElementById('dynamicPositionDialog').ej2_instances[0];
        
        switch(position) {
            case 'top-left':
                dialog.setProperties({positionX: 'left', positionY: 'top'});
                break;
            case 'center':
                dialog.setProperties({positionX: 'center', positionY: 'center'});
                break;
            case 'bottom-right':
                dialog.setProperties({positionX: 'right', positionY: 'bottom'});
                break;
        }
    }
</script>
```

## Responsive Sizing

### Percentage-Based Width/Height

Create responsive dialogs that scale with viewport:

```csharp
<ejs-dialog id="responsiveDialog" 
    header="Responsive Dialog" 
    width="90%" 
    height="80%" 
    position-x="center" 
    position-y="center">
    <e-content-template>
        <p>This dialog takes up 90% of viewport width and 80% of height.</p>
        <p>It will resize when the window is resized.</p>
    </e-content-template>
</ejs-dialog>
```

### Media Query Responsive

Adjust dialog size based on screen size:

```csharp
<ejs-dialog id="mediaQueryDialog" 
    header="Screen-Aware Dialog" 
    width="600px" 
    height="400px" 
    position-x="center" 
    position-y="center">
    <e-content-template>
        <p>This dialog is 600x400 on desktop.</p>
    </e-content-template>
</ejs-dialog>

<script>
    function adjustDialogForScreen() {
        var dialog = document.getElementById('mediaQueryDialog').ej2_instances[0];
        
        if (window.innerWidth < 768) {
            // Mobile: smaller dialog
            dialog.setProperties({width: '95%', height: '90%'});
        } else if (window.innerWidth < 1024) {
            // Tablet: medium dialog
            dialog.setProperties({width: '75%', height: '70%'});
        } else {
            // Desktop: normal size
            dialog.setProperties({width: '600px', height: '400px'});
        }
    }
    
    window.addEventListener('resize', adjustDialogForScreen);
    adjustDialogForScreen();
</script>
```

## Viewport Constraints

### Stay Within Viewport

Ensure dialog doesn't overflow the viewport:

```csharp
<ejs-dialog id="constrainedDialog" 
    header="Viewport Constrained" 
    width="800px" 
    position-x="center" 
    position-y="center">
    <e-content-template>
        <p>This dialog is constrained within the viewport boundaries.</p>
    </e-content-template>
</ejs-dialog>
```

### Target Container Position

Position dialog within a specific container (not body):

```csharp
<div id="containerDiv" style="width: 1000px; height: 600px; border: 1px solid #ccc; margin: 50px auto; position: relative;">
    <ejs-dialog id="containerDialog" 
        header="In Container" 
        position-x="center" 
        position-y="center" 
        width="400px"
        target="#containerDiv">
        <e-content-template>
            <p>This dialog positions within the container, not the page body.</p>
        </e-content-template>
    </ejs-dialog>
</div>
```

## Sizing Examples

### Small Dialog (Alert)

```csharp
<ejs-dialog id="smallDialog" 
    header="Alert" 
    width="300px" 
    position-x="center" 
    position-y="center">
    <e-dialog-buttons>
        <e-dialog-button content="OK" is-primary="true" on-click="onOk"></e-dialog-button>
    </e-dialog-buttons>
    <e-content-template>
        <p>Simple alert message.</p>
    </e-content-template>
</ejs-dialog>
```

### Medium Dialog (Confirmation)

```csharp
<ejs-dialog id="mediumDialog" 
    header="Confirm Action" 
    width="450px" 
    height="250px" 
    position-x="center" 
    position-y="center">
    <e-dialog-buttons>
        <e-dialog-button content="Confirm" is-primary="true" on-click="onConfirm"></e-dialog-button>
        <e-dialog-button content="Cancel" on-click="onCancel"></e-dialog-button>
    </e-dialog-buttons>
    <e-content-template>
        <p>Detailed confirmation message goes here.</p>
    </e-content-template>
</ejs-dialog>
```

### Large Dialog (Full Form)

```csharp
<ejs-dialog id="largeDialog" 
    header="User Registration" 
    width="700px" 
    height="600px" 
    position-x="center" 
    position-y="center"
    allow-resizing="true">
    <e-content-template>
        <form>
            <!-- Form fields here -->
        </form>
    </e-content-template>
</ejs-dialog>
```

## Edge Cases

### Dialog Overflow
If content is larger than dialog bounds, add scroll:

```css
/* In your CSS */
.e-dialog-content {
    overflow: auto;
    max-height: calc(100% - 100px);
}
```

### Viewport Too Small
Dialog may exceed viewport on mobile. Always test responsiveness and provide fallbacks.

