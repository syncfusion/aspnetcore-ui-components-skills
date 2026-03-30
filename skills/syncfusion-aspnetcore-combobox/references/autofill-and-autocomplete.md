# Autofill & Autocomplete in ComboBox

## Overview

The ComboBox's **autofill** feature automatically completes typed input by suggesting matching items from the data source. When enabled, as soon as the user types a character, ComboBox suggests the first matching item and highlights the untyped portion.

**Key Benefits:**
- Faster data entry for users
- Reduces typing errors
- Improves user experience
- Works with keyboard navigation

## Enable Autofill

Use the `autofill` property to enable autocomplete behavior.

```html
@{
    var countries = new List<Country>
    {
        new Country { Name = "Australia", Code = "AU" },
        new Country { Name = "Austria", Code = "AT" },
        new Country { Name = "Afghanistan", Code = "AF" },
        new Country { Name = "Albania", Code = "AL" },
        new Country { Name = "Algeria", Code = "DZ" }
    };
}

<ejs-combobox id="country"
              dataSource="@countries"
              fields-text="Name"
              fields-value="Code"
              autofill="true"
              placeholder="Type a country name">
</ejs-combobox>
```

**Behavior:**
1. User types "A" → ComboBox suggests "Australia" (highlights "ustralia")
2. User types "Au" → Suggests "Austria" (highlights "stria")
3. User types "Aus" → Suggests "Australia" (highlights "tralia")
4. Press Tab or Enter → Accepts suggestion, moves to next field

## How Autofill Works

**Basic Example:**

```html
<!-- Simple autofill with list of items -->
<ejs-combobox id="sport"
              dataSource="@(new List<string> { 'Cricket', 'Basketball', 'Football', 'Badminton' })"
              autofill="true"
              placeholder="Type sport name">
</ejs-combobox>
```

**Step-by-step behavior:**
```
User Input: "C"
Data: Cricket, Basketball, Football, Badminton
Result: Suggests "Cricket" (C is user-typed, ricket is highlighted)

User Input: "Ba"
Result: Suggests "Basketball" (Ba is user-typed, sketball is highlighted)

User Input: "F"
Result: Suggests "Football" (F is user-typed, ootball is highlighted)
```

## Autofill with Custom Objects

When binding complex data, autofill matches based on the `fields-text` property.

```csharp
public class Employee
{
    public int EmployeeId { get; set; }
    public string EmployeeName { get; set; }
    public string Department { get; set; }
}
```

**View:**
```html
@{
    var employees = new List<Employee>
    {
        new Employee { EmployeeId = 1, EmployeeName = "Michael", Department = "Engineering" },
        new Employee { EmployeeId = 2, EmployeeName = "Michael S", Department = "Sales" },
        new Employee { EmployeeId = 3, EmployeeName = "Mary", Department = "HR" },
        new Employee { EmployeeId = 4, EmployeeName = "Margaret", Department = "Marketing" }
    };
}

<ejs-combobox id="employee"
              dataSource="@employees"
              fields-text="EmployeeName"
              fields-value="EmployeeId"
              autofill="true"
              placeholder="Select employee">
</ejs-combobox>
```

**Typing behavior:**
```
User types "Mi"
Result: Suggests "Michael" (highlights chael)

User types "Ma"
Result: Suggests "Margaret" (highlights rgaret)

User types "Mar"
Result: Suggests "Margaret" (highlights garet)
```

## Autofill + Filtering Combined

Enable both autofill and filtering for powerful search.

```html
<ejs-combobox id="country"
              dataSource="@countries"
              fields-text="Name"
              fields-value="Code"
              autofill="true"
              allowFiltering="true"
              placeholder="Type to search & autofill">
</ejs-combobox>
```

**Combined behavior:**
1. User types "U" → Autofills "United States", shows filtered list
2. Displays only items starting with "U": United States, United Kingdom, Uruguay
3. User can press Down arrow to see other matches
4. Press Enter to accept current suggestion

## Autofill with Remote Data

For remote data sources, autofill works with server data.

