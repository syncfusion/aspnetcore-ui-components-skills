# Common Tasks and Practical Patterns

## Table of Contents
- [Adding Items Dynamically](#adding-items-dynamically)
- [Removing and Disabling Items](#removing-and-disabling-items)
- [Enabling Scroller](#enabling-scroller)
- [Filtering ListBox Data](#filtering-listbox-data)
- [Form Submission with Selected Values](#form-submission-with-selected-values)
- [Common Troubleshooting](#common-troubleshooting)
- [Best Practices](#best-practices)

## Adding Items Dynamically

### Add Single Item

```javascript
var listbox = document.getElementById('myListBox').ej2_instances[0];

function addItem(newItem) {
    listbox.dataSource.push(newItem);
    listbox.refresh();
}

// Usage
addItem('New Item');
```

### Add Multiple Items

```javascript
function addItems(newItems) {
    listbox.dataSource = [...listbox.dataSource, ...newItems];
    listbox.refresh();
}

// Usage
addItems(['Item 1', 'Item 2', 'Item 3']);
```

### Add from User Input

```cshtml
<div class="input-group">
    <input type="text" id="newItemInput" placeholder="Enter item name" />
    <button onclick="addItemFromInput()" class="btn btn-primary">Add</button>
</div>

<ejs-listbox id="dynamicListBox" dataSource="@Model.Items"></ejs-listbox>

<script>
    function addItemFromInput() {
        var input = document.getElementById('newItemInput');
        var newItem = input.value.trim();
        
        if (newItem) {
            var listbox = document.getElementById('dynamicListBox').ej2_instances[0];
            listbox.dataSource.push(newItem);
            listbox.refresh();
            
            // Clear input
            input.value = '';
            input.focus();
        }
    }
    
    // Allow Enter key to add
    document.getElementById('newItemInput').addEventListener('keypress', function(e) {
        if (e.key === 'Enter') {
            addItemFromInput();
        }
    });
</script>
```

### Add with Validation

```javascript
function addItemWithValidation(newItem) {
    var listbox = document.getElementById('myListBox').ej2_instances[0];
    
    // Check if item already exists
    if (listbox.dataSource.some(item => item.toLowerCase() === newItem.toLowerCase())) {
        alert('Item already exists');
        return false;
    }
    
    // Check if item is empty
    if (!newItem || newItem.trim() === '') {
        alert('Item cannot be empty');
        return false;
    }
    
    // Add item
    listbox.dataSource.push(newItem);
    listbox.refresh();
    return true;
}
```

## Removing and Disabling Items

### Remove Selected Item

```javascript
function removeSelected() {
    var listbox = document.getElementById('myListBox').ej2_instances[0];
    
    if (listbox.value) {
        listbox.dataSource = listbox.dataSource.filter(
            item => item !== listbox.value
        );
        listbox.refresh();
    }
}

// Usage
<button onclick="removeSelected()">Remove Selected</button>
```

### Remove Multiple Items

```javascript
function removeSelectedItems() {
    var listbox = document.getElementById('myListBox').ej2_instances[0];
    
    if (listbox.value && listbox.value.length > 0) {
        listbox.dataSource = listbox.dataSource.filter(
            item => !listbox.value.includes(item)
        );
        listbox.refresh();
    }
}
```

### Remove Item by Index

```javascript
function removeItemByIndex(index) {
    var listbox = document.getElementById('myListBox').ej2_instances[0];
    
    if (index >= 0 && index < listbox.dataSource.length) {
        listbox.dataSource.splice(index, 1);
        listbox.refresh();
    }
}

// Usage
removeItemByIndex(2);  // Remove 3rd item
```

### Clear All Items

```javascript
function clearAllItems() {
    var listbox = document.getElementById('myListBox').ej2_instances[0];
    
    if (confirm('Clear all items?')) {
        listbox.dataSource = [];
        listbox.refresh();
    }
}
```

### Disable Specific Items

Items in ListBox can be disabled using CSS:

```csharp
public class Item
{
    public int Id { get; set; }
    public string Name { get; set; }
    public bool IsDisabled { get; set; }
}

public List<Item> Items { get; set; }

public void OnGet()
{
    Items = new List<Item>
    {
        new Item { Id = 1, Name = "Available Item", IsDisabled = false },
        new Item { Id = 2, Name = "Disabled Item", IsDisabled = true }
    };
}
```

```cshtml
<ejs-listbox id="selectiveListBox" 
             dataSource="@Model.Items"
             fields="@(new Syncfusion.EJ2.DropDowns.ListBoxFieldSettings { Text = "Name", Value = "Id" })"
             itemTemplate="@Html.Raw("<span class='${IsDisabled ? \"disabled-item\" : \"\"}'>${Name}</span>")"
             select="onItemSelect">
</ejs-listbox>

<script>
    function onItemSelect(args) {
        var listbox = document.getElementById('selectiveListBox').ej2_instances[0];
        var item = listbox.dataSource.find(x => x.Id === args.value);
        
        if (item && item.IsDisabled) {
            args.preventDefault();
        }
    }
</script>

<style>
    .disabled-item {
        color: #999;
        cursor: not-allowed;
        opacity: 0.6;
    }
</style>
```

## Enabling Scroller

### Fixed Height with Scroll

```cshtml
<ejs-listbox id="scrollListBox" 
             dataSource="@Model.LargeItemList"
             height="300px">
</ejs-listbox>

<style>
    #scrollListBox {
        border: 1px solid #ddd;
        overflow-y: auto;
    }
</style>
```

### Responsive Height Scroller

```cshtml
<ejs-listbox id="responsiveScrollList" 
             dataSource="@Model.Items"
             height="400px">
</ejs-listbox>

<style>
    @media (max-width: 768px) {
        #responsiveScrollList {
            height: 250px;
        }
    }
    
    @media (max-width: 480px) {
        #responsiveScrollList {
            height: 200px;
        }
    }
</style>
```

### Scroll to Selected Item

```javascript
function scrollToSelected() {
    var listbox = document.getElementById('myListBox').ej2_instances[0];
    
    if (listbox.value) {
        var selectedIndex = listbox.dataSource.findIndex(
            item => item === listbox.value
        );
        
        // Calculate scroll position
        var itemHeight = 32;  // Approximate height of list item
        var scrollTop = selectedIndex * itemHeight - 100;  // Center view
        
        listbox.element.scrollTop = scrollTop;
    }
}
```

### Scroll Position Monitoring

```javascript
var listbox = document.getElementById('myListBox').ej2_instances[0];

listbox.element.addEventListener('scroll', function() {
    var scrollPercentage = (this.scrollTop / (this.scrollHeight - this.clientHeight)) * 100;
    console.log('Scroll position: ' + scrollPercentage.toFixed(2) + '%');
    
    // Load more data at 80% scroll
    if (scrollPercentage > 80) {
        console.log('Load more data');
    }
});
```

## Filtering ListBox Data

### Basic Search Filter

```cshtml
<div class="mb-3">
    <input type="text" id="searchInput" placeholder="Search items..." class="form-control" />
</div>

<ejs-listbox id="filterableListBox" dataSource="@Model.AllItems"></ejs-listbox>

<script>
    const originalData = @Html.Raw(System.Text.Json.JsonSerializer.Serialize(Model.AllItems));
    var listbox = document.getElementById('filterableListBox').ej2_instances[0];
    
    document.getElementById('searchInput').addEventListener('keyup', function(e) {
        const searchTerm = e.target.value.toLowerCase();
        
        if (searchTerm === '') {
            listbox.dataSource = originalData;
        } else {
            listbox.dataSource = originalData.filter(item => 
                item.toLowerCase().includes(searchTerm)
            );
        }
    });
</script>
```

### Filter with Debounce

```javascript
function debounce(fn, delay) {
    let timeout;
    return function(...args) {
        clearTimeout(timeout);
        timeout = setTimeout(() => fn.apply(this, args), delay);
    };
}

const originalData = @Html.Raw(System.Text.Json.JsonSerializer.Serialize(Model.AllItems));
var listbox = document.getElementById('filterableListBox').ej2_instances[0];

const filterItems = debounce(function(searchTerm) {
    searchTerm = searchTerm.toLowerCase();
    
    if (searchTerm === '') {
        listbox.dataSource = originalData;
    } else {
        listbox.dataSource = originalData.filter(item => 
            item.toLowerCase().includes(searchTerm)
        );
    }
}, 300);  // Wait 300ms after typing stops

document.getElementById('searchInput').addEventListener('keyup', function(e) {
    filterItems(e.target.value);
});
```

### Advanced Filtering (Complex Objects)

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Category { get; set; }
    public decimal Price { get; set; }
}
```

```javascript
const originalData = @Html.Raw(System.Text.Json.JsonSerializer.Serialize(Model.Products));
var listbox = document.getElementById('productList').ej2_instances[0];

function filterByCategory(category) {
    if (category === '') {
        listbox.dataSource = originalData;
    } else {
        listbox.dataSource = originalData.filter(p => p.Category === category);
    }
}

function searchByName(searchTerm) {
    const term = searchTerm.toLowerCase();
    listbox.dataSource = originalData.filter(p => 
        p.Name.toLowerCase().includes(term)
    );
}

function priceRange(minPrice, maxPrice) {
    listbox.dataSource = originalData.filter(p => 
        p.Price >= minPrice && p.Price <= maxPrice
    );
}
```

## Form Submission with Selected Values

### Submit Single Selection

```cshtml
<form method="post">
    <ejs-listbox id="categorySelect" 
                 dataSource="@Model.Categories"
                 selectionSettings="@(new Syncfusion.EJ2.DropDowns.ListBoxSelectionSettings { Mode = Syncfusion.EJ2.DropDowns.SelectionMode.Single })">
    </ejs-listbox>
    
    <!-- Hidden input to store selected value -->
    <input type="hidden" id="selectedCategory" name="selectedCategory" />
    
    <button type="submit" class="btn btn-primary">Submit</button>
</form>

<script>
    document.querySelector('form').addEventListener('submit', function(e) {
        var listbox = document.getElementById('categorySelect').ej2_instances[0];
        document.getElementById('selectedCategory').value = listbox.value;
    });
</script>
```

### Submit Multiple Selections

```cshtml
<form method="post">
    <ejs-listbox id="tagsSelect" 
                 dataSource="@Model.Tags"
                 selectionSettings="@(new Syncfusion.EJ2.DropDowns.ListBoxSelectionSettings { Mode = Syncfusion.EJ2.DropDowns.SelectionMode.Multiple })">
    </ejs-listbox>
    
    <!-- Hidden input to store selected values as JSON -->
    <input type="hidden" id="selectedTags" name="selectedTags" />
    
    <button type="submit" class="btn btn-primary">Submit</button>
</form>

<script>
    document.querySelector('form').addEventListener('submit', function(e) {
        var listbox = document.getElementById('tagsSelect').ej2_instances[0];
        
        // Store as JSON array
        document.getElementById('selectedTags').value = JSON.stringify(listbox.value);
    });
</script>
```

```csharp
// Page handler
[BindProperty]
public string SelectedTags { get; set; }

public IActionResult OnPost()
{
    // Parse JSON back to array
    var tags = JsonSerializer.Deserialize<string[]>(SelectedTags);
    
    // Process tags
    foreach (var tag in tags)
    {
        // Do something with each tag
    }
    
    return Page();
}
```

### Validation Before Submit

```cshtml
<form method="post" onsubmit="return validateForm()">
    <ejs-listbox id="requiredSelect" dataSource="@Model.Options"></ejs-listbox>
    <span id="validationError" style="color: red; display: none;">Please select an option</span>
    <input type="hidden" id="selectedOption" name="selectedOption" />
    <button type="submit">Submit</button>
</form>

<script>
    function validateForm() {
        var listbox = document.getElementById('requiredSelect').ej2_instances[0];
        var error = document.getElementById('validationError');
        
        if (!listbox.value) {
            error.style.display = 'block';
            return false;
        }
        
        error.style.display = 'none';
        document.getElementById('selectedOption').value = listbox.value;
        return true;
    }
</script>
```

## Common Troubleshooting

| Issue | Solution |
|-------|----------|
| **ListBox not visible** | Check if `height` is set; verify theme CSS is loaded |
| **Data not displaying** | Verify `dataSource` is populated; check field mappings match data |
| **Events not firing** | Ensure `ejs-scripts` manager is registered; check event names |
| **Selection not working** | Verify `selectionSettings` is configured; check `value` property binding |
| **Styling looks wrong** | Verify theme CSS loads before custom styles; check for CSS conflicts |
| **Scroll not working** | Set explicit `height` on ListBox; verify container has overflow property |
| **Performance issues with large lists** | Use virtual scrolling; consider pagination instead |
| **Drag-drop not working** | Ensure `allowDragAndDrop="true"` is set; check browser console for errors |

## Best Practices

### Practice 1: Always Validate User Input

```javascript
function updateListBox(data) {
    if (!Array.isArray(data) || data.length === 0) {
        console.warn('Invalid data provided to ListBox');
        return;
    }
    
    var listbox = document.getElementById('myListBox').ej2_instances[0];
    listbox.dataSource = data;
}
```

### Practice 2: Handle Async Operations Gracefully

```javascript
async function loadDataAsync() {
    var listbox = document.getElementById('myListBox').ej2_instances[0];
    
    try {
        const response = await fetch('/api/items');
        if (!response.ok) throw new Error('Network error');
        
        const data = await response.json();
        listbox.dataSource = data;
    } catch (error) {
        console.error('Error loading data:', error);
        listbox.dataSource = [];  // Set empty on error
    }
}
```

### Practice 3: Clean Up Resources

```javascript
function cleanupListBox() {
    var element = document.getElementById('myListBox');
    
    if (element && element.ej2_instances && element.ej2_instances[0]) {
        var listbox = element.ej2_instances[0];
        listbox.destroy();  // Properly destroy instance
    }
}

// Call on page unload
window.addEventListener('beforeunload', cleanupListBox);
```

### Practice 4: Use Event Delegation for Dynamic Items

```javascript
var listbox = document.getElementById('myListBox').ej2_instances[0];

// Use change event instead of attaching to individual items
listbox.addEventListener('change', function(args) {
    console.log('Selection changed: ' + args.value);
});
```

### Practice 5: Cache Original Data for Filtering

```javascript
const originalData = [...listbox.dataSource];

function resetFilter() {
    listbox.dataSource = [...originalData];  // Restore from cache
}
```

## Next Steps

- **Features** → Explore advanced features in [references/features.md](features.md)
- **Styling** → Custom appearance patterns in [references/styling-and-appearance.md](styling-and-appearance.md)
- **Data Binding** → Complex data patterns in [references/data-binding.md](data-binding.md)
- **Back to Overview** → Return to [SKILL.md](../SKILL.md)
