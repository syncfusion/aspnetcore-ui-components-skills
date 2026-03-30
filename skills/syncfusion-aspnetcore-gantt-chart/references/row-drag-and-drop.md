``markdown
# Row Drag and Drop — Syncfusion ASP.NET Core Gantt Chart

## Table of Contents
- [Overview](#overview)
- [Enable Row Drag and Drop](#enable-row-drag-and-drop)
- [Multiple Row Drag and Drop](#multiple-row-drag-and-drop)
- [Taskbar Drag and Drop Between Rows](#taskbar-drag-and-drop-between-rows)
- [Drag and Drop Events](#drag-and-drop-events)
- [Prevent Dragging a Specific Record](#prevent-dragging-a-specific-record)
- [Validate Drop Position](#validate-drop-position)
- [Prevent Reorder as Child](#prevent-reorder-as-child)
- [Programmatic Row Reordering](#programmatic-row-reordering)

---

## Overview

The Gantt Chart supports interactive row drag and drop, allowing users to rearrange task records by dragging rows and dropping them **above**, **below**, or as a **child** of another row. This feature is controlled by the llowRowDragAndDrop property.

---

## Enable Row Drag and Drop

Set llowRowDragAndDrop="true" to enable the drag-and-drop handle on each row. Users can then drag any row and drop it at any position in the hierarchy.

`cshtml
<ejs-gantt id='DragAndDrop' dataSource="ViewBag.dataSource" height="450px"
           allowRowDragAndDrop="true" highlightWeekends="true" treeColumnIndex="1"
           projectStartDate="03/24/2019" projectEndDate="07/06/2019">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
        endDate="EndDate" duration="Duration" progress="Progress" dependency="Predecessor"
        child="SubTasks">
    </e-gantt-taskfields>
</ejs-gantt>
`

**Drop positions available:**

| Position | Description |
|---|---|
| Above (	opSegment) | Drops the row as a sibling above the target row |
| Below (ottomSegment) | Drops the row as a sibling below the target row |
| Child (middleSegment) | Drops the row as a child of the target row |

---

## Multiple Row Drag and Drop

To drag multiple rows simultaneously, enable SelectionSettings.type = "Multiple" along with llowRowDragAndDrop="true". Users can select multiple rows and drag them together.

`cshtml
<ejs-gantt id='DragAndDrop' dataSource="ViewBag.dataSource" height="450px"
           allowRowDragAndDrop="true" highlightWeekends="true" treeColumnIndex="1"
           projectStartDate="03/24/2019" projectEndDate="07/06/2019">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
        endDate="EndDate" duration="Duration" progress="Progress" dependency="Predecessor"
        child="SubTasks">
    </e-gantt-taskfields>
    <e-gantt-selectionsettings type="Multiple"></e-gantt-selectionsettings>
</ejs-gantt>
`

> Hold **Ctrl** or **Shift** to select multiple rows before dragging.

---

## Taskbar Drag and Drop Between Rows

Set llowTaskbarDragAndDrop="true" in addition to llowRowDragAndDrop="true" to enable row reordering by dragging the taskbar itself in the chart pane. Editing must also be enabled (mode="Auto").

`cshtml
<ejs-gantt id='DragAndDrop' dataSource="ViewBag.dataSource" height="450px"
           allowRowDragAndDrop="true" allowTaskbarDragAndDrop="true"
           highlightWeekends="true" treeColumnIndex="1"
           projectStartDate="03/24/2019" projectEndDate="07/06/2019">
    <e-gantt-editsettings allowEditing="true" mode="Auto"></e-gantt-editsettings>
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
        endDate="EndDate" duration="Duration" progress="Progress" dependency="Predecessor"
        child="SubTasks">
    </e-gantt-taskfields>
</ejs-gantt>
`

| Property | Required for taskbar drag | Description |
|---|---|---|
| llowRowDragAndDrop | Yes | Enables the drag-and-drop feature |
| llowTaskbarDragAndDrop | Yes | Allows dragging via the taskbar in chart pane |
| editSettings.allowEditing | Yes | Editing must be enabled |
| editSettings.mode | Yes | Set to "Auto" |

---

## Drag and Drop Events

Use these events to hook into the drag-and-drop lifecycle and customise or cancel behaviour.

| Event | Trigger point |
|---|---|
| 
owDragStartHelper | When the drag icon or row is first clicked — use to cancel drag |
| 
owDragStart | When drag action begins |
| 
owDrag | While dragging (continuous) |
| 
owDrop | When the dragged row is dropped on a target — use to cancel drop or change position |

---

## Prevent Dragging a Specific Record

Use 
owDragStartHelper and set rgs.cancel = true to prevent specific rows from being dragged. The following example prevents dragging of Task IDs 1–4 and all their children.

`cshtml
<ejs-gantt id='DragAndDrop' dataSource="ViewBag.dataSource" height="450px"
           rowDragStartHelper="rowDragStartHelper" allowRowDragAndDrop="true"
           highlightWeekends="true" treeColumnIndex="1"
           projectStartDate="03/24/2019" projectEndDate="07/06/2019">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
        endDate="EndDate" duration="Duration" progress="Progress" dependency="Predecessor"
        child="SubTasks">
    </e-gantt-taskfields>
</ejs-gantt>

<script>
    function rowDragStartHelper(args) {
        var record = args.data[0] ? args.data[0] : args.data;
        var taskId = record.ganttProperties.taskId;
        if (taskId <= 4) {
            args.cancel = true;
        }
    }
</script>
`

**
owDragStartHelper args properties:**

| Property | Type | Description |
|---|---|---|
| rgs.data | object/array | The dragged record(s) |
| rgs.cancel | boolean | Set to 	rue to prevent drag |

---

## Validate Drop Position

Use 
owDrop and set rgs.cancel = true to prevent dropping at specific positions. The example below prevents dropping a row as a child (i.e., onto a middleSegment).

`cshtml
<ejs-gantt id='DragAndDrop' dataSource="ViewBag.dataSource" height="450px"
           rowDrop="rowDrop" allowRowDragAndDrop="true" highlightWeekends="true"
           treeColumnIndex="1" projectStartDate="03/24/2019" projectEndDate="07/06/2019">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
        endDate="EndDate" duration="Duration" progress="Progress" dependency="Predecessor"
        child="SubTasks">
    </e-gantt-taskfields>
</ejs-gantt>

<script>
    function rowDrop(args) {
        if (args.dropPosition == "middleSegment") {
            args.cancel = true;
        }
    }
</script>
`

**
owDrop args properties:**

| Property | Type | Description |
|---|---|---|
| rgs.dropPosition | string | "topSegment", "bottomSegment", or "middleSegment" |
| rgs.fromIndex | number | Source row index |
| rgs.dropIndex | number | Target row index |
| rgs.cancel | boolean | Set to 	rue to cancel the drop |

---

## Prevent Reorder as Child

Cancel the drop and use 
eorderRows to force the dropped row into a different position. In the example below, drops that would create a child relationship are redirected to be placed **above** the target row instead.

`cshtml
<ejs-gantt id='DragAndDrop' dataSource="@data" height="450px"
           rowDrop="rowDrop" allowRowDragAndDrop="true" highlightWeekends="true"
           treeColumnIndex="1" projectStartDate="03/24/2019" projectEndDate="07/06/2019">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
        endDate="EndDate" duration="Duration" progress="Progress" dependency="Predecessor"
        child="SubTasks">
    </e-gantt-taskfields>
</ejs-gantt>

<script>
    function rowDrop(args) {
        if (args.dropPosition == 'middleSegment') {
            var ganttObj = document.getElementById('DragAndDrop').ej2_instances[0];
            args.cancel = true;
            ganttObj.reorderRows([args.fromIndex], args.dropIndex, 'above');
        }
    }
</script>
`

---

## Programmatic Row Reordering

Use the 
eorderRows method to move rows programmatically (e.g., from a button click), without requiring the user to drag.

**Syntax:**
`javascript
ganttObj.reorderRows(fromIndexes, toIndex, position);
`

| Parameter | Type | Description |
|---|---|---|
| romIndexes | number[] | Array of source row index values |
| 	oIndex | number | Index of the target row |
| position | string | "above", "below", or "child" |

**Example — Drop rows 1, 2, 3 as children of row 4 on button click:**

`cshtml
<ejs-button id="DynamicDrag" content="Drop records as child" cssClass="e-primary"></ejs-button>
<ejs-gantt id='DragAndDrop' dataSource="ViewBag.dataSource" height="450px"
           allowRowDragAndDrop="true" highlightWeekends="true" treeColumnIndex="1"
           projectStartDate="03/24/2019" projectEndDate="07/06/2019">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
        endDate="EndDate" duration="Duration" progress="Progress" dependency="Predecessor"
        child="SubTasks">
    </e-gantt-taskfields>
</ejs-gantt>

<script>
    document.getElementById('DynamicDrag').addEventListener('click', function () {
        var ganttObj = document.getElementById('DragAndDrop').ej2_instances[0];
        ganttObj.reorderRows([1, 2, 3], 4, 'child');
    });
</script>
`

---
