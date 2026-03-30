# Advanced Scenarios in DropdownList

## Table of Contents
- [Cascading Dropdowns](#cascading-dropdowns)
- [Multi-Select Patterns](#multi-select-patterns)
- [Virtual Scrolling](#virtual-scrolling)
- [Search and Autocomplete](#search-and-autocomplete)
- [API Integration](#api-integration)
- [Error Handling](#error-handling)
- [Performance Optimization](#performance-optimization)
- [Best Practices](#best-practices)

## Cascading Dropdowns

Dependent dropdowns where child selection depends on parent.

### Two-Level Cascading

```csharp
// Models
public class Country
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public class State
{
    public int Id { get; set; }
    public int CountryId { get; set; }
    public string Name { get; set; }
}

// Controller
public IActionResult Index()
{
    ViewBag.Countries = _context.Countries.ToList();
    return View();
}

[HttpGet]
public IActionResult GetStates(int countryId)
{
    var states = _context.States
        .Where(s => s.CountryId == countryId)
        .ToList();
    return Json(states);
}

// View
@Html.EJ2().DropDownList()
    .Id("Countries")
    .DataSource(ViewBag.Countries)
    .Fields(f => f.Text("Name").Value("Id"))
    .Change("onCountryChange")
    .Placeholder("Select country")
    .Render()

@Html.EJ2().DropDownList()
    .Id("States")
    .Fields(f => f.Text("Name").Value("Id"))
    .Placeholder("Select state")
    .Change("onStateChange")
    .Render()

<script>
function onCountryChange(args) {
    if (args.value) {
        // Clear dependent dropdown
        var stateDropdown = document.getElementById('States').ej2_instances[0];
        stateDropdown.value = null;
        
        // Load states for selected country
        fetch('/Home/GetStates?countryId=' + args.value)
            .then(response => response.json())
            .then(states => {
                stateDropdown.dataSource = states;
                stateDropdown.dataBind();
            })
            .catch(error => console.error('Error:', error));
    }
}

function onStateChange(args) {
    console.log('Selected state:', args.value);
}
</script>
```

### Three-Level Cascading

```csharp
// Extended for Country -> State -> City

// Controller
[HttpGet]
public IActionResult GetCities(int stateId)
{
    var cities = _context.Cities
        .Where(c => c.StateId == stateId)
        .ToList();
    return Json(cities);
}

// View
@Html.EJ2().DropDownList()
    .Id("Cities")
    .Placeholder("Select city")
    .Render()

// Script - add to onStateChange
<script>
function onStateChange(args) {
    if (args.value) {
        var cityDropdown = document.getElementById('Cities').ej2_instances[0];
        cityDropdown.value = null;
        
        fetch('/Home/GetCities?stateId=' + args.value)
            .then(response => response.json())
            .then(cities => {
                cityDropdown.dataSource = cities;
                cityDropdown.dataBind();
            });
    }
}
</script>
```

### Reset on Parent Change

```javascript
// Clear all dependent dropdowns when parent changes
function onCountryChange(args) {
    var stateDropdown = document.getElementById('States').ej2_instances[0];
    var cityDropdown = document.getElementById('Cities').ej2_instances[0];
    
    // Reset both dependent dropdowns
    stateDropdown.dataSource = [];
    stateDropdown.value = null;
    cityDropdown.dataSource = [];
    cityDropdown.value = null;
    
    // Load new data
    if (args.value) {
        loadStates(args.value);
    }
}
```

## Multi-Select Patterns

While DropdownList is single-select, implement multi-select behavior:

### Multiple Dropdowns Approach

```csharp
// Allow selecting multiple items across separate dropdowns

<div class="multi-select-container">
    <h3>Select Features</h3>
    
    @for (int i = 1; i <= 3; i++)
    {
        <div class="feature-select">
            <label>Feature @i</label>
            @Html.EJ2().DropDownList()
                .Id($"Feature{i}")
                .DataSource(ViewBag.Features)
                .Fields(f => f.Text("Name").Value("Id"))
                .Render()
        </div>
    }
</div>

<button onclick="getSelectedFeatures()">Get Selected</button>

<script>
function getSelectedFeatures() {
    var selected = [];
    for (var i = 1; i <= 3; i++) {
        var dropdown = document.getElementById('Feature' + i).ej2_instances[0];
        if (dropdown.value) {
            selected.push({
                featureNumber: i,
                value: dropdown.value,
                text: dropdown.text
            });
        }
    }
    console.log('Selected Features:', selected);
    // Send to server
    submitSelection(selected);
}
</script>
```

### Array Accumulator Pattern

```csharp
<div class="selected-items-list">
    <h4>Selected Items: <span id="count">0</span></h4>
    <ul id="selectedList"></ul>
</div>

@Html.EJ2().DropDownList()
    .Id("ItemSelector")
    .DataSource(ViewBag.Items)
    .Fields(f => f.Text("Name").Value("Id"))
    .Change("onItemSelect")
    .Render()

<button onclick="clearAllSelections()">Clear All</button>

<script>
var selectedItems = [];

function onItemSelect(args) {
    if (args.value && !selectedItems.find(x => x.id === args.value)) {
        selectedItems.push({
            id: args.value,
            text: args.text
        });
        updateSelectedList();
        
        // Reset dropdown for next selection
        document.getElementById('ItemSelector').ej2_instances[0].value = null;
    }
}

function updateSelectedList() {
    var list = document.getElementById('selectedList');
    list.innerHTML = '';
    
    selectedItems.forEach(item => {
        var li = document.createElement('li');
        li.innerHTML = item.text + ' <button onclick="removeItem(' + item.id + ')">Remove</button>';
        list.appendChild(li);
    });
    
    document.getElementById('count').textContent = selectedItems.length;
}

function removeItem(id) {
    selectedItems = selectedItems.filter(x => x.id !== id);
    updateSelectedList();
}

function clearAllSelections() {
    selectedItems = [];
    updateSelectedList();
}
</script>
```

## Virtual Scrolling

For large datasets (1000+ items), enable virtual rendering:

### Basic Virtual Scrolling

```csharp
@Html.EJ2().DropDownList()
    .Id("LargeList")
    .DataSource(ViewBag.LargeDataSet)  // 5000+ items
    .Fields(f => f.Text("Name").Value("Id"))
    .EnableVirtualization(true)        // Virtual rendering
    .PopupHeight("300px")
    .AllowFiltering(true)              // Filter large lists
    .Render()
```

### Controller with Large Dataset

```csharp
public IActionResult Index()
{
    // Generate large dataset
    var largeDataSet = Enumerable.Range(1, 10000)
        .Select(i => new Item
        {
            Id = i,
            Name = $"Item {i}",
            Category = $"Category {i % 10}"
        })
        .ToList();
    
    ViewBag.LargeDataSet = largeDataSet;
    return View();
}
```

### Performance Comparison

```csharp
// ❌ Without Virtual Scrolling - Slow
@Html.EJ2().DropDownList()
    .Id("AllItems")
    .DataSource(ViewBag.LargeDataSet)  // 5000 items rendered
    .Render()
// Result: Slow rendering, high memory use

// ✅ With Virtual Scrolling - Fast
@Html.EJ2().DropDownList()
    .Id("AllItems")
    .DataSource(ViewBag.LargeDataSet)
    .EnableVirtualization(true)        // Only visible items rendered
    .Render()
// Result: Instant rendering, optimal memory
```

## Search and Autocomplete

### Searchable Dropdown with Highlighting

```csharp
@Html.EJ2().DropDownList()
    .Id("SearchBox")
    .DataSource(ViewBag.Products)
    .Fields(f => f.Text("ProductName").Value("ProductId"))
    .AllowFiltering(true)
    .FilterType(FilterType.Contains)
    .ItemTemplate(@"
        <div class='search-item'>
            <span class='item-name'>${ProductName}</span>
            <span class='item-category'>${Category}</span>
        </div>
    ")
    .Placeholder("Search products...")
    .Render()

<style>
.search-item {
    display: flex;
    justify-content: space-between;
    padding: 8px;
}

.item-category {
    font-size: 12px;
    color: #999;
}
</style>
```

### Autocomplete with Min Characters

```csharp
@Html.EJ2().DropDownList()
    .Id("AutocompleteSearch")
    .DataSource(d => d.Url("/api/search"))
    .AllowFiltering(true)
    .MinFilterLength(3)                // Require 3+ chars
    .Placeholder("Type at least 3 characters..."")
    .Filtering("onFiltering")
    .Render()

<script>
function onFiltering(args) {
    // Called when user types
    console.log('Filter text:', args.text);
}
</script>
```

## API Integration

### Loading Data from External API

```csharp
// Controller - fetch from external API
[HttpGet]
public async Task<IActionResult> GetExternalData()
{
    using (var client = new HttpClient())
    {
        try
        {
            var response = await client.GetAsync("https://api.example.com/items");
            var content = await response.Content.ReadAsStringAsync();
            var items = JsonConvert.DeserializeObject<List<ExternalItem>>(content);
            return Json(items);
        }
        catch (Exception ex)
        {
            return Json(new { error = "Failed to load data" });
        }
    }
}

// View
@Html.EJ2().DropDownList()
    .Id("ExternalData")
    .DataSource(d => d.Url("/Home/GetExternalData"))
    .Fields(f => f.Text("Name").Value("Id"))
    .Placeholder("Loading data..."")
    .Render()
```

### Polling API with Refresh

```csharp
<div class="api-dropdown-container">
    @Html.EJ2().DropDownList()
        .Id("LiveData")
        .Placeholder("Select from live data")
        .Render()
    
    <button onclick="refreshData()" class="btn-refresh">
        Refresh Data
    </button>
</div>

<script>
// Auto-refresh every 5 minutes
var refreshInterval = setInterval(refreshData, 5 * 60 * 1000);

function refreshData() {
    fetch('/Home/GetExternalData')
        .then(response => response.json())
        .then(data => {
            var dropdown = document.getElementById('LiveData').ej2_instances[0];
            dropdown.dataSource = data;
            dropdown.dataBind();
            console.log('Data refreshed at', new Date());
        })
        .catch(error => {
            console.error('Refresh failed:', error);
            showNotification('Failed to refresh data', 'error');
        });
}

function stopRefresh() {
    clearInterval(refreshInterval);
}
</script>
```

## Error Handling

### Handling Missing Data

```csharp
@Html.EJ2().DropDownList()
    .Id("SafeDropdown")
    .DataSource(ViewBag.Data ?? new List<Item>())
    .Fields(f => f.Text("Name").Value("Id"))
    .NoRecordsTemplate("<div class='no-records'>No items available</div>")
    .Render()
```

### Validation and Error Messages

```csharp
<form id="serviceForm">
    <div class="form-group">
        <label for="Category">Service Category *</label>
        @Html.EJ2().DropDownList()
            .Id("Category")
            .DataSource(ViewBag.Categories)
            .Fields(f => f.Text("Name").Value("Id"))
            .Required(true)
            .Render()
        <span class="error-message" id="categoryError"></span>
    </div>
    
    <button type="button" onclick="validateForm()">Submit</button>
</form>

<script>
function validateForm() {
    var dropdown = document.getElementById('Category').ej2_instances[0];
    
    if (!dropdown.value) {
        document.getElementById('categoryError').textContent = 
            'Please select a category';
        return false;
    }
    
    // Form is valid
    submitForm();
}
</script>
```

### Error Handling in Cascading

```csharp
<script>
async function onCountryChange(args) {
    if (!args.value) return;
    
    try {
        const response = await fetch('/Home/GetStates?countryId=' + args.value);
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const states = await response.json();
        var stateDropdown = document.getElementById('States').ej2_instances[0];
        stateDropdown.dataSource = states;
        stateDropdown.dataBind();
        
    } catch (error) {
        console.error('Error loading states:', error);
        showNotification('Failed to load states. Please try again.', 'error');
        
        // Disable state dropdown on error
        var stateDropdown = document.getElementById('States').ej2_instances[0];
        stateDropdown.enabled = false;
    }
}
</script>
```

## Performance Optimization

### Lazy Loading

Load data only when dropdown opens:

```csharp
@Html.EJ2().DropDownList()
    .Id("LazyDropdown")
    .Fields(f => f.Text("Name").Value("Id"))
    .Open("onDropdownOpen")
    .Render()

<script>
var dataLoaded = false;

function onDropdownOpen(args) {
    if (!dataLoaded) {
        loadData();
        dataLoaded = true;
    }
}

function loadData() {
    fetch('/Home/GetDropdownData')
        .then(response => response.json())
        .then(data => {
            var dropdown = document.getElementById('LazyDropdown').ej2_instances[0];
            dropdown.dataSource = data;
            dropdown.dataBind();
        });
}
</script>
```

### Memoization Pattern

```javascript
// Cache API responses to avoid repeated requests

const apiCache = {};

async function fetchDataWithCache(url) {
    if (apiCache[url]) {
        console.log('Returning cached data for:', url);
        return apiCache[url];
    }
    
    try {
        const response = await fetch(url);
        const data = await response.json();
        apiCache[url] = data;
        return data;
    } catch (error) {
        console.error('Error fetching:', url, error);
        throw error;
    }
}

// Usage
async function onCountryChange(args) {
    const states = await fetchDataWithCache(`/Home/GetStates?countryId=${args.value}`);
    // Use states...
}
```

### Debouncing Filter Requests

```csharp
<script>
let filterTimeout;

function onFiltering(args) {
    clearTimeout(filterTimeout);
    
    // Wait 300ms before making request
    filterTimeout = setTimeout(() => {
        performFilter(args.text);
    }, 300);
}

function performFilter(searchText) {
    fetch('/api/search?q=' + encodeURIComponent(searchText))
        .then(response => response.json())
        .then(data => {
            var dropdown = document.getElementById('SearchBox').ej2_instances[0];
            dropdown.dataSource = data;
            dropdown.dataBind();
        });
}
</script>
```

## Best Practices

### DO

✅ **Use aria-label** for screen readers
✅ **Provide placeholder** text for guidance
✅ **Group related items** for clarity
✅ **Enable filtering** for 10+ items
✅ **Handle empty data** gracefully
✅ **Test keyboard navigation**
✅ **Use server-side filtering** for large datasets
✅ **Provide error messages** clearly

### DON'T

❌ **Never skip field mapping** for object data
❌ **Don't use color alone** to show status
❌ **Avoid extremely long item lists** without virtual scrolling
❌ **Don't break keyboard navigation**
❌ **Never disable without explanation**
❌ **Avoid inconsistent cascading behavior**
❌ **Don't make filtering hard to discover**

### Code Quality Checklist

- [ ] Properly handle null/undefined values
- [ ] Implement error boundaries
- [ ] Use async/await for async operations
- [ ] Cache repeated API calls
- [ ] Debounce frequent events
- [ ] Dispose resources on destroy
- [ ] Test with various data sizes
- [ ] Monitor performance metrics
- [ ] Document complex logic

---

## Next Steps

- Review [Data Binding](data-binding.md) for complex data scenarios
- Check [Accessibility and Features](accessibility-features.md) for keyboard support
- Explore [Templates and Styling](templates-styling.md) for custom rendering
