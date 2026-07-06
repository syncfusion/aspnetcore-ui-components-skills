# Events — MultiColumn ComboBox

## Table of Contents
- [Selection Events](#selection-events)
- [Filtering Events](#filtering-events)
- [Data Binding Events](#data-binding-events)
- [UI Events](#ui-events)
- [Event Handling Patterns](#event-handling-patterns)

---

## Selection Events

### Select Event

Fires when an item is selected from the dropdown list.

```cshtml
<ejs-multicolumncombobox id="combo"
    dataSource="@ViewBag.employees"
    select="onSelect"
    height="300px">
    <e-multicolumncombobox-fields text="EmployeeName" value="EmployeeID"></e-multicolumncombobox-fields>
    <e-multicolumncombobox-columns>
        <e-multicolumncombobox-column field="EmployeeID" header="ID" width="60px"></e-multicolumncombobox-column>
        <e-multicolumncombobox-column field="EmployeeName" header="Name" width="150px"></e-multicolumncombobox-column>
        <e-multicolumncombobox-column field="Designation" header="Designation" width="120px"></e-multicolumncombobox-column>
    </e-multicolumncombobox-columns>
</ejs-multicolumncombobox>

<script>
function onSelect(args) {
    // args.itemData - Selected item data
    // args.value - Selected value
    // args.text - Selected text
    console.log('Selected:', args.itemData);
    console.log('Value:', args.value);
    console.log('Text:', args.text);
}
</script>
```

### Change Event

Fires when the value of the combo box changes.

```cshtml
<ejs-multicolumncombobox id="combo"
    dataSource="@ViewBag.employees"
    change="onChange"
    height="300px">
    <e-multicolumncombobox-fields text="EmployeeName" value="EmployeeID"></e-multicolumncombobox-fields>
    <e-multicolumncombobox-columns>
        <e-multicolumncombobox-column field="EmployeeID" header="ID" width="60px"></e-multicolumncombobox-column>
        <e-multicolumncombobox-column field="EmployeeName" header="Name" width="150px"></e-multicolumncombobox-column>
    </e-multicolumncombobox-columns>
</ejs-multicolumncombobox>

<script>
function onChange(args) {
    let oldValue = args.previousValue;
    let newValue = args.value;
    
    console.log(`Changed from ${oldValue} to ${newValue}`);
    
    // Perform action based on selection
    loadRelatedData(newValue);
}

function loadRelatedData(employeeId) {
    fetch(`/api/employees/${employeeId}/details`)
        .then(response => response.json())
        .then(data => {
            // Process related data
            console.log('Related data:', data);
        });
}
</script>
```

### CustomValueSpecifier Event

Fires when a custom value is entered.

```cshtml
<ejs-multicolumncombobox id="combo"
    dataSource="@ViewBag.employees"
    customValueSpecifier="onCustomValueSpecifier"
    allowCustom="true"
    height="300px">
    <e-multicolumncombobox-fields text="EmployeeName" value="EmployeeID"></e-multicolumncombobox-fields>
    <e-multicolumncombobox-columns>
        <e-multicolumncombobox-column field="EmployeeID" header="ID" width="60px"></e-multicolumncombobox-column>
        <e-multicolumncombobox-column field="EmployeeName" header="Name" width="150px"></e-multicolumncombobox-column>
    </e-multicolumncombobox-columns>
</ejs-multicolumncombobox>

<script>
function onCustomValueSpecifier(args) {
    // Handle custom value
    let customItem = {
        EmployeeID: 0,
        EmployeeName: args.text
    };
    
    return customItem;
}
</script>
```

---

## Filtering Events

### Filtering Event

Fires during filtering of combo box list items.

```cshtml
<ejs-multicolumncombobox id="combo"
    dataSource="@ViewBag.employees"
    filtering="onFiltering"
    allowFiltering="true"
    height="300px">
    <e-multicolumncombobox-fields text="EmployeeName" value="EmployeeID"></e-multicolumncombobox-fields>
    <e-multicolumncombobox-columns>
        <e-multicolumncombobox-column field="EmployeeID" header="ID" width="60px"></e-multicolumncombobox-column>
        <e-multicolumncombobox-column field="EmployeeName" header="Name" width="150px"></e-multicolumncombobox-column>
    </e-multicolumncombobox-columns>
</ejs-multicolumncombobox>

<script>
function onFiltering(args) {
    let query = args.text;
    console.log('Filtering by:', query);
    
    // Server-side filtering
    if (query.length > 2) {
        fetch(`/api/employees/search?query=${query}`)
            .then(response => response.json())
            .then(data => {
                let combo = document.getElementById('combo').ej2_instances[0];
                combo.dataSource = data;
                combo.refresh();
            });
    }
}
</script>
```

---

## Data Binding Events

### DataBound Event

Fires after data binding is complete.

```cshtml
<ejs-multicolumncombobox id="combo"
    dataSource="@ViewBag.employees"
    dataBound="onDataBound"
    height="300px">
    <e-multicolumncombobox-fields text="EmployeeName" value="EmployeeID"></e-multicolumncombobox-fields>
    <e-multicolumncombobox-columns>
        <e-multicolumncombobox-column field="EmployeeID" header="ID" width="60px"></e-multicolumncombobox-column>
        <e-multicolumncombobox-column field="EmployeeName" header="Name" width="150px"></e-multicolumncombobox-column>
    </e-multicolumncombobox-columns>
</ejs-multicolumncombobox>

<script>
function onDataBound(args) {
    console.log('Data binding completed');
    console.log('Total records:', args.result ? args.result.length : 0);
    
    // Set first item as selected
    let combo = document.getElementById('combo').ej2_instances[0];
    if (combo.dataSource && combo.dataSource.length > 0) {
        combo.value = combo.dataSource[0].EmployeeID;
    }
}
</script>
```

### ActionBegin Event

Fires when grid action begins (like sorting, filtering, paging).

```cshtml
<ejs-multicolumncombobox id="combo"
    dataSource="@ViewBag.employees"
    actionBegin="onActionBegin"
    height="300px">
    <e-multicolumncombobox-fields text="EmployeeName" value="EmployeeID"></e-multicolumncombobox-fields>
    <e-multicolumncombobox-columns>
        <e-multicolumncombobox-column field="EmployeeID" header="ID" width="60px"></e-multicolumncombobox-column>
        <e-multicolumncombobox-column field="EmployeeName" header="Name" width="150px"></e-multicolumncombobox-column>
    </e-multicolumncombobox-columns>
</ejs-multicolumncombobox>

<script>
function onActionBegin(args) {
    if (args.requestType === 'filtering') {
        console.log('Filter request:', args.filterModel);
    }
    if (args.requestType === 'sorting') {
        console.log('Sort request:', args.sortModel);
    }
}
</script>
```

### ActionComplete Event

Fires when grid action completes.

```cshtml
<ejs-multicolumncombobox id="combo"
    dataSource="@ViewBag.employees"
    actionComplete="onActionComplete"
    height="300px">
    <e-multicolumncombobox-fields text="EmployeeName" value="EmployeeID"></e-multicolumncombobox-fields>
    <e-multicolumncombobox-columns>
        <e-multicolumncombobox-column field="EmployeeID" header="ID" width="60px"></e-multicolumncombobox-column>
        <e-multicolumncombobox-column field="EmployeeName" header="Name" width="150px"></e-multicolumncombobox-column>
    </e-multicolumncombobox-columns>
</ejs-multicolumncombobox>

<script>
function onActionComplete(args) {
    console.log('Action completed:', args.requestType);
    console.log('Result count:', args.result ? args.result.length : 0);
}
</script>
```

---

## UI Events

### Focus Event

Fires when the combo box input gets focus.

```cshtml
<ejs-multicolumncombobox id="combo"
    dataSource="@ViewBag.employees"
    focus="onFocus"
    height="300px">
    <e-multicolumncombobox-fields text="EmployeeName" value="EmployeeID"></e-multicolumncombobox-fields>
    <e-multicolumncombobox-columns>
        <e-multicolumncombobox-column field="EmployeeID" header="ID" width="60px"></e-multicolumncombobox-column>
        <e-multicolumncombobox-column field="EmployeeName" header="Name" width="150px"></e-multicolumncombobox-column>
    </e-multicolumncombobox-columns>
</ejs-multicolumncombobox>

<script>
function onFocus(args) {
    console.log('Focus received');
    // Auto-open dropdown when focused
    let combo = document.getElementById('combo').ej2_instances[0];
    combo.showPopup();
}
</script>
```

### Blur Event

Fires when the combo box input loses focus.

```cshtml
<ejs-multicolumncombobox id="combo"
    dataSource="@ViewBag.employees"
    blur="onBlur"
    height="300px">
    <e-multicolumncombobox-fields text="EmployeeName" value="EmployeeID"></e-multicolumncombobox-fields>
    <e-multicolumncombobox-columns>
        <e-multicolumncombobox-column field="EmployeeID" header="ID" width="60px"></e-multicolumncombobox-column>
        <e-multicolumncombobox-column field="EmployeeName" header="Name" width="150px"></e-multicolumncombobox-column>
    </e-multicolumncombobox-columns>
</ejs-multicolumncombobox>

<script>
function onBlur(args) {
    console.log('Focus lost');
    // Validate selection when losing focus
    let combo = document.getElementById('combo').ej2_instances[0];
    if (!combo.value) {
        console.warn('No selection made');
    }
}
</script>
```

### Open Event

Fires when the dropdown popup opens.

```cshtml
<ejs-multicolumncombobox id="combo"
    dataSource="@ViewBag.employees"
    open="onOpen"
    height="300px">
    <e-multicolumncombobox-fields text="EmployeeName" value="EmployeeID"></e-multicolumncombobox-fields>
    <e-multicolumncombobox-columns>
        <e-multicolumncombobox-column field="EmployeeID" header="ID" width="60px"></e-multicolumncombobox-column>
        <e-multicolumncombobox-column field="EmployeeName" header="Name" width="150px"></e-multicolumncombobox-column>
    </e-multicolumncombobox-columns>
</ejs-multicolumncombobox>

<script>
function onOpen(args) {
    console.log('Dropdown opened');
    // Track analytics
    logUserInteraction('combo_opened');
}
</script>
```

### Close Event

Fires when the dropdown popup closes.

```cshtml
<ejs-multicolumncombobox id="combo"
    dataSource="@ViewBag.employees"
    close="onClose"
    height="300px">
    <e-multicolumncombobox-fields text="EmployeeName" value="EmployeeID"></e-multicolumncombobox-fields>
    <e-multicolumncombobox-columns>
        <e-multicolumncombobox-column field="EmployeeID" header="ID" width="60px"></e-multicolumncombobox-column>
        <e-multicolumncombobox-column field="EmployeeName" header="Name" width="150px"></e-multicolumncombobox-column>
    </e-multicolumncombobox-columns>
</ejs-multicolumncombobox>

<script>
function onClose(args) {
    console.log('Dropdown closed');
}
</script>
```

---

## Event Handling Patterns

### Multiple Event Handlers

```cshtml
<ejs-multicolumncombobox id="combo"
    dataSource="@ViewBag.employees"
    select="onSelect"
    change="onChange"
    focus="onFocus"
    blur="onBlur"
    open="onOpen"
    close="onClose"
    height="300px">
    <e-multicolumncombobox-fields text="EmployeeName" value="EmployeeID"></e-multicolumncombobox-fields>
    <e-multicolumncombobox-columns>
        <e-multicolumncombobox-column field="EmployeeID" header="ID" width="60px"></e-multicolumncombobox-column>
        <e-multicolumncombobox-column field="EmployeeName" header="Name" width="150px"></e-multicolumncombobox-column>
    </e-multicolumncombobox-columns>
</ejs-multicolumncombobox>

<script>
let eventLog = [];

function logEvent(eventName, data) {
    eventLog.push({ event: eventName, data: data, timestamp: new Date() });
    console.log(`[${eventName}]`, data);
}

function onSelect(args) {
    logEvent('select', args.itemData);
}

function onChange(args) {
    logEvent('change', { old: args.previousValue, new: args.value });
}

function onFocus(args) {
    logEvent('focus', null);
}

function onBlur(args) {
    logEvent('blur', null);
}

function onOpen(args) {
    logEvent('open', null);
}

function onClose(args) {
    logEvent('close', null);
}

// Get event log
function getEventLog() {
    return eventLog;
}
</script>
```

### Prevent Selection

```cshtml
<ejs-multicolumncombobox id="combo"
    dataSource="@ViewBag.employees"
    select="onSelectWithValidation"
    height="300px">
    <e-multicolumncombobox-fields text="EmployeeName" value="EmployeeID"></e-multicolumncombobox-fields>
    <e-multicolumncombobox-columns>
        <e-multicolumncombobox-column field="EmployeeID" header="ID" width="60px"></e-multicolumncombobox-column>
        <e-multicolumncombobox-column field="EmployeeName" header="Name" width="150px"></e-multicolumncombobox-column>
        <e-multicolumncombobox-column field="Status" header="Status" width="100px"></e-multicolumncombobox-column>
    </e-multicolumncombobox-columns>
</ejs-multicolumncombobox>

<script>
function onSelectWithValidation(args) {
    // Prevent selection of inactive employees
    if (args.itemData.Status === 'Inactive') {
        args.cancel = true;
        alert('Cannot select inactive employees');
    }
}
</script>
```
