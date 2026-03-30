# Annotations in Image Editor

## Table of Contents
- [Overview](#overview)
- [Eight Annotation Types](#eight-annotation-types)
- [Text Annotations](#text-annotations)
- [Shape Annotations](#shape-annotations)
- [Freehand Drawing](#freehand-drawing)
- [Annotation Management](#annotation-management)
- [Practical Examples](#practical-examples)

## Overview

Annotations allow users to add markup to images. All annotations can be:
- Repositioned and resized
- Rotated (except text)
- Customized via contextual toolbar
- Included in exported/saved image
- Tracked in undo/redo history

## Eight Annotation Types

The Image Editor supports 8 core annotation methods:

1. **Text** - Add text labels, captions, notes
2. **Freehand Draw** - Hand-drawn marks and sketches
3. **Line** - Draw straight lines
4. **Rectangle** - Create rectangular shapes
5. **Ellipse** - Draw circles and ovals
6. **Path** - Create freeform curved paths
7. **Arrow** - Draw directional arrows
8. **Image** - Embed additional images

## Text Annotations

Text annotations enable adding text directly to images with full customization.

### Add Text via Toolbar

1. Click **Annotate** button
2. Select **Text** from annotation menu
3. Click on canvas to place text
4. Type your text
5. Customize via quick access toolbar

### drawText() Method

```javascript
imageEditor.drawText(x, y, text, fontFamily, fontSize, bold, italic, 
                     color, isSelected, degree, fillColor, strokeColor, strokeWidth);
```

### Text Examples

**Simple Text:**
```html
<div class="col-lg-12 control-section e-img-editor-sample">
    @Html.EJS().ImageEditor("image-editor").Created("created").Toolbar(new string[] { }).Render()
</div>
@Html.EJS().Button("addText").CssClass("e-primary").Content("Add Text").Click("addTextHandler").Render()

<script>
    function created() {
        var imageEditorObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        if (ej.base.Browser.isDevice) {
            imageEditorObj.open('https://ej2.syncfusion.com/demos/src/image-editor/images/flower.png');
        } else {
            imageEditorObj.open('https://ej2.syncfusion.com/demos/src/image-editor/images/bridge.png');
        }
    }

    function addTextHandler() {
        var imageEditorObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        var dimension = imageEditorObj.getImageDimension();
        imageEditorObj.drawText(dimension.x, dimension.y, 'Syncfusion');
    }
</script>
```

**Text with Outline:**
```javascript
function outlineHandler() {
    var imageEditorObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
    var dimension = imageEditorObj.getImageDimension();
    // drawText with green stroke outline
    imageEditorObj.drawText(dimension.x + 50, dimension.y + 50, 'Syncfusion', 
                           'Arial', 30, false, false, '', false, null, '', 'green', 8);
}
```

**Text with Background Color:**
```javascript
function backgroundHandler() {
    var imageEditorObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
    var dimension = imageEditorObj.getImageDimension();
    // drawText with red background fill
    imageEditorObj.drawText(dimension.x + 100, dimension.y + 100, 'Syncfusion', 
                           'Arial', 30, false, false, '', false, null, 'red', '', null);
}
```

### Delete Text Annotation

```javascript
function deleteBtnClickHandler() {
    var imageEditorObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
    // Delete specific shape by ID
    imageEditorObj.deleteShape('shape_1');
}
```

## Shape Annotations

Create various shapes for marking up images: Rectangle, Ellipse, Arrow, Line, and Path.

### Shape Drawing Methods

```html
<div class="col-lg-12 control-section e-img-editor-sample">
    @Html.EJS().ImageEditor("image-editor").Created("created").Toolbar(new string[] { }).Render()
</div>
@Html.EJS().Button("btnClick1").CssClass("e-primary").Content("Rectangle").Click("clickHandler1").Render()
@Html.EJS().Button("btnClick2").CssClass("e-primary").Content("Ellipse").Click("clickHandler2").Render()
@Html.EJS().Button("btnClick3").CssClass("e-primary").Content("Line").Click("clickHandler3").Render()
@Html.EJS().Button("btnClick4").CssClass("e-primary").Content("Arrow").Click("clickHandler4").Render()
@Html.EJS().Button("btnClick5").CssClass("e-primary").Content("Path").Click("clickHandler5").Render()

<script>
    function created() {
        var imageEditorObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        if (ej.base.Browser.isDevice) {
            imageEditorObj.open('https://ej2.syncfusion.com/demos/src/image-editor/images/flower.png');
        } else {
            imageEditorObj.open('https://ej2.syncfusion.com/demos/src/image-editor/images/bridge.png');
        }
    }

    function clickHandler1() {
        var imgObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        var dimension = imgObj.getImageDimension();
        imgObj.drawRectangle(dimension.x + 10, dimension.y + 60, 150, 70);
        imgObj.drawRectangle(dimension.x + 250, dimension.y + 60, 150, 70, null, '', '', null, null, 8);
    }

    function clickHandler2() {
        var imgObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        var dimension = imgObj.getImageDimension();
        imgObj.drawEllipse(500, 500, 400, 400, 2, "#fff", 'green');
    }

    function clickHandler3() {
        var imgObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        imgObj.drawLine(600, 600, 40, 40, 8, "blue");
    }

    function clickHandler4() {
        var imgObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        var dimension = imgObj.getImageDimension();
        imgObj.drawArrow(dimension.x + 150, dimension.y + 150, dimension.x + 300, dimension.y + 150, 10);
    }

    function clickHandler5() {
        var imgObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        var dimension = imgObj.getImageDimension();
        imgObj.drawPath([{ x: dimension.x, y: dimension.y }, { x: dimension.x + 50, y: dimension.y + 50 }, { x: dimension.x + 20, y: dimension.y + 50 }], 8);
    }
</script>
```

### Shape Method Signatures

```javascript
// Rectangle
drawRectangle(x, y, width, height, strokeWidth, fillColor, strokeColor, opacity, degree, cornerRadius)

// Ellipse
drawEllipse(x, y, radiusX, radiusY, strokeWidth, fillColor, strokeColor)

// Line
drawLine(x1, y1, x2, y2, strokeWidth, color)

// Arrow
drawArrow(startX, startY, endX, endY, strokeWidth, color)

// Path (freeform)
drawPath(pathPoints, strokeWidth, color)
```

## Freehand Drawing

Allow users to draw freehand marks on images using the pen tool.

### freeHandDraw() Method

Enable freehand drawing mode:

```javascript
imageEditor.freeHandDraw(true);   // Enable drawing mode
imageEditor.freeHandDraw(false);  // Finish drawing
```

### Freehand Drawing Example

```html
<div class="col-lg-12 control-section e-img-editor-sample">
    @Html.EJS().ImageEditor("image-editor").Created("created").Toolbar(new string[] { }).Render()
</div>
@Html.EJS().Button("btnClick").CssClass("e-primary").Content("Start Drawing").Click("clickHandler").Render()
@Html.EJS().Button("applyBtnClick").CssClass("e-primary").Content("Finish Drawing").Click("applyClickHandler").Render()

<script>
    function created() {
        var imageEditorObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        if (ej.base.Browser.isDevice) {
            imageEditorObj.open('https://ej2.syncfusion.com/demos/src/image-editor/images/flower.png');
        } else {
            imageEditorObj.open('https://ej2.syncfusion.com/demos/src/image-editor/images/bridge.png');
        }
    }

    function clickHandler() {
        var imageEditorObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        imageEditorObj.freeHandDraw(true);
    }

    function applyClickHandler() {
        var imageEditorObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        imageEditorObj.freeHandDraw(false);
    }
</script>
```

## Image Embedding

Embed additional images as annotations.

### drawImage() Method

Add images to the canvas:

```javascript
function addImage() {
    var imageEditorObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
    imageEditorObj.drawImage(
        'https://ej2.syncfusion.com/demos/src/image-editor/images/flower.png',  // image URL
        500,   // x position
        100,   // y position
        200,   // width
        80,    // height
        true,  // isSelected
        0      // degree (rotation)
    );
}
```

### Image Embedding Example

```html
<div class="col-lg-12 control-section e-img-editor-sample">
    @Html.EJS().ImageEditor("image-editor").Created("created").Toolbar(new string[] { }).Render()
</div>
@Html.EJS().Button("btnClick").CssClass("e-primary").Content("Add Image").Click("btnClick").Render()

<script>
    function created() {
        var imageEditorObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        if (ej.base.Browser.isDevice) {
            imageEditorObj.open('https://ej2.syncfusion.com/demos/src/image-editor/images/flower.png');
        } else {
            imageEditorObj.open('https://ej2.syncfusion.com/demos/src/image-editor/images/bridge.png');
        }
    }

    function btnClick() {
        var imgObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        imgObj.drawImage('https://ej2.syncfusion.com/demos/src/image-editor/images/flower.png', 500, 100, 200, 80, true, 0);
    }
</script>
```

## Annotation Management

### Position and Resize

- **Move:** Click and drag annotation
- **Resize:** Drag corner/edge handles
- **Rotate:** Grab rotation circle at bottom (shapes only, not text)

### Delete Shape

Remove annotations by shape ID:

```javascript
function deleteShape() {
    var imageEditorObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
    imageEditorObj.deleteShape('shape_1');
}
```

### Get Image Dimensions

Get current image dimensions for positioning:

```javascript
function getImageDimensions() {
    var imageEditorObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
    var dimension = imageEditorObj.getImageDimension();
    
    console.log('Image X:', dimension.x);
    console.log('Image Y:', dimension.y);
    console.log('Image Width:', dimension.width);
    console.log('Image Height:', dimension.height);
}
```

## Practical Examples

### Complete Annotation Suite

```html
<div class="col-lg-12 control-section e-img-editor-sample">
    @Html.EJS().ImageEditor("image-editor").Created("created").Toolbar(new string[] { }).Render()
</div>

<div class="annotation-controls">
    <h4>Text Annotations</h4>
    <button class="e-btn e-primary" onclick="addSimpleText()">Simple Text</button>
    <button class="e-btn e-primary" onclick="addTextOutline()">Text with Outline</button>
    <button class="e-btn e-primary" onclick="addTextBackground()">Text with Background</button>
    
    <h4>Shape Annotations</h4>
    <button class="e-btn e-primary" onclick="drawRectangle()">Rectangle</button>
    <button class="e-btn e-primary" onclick="drawEllipse()">Ellipse</button>
    <button class="e-btn e-primary" onclick="drawLine()">Line</button>
    <button class="e-btn e-primary" onclick="drawArrow()">Arrow</button>
    <button class="e-btn e-primary" onclick="drawPath()">Path</button>
    
    <h4>Advanced</h4>
    <button class="e-btn e-primary" onclick="startFreehand()">Start Freehand</button>
    <button class="e-btn e-primary" onclick="finishFreehand()">Finish Freehand</button>
    <button class="e-btn e-primary" onclick="addImage()">Add Image</button>
</div>

<script>
    function created() {
        var imageEditorObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        imageEditorObj.open('https://ej2.syncfusion.com/demos/src/image-editor/images/bridge.png');
    }

    function addSimpleText() {
        var imageEditorObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        var dimension = imageEditorObj.getImageDimension();
        imageEditorObj.drawText(dimension.x, dimension.y, 'Syncfusion');
    }

    function addTextOutline() {
        var imageEditorObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        var dimension = imageEditorObj.getImageDimension();
        imageEditorObj.drawText(dimension.x + 50, dimension.y + 50, 'Syncfusion', 
                               'Arial', 30, false, false, '', false, null, '', 'green', 8);
    }

    function addTextBackground() {
        var imageEditorObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        var dimension = imageEditorObj.getImageDimension();
        imageEditorObj.drawText(dimension.x + 100, dimension.y + 100, 'Syncfusion', 
                               'Arial', 30, false, false, '', false, null, 'red', '', null);
    }

    function drawRectangle() {
        var imgObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        var dimension = imgObj.getImageDimension();
        imgObj.drawRectangle(dimension.x + 10, dimension.y + 60, 150, 70);
    }

    function drawEllipse() {
        var imgObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        imgObj.drawEllipse(500, 500, 400, 400, 2, "#fff", 'green');
    }

    function drawLine() {
        var imgObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        imgObj.drawLine(600, 600, 40, 40, 8, "blue");
    }

    function drawArrow() {
        var imgObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        var dimension = imgObj.getImageDimension();
        imgObj.drawArrow(dimension.x + 150, dimension.y + 150, dimension.x + 300, dimension.y + 150, 10);
    }

    function drawPath() {
        var imgObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        var dimension = imgObj.getImageDimension();
        imgObj.drawPath([{ x: dimension.x, y: dimension.y }, 
                        { x: dimension.x + 50, y: dimension.y + 50 }, 
                        { x: dimension.x + 20, y: dimension.y + 50 }], 8);
    }

    function startFreehand() {
        var imageEditorObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        imageEditorObj.freeHandDraw(true);
    }

    function finishFreehand() {
        var imageEditorObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        imageEditorObj.freeHandDraw(false);
    }

    function addImage() {
        var imgObj = ej.base.getComponent(document.getElementById('image-editor'), 'image-editor');
        imgObj.drawImage('https://ej2.syncfusion.com/demos/src/image-editor/images/flower.png', 500, 100, 200, 80, true, 0);
    }
</script>

<style>
    .annotation-controls {
        padding: 20px;
        border: 1px solid #ddd;
        margin-top: 20px;
    }
    
    .annotation-controls h4 {
        margin-top: 15px;
        margin-bottom: 10px;
    }
    
    .annotation-controls button {
        margin: 5px;
    }
    
    .e-img-editor-sample {
        height: 600px;
        width: 100%;
    }
</style>
```

## Best Practices

### 1. **Use getImageDimension() for Positioning**

Always get image dimensions to ensure annotations are placed correctly:

```javascript
var dimension = imageEditorObj.getImageDimension();
imageEditorObj.drawText(dimension.x, dimension.y, 'Text Here');
```

### 2. **Combine Text with Styling**

Enhance text visibility with outline or background:

```javascript
// Text with green outline
imageEditorObj.drawText(x, y, 'Text', 'Arial', 20, false, false, '', false, null, '', 'green', 2);

// Text with red background
imageEditorObj.drawText(x, y, 'Text', 'Arial', 20, false, false, '', false, null, 'red', '', null);
```

### 3. **Manage Freehand Mode**

Always finish freehand drawing explicitly:

```javascript
imageEditorObj.freeHandDraw(true);   // Start
// User draws...
imageEditorObj.freeHandDraw(false);  // Finish
```

### 4. **Delete by Shape ID**

Reference shapes by their IDs for management:

```javascript
imageEditorObj.deleteShape('shape_1');
```

## Troubleshooting

**Issue:** Annotations not positioned correctly
- **Solution:** Use getImageDimension() to get relative coordinates
- **Solution:** Verify coordinates are positive and within image bounds

**Issue:** Text too small or invisible
- **Solution:** Increase fontSize parameter
- **Solution:** Check text color contrasts with background

**Issue:** Freehand marks not appearing
- **Solution:** Ensure freeHandDraw(true) is called first
- **Solution:** Call freeHandDraw(false) to finalize the drawing

**Issue:** Shapes not visible with colors
- **Solution:** Use valid color values (hex or named colors)
- **Solution:** Adjust strokeWidth for better visibility
