# Selection — Syncfusion ASP.NET Core Gantt Chart

## Table of Contents
- [Overview](#overview)
- [Disable Selection](#disable-selection)
- [Selection Mode](#selection-mode)
- [Selection Type](#selection-type)
- [Toggle Selection](#toggle-selection)
- [Hover Highlighting](#hover-highlighting)
- [Row Selection](#row-selection)
  - [Select a Row on Initial Load](#select-a-row-on-initial-load)
  - [Select a Row Dynamically](#select-a-row-dynamically)
  - [Multiple Row Selection](#multiple-row-selection)
  - [Conditional Row Selection](#conditional-row-selection)
  - [Customize Row Selection Action](#customize-row-selection-action)
- [Cell Selection](#cell-selection)
  - [Multiple Cell Selection](#multiple-cell-selection)
  - [Select a Cell Dynamically](#select-a-cell-dynamically)
  - [Customize Cell Selection Action](#customize-cell-selection-action)
- [Get Selected Row Indexes and Records](#get-selected-row-indexes-and-records)
- [Clear Selection](#clear-selection)
- [Touch Interaction](#touch-interaction)

---

## Overview

Selection provides an option to highlight a row or a cell in the Gantt Chart. It can be triggered by clicking or using arrow keys. Selection is enabled by default and configured via `<e-gantt-selectionsettings>`.

---

## Disable Selection

Set `allowSelection="false"` on `<ejs-gantt>` to disable all selection behaviour:

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.dataSource" height="450px" allowSelection="false">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
                        endDate="EndDate" duration="Duration" progress="Progress" child="SubTasks">
    </e-gantt-taskfields>
</ejs-gantt>
```

> `Row` selection is the default selection mode when `allowSelection` is `true`.

---

## Selection Mode

Set `mode` on `<e-gantt-selectionsettings>` to control what can be selected:

| `mode` Value | Description |
|---|---|
| `Row` (default) | Selects the entire row |
| `Cell` | Selects individual cells |
| `Both` | Selects both rows and cells simultaneously |

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.dataSource" height="450px">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
                        endDate="EndDate" duration="Duration" progress="Progress" child="SubTasks">
    </e-gantt-taskfields>
    <e-gantt-selectionsettings mode="Both"></e-gantt-selectionsettings>
</ejs-gantt>
```

---

## Selection Type

Set `type` on `<e-gantt-selectionsettings>` to allow single or multiple selections:

| `type` Value | Description |
|---|---|
| `Single` (default) | Only one row or cell can be selected at a time |
| `Multiple` | Multiple rows or cells can be selected by holding **Ctrl** while clicking |

---

## Toggle Selection

Set `enableToggle="true"` on `<e-gantt-selectionsettings>` to allow deselecting an already-selected row or cell by clicking it again. Default is `false`.

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.dataSource" height="450px" allowSelection="true">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
                        endDate="EndDate" duration="Duration" progress="Progress" child="SubTasks">
    </e-gantt-taskfields>
    <e-gantt-selectionsettings mode="Row" type="Multiple" enableToggle="true">
    </e-gantt-selectionsettings>
</ejs-gantt>

<script>
    // Disable toggle at runtime
    document.getElementById('toggle').addEventListener('click', function () {
        var ganttObj = document.getElementById('Gantt').ej2_instances[0];
        ganttObj.selectionSettings.enableToggle = false;
    });
</script>
```

---

## Hover Highlighting

Set `enableHover="true"` on `<ejs-gantt>` to highlight tree grid rows, chart taskbars, header cells, and timeline cells when the mouse hovers over them. This makes it easier to track tasks in complex project timelines:

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.dataSource"
           enableHover="true" allowSelection="true" height="450px">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
                        endDate="EndDate" duration="Duration" progress="Progress" parentID="ParentID">
    </e-gantt-taskfields>
    <e-gantt-selectionSettings mode="Row" type="Multiple"></e-gantt-selectionSettings>
</ejs-gantt>
```

---

## Row Selection

### Select a Row on Initial Load

Use `selectedRowIndex` on `<ejs-gantt>` to pre-select a row when the component first renders. The index is **0-based**:

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.dataSource" height="450px"
           allowSelection="true" selectedRowIndex="3">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
                        endDate="EndDate" duration="Duration" progress="Progress" child="SubTasks">
    </e-gantt-taskfields>
</ejs-gantt>
```

---

### Select a Row Dynamically

Use `selectionModule.selectRow(index)` to select a single row, or `selectionModule.selectRows(indexes)` to select multiple rows programmatically:

```cshtml
<ejs-button id="selectRow" content="Select Row" cssClass="e-primary"></ejs-button>
<ejs-button id="selectRows" content="Select Rows" cssClass="e-primary"></ejs-button>

<ejs-gantt id="Gantt" dataSource="ViewBag.dataSource" height="450px" allowSelection="true">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
                        endDate="EndDate" duration="Duration" progress="Progress" child="SubTasks">
    </e-gantt-taskfields>
    <e-gantt-selectionsettings mode="Row" type="Multiple"></e-gantt-selectionsettings>
</ejs-gantt>

<script>
    document.getElementById('selectRow').addEventListener('click', function () {
        var ganttObj = document.getElementById('Gantt').ej2_instances[0];
        ganttObj.selectionModule.selectRow(2);      // select row at index 2
    });
    document.getElementById('selectRows').addEventListener('click', function () {
        var ganttObj = document.getElementById('Gantt').ej2_instances[0];
        ganttObj.selectionModule.selectRows([1, 2, 3]);  // select rows at indexes 1, 2, 3
    });
</script>
```

---

### Multiple Row Selection

Set `type="Multiple"` on `<e-gantt-selectionsettings>` to allow selecting more than one row. Hold **Ctrl** while clicking to add rows to the selection:

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.dataSource" height="450px" allowSelection="true">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
                        endDate="EndDate" duration="Duration" progress="Progress" child="SubTasks">
    </e-gantt-taskfields>
    <e-gantt-selectionSettings mode="Row" type="Multiple"></e-gantt-selectionSettings>
</ejs-gantt>
```

---

### Conditional Row Selection

Use `selectionModule.selectRows()` inside the `dataBound` event to select specific rows based on data conditions on initial load:

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.dataSource" height="450px"
           allowSelection="true" dataBound="dataBound">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
                        endDate="EndDate" duration="Duration" progress="Progress" child="SubTasks">
    </e-gantt-taskfields>
    <e-gantt-selectionSettings mode="Row" type="Multiple"></e-gantt-selectionSettings>
</ejs-gantt>

<script>
    function dataBound(args) {
        var ganttObj = document.getElementById("Gantt").ej2_instances[0];
        var rowIndexes = [];
        ganttObj.treeGrid.grid.dataSource.forEach(function (data, index) {
            if (data.TaskId === 3 || data.TaskId === 4) {
                rowIndexes.push(index);
            }
        });
        ganttObj.selectionModule.selectRows(rowIndexes);
    }
</script>
```

> In the example above, rows where `TaskId` is 3 or 4 are selected at initial rendering.

---

### Customize Row Selection Action

The `rowSelecting` event fires when a row selection begins (before completion). Use `args.cancel = true` to prevent selection of a specific row. The `rowSelected` event fires after a row is fully selected.

When a row is deselected (by clicking it again), `rowDeselecting` fires before and `rowDeselected` fires after the deselection completes.

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.dataSource" height="450px"
           allowSelection="true" rowSelecting="rowSelecting">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
                        endDate="EndDate" duration="Duration" progress="Progress" child="SubTasks">
    </e-gantt-taskfields>
</ejs-gantt>

<script>
    function rowSelecting(args) {
        if (args.rowIndex === 3) {
            args.cancel = true;   // prevent selection of row at index 3
        }
    }
</script>
```

| Event | Trigger point | Key `args` properties |
|---|---|---|
| `rowSelecting` | Before row selection completes | `rowIndex`, `data`, `cancel` |
| `rowSelected` | After row selection completes | `rowIndex`, `data` |
| `rowDeselecting` | Before row deselection completes | `rowIndex`, `data`, `cancel` |
| `rowDeselected` | After row deselection completes | `rowIndex`, `data` |

---

## Cell Selection

Set `mode="Cell"` on `<e-gantt-selectionsettings>` to enable cell-level selection. Use `getSelectedRowCellIndexes()` to retrieve selected cell information — it returns an object collection with `cellIndexes` and `rowIndex` for each selected cell:

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.dataSource" height="450px" allowSelection="true">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
                        endDate="EndDate" duration="Duration" progress="Progress" child="SubTasks">
    </e-gantt-taskfields>
    <e-gantt-selectionSettings mode="Cell" type="Single"></e-gantt-selectionSettings>
</ejs-gantt>
```

> Cell-based selection is **not supported** when virtualization is enabled.

---

### Multiple Cell Selection

Set `type="Multiple"` with `mode="Cell"` to allow selecting more than one cell. Hold **Ctrl** while clicking to add cells to the selection:

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.dataSource" height="450px" allowSelection="true">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
                        endDate="EndDate" duration="Duration" progress="Progress" child="SubTasks">
    </e-gantt-taskfields>
    <e-gantt-selectionSettings mode="Cell" type="Multiple"></e-gantt-selectionSettings>
</ejs-gantt>
```

---

### Select a Cell Dynamically

Use `selectionModule.selectCell(index)` to select a cell programmatically by passing the cell index:

```cshtml
<ejs-button id="selectCell" content="Select Cell" cssClass="e-primary"></ejs-button>

<ejs-gantt id="Gantt" dataSource="ViewBag.dataSource" height="450px" allowSelection="true">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
                        endDate="EndDate" duration="Duration" progress="Progress" child="SubTasks">
    </e-gantt-taskfields>
</ejs-gantt>

<script>
    document.getElementById('selectCell').addEventListener('click', function () {
        var ganttObj = document.getElementById('Gantt').ej2_instances[0];
        ganttObj.selectionModule.selectCell(2);   // select cell at index 2
    });
</script>
```

---

### Customize Cell Selection Action

The `cellSelecting` event fires when a cell selection begins. Use `args.cancel = true` to prevent selection of a specific cell. The `cellSelected` event fires after the selection completes.

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.dataSource" height="450px"
           allowSelection="true" cellSelecting="cellSelecting">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
                        endDate="EndDate" duration="Duration" progress="Progress" child="SubTasks">
    </e-gantt-taskfields>
</ejs-gantt>

<script>
    function cellSelecting(args) {
        if (args.cellIndex === 3) {
            args.cancel = true;   // prevent selection of cell at index 3
        }
    }
</script>
```

| Event | Trigger point | Key `args` properties |
|---|---|---|
| `cellSelecting` | Before cell selection completes | `cellIndex`, `rowIndex`, `cancel` |
| `cellSelected` | After cell selection completes | `cellIndex`, `rowIndex` |

---

## Get Selected Row Indexes and Records

Use `getSelectedRowIndexes()` to retrieve the indexes of all selected rows, and `getSelectedRecords()` to retrieve the full data objects of selected rows. Both are accessed via `selectionModule`:

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.dataSource" height="450px"
           allowSelection="true" rowSelected="rowSelected">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
                        endDate="EndDate" duration="Duration" progress="Progress" child="SubTasks">
    </e-gantt-taskfields>
    <e-gantt-selectionSettings mode="Row" type="Multiple"></e-gantt-selectionSettings>
</ejs-gantt>

<script>
    function rowSelected(args) {
        var ganttObj = document.getElementById("Gantt").ej2_instances[0];
        var selectedIndexes = ganttObj.selectionModule.getSelectedRowIndexes();  // [0, 2, 3]
        var selectedRecords = ganttObj.selectionModule.getSelectedRecords();     // task data objects
        console.log(selectedIndexes);
        console.log(selectedRecords);
    }
</script>
```

---

## Clear Selection

Call `clearSelection()` directly on the Gantt instance to deselect all currently selected rows or cells:

```cshtml
<ejs-button id="selectRows" content="Select Rows" cssClass="e-primary"></ejs-button>
<ejs-button id="clearSelection" content="Clear Selection" cssClass="e-primary"></ejs-button>

<ejs-gantt id="Gantt" dataSource="ViewBag.dataSource" height="450px" allowSelection="true">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
                        endDate="EndDate" duration="Duration" progress="Progress" child="SubTasks">
    </e-gantt-taskfields>
    <e-gantt-selectionsettings mode="Row" type="Multiple"></e-gantt-selectionsettings>
</ejs-gantt>

<script>
    document.getElementById('selectRows').addEventListener('click', function () {
        var ganttObj = document.getElementById('Gantt').ej2_instances[0];
        ganttObj.selectionModule.selectRows([1, 2, 3]);
    });
    document.getElementById('clearSelection').addEventListener('click', function () {
        var ganttObj = document.getElementById('Gantt').ej2_instances[0];
        ganttObj.clearSelection();
    });
</script>
```

---

## Touch Interaction

The Gantt Chart supports touch-based selection on mobile and tablet devices:

- **Single row selection**: Tap a row to select it.
- **Multiple row selection**: When you tap a row, a popup appears indicating the multi-row selection option. Tap the popup, then tap additional rows to build a multi-row selection.

Touch interaction follows the same `selectionSettings.mode` and `selectionSettings.type` configuration as pointer-based selection.
