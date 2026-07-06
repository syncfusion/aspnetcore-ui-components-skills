# Selection Modes — ListBox

## Table of Contents
- [Selection Types](#selection-types)
- [Single Selection](#single-selection)
- [Multiple Selection](#multiple-selection)
- [Checkbox Selection](#checkbox-selection)
- [Getting Selected Items](#getting-selected-items)
- [Setting Selected Items](#setting-selected-items)
- [Events](#events)

---

## Selection Types

| Mode | Type | Use Case |
|------|------|----------|
| `Single` | Default | Choose one option |
| `Multiple` | Default | Choose multiple options (click) |
| `Checkbox` | Visual | Choose multiple options (checkbox) |

---

## Single Selection

### Basic Single Selection

```cshtml
<ejs-listbox id="single"
    dataSource="@ViewBag.items"
    height="300px"
    placeholder="Select one item">
    <e-listbox-fields text="Name" value="Id"></e-listbox-fields>
</ejs-listbox>
```

### Controller

```csharp
public IActionResult Index()
{
    ViewBag.items = new List<object>
    {
        new { Id = 1, Name = "Item 1" },
        new { Id = 2, Name = "Item 2" },
        new { Id = 3, Name = "Item 3" }
    };
    return View();
}
```

### Handle Selection

```javascript
let listbox = document.getElementById('single').ej2_instances[0];

listbox.addEventListener('change', function(args) {
    console.log('Selected:', args.value);
});
```

---

## Multiple Selection

### Multiple Selection (Click-Based)

```cshtml
<ejs-listbox id="multiple"
    dataSource="@ViewBag.items"
    height="300px"
    placeholder="Select multiple items">
    <e-listbox-fields text="Name" value="Id"></e-listbox-fields>
    <e-listbox-selectionsettings mode="Multiple"></e-listbox-selectionsettings>
</ejs-listbox>
```

**How it works:**
- Click item to select
- Click again to deselect
- Hold `Ctrl` and click to multi-select
- Hold `Shift` and click to range-select

### Get Multiple Selected Items

```javascript
let listbox = document.getElementById('multiple').ej2_instances[0];

function getSelected() {
    let selected = listbox.getSelectedItems();
    console.log('Selected items:', selected.text);
    console.log('Selected values:', selected.value);
    console.log('Selected indices:', selected.index);
}
```

---

## Checkbox Selection

### Enable Checkbox Mode

```cshtml
<ejs-listbox id="checkbox"
    dataSource="@ViewBag.items"
    height="300px"
    placeholder="Select with checkboxes">
    <e-listbox-fields text="Name" value="Id"></e-listbox-fields>
    <e-listbox-selectionsettings mode="Multiple" type="Checkbox"></e-listbox-selectionsettings>
</ejs-listbox>
```

**Visual Indicators:**
- ☐ Unchecked
- ☑ Checked
- ▪ Indeterminate (some selected in group)

### Controller Data

```csharp
public class Item
{
    public int Id { get; set; }
    public string Name { get; set; }
    public bool Selected { get; set; } // Pre-select items
}

public IActionResult Index()
{
    ViewBag.items = new List<Item>
    {
        new Item { Id = 1, Name = "JavaScript", Selected = true },
        new Item { Id = 2, Name = "React", Selected = true },
        new Item { Id = 3, Name = "Vue", Selected = false },
        new Item { Id = 4, Name = "Angular", Selected = false }
    };
    return View();
}
```

### View with Pre-Selection

```cshtml
<ejs-listbox id="checkbox"
    dataSource="@ViewBag.items"
    value="@ViewBag.selectedIds"
    height="300px">
    <e-listbox-fields text="Name" value="Id"></e-listbox-fields>
    <e-listbox-selectionsettings mode="Multiple" type="Checkbox"></e-listbox-selectionsettings>
</ejs-listbox>

<script>
function preselectItems() {
    let listbox = document.getElementById('checkbox').ej2_instances[0];
    listbox.value = [1, 2]; // Pre-select items with Id 1 and 2
}

preselectItems();
</script>
```

---

## Getting Selected Items

### Get Selected Items

```javascript
let listbox = document.getElementById('checkbox').ej2_instances[0];

function getSelectedData() {
    let selected = listbox.getSelectedItems();
    
    // Get text values
    let texts = selected.text;  // ["JavaScript", "React"]
    
    // Get actual values
    let values = selected.value;  // [1, 2]
    
    // Get indices
    let indices = selected.index;  // [0, 1]
    
    // Get full data objects
    let data = selected.data;  // [{ Id: 1, Name: "JavaScript" }, ...]
    
    console.log('Texts:', texts);
    console.log('Values:', values);
    console.log('Indices:', indices);
    console.log('Data:', data);
    
    return {
        texts: texts,
        values: values,
        data: data
    };
}
```

### Get Selected Count

```javascript
function countSelected() {
    let listbox = document.getElementById('checkbox').ej2_instances[0];
    let selected = listbox.getSelectedItems();
    let count = selected.value.length;
    
    console.log(`${count} items selected`);
    return count;
}
```

### Get All Items (Selected + Unselected)

```javascript
function getAllItems() {
    let listbox = document.getElementById('checkbox').ej2_instances[0];
    let allItems = listbox.getItems();  // Get all items
    
    console.log('Total items:', allItems.length);
    return allItems;
}
```

---

## Setting Selected Items

### Pre-Select Items on Load

```cshtml
<ejs-listbox id="listbox"
    dataSource="@ViewBag.items"
    value="@ViewBag.preSelectedIds"
    height="300px">
    <e-listbox-fields text="Name" value="Id"></e-listbox-fields>
    <e-listbox-selectionsettings mode="Multiple" type="Checkbox"></e-listbox-selectionsettings>
</ejs-listbox>
```

**Controller:**

```csharp
public IActionResult Index()
{
    ViewBag.items = GetItems();
    ViewBag.preSelectedIds = new List<int> { 1, 3, 5 };  // Pre-select by Id
    return View();
}
```

### Programmatically Select Items

```javascript
let listbox = document.getElementById('listbox').ej2_instances[0];

// Select by value
function selectByValue() {
    listbox.value = [1, 2, 3];  // Select items with Id 1, 2, 3
}

// Select by index
function selectByIndex() {
    listbox.selectItemByIndices([0, 2, 4]);  // Select 1st, 3rd, 5th items
}

// Select all
function selectAll() {
    listbox.selectAll();
}

// Clear selection
function clearSelection() {
    listbox.clearSelection();
}
```

### Selection with Events

```cshtml
<ejs-listbox id="listbox"
    dataSource="@ViewBag.items"
    select="onSelect"
    unSelect="onUnselect"
    selectAll="onSelectAll"
    unSelectAll="onUnselectAll"
    height="300px">
    <e-listbox-fields text="Name" value="Id"></e-listbox-fields>
    <e-listbox-selectionsettings mode="Multiple" type="Checkbox"></e-listbox-selectionsettings>
</ejs-listbox>

<script>
function onSelect(args) {
    console.log('Item selected:', args.text);
}

function onUnselect(args) {
    console.log('Item unselected:', args.text);
}

function onSelectAll(args) {
    console.log('Select all triggered');
}

function onUnselectAll(args) {
    console.log('Unselect all triggered');
}
</script>
```

---

## Events

### Selection Events

```cshtml
<ejs-listbox id="listbox"
    dataSource="@ViewBag.items"
    select="onSelect"
    unSelect="onUnselect"
    selectAll="onSelectAll"
    unSelectAll="onUnselectAll"
    change="onChange"
    height="300px">
    <e-listbox-fields text="Name" value="Id"></e-listbox-fields>
    <e-listbox-selectionsettings mode="Multiple" type="Checkbox"></e-listbox-selectionsettings>
</ejs-listbox>

<div id="log"></div>

<script>
let log = document.getElementById('log');

function onSelect(args) {
    log.innerHTML += `<p>Selected: ${args.text}</p>`;
    console.log('Event: select', args);
}

function onUnselect(args) {
    log.innerHTML += `<p>Unselected: ${args.text}</p>`;
    console.log('Event: unSelect', args);
}

function onSelectAll(args) {
    log.innerHTML += `<p>All items selected</p>`;
    console.log('Event: selectAll', args);
}

function onUnselectAll(args) {
    log.innerHTML += `<p>All items unselected</p>`;
    console.log('Event: unSelectAll', args);
}

function onChange(args) {
    let listbox = document.getElementById('listbox').ej2_instances[0];
    let selected = listbox.getSelectedItems();
    log.innerHTML += `<p>Change event: ${selected.value.length} items now selected</p>`;
    console.log('Event: change', args);
}
</script>
```

| Event | Triggered | Use Case |
|-------|-----------|----------|
| `select` | When item is selected | Track individual selections |
| `unSelect` | When item is deselected | Track deselections |
| `selectAll` | When select all is called | Bulk selection action |
| `unSelectAll` | When deselect all is called | Bulk deselection action |
| `change` | Any selection change | Update UI/save state |

### Advanced: Validation Before Selection

```javascript
let listbox = document.getElementById('listbox').ej2_instances[0];

// Intercept selection
listbox.allowSelection = false; // Disable default

document.getElementById('listbox').addEventListener('mousedown', function(e) {
    if (e.target.closest('.e-list-item')) {
        let item = e.target.closest('.e-list-item');
        let text = item.textContent;
        
        // Validate
        if (validateSelection(text)) {
            listbox.selectItem(item);
        } else {
            alert('Cannot select this item');
        }
    }
});

function validateSelection(itemText) {
    // Custom validation logic
    return itemText.length > 0;
}
```
