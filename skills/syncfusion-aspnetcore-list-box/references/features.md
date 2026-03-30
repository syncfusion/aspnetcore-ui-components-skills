# Advanced Features and Configurations

## Table of Contents
- [Drag and Drop](#drag-and-drop)
- [Icons and Custom Templates](#icons-and-custom-templates)
- [Dual List Box](#dual-list-box)
- [Sorting and Grouping](#sorting-and-grouping)
- [Scroller Configuration](#scroller-configuration)
- [Complex Selection Scenarios](#complex-selection-scenarios)
- [Event Handling Best Practices](#event-handling-best-practices)

## Drag and Drop

Enable users to reorder items by dragging.

### Basic Drag and Drop

```cshtml
<ejs-listbox id="draggableListBox" 
             dataSource="@Model.Items"
             allowDragAndDrop="true">
</ejs-listbox>
```

**Result:** Users can drag items within the same list to reorder.

### Drag Between Two Lists

Enable drag-drop between separate ListBox instances:

```csharp
// Page Model
public List<string> AvailableItems { get; set; }
public List<string> SelectedItems { get; set; }

public void OnGet()
{
    AvailableItems = new List<string> { "Item 1", "Item 2", "Item 3", "Item 4" };
    SelectedItems = new List<string> { "Item 5" };
}
```

```cshtml
<div class="row">
    <div class="col-md-6">
        <h5>Available</h5>
        <ejs-listbox id="availableListBox" 
                     dataSource="@Model.AvailableItems"
                     allowDragAndDrop="true"
                     dragStart="onDragStart">
        </ejs-listbox>
    </div>
    <div class="col-md-6">
        <h5>Selected</h5>
        <ejs-listbox id="selectedListBox" 
                     dataSource="@Model.SelectedItems"
                     allowDragAndDrop="true">
        </ejs-listbox>
    </div>
</div>

<script>
    function onDragStart(args) {
        // Optional: Handle drag start event
        console.log('Dragging: ' + args.text);
    }
</script>
```

### Scope Drag and Drop

Control which lists items can be dragged to:

```cshtml
<!-- ListBox A: Can drag to ListBox B -->
<ejs-listbox id="listA" 
             dataSource="@Model.ItemsA"
             allowDragAndDrop="true"
             scope="shared">
</ejs-listbox>

<!-- ListBox B: Can receive from ListBox A -->
<ejs-listbox id="listB" 
             dataSource="@Model.ItemsB"
             allowDragAndDrop="true"
             scope="shared">
</ejs-listbox>
```

## Icons and Custom Templates

### Display Icons with Items

```csharp
public class MenuItem
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string IconClass { get; set; }  // Font Awesome or Material Icons
}

public List<MenuItem> MenuItems { get; set; }

public void OnGet()
{
    MenuItems = new List<MenuItem>
    {
        new MenuItem { Id = 1, Title = "Home", IconClass = "fa-home" },
        new MenuItem { Id = 2, Title = "Settings", IconClass = "fa-cog" },
        new MenuItem { Id = 3, Title = "Help", IconClass = "fa-question" }
    };
}
```

```cshtml
<!-- Add Font Awesome to _Layout.cshtml -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" />

<!-- ListBox with icons -->
<ejs-listbox id="menuListBox" 
             dataSource="@Model.MenuItems"
             fields="@(new Syncfusion.EJ2.DropDowns.ListBoxFieldSettings { Text = "Title", Value = "Id" })"
             itemTemplate="<span class='menu-item'><i class='fas ${IconClass}' style='margin-right: 8px;'></i>${Title}</span>">
</ejs-listbox>

<style>
    .menu-item {
        display: flex;
        align-items: center;
        padding: 8px 12px;
    }
</style>
```

### Template with Complex HTML

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
    public string Image { get; set; }
}

public List<Product> Products { get; set; }
```

```cshtml
<ejs-listbox id="productListBox" 
             dataSource="@Model.Products"
             fields="@(new Syncfusion.EJ2.DropDowns.ListBoxFieldSettings { Text = "Name", Value = "Id" })"
             itemTemplate="@Html.Raw("<div class='product-item'><img src='${Image}' alt='${Name}' /><div><span>${Name}</span><span style='color: #999;'>\$${Price}</span></div></div>")">
</ejs-listbox>

<style>
    .product-item {
        display: flex;
        gap: 12px;
        padding: 8px;
        align-items: center;
    }
    
    .product-item img {
        width: 40px;
        height: 40px;
        object-fit: cover;
        border-radius: 4px;
    }
</style>
```

### Group Templates

```cshtml
<!-- For grouped data, customize group headers -->
<ejs-listbox id="groupedListBox" 
             dataSource="@Model.GroupedItems"
             groupTemplate="<strong>${Text}</strong>">
</ejs-listbox>
```

## Dual List Box

A common UI pattern: two lists with transfer buttons between them.

### Implementation

```csharp
public class TransferListModel
{
    public List<Item> AvailableItems { get; set; }
    public List<Item> SelectedItems { get; set; }
}

public class Item
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```

```csharp
// Page Model
public TransferListModel TransferModel { get; set; }

public void OnGet()
{
    TransferModel = new TransferListModel
    {
        AvailableItems = new List<Item>
        {
            new Item { Id = 1, Name = "Feature A" },
            new Item { Id = 2, Name = "Feature B" },
            new Item { Id = 3, Name = "Feature C" }
        },
        SelectedItems = new List<Item>
        {
            new Item { Id = 4, Name = "Feature D" }
        }
    };
}
```

```cshtml
<div class="transfer-list-container">
    <div class="list-section">
        <label>Available</label>
        <ejs-listbox id="availableList" 
                     dataSource="@Model.TransferModel.AvailableItems"
                     fields="@(new Syncfusion.EJ2.DropDowns.ListBoxFieldSettings { Text = "Name", Value = "Id" })"
                     selectionSettings="@(new Syncfusion.EJ2.DropDowns.ListBoxSelectionSettings { Mode = Syncfusion.EJ2.DropDowns.SelectionMode.Multiple })">
        </ejs-listbox>
    </div>
    
    <div class="button-section">
        <button onclick="moveRight()" class="btn btn-primary">→</button>
        <button onclick="moveLeft()" class="btn btn-primary">←</button>
        <button onclick="moveAllRight()" class="btn btn-secondary">⇒ All</button>
        <button onclick="moveAllLeft()" class="btn btn-secondary">⇐ All</button>
    </div>
    
    <div class="list-section">
        <label>Selected</label>
        <ejs-listbox id="selectedList" 
                     dataSource="@Model.TransferModel.SelectedItems"
                     fields="@(new Syncfusion.EJ2.DropDowns.ListBoxFieldSettings { Text = "Name", Value = "Id" })"
                     selectionSettings="@(new Syncfusion.EJ2.DropDowns.ListBoxSelectionSettings { Mode = Syncfusion.EJ2.DropDowns.SelectionMode.Multiple })">
        </ejs-listbox>
    </div>
</div>

<script>
    function moveRight() {
        var availableList = document.getElementById('availableList').ej2_instances[0];
        var selectedList = document.getElementById('selectedList').ej2_instances[0];
        
        if (availableList.value && availableList.value.length > 0) {
            var itemsToMove = availableList.value;
            
            // Remove from available
            availableList.dataSource = availableList.dataSource.filter(
                item => !itemsToMove.includes(item.Id)
            );
            
            // Add to selected
            var movedItems = availableList.dataSource.filter(
                item => itemsToMove.includes(item.Id)
            );
            selectedList.dataSource = [...selectedList.dataSource, ...movedItems];
        }
    }
    
    function moveAllRight() {
        var availableList = document.getElementById('availableList').ej2_instances[0];
        var selectedList = document.getElementById('selectedList').ej2_instances[0];
        
        selectedList.dataSource = [...selectedList.dataSource, ...availableList.dataSource];
        availableList.dataSource = [];
    }
    
    // Implement moveLeft and moveAllLeft similarly
</script>

<style>
    .transfer-list-container {
        display: flex;
        gap: 20px;
        align-items: center;
        margin: 20px 0;
    }
    
    .list-section {
        flex: 1;
    }
    
    .list-section label {
        display: block;
        margin-bottom: 8px;
        font-weight: bold;
    }
    
    .button-section {
        display: flex;
        flex-direction: column;
        gap: 8px;
    }
</style>
```

## Sorting and Grouping

### Enable Built-in Sorting

Sort data before binding or after:

```csharp
// Sort before binding
public List<string> SortedItems { get; set; }

public void OnGet()
{
    var items = new List<string> { "Zebra", "Apple", "Mango", "Banana" };
    SortedItems = items.OrderBy(x => x).ToList();  // ["Apple", "Banana", "Mango", "Zebra"]
}
```

### Grouping via Data Structure

ListBox doesn't have built-in grouping, but simulate with separators:

```csharp
public class GroupedItem
{
    public string Name { get; set; }
    public bool IsGroup { get; set; }
    public string CssClass { get; set; }
}

public List<GroupedItem> GroupedData { get; set; }

public void OnGet()
{
    GroupedData = new List<GroupedItem>
    {
        new GroupedItem { Name = "Fruits", IsGroup = true, CssClass = "group-header" },
        new GroupedItem { Name = "Apple", IsGroup = false },
        new GroupedItem { Name = "Banana", IsGroup = false },
        
        new GroupedItem { Name = "Vegetables", IsGroup = true, CssClass = "group-header" },
        new GroupedItem { Name = "Carrot", IsGroup = false },
        new GroupedItem { Name = "Potato", IsGroup = false }
    };
}
```

```cshtml
<ejs-listbox id="groupedList" 
             dataSource="@Model.GroupedData"
             itemTemplate="@Html.Raw("<span class='${CssClass}'>${Name}</span>")"
             select="onItemSelect">
</ejs-listbox>

<script>
    function onItemSelect(args) {
        var listbox = document.getElementById('groupedList').ej2_instances[0];
        var isGroup = listbox.dataSource[args.index].IsGroup;
        
        if (isGroup) {
            // Prevent selecting group headers
            listbox.value = listbox.previousValue;
        }
    }
</script>

<style>
    .group-header {
        font-weight: bold;
        font-size: 14px;
        color: #666;
        background-color: #f5f5f5;
        padding: 8px 12px;
        display: block;
    }
</style>
```

## Scroller Configuration

Handle large datasets efficiently with scrolling.

### Enable Virtualization (Auto-scrolling)

For large lists, enable virtual scrolling to improve performance:

```cshtml
<ejs-listbox id="largeListBox" 
             dataSource="@Model.LargeDataset"
             height="300px">
</ejs-listbox>

<style>
    #largeListBox {
        overflow: auto;
        border: 1px solid #ddd;
    }
</style>
```

When the list exceeds the container height, a scrollbar appears automatically.

### Scroll Events

```cshtml
<ejs-listbox id="scrollableList" 
             dataSource="@Model.Items"
             scroll="onListScroll"
             height="400px">
</ejs-listbox>

<script>
    function onListScroll(args) {
        console.log('Scroll position:', args.scrollTop);
        
        // Load more data when scrolling near bottom
        if (args.scrollTop > (args.element.scrollHeight - args.element.clientHeight - 100)) {
            console.log('Near bottom - load more items');
        }
    }
</script>
```

### Infinite Scroll Pattern

```javascript
let currentPage = 1;
const listbox = document.getElementById('infiniteScrollList').ej2_instances[0];

function loadMoreData() {
    fetch(`/api/items?page=${currentPage}&pageSize=20`)
        .then(r => r.json())
        .then(data => {
            listbox.dataSource = [...listbox.dataSource, ...data];
            currentPage++;
        });
}

// Detect scroll near bottom
listbox.element.addEventListener('scroll', function() {
    if (this.scrollTop + this.clientHeight >= this.scrollHeight - 100) {
        loadMoreData();
    }
});
```

## Complex Selection Scenarios

### Scenario 1: Conditional Selection

Allow selection only based on certain conditions:

```javascript
var listbox = document.getElementById('conditionalListBox').ej2_instances[0];

listbox.addEventListener('select', function(args) {
    // Only allow selecting premium items
    var item = listbox.dataSource.find(x => x.Id === args.value);
    
    if (item.Type !== 'premium') {
        args.preventDefault();
        alert('Only premium items can be selected');
    }
});
```

### Scenario 2: Dependent Selections

Selecting one item affects others:

```javascript
var listbox = document.getElementById('dependentList').ej2_instances[0];

listbox.addEventListener('change', function(args) {
    var selectedId = args.value;
    
    // Automatically select related items
    var relatedIds = getRelatedItems(selectedId);
    listbox.value = [...listbox.value, ...relatedIds];
});
```

### Scenario 3: Maximum Selection Limit

Limit users to selecting N items:

```javascript
const MAX_SELECTIONS = 3;
var listbox = document.getElementById('limitedList').ej2_instances[0];

listbox.addEventListener('change', function(args) {
    if (listbox.value.length > MAX_SELECTIONS) {
        alert(`Maximum ${MAX_SELECTIONS} items can be selected`);
        listbox.value = listbox.value.slice(0, MAX_SELECTIONS);
    }
});
```

## Event Handling Best Practices

### Debounce Heavy Operations

```javascript
function debounce(func, delay) {
    let timeout;
    return function(...args) {
        clearTimeout(timeout);
        timeout = setTimeout(() => func(...args), delay);
    };
}

var listbox = document.getElementById('myListBox').ej2_instances[0];

// Debounce change event to avoid excessive API calls
listbox.addEventListener('change', debounce(function(args) {
    // Save to server
    saveSelectionToServer(args.value);
}, 500));
```

### Error Handling for Events

```javascript
listbox.addEventListener('change', function(args) {
    try {
        processSelection(args.value);
    } catch (error) {
        console.error('Selection error:', error);
        // Revert selection
        listbox.value = listbox.previousValue;
    }
});
```

### Remove Event Listeners When Destroying

```javascript
function cleanupListBox() {
    var listbox = document.getElementById('myListBox').ej2_instances[0];
    
    // Remove all event listeners
    listbox.destroy();
    
    // Or specific listener
    listbox.off('change', eventHandler);
}
```

## Next Steps

- **Selection** → Combine features with selection modes in [references/selection-modes.md](selection-modes.md)
- **Data Binding** → Load complex data for features in [references/data-binding.md](data-binding.md)
- **Styling** → Style featured items in [references/styling-and-appearance.md](styling-and-appearance.md)
- **Common Tasks** → Practical patterns in [references/howto-common-tasks.md](howto-common-tasks.md)
