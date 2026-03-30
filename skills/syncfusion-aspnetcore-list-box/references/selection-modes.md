# Selection Modes and Interaction

## Table of Contents
- [Overview](#overview)
- [Single Selection Mode](#single-selection-mode)
- [Multiple Selection Mode](#multiple-selection-mode)
- [Selection Events](#selection-events)
- [Keyboard Navigation](#keyboard-navigation)
- [Select All Functionality](#select-all-functionality)
- [Advanced Selection Patterns](#advanced-selection-patterns)

## Overview

The ListBox provides flexible selection mechanisms through the `selectionSettings` property. Choose between:

- **Single**: User selects one item at a time
- **Multiple**: User selects multiple items (default)

Selection behavior is controlled via the `SelectionMode` enum and related configuration options.

## Single Selection Mode

Use single selection when users should pick **exactly one item** (like a radio button).

### Basic Single Selection

```csharp
// ~/Pages/Index.cshtml.cs
using Syncfusion.EJ2.DropDowns;

public class IndexModel : PageModel
{
    public List<string> Categories { get; set; }
    
    public void OnGet()
    {
        Categories = new List<string> 
        { 
            "Electronics", 
            "Clothing", 
            "Books", 
            "Home & Garden" 
        };
    }
}
```

```cshtml
<!-- ~/Pages/Index.cshtml -->
@model IndexModel

<ejs-listbox id="categoryListBox" 
             dataSource="@Model.Categories"
             selectionSettings="@(new ListBoxSelectionSettings 
             { 
                 Mode = SelectionMode.Single 
             })"
             change="onCategoryChange">
</ejs-listbox>

<script>
    function onCategoryChange(args) {
        console.log('Selected category: ' + args.text);
    }
</script>
```

### Getting Selected Value

```javascript
var listbox = document.getElementById('categoryListBox').ej2_instances[0];
var selectedItem = listbox.value;        // Gets value
var selectedText = listbox.text;         // Gets display text
```

### Set Initial Selection

```cshtml
@{ 
    var initialValue = new List<string> { "Electronics" };
}

<ejs-listbox id="categoryListBox" 
             dataSource="@Model.Categories"
             value="@initialValue"
             selectionSettings="@(new ListBoxSelectionSettings 
             { 
                 Mode = SelectionMode.Single 
             })">
</ejs-listbox>
```

## Multiple Selection Mode

Use multiple selection when users should pick **several items** (like checkboxes).

### Basic Multiple Selection

```cshtml
@model IndexModel

<ejs-listbox id="tagsListBox" 
             dataSource="@Model.Tags"
             selectionSettings="@(new ListBoxSelectionSettings 
             { 
                 Mode = SelectionMode.Multiple 
             })"
             change="onTagsChange">
</ejs-listbox>

<script>
    function onTagsChange(args) {
        console.log('Selected items: ' + args.value);
    }
</script>
```

**Default behavior:**
- Hold **CTRL** (Cmd on Mac) to select multiple
- Hold **SHIFT** to select range
- Click to deselect

### Multiple Selection with Checkboxes

Add checkboxes for visual indication:

```cshtml
<ejs-listbox id="tagsListBox" 
             dataSource="@Model.Tags"
             selectionSettings="@(new ListBoxSelectionSettings 
             { 
                 Mode = SelectionMode.Multiple,
                 CheckboxPosition = CheckBoxPosition.Before
             })">
</ejs-listbox>
```

**Options for checkbox position:**
- `CheckBoxPosition.Before` - Checkbox before text
- `CheckBoxPosition.After` - Checkbox after text

### Getting Multiple Selected Values

```javascript
var listbox = document.getElementById('tagsListBox').ej2_instances[0];
var selectedValues = listbox.value;      // Array of values
var selectedTexts = listbox.text;        // Array of display texts

// Example: ["tag1", "tag2", "tag3"]
console.log('Selected count: ' + selectedValues.length);
```

### Set Initial Multiple Selection

```csharp
// ~/Pages/Index.cshtml.cs
public List<string> InitialTags { get; set; }

public void OnGet()
{
    Tags = new List<string> { "JavaScript", "ASP.NET", "React", "Vue", "Angular" };
    InitialTags = new List<string> { "JavaScript", "ASP.NET" };  // Pre-selected
}
```

```cshtml
<ejs-listbox id="tagsListBox" 
             dataSource="@Model.Tags"
             value="@Model.InitialTags"
             selectionSettings="@(new ListBoxSelectionSettings 
             { 
                 Mode = SelectionMode.Multiple 
             })">
</ejs-listbox>
```

## Selection Events

Handle selection changes with event handlers:

### Change Event (Fired on Any Change)

```cshtml
<ejs-listbox id="listBox" 
             dataSource="@Model.Items"
             change="onSelectionChange">
</ejs-listbox>

<script>
    function onSelectionChange(args) {
        console.log('Event Args:');
        console.log('  Index:', args.index);         // Position in list
        console.log('  Text:', args.text);           // Display text
        console.log('  Value:', args.value);         // Item value
        console.log('  Items:', args.items);         // All selected items
        console.log('  IsChecked:', args.isChecked); // For checkbox mode
    }
</script>
```

### Double-Click Event

```cshtml
<ejs-listbox id="listBox" 
             dataSource="@Model.Items"
             doubleClick="onItemDoubleClick">
</ejs-listbox>

<script>
    function onItemDoubleClick(args) {
        console.log('Double-clicked: ' + args.text);
    }
</script>
```

### Select Event (Item Click)

```cshtml
<ejs-listbox id="listBox" 
             dataSource="@Model.Items"
             select="onItemSelect">
</ejs-listbox>

<script>
    function onItemSelect(args) {
        console.log('Item clicked: ' + args.text);
    }
</script>
```

## Keyboard Navigation

The ListBox supports full keyboard interaction:

### Navigation Keys

| Key | Action |
|-----|--------|
| **Arrow Up** | Move to previous item |
| **Arrow Down** | Move to next item |
| **Home** | Jump to first item |
| **End** | Jump to last item |
| **Space** | Select/deselect item (multiple mode) |
| **Ctrl + A** | Select all (multiple mode) |
| **Shift + Arrow** | Select range (multiple mode) |

### Keyboard-First Selection Example

```cshtml
<ejs-listbox id="keyboardListBox" 
             dataSource="@Model.Items"
             selectionSettings="@(new ListBoxSelectionSettings 
             { 
                 Mode = SelectionMode.Multiple,
                 CheckboxPosition = CheckBoxPosition.Before
             })"
             allowDragAndDrop="false">
</ejs-listbox>

<!-- Usage instructions -->
<p>
    Use <strong>Arrow keys</strong> to navigate<br>
    Use <strong>Space</strong> to select/deselect<br>
    Use <strong>Ctrl + A</strong> to select all
</p>
```

### Tab Navigation

The ListBox is tab-accessible:
- **Tab**: Move focus to ListBox
- **Tab again**: Move focus out of ListBox
- Use arrow keys within

## Select All Functionality

### Display Select-All Checkbox

Enable with `showSelectAll`:

```cshtml
<ejs-listbox id="listBoxWithSelectAll" 
             dataSource="@Model.Items"
             selectionSettings="@(new ListBoxSelectionSettings 
             { 
                 Mode = SelectionMode.Multiple,
                 CheckboxPosition = CheckBoxPosition.Before,
                 ShowSelectAll = true
             })">
</ejs-listbox>
```

**Result:** A checkbox appears at the top to select/deselect all items.

### Programmatic Select All

```javascript
var listbox = document.getElementById('listBoxWithSelectAll').ej2_instances[0];

// Select all items
listbox.selectAll();

// Deselect all items
listbox.clearSelection();

// Select specific items
listbox.value = ['item1', 'item2', 'item3'];
```

### Detect Select-All Toggle

```cshtml
<ejs-listbox id="listBox" 
             dataSource="@Model.Items"
             change="onSelectionChange"
             selectionSettings="@(new ListBoxSelectionSettings 
             { 
                 Mode = SelectionMode.Multiple,
                 ShowSelectAll = true
             })">
</ejs-listbox>

<script>
    function onSelectionChange(args) {
        if (args.value.length === @Model.Items.Count) {
            console.log('All items selected');
        } else if (args.value.length === 0) {
            console.log('No items selected');
        } else {
            console.log('Partial selection: ' + args.value.length + ' of @Model.Items.Count');
        }
    }
</script>
```

## Advanced Selection Patterns

### Pattern 1: Master-Detail with Selection

```csharp
// Page Model
public List<Category> Categories { get; set; }
public List<Item> ItemsInCategory { get; set; }

public void OnGet()
{
    Categories = new List<Category> 
    { 
        new Category { Id = 1, Name = "Electronics" },
        new Category { Id = 2, Name = "Books" }
    };
}

public IActionResult OnPostSelectCategory(int categoryId)
{
    ItemsInCategory = GetItemsForCategory(categoryId);
    return new JsonResult(ItemsInCategory);
}
```

```cshtml
<div class="row">
    <div class="col-md-6">
        <h5>Categories</h5>
        <ejs-listbox id="categoryListBox" 
                     dataSource="@Model.Categories"
                     fields="@(new ListBoxFieldSettings { Text = "Name", Value = "Id" })"
                     change="onCategorySelect"
                     selectionSettings="@(new ListBoxSelectionSettings { Mode = SelectionMode.Single })">
        </ejs-listbox>
    </div>
    <div class="col-md-6">
        <h5>Items in Category</h5>
        <ejs-listbox id="itemListBox" dataSource="@new List<Item>()"></ejs-listbox>
    </div>
</div>

<script>
    function onCategorySelect(args) {
        var categoryId = args.value;
        fetch('/Index?handler=SelectCategory&categoryId=' + categoryId)
            .then(r => r.json())
            .then(data => {
                var itemListBox = document.getElementById('itemListBox').ej2_instances[0];
                itemListBox.dataSource = data;
            });
    }
</script>
```

### Pattern 2: Inverse Selection

```javascript
var listbox = document.getElementById('listBox').ej2_instances[0];
var allValues = listbox.dataSource.map(item => item.value);
var selectedValues = listbox.value;
var inverseSelection = allValues.filter(val => !selectedValues.includes(val));

listbox.value = inverseSelection;
```

### Pattern 3: Toggle Selection State

```javascript
function toggleItem(itemValue) {
    var listbox = document.getElementById('listBox').ej2_instances[0];
    var isSelected = listbox.value.includes(itemValue);
    
    if (isSelected) {
        listbox.value = listbox.value.filter(v => v !== itemValue);
    } else {
        listbox.value = [...listbox.value, itemValue];
    }
}
```

## Next Steps

- **Data Binding** → Connect to dynamic data sources in [references/data-binding.md](data-binding.md)
- **Styling** → Customize selection appearance in [references/styling-and-appearance.md](styling-and-appearance.md)
- **Features** → Combine with drag-drop and templates in [references/features.md](features.md)
- **Common Tasks** → See form integration patterns in [references/howto-common-tasks.md](howto-common-tasks.md)