**View:**
```html
<ejs-combobox id="customer"
              dataSource="@(new Syncfusion.EJ2.Base.DataManager { Url = \"/api/customers\" })"
              fields-text="CustomerName"
              fields-value="CustomerId"
              autofill="true"
              allowFiltering="true"
              placeholder="Search customers">
</ejs-combobox>

<script>
function onFiltering(args) {
    // Server returns filtered matches
    var dataManager = args.baseEventArgs.model.dataSource;
    dataManager.url = `/api/customers?search=${args.text}`;
}
</script>
```

**Web API:**
```csharp
[HttpGet("/api/customers")]
public IActionResult GetCustomers([FromQuery] string search = "")
{
    var allCustomers = GetAllCustomers();
    
    if (!string.IsNullOrEmpty(search))
    {
        return Ok(allCustomers
            .Where(c => c.CustomerName.StartsWith(search, StringComparison.OrdinalIgnoreCase))
            .ToList());
    }
    return Ok(allCustomers);
}
```

## Keyboard Shortcuts with Autofill

**Essential keyboard controls:**
- **Tab** - Accept suggestion, move to next field
- **Enter** - Accept suggestion, keep focus
- **Escape** - Reject suggestion, clear input
- **Backspace** - Delete character, disable autofill for current
- **↓ Arrow** - Navigate between suggestions (when filtering enabled)
- **↑ Arrow** - Navigate up (when filtering enabled)

**Example handling:**
```html
<ejs-combobox id="combobox"
              autofill="true"
              dataSource="@data">
</ejs-combobox>

<script>
document.getElementById('combobox').addEventListener('keydown', function(e) {
    if (e.key === 'Tab') {
        // Accept autofill suggestion
        console.log("Tab pressed - accepting suggestion");
    }
    if (e.key === 'Escape') {
        // Reject autofill
        console.log("Escape pressed - clearing input");
    }
});
</script>
```

## Common Patterns

**Pattern 1: Autofill with validation**
```html
<ejs-combobox id="country"
              dataSource="@countries"
              autofill="true"
              blur="onBlur">
</ejs-combobox>

<script>
function onBlur(args) {
    var combobox = args.baseEventArgs.model;
    
    // Verify that accepted value is valid
    if (combobox.text && !combobox.value) {
        console.log("Invalid entry:", combobox.text);
        // Clear invalid input
        combobox.value = null;
    }
}
</script>
```

**Pattern 2: Case-insensitive autofill**
```html
@{
    var items = new List<string> { "Apple", "Apricot", "Banana" };
}

<ejs-combobox id="fruit"
              dataSource="@items"
              autofill="true"
              placeholder="Type (case-insensitive)">
</ejs-combobox>

<!-- User can type "app" and will autofill to "Apple" -->
```

**Pattern 3: Autofill with icons**
```csharp
public class Item
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Icon { get; set; }
}
```

**View:**
```html
<ejs-combobox id="item"
              dataSource="@items"
              fields-text="Name"
              autofill="true">
    <e-combobox-templates>
        <e-template type="ItemTemplate">
            <div class="item-with-icon">
                <span class="icon">${Icon}</span>
                <span>${Name}</span>
            </div>
        </e-template>
    </e-combobox-templates>
</ejs-combobox>

<style>
    .item-with-icon {
        display: flex;
        gap: 8px;
        align-items: center;
    }
</style>
```

## When NOT to Use Autofill

❌ **Large datasets** - Can be slow (use virtual scrolling instead)
❌ **Exact matches required** - User may miss untyped portion (add validation)
❌ **Multiple starting characters** - May confuse users (clarify expectation)
❌ **Accessibility focus** - Some screen readers struggle (test thoroughly)

## Troubleshooting

**Issue: Autofill not working**
- ✓ Verify `autofill="true"` is set
- ✓ Check that dataSource is not empty
- ✓ Ensure `fields-text` matches your data property
- ✓ Check browser console for errors

**Issue: Autofill too aggressive (always suggests)**
- ✓ This is normal behavior - user can press Escape to reject
- ✓ Use `filtering="onFilter"` to filter suggestions
- ✓ Implement `minFilterChars` to require minimum input

**Issue: Autofill conflicts with custom values**
- ✓ If `allowCustom="true"`, autofill may interfere
- ✓ User can press Escape to bypass suggestion
- ✓ Or disable autofill if custom entry is priority

**Issue: Performance slow with many items**
- ✓ Enable `enableVirtualization="true"`
- ✓ Use remote data with server-side filtering
- ✓ Monitor network requests and data size
