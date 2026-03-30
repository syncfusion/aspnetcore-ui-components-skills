# Interaction and Tools in Syncfusion ASP.NET Core Diagram

## Table of Contents
- [Selection](#selection)
- [Drag, Resize, and Rotate](#drag-resize-and-rotate)
- [Drawing Tools](#drawing-tools)
- [Diagram Tools Enum](#diagram-tools-enum)
- [Constraints](#constraints)
- [Undo and Redo](#undo-and-redo)
- [Custom History](#custom-history)
- [Context Menu](#context-menu)
- [Keyboard Shortcuts](#keyboard-shortcuts)

## Selection

### Programmatic Selection

```javascript
var diagram = document.getElementById('diagram').ej2_instances[0];

// Select a single node by reference
var node = diagram.getObject('node1');
diagram.select([node]);

// Select all nodes and connectors
diagram.selectAll();

// Clear selection
diagram.clearSelection();
```

### Inspecting Selected Items

```javascript
// Get selected nodes and connectors
var selectedNodes = diagram.selectedItems.nodes;
var selectedConnectors = diagram.selectedItems.connectors;

console.log('Selected nodes:', selectedNodes.map(n => n.id));
```

### selectionChange Event

```cshtml
<ejs-diagram id="diagram" selectionChange="onSelectionChange">
</ejs-diagram>
<script>
    function onSelectionChange(args) {
        // args.newValue = array of newly selected elements
        // args.oldValue = array of previously selected elements
        // args.cause = 'Interaction' | 'Programmatic'
        if (args.newValue.length > 0) {
            console.log('Selected:', args.newValue[0].id);
        }
    }
</script>
```

## Drag, Resize, and Rotate

### positionChange Event

```cshtml
<ejs-diagram id="diagram" positionChange="onPositionChange">
</ejs-diagram>
<script>
    function onPositionChange(args) {
        // args.element = moved element
        // args.oldValue = { offsetX, offsetY }
        // args.newValue = { offsetX, offsetY }
        console.log('Moved to', args.newValue.offsetX, args.newValue.offsetY);
    }
</script>
```

### sizeChange Event

```cshtml
<ejs-diagram id="diagram" sizeChange="onSizeChange">
</ejs-diagram>
<script>
    function onSizeChange(args) {
        // args.element = resized element
        // args.oldValue = { width, height, offsetX, offsetY }
        // args.newValue = { width, height, offsetX, offsetY }
    }
</script>
```

### rotateChange Event

```cshtml
<ejs-diagram id="diagram" rotateChange="onRotateChange">
</ejs-diagram>
<script>
    function onRotateChange(args) {
        // args.element = rotated element
        // args.oldValue = { rotateAngle }
        // args.newValue = { rotateAngle }
    }
</script>
```

## Drawing Tools

Enable drawing mode to allow users to draw shapes on the canvas:

### Draw Rectangle

```javascript
var diagram = document.getElementById('diagram').ej2_instances[0];

diagram.drawingObject = {
    shape: { type: 'Basic', shape: 'Rectangle' },
    style: { fill: '#6BA5D7', strokeColor: '#4674CE' }
};
diagram.tool = ej.diagram.DiagramTools.DrawOnce;
```

### Draw Polygon/Path

```javascript
diagram.drawingObject = {
    shape: { type: 'Basic', shape: 'Polygon' }
};
diagram.tool = ej.diagram.DiagramTools.ContinuesDraw;
```

### Draw Connector

```javascript
diagram.drawingObject = {
    type: 'Straight',
    targetDecorator: { shape: 'Arrow' }
};
diagram.tool = ej.diagram.DiagramTools.DrawOnce;
```

### elementDraw Event

```cshtml
<ejs-diagram id="diagram" elementDraw="onElementDraw">
</ejs-diagram>
<script>
    function onElementDraw(args) {
        // args.element = the newly drawn node or connector
        // args.source = source node (for connectors)
        console.log('Drew:', args.element.id);
    }
</script>
```

## Diagram Tools Enum

```javascript
// Single tool values
ej.diagram.DiagramTools.None              // Disable all interactions
ej.diagram.DiagramTools.SingleSelect      // Select one element at a time
ej.diagram.DiagramTools.MultipleSelect    // Rubber-band multi-select
ej.diagram.DiagramTools.ZoomPan           // Pan and zoom only
ej.diagram.DiagramTools.DrawOnce          // Draw one shape, then revert to select
ej.diagram.DiagramTools.ContinuesDraw     // Draw shapes repeatedly

// Combine tools with bitwise OR
diagram.tool = ej.diagram.DiagramTools.DrawOnce | ej.diagram.DiagramTools.ZoomPan;
```

Set tool via ViewBag (at page load):

```csharp
ViewBag.tool = DiagramTools.MultipleSelect;
```

```cshtml
<ejs-diagram id="diagram" tool="@ViewBag.tool">
</ejs-diagram>
```

## Constraints

### DiagramConstraints

```csharp
// Enable line routing
ViewBag.constraints = DiagramConstraints.Default | DiagramConstraints.LineRouting;

// Enable virtualization for large diagrams
ViewBag.constraints = DiagramConstraints.Default | DiagramConstraints.Virtualization;

// Enable bridging (line-crossing arcs)
ViewBag.constraints = DiagramConstraints.Default | DiagramConstraints.Bridging;

// Combined
ViewBag.constraints = DiagramConstraints.Default
    | DiagramConstraints.LineRouting
    | DiagramConstraints.Bridging
    | DiagramConstraints.Virtualization;
```

```cshtml
<ejs-diagram id="diagram" constraints="@ViewBag.constraints">
</ejs-diagram>
```

| Value | Description |
|-------|-------------|
| `Default` | Standard interact — select, drag, resize, rotate |
| `PageEditable` | Edit page background |
| `Bridging` | Draw arcs where connectors cross |
| `LineRouting` | Auto-route connectors around nodes |
| `Virtualization` | Render only visible elements |
| `UndoRedo` | Enable undo/redo history |
| `Zoom` | Allow zoom |
| `Pan` | Allow pan |

### NodeConstraints

```csharp
// Remove specific constraints (bitwise NOT)
var node = new DiagramNode
{
    Id = "lockedNode",
    Constraints = NodeConstraints.Default & ~NodeConstraints.Drag     // cannot be moved
};

// Disable rotation only
var node2 = new DiagramNode
{
    Id = "noRotate",
    Constraints = NodeConstraints.Default & ~NodeConstraints.Rotate
};

// Read-only node
var node3 = new DiagramNode
{
    Id = "readOnly",
    Constraints = NodeConstraints.None | NodeConstraints.Select   // select but can't edit
};
```

| Value | Description |
|-------|-------------|
| `Default` | Full interaction |
| `Select` | Allow selection |
| `Drag` | Allow drag |
| `Resize` | Allow resize |
| `Rotate` | Allow rotation |
| `Connect` | Allow connecting from this node |
| `Delete` | Allow delete |
| `Tooltip` | Show tooltip |
| `Shadow` | Render shadow |
| `None` | No interaction |

### ConnectorConstraints

```csharp
var connector = new DiagramConnector
{
    Id = "conn1",
    // Remove ability to select
    Constraints = ConnectorConstraints.Default & ~ConnectorConstraints.Select
};
```

### AnnotationConstraints

```csharp
var annotation = new DiagramNodeAnnotation
{
    Content = "Label",
    Constraints = AnnotationConstraints.Default | AnnotationConstraints.Drag
};
```

## Undo and Redo

```javascript
var diagram = document.getElementById('diagram').ej2_instances[0];

// Undo last action
diagram.undo();

// Redo last undone action
diagram.redo();
```

Keyboard shortcuts work automatically: `Ctrl+Z` (undo), `Ctrl+Y` (redo).

### Batch History Actions

Group multiple actions into a single undo step:

```javascript
diagram.startGroupAction();

// All changes between start and end are one history entry
diagram.nodes[0].offsetX += 50;
diagram.dataBind();
diagram.nodes[1].offsetX += 50;
diagram.dataBind();

diagram.endGroupAction();
// Now Ctrl+Z undoes all moves at once
```

### historyChange Event

```cshtml
<ejs-diagram id="diagram" historyChange="onHistoryChange">
</ejs-diagram>
<script>
    function onHistoryChange(args) {
        // args.action = 'AddEntry' | 'Undo' | 'Redo'
        // args.undoStack = current undo stack
        // args.redoStack = current redo stack
        console.log('History action:', args.action);
        console.log('Undo stack size:', args.undoStack.length);
    }
</script>
```

### Inspect Undo/Redo Stacks

```javascript
console.log('Undo entries:', diagram.historyList.undoStack);
console.log('Redo entries:', diagram.historyList.redoStack);
```

## Custom History

Push custom entries to the undo/redo stack:

```javascript
var customEntry = {
    undoObject: { nodeId: 'node1', oldColor: '#6BA5D7' },
    redoObject: { nodeId: 'node1', newColor: '#FF6347' }
};

// canLog controls whether this entry is saved
diagram.historyList.canLog = function(entry) {
    return entry;  // return null to skip logging
};

diagram.historyList.push(customEntry);
```

Handle custom undo/redo:

```javascript
diagram.historyList.undo = function(args) {
    var node = diagram.getObject(args.undoObject.nodeId);
    node.style.fill = args.undoObject.oldColor;
    diagram.dataBind();
};

diagram.historyList.redo = function(args) {
    var node = diagram.getObject(args.redoObject.nodeId);
    node.style.fill = args.redoObject.newColor;
    diagram.dataBind();
};
```

## Context Menu

Enable built-in context menu:

```cshtml
<ejs-diagram id="diagram"
    contextMenuSettings="@ViewBag.contextMenuSettings"
    contextMenuClick="onContextMenuClick">
</ejs-diagram>
```

```csharp
ViewBag.contextMenuSettings = new DiagramContextMenuSettings
{
    Show = true
};
```

### Custom Context Menu Items

```csharp
ViewBag.contextMenuSettings = new DiagramContextMenuSettings
{
    Show = true,
    Items = new List<ContextMenuItemModel>
    {
        new ContextMenuItemModel { Text = "Clone", Id = "clone", Target = ".e-diagramcontent" },
        new ContextMenuItemModel { Text = "Lock",  Id = "lock",  Target = ".e-diagramcontent" }
    },
    ShowCustomMenuOnly = false   // false = show built-in + custom; true = custom only
};
```

```javascript
function onContextMenuClick(args) {
    var diagram = document.getElementById('diagram').ej2_instances[0];
    if (args.item.id === 'clone') {
        var nodes = diagram.selectedItems.nodes;
        nodes.forEach(function(n) { diagram.copy(); diagram.paste(); });
    }
    if (args.item.id === 'lock') {
        var nodes = diagram.selectedItems.nodes;
        nodes.forEach(function(n) {
            n.constraints = ej.diagram.NodeConstraints.Default & ~ej.diagram.NodeConstraints.Drag;
            diagram.dataBind();
        });
    }
}
```

## Keyboard Shortcuts

Default keyboard shortcuts available in the diagram:

| Shortcut | Action |
|----------|--------|
| `Ctrl+Z` | Undo |
| `Ctrl+Y` | Redo |
| `Ctrl+C` | Copy |
| `Ctrl+X` | Cut |
| `Ctrl+V` | Paste |
| `Delete` / `Backspace` | Delete selected |
| `Ctrl+A` | Select all |
| `Ctrl+D` | Duplicate |
| `Ctrl++ / Ctrl+-` | Zoom in/out |
| `Ctrl+Shift+H` | Fit page |
| Arrow keys | Nudge selected elements |
| `Escape` | Cancel action / deselect |
| `Tab` | Select next element |
