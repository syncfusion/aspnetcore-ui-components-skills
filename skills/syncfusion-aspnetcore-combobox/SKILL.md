---
name: syncfusion-aspnetcore-combobox
description: Implement Syncfusion ASP.NET Core ComboBox component for dropdown selection with filtering, data binding, and customization. Use this when creating dropdown controls, enabling user selection from lists, implementing autocomplete/search functionality, or handling cascading dropdowns. Covers installation, basic setup, data binding, filtering, templates, grouping, and advanced features.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  platform: "ASP.NET Core"
  category: "Dropdowns"
---

# Implementing Syncfusion ASP.NET Core ComboBox

## When to Use This Skill

Use this skill when you need to:
- **Create a dropdown or selection control** in an ASP.NET Core application
- **Enable user selection from predefined options** with autocomplete/search
- **Bind data from local or remote sources** to a ComboBox
- **Customize the dropdown appearance** with templates and styling
- **Add filtering, grouping, or sorting** to dropdown items
- **Implement advanced scenarios** like cascading dropdowns, virtual scrolling, or keyboard navigation
- **Handle accessibility requirements** (WCAG compliance, screen readers)

## Component Overview

The ComboBox is a powerful dropdown control that combines:
- **User-friendly selection** with searchable/filterable options
- **Data binding support** for local arrays and remote data sources
- **Rich customization** through templates and styling
- **Advanced features** like grouping, sorting, virtual scrolling, and cascading
- **Accessibility** built-in for keyboard navigation and screen readers
- **ASP.NET Core integration** via Tag Helper syntax

**Key Capabilities:**
- Type-ahead/autocomplete with filtering
- Custom values (allowing user-entered data not in list)
- Data grouping and sorting
- Dropdown templates for list items, headers, footers
- Remote data binding with DataManager
- Virtual scrolling for large datasets
- RTL (right-to-left) support
- WCAG 2.1 AA compliance

## Documentation and Navigation Guide

Read the following references based on your needs:

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and NuGet package setup
- Adding ComboBox to your ASP.NET Core project
- Minimal working example with Tag Helper syntax
- CSS/script imports and configuration
- Running your first ComboBox

### Data Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Binding local array data (strings and objects)
- Binding remote data via DataManager
- Mapping complex data structures
- Custom value support
- OData and Web API integration

### Filtering & Search
📄 **Read:** [references/filtering-and-search.md](references/filtering-and-search.md)
- Enabling autocomplete and filtering
- Search as user types
- Changing filter type (contains, startsWith, endsWith)
- Case-sensitive vs insensitive filtering
- Setting minimum character threshold
- Remote filtering from server

### Templates & Customization
📄 **Read:** [references/templates-and-customization.md](references/templates-and-customization.md)
- Item templates for list display
- Header and footer templates
- Group header customization
- CSS class customization
- Theme customization with CSS variables
- Popup height/width configuration

### Grouping & Sorting
📄 **Read:** [references/grouping-and-sorting.md](references/grouping-and-sorting.md)
- Grouping items by category
- Sorting in ascending/descending order
- Custom sort functions
- Combining grouping with sorting
- Group header styling

### Autofill & Autocomplete
📄 **Read:** [references/autofill-and-autocomplete.md](references/autofill-and-autocomplete.md)
- Enable autofill for faster data entry
- Autocomplete as user types
- Combine autofill with filtering
- Keyboard handling for suggestions
- Performance with large datasets

### Popup Resizing
📄 **Read:** [references/popup-resize.md](references/popup-resize.md)
- Enable user-resizable popup
- Set initial width/height
- Persist resized dimensions
- Responsive resize behavior
- Resize handle styling

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Creating cascading ComboBoxes
- Virtual scrolling for large datasets
- Keyboard navigation (Tab, Enter, Arrow keys)
- Accessibility attributes (ARIA labels)
- RTL (right-to-left) support
- Custom validation
- Event handling (change, selecting, select, filtering)

## Quick Start Example

Here's a minimal ComboBox setup in ASP.NET Core:

```html
<!-- In _Layout.cshtml or Index.cshtml -->
@{
    ViewData["Title"] = "ComboBox Example";
    List<string> sports = new List<string> { "Cricket", "Badminton", "Basketball", "Tennis" };
}

<ejs-combobox id="games" dataSource="@sports" placeholder="Select a sport"></ejs-combobox>
```

With data binding:

```html
@{
    var data = new List<object>
    {
        new { Id = 1, Text = "Microsoft", Code = "US" },
        new { Id = 2, Text = "Google", Code = "US" },
        new { Id = 3, Text = "Apple", Code = "US" }
    };
}

<ejs-combobox id="countries" 
              dataSource="@data" 
              fields-text="Text" 
              fields-value="Id"
              placeholder="Select a company">
</ejs-combobox>
```

## How-To Guides

### Quick Autofill
Use autofill for faster user input with autocomplete suggestions:
```html
<ejs-combobox autofill="true" allowFiltering="true"></ejs-combobox>
```

### Resizable Popup
Let users resize the dropdown popup for better visibility:
```html
<ejs-combobox allowResize="true" popupWidth="400px" popupHeight="300px"></ejs-combobox>
```

## Common Patterns

**Pattern 1: Filtered Search**
Enable filtering so users can search while typing:
```html
<ejs-combobox id="combobox" 
              dataSource="@data" 
              allowFiltering="true"
              placeholder="Type to search">
</ejs-combobox>
```

**Pattern 2: Cascading ComboBoxes**
Second ComboBox data depends on first selection. See [advanced-features.md](references/advanced-features.md#cascading-dropdowns) for complete example.

**Pattern 3: Custom Value Entry**
Allow users to enter values not in the list:
```html
<ejs-combobox id="combobox" 
              allowCustom="true" 
              dataSource="@data">
</ejs-combobox>
```

**Pattern 4: Grouped Data**
Group items by category:
```html
<ejs-combobox id="combobox" 
              dataSource="@data" 
              fields-groupBy="Category"
              fields-text="Name">
</ejs-combobox>
```

## Key Properties

| Property | Type | Purpose |
|----------|------|---------|
| `dataSource` | array or DataManager | Items to display in dropdown |
| `fields-text` | string | Which field to display as label |
| `fields-value` | string | Which field to use as selected value |
| `placeholder` | string | Hint text when empty |
| `allowFiltering` | bool | Enable search/filter as you type |
| `allowCustom` | bool | Allow user to enter values not in list |
| `fields-groupBy` | string | Field to group items by |
| `sortOrder` | Ascending/Descending | Sort order of items |
| `popupHeight` | string | Height of dropdown (e.g., "300px") |
| `popupWidth` | string | Width of dropdown |
| `read-only` | bool | Prevent user input, only allow selection |
| `enabled` | bool | Enable/disable the control |

## Common Use Cases

1. **Selecting from a list of countries/cities** → Use data binding + optional grouping
2. **Searching through large datasets** → Use filtering + virtual scrolling
3. **Creating dependent dropdowns** → Use cascading pattern (see advanced features)
4. **Displaying categorized items** → Use grouping and custom templates
5. **Matching API response data** → Use DataManager with fields mapping
6. **Building accessible forms** → Use ARIA attributes (see advanced features)

---

**Next Steps:**
- Start with [Getting Started](references/getting-started.md) to install and set up
- Move to [Data Binding](references/data-binding.md) for connecting your data
- Explore [Advanced Features](references/advanced-features.md) for complex scenarios

For issues or troubleshooting, check individual reference files for gotchas and edge cases.
