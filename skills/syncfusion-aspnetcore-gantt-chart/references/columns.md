# Columns — Syncfusion ASP.NET Core Gantt Chart

## Table of Contents
- [Defining Columns](#defining-columns)
- [Column Properties Reference](#column-properties-reference)
- [Custom Column Header](#custom-column-header)
- [Column Template](#column-template)
- [Column Type](#column-type)
- [Column Format](#column-format)
- [Checkbox Column](#checkbox-column)
- [Show or Hide Columns Dynamically](#show-or-hide-columns-dynamically)
- [Controlling Column Actions](#controlling-column-actions)
- [Frozen Columns](#frozen-columns)
- [Column Reordering](#column-reordering)
- [Column Resizing](#column-resizing)
- [Column Spanning](#column-spanning)
- [Responsive Columns](#responsive-columns)
- [WBS Column](#wbs-column)
- [Column Menu](#column-menu)
- [TreeColumnIndex](#treecolumnindex)

---

## Defining Columns

Use `<e-gantt-columns>` to explicitly define which columns appear in the TreeGrid section. If omitted, Gantt auto-generates columns from `taskFields`.

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.DataSource" height="450px">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
                        endDate="EndDate" duration="Duration" progress="Progress"
                        child="SubTasks">
    </e-gantt-taskfields>
    <e-gantt-columns>
        <e-gantt-column field="TaskId"   headerText="ID"        width="60"  textAlign="Right" isPrimaryKey="true"></e-gantt-column>
        <e-gantt-column field="TaskName" headerText="Task Name" width="250" clipMode="EllipsisWithTooltip"></e-gantt-column>
        <e-gantt-column field="StartDate" headerText="Start"   width="120" format="yMd"></e-gantt-column>
        <e-gantt-column field="Duration"  headerText="Days"    width="80"  textAlign="Right"></e-gantt-column>
        <e-gantt-column field="Progress"  headerText="Progress" width="100" textAlign="Right"></e-gantt-column>
    </e-gantt-columns>
</ejs-gantt>
```

---

## Column Properties Reference

| Property | Type | Description |
|---|---|---|
| `field` | string | Maps to the data model property name |
| `headerText` | string | Column header label |
| `width` | number/string | Column width in pixels or percentage |
| `textAlign` | string | `Left` \| `Right` \| `Center` \| `Justify` |
| `format` | string | Date/number format (e.g., `yMd`, `C2`, `n2`) |
| `isPrimaryKey` | bool | Must be `true` on the ID column for CRUD |
| `visible` | bool | Show or hide the column |
| `allowEditing` | bool | Allow cell editing for this column |
| `allowSorting` | bool | Allow sorting on this column |
| `allowFiltering` | bool | Allow filtering on this column |
| `clipMode` | string | `Clip` \| `Ellipsis` \| `EllipsisWithTooltip` |
| `editType` | string | `numericedit` \| `defaultedit` \| `dropdownedit` \| `booleanedit` \| `datepickeredit` |
| `freeze` | string | `Left` \| `Right` to freeze the column |
| `minWidth` | number | Minimum column width during resizing |
| `maxWidth` | number | Maximum column width during resizing |

---

## Custom Column Header

Use `headerText` for a simple rename, or `headerTemplate` for rich HTML content:

```cshtml
@* Simple header rename *@
<e-gantt-column field="TaskName" headerText="Activity Description" width="250"></e-gantt-column>

@* Header with template — reference a script ID string via the headerTemplate attribute *@
<ejs-gantt id="Gantt" dataSource="ViewBag.dataSource" height="450px">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
                        endDate="EndDate" duration="Duration" progress="Progress" child="SubTasks">
    </e-gantt-taskfields>
    <e-gantt-columns>
        <e-gantt-column field="TaskName" headerTemplate="#projectName"  width="150"></e-gantt-column>
        <e-gantt-column field="StartDate" headerTemplate="#dateTemplate" width="150"></e-gantt-column>
        <e-gantt-column field="Duration" headerTemplate="#durationTemplate" width="150"></e-gantt-column>
        <e-gantt-column field="Progress" headerTemplate="#progressTemplate" width="150"></e-gantt-column>
    </e-gantt-columns>
</ejs-gantt>

<script type="text/x-template" id="projectName">
    <div><img src="taskname.png" width="20" height="20" class="e-image" />  Task Name</div>
</script>
<script type="text/x-template" id="dateTemplate">
    <div><img src="startdate.png" width="20" height="20" class="e-image" />  Start Date</div>
</script>
<script type="text/x-template" id="durationTemplate">
    <div><img src="duration.png" width="20" height="20" class="e-image" />  Duration</div>
</script>
<script type="text/x-template" id="progressTemplate">
    <div><img src="progress.png" width="20" height="20" class="e-image" />  Progress</div>
</script>
```

> Header templates use `type="text/x-template"` script blocks. Reference them by script ID string on the `headerTemplate` attribute (e.g., `headerTemplate="#projectName"`). There is no `<e-gantt-column-headertemplate>` child tag.

---

## Column Template

Render custom HTML inside cells using a column template — reference a script ID string via the `template` attribute:

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.dataSource" height="450px"
           resources="ViewBag.projectResources">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
                        endDate="EndDate" duration="Duration" progress="Progress"
                        child="SubTasks" resourceInfo="ResourceId">
    </e-gantt-taskfields>
    <e-gantt-resourcefields id="ResourceId" name="ResourceName"></e-gantt-resourcefields>
    <e-gantt-columns>
        <e-gantt-column field="TaskId"     headerText="Task Id"   width="50"></e-gantt-column>
        <e-gantt-column field="TaskName"   headerText="Task Name" width="250"></e-gantt-column>
        <e-gantt-column field="ResourceId" headerText="Resources" template="#columnTemplate"></e-gantt-column>
        <e-gantt-column field="StartDate"></e-gantt-column>
        <e-gantt-column field="Duration"></e-gantt-column>
        <e-gantt-column field="Progress"></e-gantt-column>
    </e-gantt-columns>
</ejs-gantt>

<script type="text/x-jsrender" id="columnTemplate">
    ${if(ganttProperties.resourceNames)}
    <div class="image">
        <img src="${TaskID}.png" style="height:40px;width:40px" />
        <div style="display:inline-block;width:100%;position:relative;left:30px;top:-14px">
            ${ganttProperties.resourceNames}
        </div>
    </div>
    ${/if}
</script>
```

> Column templates use `type="text/x-jsrender"` script blocks. Reference them by script ID string on the `template` attribute (e.g., `template="#columnTemplate"`). There is no `<e-gantt-column-template>` child tag. Access gantt-computed values via `${ganttProperties.resourceNames}`, and raw task fields via `${TaskName}`, `${Progress}`, etc.

---

## Column Type

Use `columns.type` to specify the data type of a column. If `format` is defined, Gantt uses `type` to choose between number or date formatting.

Supported types:
- `string`
- `number`
- `boolean`
- `date`
- `dateTime`

```cshtml
<e-gantt-column field="Progress"  type="number"  headerText="Progress" width="100"></e-gantt-column>
<e-gantt-column field="StartDate" type="date"    headerText="Start Date" format="yMd" width="120"></e-gantt-column>
<e-gantt-column field="IsActive"  type="boolean" headerText="Active" displayAsCheckBox="true" width="80"></e-gantt-column>
```

> If `type` is not defined, it is inferred from the first record of `dataSource`. If the first record has a `null`/blank value for a column, you must explicitly define the `type` for that column.

---

## Column Format

Format cell values using the `columns.format` property. Gantt uses the [Internationalization](../../common/internationalization/) library for number and date values. Default culture is `en-US`.

### Number Formatting

| Format | Description |
|--------|-------------|
| `N` | Numeric — followed by precision integer (e.g., `N2`, `N3`) |
| `C` | Currency — followed by precision integer (e.g., `C2`, `C3`) |
| `P` | Percentage — input must be in range 0–100; `0.2` renders as `20%` (e.g., `P2`) |

```cshtml
<e-gantt-column field="Cost"     format="C2" textAlign="Right" width="100"></e-gantt-column>
<e-gantt-column field="Progress" format="P2" textAlign="Right" width="100"></e-gantt-column>
```

### Date Formatting

Pass a built-in skeleton string or a custom format object to `format`:

| Format | Rendered Value |
|--------|---------------|
| `{ type:'date', format:'dd/MM/yyyy' }` | 04/07/2019 |
| `{ type:'date', format:'dd.MM.yyyy' }` | 04.07.2019 |
| `{ type:'date', skeleton:'short' }` | 7/4/19 |
| `{ type:'dateTime', format:'dd/MM/yyyy hh:mm a' }` | 04/07/2019 12:00 AM |
| `{ type:'dateTime', format:'MM/dd/yyyy hh:mm:ss a' }` | 07/04/2019 12:00:00 AM |

```cshtml
@* Built-in skeleton *@
<e-gantt-column field="StartDate" format="yMd" width="120"></e-gantt-column>

@* Custom format object — pass as a JSON string *@
<e-gantt-column field="StartDate" format="{ type:'date', format:'dd/MM/yyyy' }" width="120"></e-gantt-column>
```

---

## Checkbox Column

Add a boolean checkbox column for selection or boolean data fields:

```cshtml
<e-gantt-column field="IsCompleted" headerText="Done"
                editType="booleanedit"
                displayAsCheckBox="true"
                width="80" textAlign="Center">
</e-gantt-column>
```

To show a selection checkbox column (for multi-select), use the built-in `checkboxSelection` property on the Gantt itself:

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.DataSource"
           height="450px" checkboxSelection="true">
    ...
</ejs-gantt>
```

---

## Show or Hide Columns Dynamically

Use the `showColumn` and `hideColumn` methods to toggle column visibility programmatically via external buttons. Pass the column's `headerText` value to identify the column.

```cshtml
<button onclick="show()">Show</button>
<button onclick="hide()">Hide</button>

<ejs-gantt id="Gantt" dataSource="ViewBag.DataSource" height="450px">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
                        endDate="EndDate" duration="Duration" progress="Progress"
                        child="SubTasks">
    </e-gantt-taskfields>
    <e-gantt-columns>
        <e-gantt-column field="TaskId"    headerText="Task Id"   width="100"></e-gantt-column>
        <e-gantt-column field="TaskName"  headerText="Task Name" width="250"></e-gantt-column>
        <e-gantt-column field="StartDate" headerText="Start Date" width="150"></e-gantt-column>
        <e-gantt-column field="Duration"  headerText="Duration"   width="100"></e-gantt-column>
        <e-gantt-column field="Progress"  headerText="Progress"   width="100"></e-gantt-column>
    </e-gantt-columns>
</ejs-gantt>

<script>
function show() {
    var ganttObj = document.getElementById('Gantt').ej2_instances[0];
    ganttObj.showColumn(['Progress']);   // pass headerText values
}
function hide() {
    var ganttObj = document.getElementById('Gantt').ej2_instances[0];
    ganttObj.hideColumn(['Progress']);  // pass headerText values
}
</script>
```

> Pass the column **headerText** (not the field name) to `showColumn`/`hideColumn`.

---

## Controlling Column Actions

Enable or disable specific Gantt actions per column using the following boolean properties on `<e-gantt-column>`:

| Property | Description |
|----------|-------------|
| `allowFiltering` | Enables or disables filtering for the column |
| `allowSorting` | Enables or disables sorting for the column |
| `allowReordering` | Enables or disables drag reordering for the column |
| `allowEditing` | Enables or disables cell editing for the column |

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.DataSource" height="450px"
           allowFiltering="true" allowSorting="true"
           allowReordering="true">
    <e-gantt-editsettings allowEditing="true"></e-gantt-editsettings>
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
                        endDate="EndDate" duration="Duration" progress="Progress"
                        child="SubTasks">
    </e-gantt-taskfields>
    <e-gantt-columns>
        <e-gantt-column field="TaskId"    headerText="Task Id"    isPrimaryKey="true" width="100"></e-gantt-column>
        <e-gantt-column field="TaskName"  headerText="Task Name"  allowSorting="false"   allowFiltering="false" width="250"></e-gantt-column>
        <e-gantt-column field="StartDate" headerText="Start Date" allowEditing="false" width="150"></e-gantt-column>
        <e-gantt-column field="Duration"  headerText="Duration"   allowReordering="false" width="100"></e-gantt-column>
        <e-gantt-column field="Progress"  headerText="Progress"   width="100"></e-gantt-column>
    </e-gantt-columns>
</ejs-gantt>
```

---

## Frozen Columns

Pin columns to the left or right so they remain visible when scrolling horizontally. Use the `frozenColumns` property on `<ejs-gantt>` to freeze the first N columns from the left:

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.DataSource"
           frozenColumns="2" height="450px">
    ...
</ejs-gantt>
```

### Freeze Particular Columns

Use the `isFrozen` property at the column level to freeze a specific column at any desired index on the left side:

```cshtml
<e-gantt-column field="TaskId"   isFrozen="true" width="60"></e-gantt-column>
<e-gantt-column field="TaskName" isFrozen="true" width="200"></e-gantt-column>
```

### Freeze Direction

Use the `freeze` property to position frozen columns on the left, right, or in a fixed position:

| Value | Description |
|-------|-------------|
| `Left` | Freezes the column on the left side |
| `Right` | Freezes the column on the right side |
| `Fixed` | Locks the column at a fixed position — always visible during horizontal scroll |

```cshtml
<e-gantt-column field="TaskId"    freeze="Left"  width="60"></e-gantt-column>
<e-gantt-column field="TaskName"  freeze="Left"  width="200"></e-gantt-column>
<e-gantt-column field="StartDate" width="120"></e-gantt-column>
<e-gantt-column field="Progress"  freeze="Fixed" width="100"></e-gantt-column>
<e-gantt-column field="Resources" freeze="Right" width="150"></e-gantt-column>
```

> The `freeze` direction is not compatible when both `isFrozen` and `frozenColumns` properties are enabled simultaneously.

### Change Default Frozen Line Color

Customize the frozen border color using CSS:

```css
/* Left frozen columns */
.e-gantt .e-leftfreeze.e-freezeleftborder {
    border-right-color: rgb(0, 255, 0) !important;
}

/* Right frozen columns */
.e-gantt .e-rightfreeze.e-freezerightborder {
    border-left-color: rgb(0, 0, 255) !important;
}

/* Fixed frozen columns — both left and right borders */
.e-gantt .e-leftfreeze.e-freezeleftborder {
    border-right-color: rgb(0, 255, 0) !important;
}
.e-gantt .e-rightfreeze.e-freezerightborder {
    border-left-color: rgb(0, 0, 255) !important;
}
```

---

## Column Reordering

Allow users to drag and drop columns to reorder them. Set `allowReordering="true"` on `<ejs-gantt>`:

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.DataSource"
           allowReordering="true" height="450px">
    ...
</ejs-gantt>
```

> To disable reordering for a specific column, set `allowReordering="false"` on that `<e-gantt-column>`.

### Reorder Events

| Event | Trigger |
|-------|---------|
| `columnDragStart` | When column header drag starts |
| `columnDrag` | While column header is being dragged continuously |
| `columnDrop` | When a column header is dropped on the target column |

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.DataSource"
           allowReordering="true"
           columnDragStart="onColumnDragStart"
           columnDrag="onColumnDrag"
           columnDrop="onColumnDrop"
           height="450px">
    ...
</ejs-gantt>

<script>
function onColumnDragStart(args) { console.log('Drag started', args.column.headerText); }
function onColumnDrag(args)      { console.log('Dragging',      args.column.headerText); }
function onColumnDrop(args)      { console.log('Dropped',       args.column.headerText); }
</script>
```

### Reorder Multiple Columns

Use the `reorderColumns` method to reorder multiple columns at once programmatically:

```javascript
var ganttObj = document.getElementById('Gantt').ej2_instances[0];
// Move 'TaskId' and 'TaskName' columns to position after 'Duration'
ganttObj.reorderColumns(['TaskId', 'TaskName'], 'Duration');
```

---

## Column Resizing

Allow users to resize column widths by clicking and dragging the right edge of the column header. Double-clicking the right edge auto-fits the column to its widest cell content. Set `allowResizing="true"` on `<ejs-gantt>`:

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.DataSource"
           allowResizing="true" height="450px">
    ...
</ejs-gantt>
```

> To disable resizing for a specific column, set `allowResizing="false"` on that `<e-gantt-column>`.

### Minimum and Maximum Column Width

Restrict how far a column can be resized using `minWidth` and `maxWidth` per column:

```cshtml
<e-gantt-column field="TaskName" minWidth="100" maxWidth="400" width="200"></e-gantt-column>
<e-gantt-column field="Duration" minWidth="60"  maxWidth="200" width="100"></e-gantt-column>
```

### Touch Interaction

On touch devices, tapping the right edge of a column header reveals a floating resize handler. Drag the floating handler to resize the column.

---

## Column Spanning

Merge adjacent cells in a row to span multiple columns using the `queryCellInfo` event:

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.DataSource"
           queryCellInfo="queryCellInfo" height="450px">
    ...
</ejs-gantt>

<script>
function queryCellInfo(args) {
    if (args.column.field === 'TaskName' && args.data.TaskId === 1) {
        args.colSpan = 2;  // span TaskName across 2 columns
    }
}
</script>
```

---

## Responsive Columns

Hide columns on smaller screen widths using `hideAtMedia`:

```cshtml
<e-gantt-column field="Notes" headerText="Notes"
                hideAtMedia="(max-width: 768px)" width="200">
</e-gantt-column>
```

The column is automatically hidden when the viewport width matches the media query.

---

## WBS Column

Display a Work Breakdown Structure (WBS) code column — automatically generated hierarchy codes. Enable WBS with `enableWBS="true"` and `enableAutoWbsUpdate="true"` on `<ejs-gantt>`, then add `WBSCode` and `WBSPredecessor` columns. Use a flat (parent-ID) data binding with `parentID` in task fields:

```cshtml
<ejs-gantt id="GanttChart" dataSource="ViewBag.DataSource"
           enableWBS="true" enableAutoWbsUpdate="true"
           treeColumnIndex="2" height="550px"
           allowSorting="true" allowFiltering="true"
           toolbar="@(new List<string>() { "Add","Edit","Update","Delete","Cancel","ExpandAll","CollapseAll","Indent","Outdent" })">
    <e-gantt-taskfields id="TaskId" name="TaskName" startDate="StartDate"
                        duration="Duration" progress="Progress"
                        dependency="Predecessor" parentID="ParentID">
    </e-gantt-taskfields>
    <e-gantt-editsettings allowAdding="true" allowEditing="true" allowDeleting="true"
                          allowTaskbarEditing="true" showDeleteConfirmDialog="true">
    </e-gantt-editsettings>
    <e-gantt-splittersettings columnIndex="4"></e-gantt-splittersettings>
    <e-gantt-columns>
        <e-gantt-column field="TaskId"         headerText="Task ID"          visible="false"></e-gantt-column>
        <e-gantt-column field="WBSCode"        headerText="WBS Code"         width="150px"></e-gantt-column>
        <e-gantt-column field="TaskName"       headerText="Task Name"        width="260px"></e-gantt-column>
        <e-gantt-column field="StartDate"      headerText="Start Date"       width="140px"></e-gantt-column>
        <e-gantt-column field="WBSPredecessor" headerText="WBS Predecessor"  width="190px"></e-gantt-column>
        <e-gantt-column field="Duration"       headerText="Duration"         allowEditing="false"></e-gantt-column>
        <e-gantt-column field="Progress"       headerText="Progress"></e-gantt-column>
    </e-gantt-columns>
</ejs-gantt>
```

| Property | Description |
|---|---|
| Property | Description |
|---|---|
| `enableWBS` | Enables WBS code generation |
| `enableAutoWbsUpdate` | Automatically recalculates WBS codes when tasks are reordered via drag-and-drop |
| `WBSCode` | Auto-generated column field (e.g., `1`, `1.1`, `1.1.1`) |
| `WBSPredecessor` | Auto-generated column that shows predecessors using WBS codes instead of task IDs |

> WBS requires **flat (parent-ID) data binding** — use `parentID` in `<e-gantt-taskfields>` instead of `child`. There is no `<e-gantt-wbscodegenerationsettings>` tag; WBS is enabled via `enableWBS` and `enableAutoWbsUpdate` attributes on `<ejs-gantt>` directly.

### Managing WBS Code Updates

For better performance, control when WBS codes are updated using the `ActionBegin` and `DataBound` events. This is useful to enable auto-update only during specific actions (e.g., row drag-and-drop):

```cshtml
<ejs-gantt id="GanttChart" dataSource="ViewBag.DataSource"
           enableWBS="true" enableAutoWbsUpdate="false"
           actionBegin="actionBegin"
           dataBound="dataBound"
           height="550px">
    ...
</ejs-gantt>

<script>
var isDragAction = false;
function actionBegin(args) {
    if (args.requestType === 'rowDragAndDrop') {
        isDragAction = true;
        var gantt = document.getElementById('GanttChart').ej2_instances[0];
        gantt.enableAutoWbsUpdate = true;
    }
}
function dataBound() {
    if (isDragAction) {
        var gantt = document.getElementById('GanttChart').ej2_instances[0];
        gantt.enableAutoWbsUpdate = false;
        isDragAction = false;
    }
}
</script>
```

### WBS Limitations

- Editing the `WBSCode` and `WBSPredecessor` columns is **not supported**.
- **Load on demand** is not supported with the WBS feature.
- `WBSCode` and `WBSPredecessor` fields **cannot be mapped** directly from the data source.

---

## Column Menu

Show a context menu on column headers for sorting, filtering, and autofit. Set `showColumnMenu="true"` on `<ejs-gantt>`:

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.DataSource"
           showColumnMenu="true"
           allowSorting="true"
           allowFiltering="true"
           height="450px">
    ...
</ejs-gantt>
```

Default column menu items:

| Item | Description |
|------|-------------|
| `SortAscending` | Sort the column in ascending order |
| `SortDescending` | Sort the column in descending order |
| `AutoFit` | Auto-fit the current column width |
| `AutoFitAll` | Auto-fit all columns |
| `Filter` | Show the filter option based on `filterSettings.type` |

> To disable the column menu for a specific column, set `showColumnMenu="false"` on that `<e-gantt-column>`.

### Column Menu Events

| Event | Trigger |
|-------|---------|
| `columnMenuOpen` | Before the column menu opens |
| `columnMenuClick` | When the user clicks a column menu item |

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.DataSource"
           showColumnMenu="true"
           columnMenuOpen="onColumnMenuOpen"
           columnMenuClick="onColumnMenuClick"
           height="450px">
    ...
</ejs-gantt>

<script>
function onColumnMenuOpen(args)  { console.log('Menu opening for:', args.column.headerText); }
function onColumnMenuClick(args) { console.log('Menu item clicked:', args.item.text); }
</script>
```

### Custom Column Menu Items

Add custom items to the column menu via `columnMenuItems`. Handle click actions in the `columnMenuClick` event:

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.DataSource"
           showColumnMenu="true"
           columnMenuItems="@(new List<object> { new { text = "Clear Sorting", id = "clearSorting" } })"
           columnMenuClick="onColumnMenuClick"
           height="450px">
    ...
</ejs-gantt>

<script>
function onColumnMenuClick(args) {
    if (args.item.id === 'clearSorting') {
        var ganttObj = document.getElementById('Gantt').ej2_instances[0];
        ganttObj.clearSorting();
    }
}
</script>
```

### Customize Menu Items Per Column

Hide specific menu items for particular columns using `columnMenuOpen` and the `columnMenuOpenEventArgs.hide` flag:

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.DataSource"
           showColumnMenu="true"
           allowFiltering="true"
           columnMenuOpen="onColumnMenuOpen"
           height="450px">
    ...
</ejs-gantt>

<script>
// Hide the 'Filter' item when the menu opens on the 'Task Name' column
function onColumnMenuOpen(args) {
    for (var item of args.items) {
        if (item.text === 'Filter' && args.column.headerText === 'Task Name') {
            args.hide = true;
        }
    }
}
</script>
```

---

## TreeColumnIndex

Control which column displays the expand/collapse tree icon (default is index 0 = first column):

```cshtml
<ejs-gantt id="Gantt" dataSource="ViewBag.DataSource"
           treeColumnIndex="1"
           height="450px">
    <e-gantt-columns>
        <e-gantt-column field="TaskId"   headerText="ID" width="60"></e-gantt-column>
        <e-gantt-column field="TaskName" headerText="Task Name" width="250"></e-gantt-column>
    </e-gantt-columns>
    ...
</ejs-gantt>
```

> Setting `treeColumnIndex="1"` puts the tree expand icon on "Task Name" instead of "ID".
