# Advanced Features in ComboBox

## Table of Contents

- [Cascading ComboBoxes](#cascading-comboboxes)
- [Virtual Scrolling](#virtual-scrolling)
- [Event Handling](#event-handling)
- [Keyboard Navigation](#keyboard-navigation)
- [Accessibility (WCAG)](#accessibility-wcag)
- [RTL Support](#rtl-support)
- [Custom Validation](#custom-validation)
- [Performance Optimization](#performance-optimization)
- [Troubleshooting](#troubleshooting)

## Cascading ComboBoxes

Create dependent dropdowns where the second control's data depends on the first selection.

**Example: Country → State → City**

```html
@{
    var countries = new List<Country>
    {
        new Country { CountryId = 1, CountryName = "United States" },
        new Country { CountryId = 2, CountryName = "Canada" }
    };
}

<div class="form-group">
    <label>Country:</label>
    <ejs-combobox id="country"
                  dataSource="@countries"
                  fields-text="CountryName"
                  fields-value="CountryId"
                  change="onCountryChange"
                  placeholder="Select a country">
    </ejs-combobox>
</div>

<div class="form-group">
    <label>State:</label>
    <ejs-combobox id="state"
                  enabled="false"
                  placeholder="Select a state">
    </ejs-combobox>
</div>

<div class="form-group">
    <label>City:</label>
    <ejs-combobox id="city"
                  enabled="false"
                  placeholder="Select a city">
    </ejs-combobox>
</div>

<script>
function onCountryChange(args) {
    var countryId = args.value;
    var stateCombo = document.getElementById('state').ej2_instances[0];
    var cityCombo = document.getElementById('city').ej2_instances[0];
    
    if (countryId) {
        // Fetch states for selected country
        fetch(`/api/states?countryId=${countryId}`)
            .then(r => r.json())
            .then(states => {
                stateCombo.dataSource = states;
                stateCombo.enabled = true;
                stateCombo.value = null; // Reset selection
                
                // Reset city
                cityCombo.dataSource = [];
                cityCombo.value = null;
                cityCombo.enabled = false;
            });
    } else {
        stateCombo.dataSource = [];
        stateCombo.enabled = false;
        cityCombo.dataSource = [];
        cityCombo.enabled = false;
    }
}

function onStateChange(args) {
    var stateId = args.value;
    var cityCombo = document.getElementById('city').ej2_instances[0];
    
    if (stateId) {
        // Fetch cities for selected state
        fetch(`/api/cities?stateId=${stateId}`)
            .then(r => r.json())
            .then(cities => {
                cityCombo.dataSource = cities;
                cityCombo.enabled = true;
                cityCombo.value = null; // Reset selection
            });
    } else {
        cityCombo.dataSource = [];
        cityCombo.enabled = false;
    }
}
</script>

<style>
    .form-group { margin-bottom: 15px; }
    .form-group label { display: block; margin-bottom: 5px; font-weight: bold; }
</style>
```

**Backend API (api/states):**
```csharp
[HttpGet("/api/states")]
public IActionResult GetStates([FromQuery] int countryId)
{
    var stateMap = new Dictionary<int, List<State>>
    {
        { 1, new List<State> { 
            new State { StateId = 1, StateName = "California" },
            new State { StateId = 2, StateName = "Texas" }
        }},
        { 2, new List<State> { 
            new State { StateId = 3, StateName = "Ontario" },
            new State { StateId = 4, StateName = "Quebec" }
        }}
    };
    
    return Ok(stateMap.ContainsKey(countryId) ? stateMap[countryId] : new List<State>());
}
```

## Virtual Scrolling

Enable virtual scrolling for large datasets to improve performance.

```html
<ejs-combobox id="large-list"
              dataSource="@(largeDataList)"
              fields-text="Name"
              fields-value="Id"
              enableVirtualization="true"
              itemsCount="5"
              placeholder="Large list with virtual scroll">
</ejs-combobox>
```

**When to use:**
- Datasets with 1000+ items
- Each item renders complex HTML
- Dropdown performance is slow

**Benefits:**
- Only visible items are rendered
- Smooth scrolling without lag
- Reduced memory usage

**Parameters:**
- `enableVirtualization="true"` - Enable the feature
- `itemsCount="5"` - Number of items visible at once

## Event Handling

Handle ComboBox events to respond to user interactions.

**Available Events:**

| Event | Fired When | Usage |
|-------|-----------|-------|
| `change` | User selects/changes value | Update dependent controls |
| `select` | User selects from list | Log selection |
| `focus` | ComboBox gets focus | Pre-populate suggestions |
| `blur` | ComboBox loses focus | Validate input |
| `filtering` | User types in filter | Custom filtering logic |
| `actionComplete` | Data binding completes | Enable/disable based on data |
| `open` | Dropdown opens | Adjust popup size |
| `close` | Dropdown closes | Clean up |

**Example: Using Multiple Events**
```html
<ejs-combobox id="combobox"
              dataSource="@data"
              change="onChange"
              select="onSelect"
              filtering="onFilter"
              blur="onBlur">
</ejs-combobox>

<script>
function onChange(args) {
    console.log("Value changed to:", args.value);
    console.log("Item data:", args.itemData);
}

function onSelect(args) {
    console.log("Item selected:", args.itemData.Name);
    // Update related form fields
    document.getElementById('selectedId').value = args.itemData.Id;
}

function onFilter(args) {
    console.log("Filtering for:", args.text);
}

function onBlur(args) {
    // Validate that a valid selection was made
    if (!args.baseEventArgs.model.value && args.baseEventArgs.model.text) {
        console.log("Invalid entry:", args.baseEventArgs.model.text);
    }
}
</script>
```

## Keyboard Navigation

ComboBox supports full keyboard navigation for accessibility and efficiency.

**Keyboard Shortcuts:**
- **↓ Arrow Down** - Move to next item
- **↑ Arrow Up** - Move to previous item
- **Enter** - Select focused item
- **Escape** - Close dropdown
- **Tab** - Move to next field
- **Shift+Tab** - Move to previous field
- **Alt+Down** - Open dropdown
- **Alt+Up** - Close dropdown
- **Backspace** - Delete typed character

**Example: Custom keyboard handler**
```html
<ejs-combobox id="combobox" 
              dataSource="@data"
              allowFiltering="true">
</ejs-combobox>

<script>
var combobox = document.getElementById('combobox').ej2_instances[0];

// Listen for custom keyboard commands
document.getElementById('combobox').addEventListener('keydown', function(e) {
    if (e.ctrlKey && e.key === 'Enter') {
        // Custom action: Ctrl+Enter to select current item
        if (combobox.focusedItemData) {
            combobox.value = combobox.focusedItemData.Id;
        }
    }
});
</script>
```

## Accessibility (WCAG)

Ensure ComboBox is accessible to all users including those with disabilities.

**WCAG 2.1 AA Compliance Features:**
- Keyboard navigation support
- ARIA labels and attributes
- Screen reader support
- Focus indicators
- Color contrast ratios

**Implementing accessibility:**
```html
<!-- Provide descriptive label -->
<label for="country" id="country-label">
    Select Your Country
</label>

<ejs-combobox id="country"
              aria-labelledby="country-label"
              aria-describedby="country-help"
              dataSource="@data"
              fields-text="CountryName"
              fields-value="CountryId"
              placeholder="Type to search countries">
</ejs-combobox>

<!-- Provide help text -->
<small id="country-help">
    Start typing to filter the list, use arrow keys to navigate, press Enter to select
</small>

<style>
    /* Ensure focus is visible */
    .e-combobox .e-input:focus {
        outline: 2px solid #4CAF50;
        outline-offset: 2px;
    }
    
    /* Ensure color contrast -->
    .e-combobox .e-input {
        color: #333; /* Dark text on light background */
    }
</style>
```

**Best Practices:**
- Always provide a label with `for` attribute
- Use `aria-describedby` for help text
- Use `aria-live="polite"` for dynamic updates
- Test with screen readers (NVDA, JAWS)
- Ensure minimum touch target size of 44x44 pixels

## RTL Support

Enable right-to-left (RTL) layout for Arabic, Hebrew, and other RTL languages.

**HTML-level RTL:**
```html
<!DOCTYPE html>
<html dir="rtl" lang="ar">
<head>
    <meta charset="UTF-8">
    <title>ComboBox RTL</title>
</head>
<body>
    <ejs-combobox id="country"
                  dataSource="@data"
                  placeholder="اختر بلد">
    </ejs-combobox>
</body>
</html>
```

**CSS-level RTL:**
```css
body {
    direction: rtl;
    text-align: right;
}

.e-combobox {
    direction: rtl;
}

.e-combobox .e-input {
    text-align: right;
    padding-right: 40px;
}
```

**Dynamic RTL:**
```html
<button onclick="toggleRTL()">Toggle RTL</button>

<ejs-combobox id="combobox" dataSource="@data"></ejs-combobox>

<script>
function toggleRTL() {
    document.body.dir = document.body.dir === 'rtl' ? 'ltr' : 'rtl';
    var combobox = document.getElementById('combobox').ej2_instances[0];
    combobox.refresh(); // Refresh to apply new direction
}
</script>
```

## Custom Validation

Add validation rules to ensure valid selections.

**Example: Require selection, disallow custom values**
```html
<ejs-combobox id="country"
              dataSource="@countries"
              allowCustom="false"
              change="onCountryChange"
              blur="onCountryBlur">
</ejs-combobox>

<span id="error-message" style="color: red;"></span>

<script>
function onCountryChange(args) {
    clearError();
}

function onCountryBlur(args) {
    var combobox = args.baseEventArgs.model;
    
    // Validate: must have selected a value
    if (!combobox.value) {
        showError("Please select a country");
    }
}

function showError(message) {
    document.getElementById('error-message').textContent = message;
}

function clearError() {
    document.getElementById('error-message').textContent = '';
}

// Form submission validation
document.getElementById('myForm').addEventListener('submit', function(e) {
    var combobox = document.getElementById('country').ej2_instances[0];
    if (!combobox.value) {
        e.preventDefault();
        showError("Country is required");
    }
});
</script>
```

**Server-side validation:**
```csharp
[HttpPost]
public IActionResult Submit(int countryId)
{
    // Validate that selected country exists in approved list
    var validCountries = GetValidCountries();
    
    if (!validCountries.Any(c => c.CountryId == countryId))
    {
        ModelState.AddModelError("country", "Invalid country selection");
        return BadRequest(ModelState);
    }
    
    // Process form
    return Ok("Success");
}
```

## Performance Optimization

Optimize ComboBox performance for large datasets.

**Techniques:**

1. **Virtual Scrolling** - Render only visible items
   ```html
   <ejs-combobox enableVirtualization="true" itemsCount="5"></ejs-combobox>
   ```

2. **Remote Filtering** - Filter on server instead of client
   ```html
   <ejs-combobox allowFiltering="true" filtering="onRemoteFilter"></ejs-combobox>
   ```

3. **Debounced Filtering** - Wait before filtering
   ```javascript
   var debounceTimer;
   function onFilter(args) {
       clearTimeout(debounceTimer);
       debounceTimer = setTimeout(() => {
           // Perform filtering
       }, 300);
   }
   ```

4. **Lazy Loading** - Load data on demand
   ```javascript
   var combobox = document.getElementById('combo').ej2_instances[0];
   combobox.open = function() {
       if (!combobox.dataSource) {
           fetch('/api/data').then(r => r.json())
               .then(data => combobox.dataSource = data);
       }
   };
   ```

## Troubleshooting

**Issue: ComboBox not responding to keyboard**
- ✓ Ensure ComboBox has focus (click on input field)
- ✓ Check browser console for JavaScript errors
- ✓ Verify keyboard events are not being prevented elsewhere

**Issue: Accessibility attributes not working**
- ✓ Use semantic HTML with proper `<label>` elements
- ✓ Test with screen readers (NVDA, JAWS)
- ✓ Ensure color contrast meets WCAG standards (4.5:1 for text)

**Issue: RTL layout looks wrong**
- ✓ Add `dir="rtl"` to `<html>` element
- ✓ Ensure CSS direction is set correctly
- ✓ Test in browser with RTL languages

**Issue: Performance slow with 10,000+ items**
- ✓ Enable virtual scrolling: `enableVirtualization="true"`
- ✓ Implement remote filtering instead of local
- ✓ Use pagination for data loading
- ✓ Monitor network requests in DevTools
