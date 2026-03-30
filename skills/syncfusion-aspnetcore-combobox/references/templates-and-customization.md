# Templates & Customization in ComboBox

## Table of Contents

- [Overview](#overview)
- [Item Templates](#item-templates)
- [Header & Footer Templates](#header--footer-templates)
- [Group Header Templates](#group-header-templates)
- [Popup Configuration](#popup-configuration)
- [CSS Customization](#css-customization)
- [Theme Customization](#theme-customization)
- [Common Patterns](#common-patterns)

## Overview

ComboBox offers extensive customization through:
- **Item Templates** - Customize each dropdown item display
- **Header/Footer Templates** - Add content above/below the list
- **Group Header Templates** - Customize group labels
- **Popup Control** - Adjust width, height, and positioning
- **CSS & Themes** - Style using CSS classes and variables

## Item Templates

Display rich content for each dropdown item instead of plain text.

**Basic Item Template:**
```html
@{
    var employees = new List<Employee>
    {
        new Employee { EmployeeId = 1, EmployeeName = "Michael", EmployeeImage = "michael.png" },
        new Employee { EmployeeId = 2, EmployeeName = "Sarah", EmployeeImage = "sarah.png" }
    };
}

<ejs-combobox id="employee"
              dataSource="@employees"
              fields-text="EmployeeName"
              fields-value="EmployeeId"
              placeholder="Select employee">
    <e-combobox-templates>
        <e-template type="ItemTemplate">
            <span class="item-container">
                <img src="~/images/${EmployeeImage}" alt="${EmployeeName}" class="item-image" />
                <span class="item-text">${EmployeeName}</span>
            </span>
        </e-template>
    </e-combobox-templates>
</ejs-combobox>

<style>
    .item-container {
        display: flex;
        align-items: center;
        gap: 10px;
        padding: 5px;
    }
    .item-image {
        width: 30px;
        height: 30px;
        border-radius: 50%;
    }
</style>
```

**Template Syntax:**
- `${PropertyName}` - Insert data property value
- HTML markup allowed - divs, spans, images, etc.
- CSS classes can be applied for styling

**Example with multiple properties:**
```html
<ejs-combobox id="product"
              dataSource="@products"
              fields-text="ProductName"
              fields-value="ProductId">
    <e-combobox-templates>
        <e-template type="ItemTemplate">
            <div class="product-item">
                <div class="product-name">${ProductName}</div>
                <div class="product-price">$${Price}</div>
                <div class="product-stock">
                    Stock: ${StockQuantity > 10 ? 'Available' : 'Low'}
                </div>
            </div>
        </e-template>
    </e-combobox-templates>
</ejs-combobox>

<style>
    .product-item {
        padding: 10px;
        border-bottom: 1px solid #eee;
    }
    .product-name { font-weight: bold; }
    .product-price { color: green; }
    .product-stock { font-size: 12px; color: #666; }
</style>
```

## Header & Footer Templates

Add content above or below the dropdown list.

**Header Template (content above list):**
```html
<ejs-combobox id="country"
              dataSource="@countries"
              fields-text="CountryName"
              fields-value="CountryId">
    <e-combobox-templates>
        <e-template type="HeaderTemplate">
            <div class="header-content">
                <strong>Select a country from below</strong>
                <small>or search by typing</small>
            </div>
        </e-template>
    </e-combobox-templates>
</ejs-combobox>

<style>
    .header-content {
        padding: 10px;
        background-color: #f5f5f5;
        border-bottom: 1px solid #ddd;
        text-align: center;
    }
</style>
```

**Footer Template (content below list):**
```html
<ejs-combobox id="country"
              dataSource="@countries"
              fields-text="CountryName"
              fields-value="CountryId">
    <e-combobox-templates>
        <e-template type="FooterTemplate">
            <div class="footer-content">
                <small>Total items: ${TotalCount}</small>
            </div>
        </e-template>
    </e-combobox-templates>
</ejs-combobox>

<style>
    .footer-content {
        padding: 10px;
        background-color: #f9f9f9;
        border-top: 1px solid #ddd;
        text-align: right;
        font-size: 12px;
        color: #666;
    }
</style>
```

**No Records Template (when no items match filter):**
```html
<ejs-combobox id="country"
              allowFiltering="true">
    <e-combobox-templates>
        <e-template type="NoRecordsTemplate">
            <div class="no-records">
                <p>😞 No matching countries found</p>
                <small>Try a different search term</small>
            </div>
        </e-template>
    </e-combobox-templates>
</ejs-combobox>

<style>
    .no-records {
        padding: 20px;
        text-align: center;
        color: #999;
    }
</style>
```

## Group Header Templates

Customize how group labels appear when grouping is enabled.

**Basic Grouping with Custom Header:**
```html
@{
    var products = new List<Product>
    {
        new Product { ProductId = 1, ProductName = "Apple", Category = "Fruits" },
        new Product { ProductId = 2, ProductName = "Orange", Category = "Fruits" },
        new Product { ProductId = 3, ProductName = "Carrot", Category = "Vegetables" },
        new Product { ProductId = 4, ProductName = "Broccoli", Category = "Vegetables" }
    };
}

<ejs-combobox id="product"
              dataSource="@products"
              fields-text="ProductName"
              fields-value="ProductId"
              fields-groupBy="Category">
    <e-combobox-templates>
        <e-template type="GroupTemplate">
            <div class="group-header">
                <strong>${Category}</strong>
                <span class="category-icon">🏷️</span>
            </div>
        </e-template>
    </e-combobox-templates>
</ejs-combobox>

<style>
    .group-header {
        display: flex;
        justify-content: space-between;
        padding: 10px 15px;
        background-color: #e8f5e9;
        font-weight: bold;
        color: #2e7d32;
        border-bottom: 2px solid #c8e6c9;
    }
</style>
```

## Popup Configuration

Control dropdown popup size and positioning.

**Adjust Width & Height:**
```html
<ejs-combobox id="country"
              dataSource="@data"
              popupHeight="350px"
              popupWidth="400px">
</ejs-combobox>
```

**Auto-adjust width to match input:**
```html
<ejs-combobox id="country"
              dataSource="@data"
              popupHeight="300px"
              popupWidth="100%">
</ejs-combobox>
```

**Dynamic popup sizing:**
```html
<ejs-combobox id="country"
              dataSource="@data"
              open="onPopupOpen">
</ejs-combobox>

<script>
function onPopupOpen(args) {
    // Adjust based on content
    var popup = args.popup;
    if (args.baseEventArgs.model.dataSource.length > 20) {
        popup.height = 400;
        popup.width = 350;
    } else {
        popup.height = 200;
        popup.width = 300;
    }
}
</script>
```

## CSS Customization

Apply custom styling to ComboBox elements.

**Basic CSS Classes:**
```css
/* Container */
.e-combobox {
    width: 300px;
}

/* Input field */
.e-combobox .e-input {
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
}

/* Input field when focused */
.e-combobox .e-input-focus {
    border-color: #007bff;
    box-shadow: 0 0 5px rgba(0, 123, 255, 0.5);
}

/* Dropdown list item */
.e-combobox .e-list-item {
    padding: 10px 15px;
}

/* Highlighted item (on hover) */
.e-combobox .e-list-item.e-hover {
    background-color: #007bff;
    color: white;
}

/* Selected item */
.e-combobox .e-list-item.e-active {
    background-color: #0056b3;
    color: white;
}

/* Dropdown popup */
.e-combobox .e-popup {
    border: 1px solid #ddd;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}
```

**Custom styling example:**
```html
<ejs-combobox id="custom" dataSource="@data"></ejs-combobox>

<style>
    #custom {
        width: 400px;
    }
    
    #custom .e-input-group {
        border: 2px solid #4CAF50;
        border-radius: 8px;
    }
    
    #custom .e-list-item {
        padding: 12px;
        font-size: 14px;
    }
    
    #custom .e-list-item.e-hover {
        background-color: #81c784;
        color: white;
    }
</style>
```

## Theme Customization

Use CSS variables to customize colors without editing entire stylesheets.

**Available CSS Variables:**
```css
/* Primary color for focus/selection */
--e-primary: #007bff;

/* Background colors */
--e-bg: #ffffff;
--e-bg-alt: #f5f5f5;

/* Text colors */
--e-text: #333333;
--e-text-muted: #999999;

/* Border color */
--e-border: #dddddd;

/* Hover background */
--e-hover-bg: #e8f4f8;

/* Selected background */
--e-selected-bg: #007bff;
--e-selected-text: #ffffff;
```

**Override theme colors:**
```html
<ejs-combobox id="themed" dataSource="@data"></ejs-combobox>

<style>
    :root {
        --e-primary: #FF6B6B;
        --e-border: #FFD3D3;
        --e-selected-bg: #FF6B6B;
    }
    
    #themed .e-input-group {
        border-color: var(--e-border);
    }
    
    #themed .e-list-item.e-active {
        background-color: var(--e-selected-bg);
    }
</style>
```

## Common Patterns

**Pattern 1: Rich item display with images and text**
```html
<ejs-combobox id="employee"
              fields-text="Name"
              fields-value="EmployeeId">
    <e-combobox-templates>
        <e-template type="ItemTemplate">
            <table class="employee-list">
                <tr>
                    <td>
                        <img src="~/images/${Image}" class="employee-photo" />
                    </td>
                    <td>
                        <div class="employee-name">${Name}</div>
                        <div class="employee-title">${Title}</div>
                    </td>
                </tr>
            </table>
        </e-template>
    </e-combobox-templates>
</ejs-combobox>

<style>
    .employee-list { width: 100%; border-collapse: collapse; }
    .employee-photo { width: 40px; height: 40px; border-radius: 50%; margin-right: 10px; }
    .employee-name { font-weight: bold; }
    .employee-title { font-size: 12px; color: #666; }
</style>
```

**Pattern 2: Item template with custom rendering logic**
```html
<ejs-combobox id="product"
              fields-text="ProductName"
              dataSource="@products">
    <e-combobox-templates>
        <e-template type="ItemTemplate">
            <div class="product-row">
                <div class="product-info">
                    <div>${ProductName}</div>
                    <small>Stock: ${Quantity}</small>
                </div>
                <div class="product-badge" style="${Quantity > 50 ? 'background-color: green;' : 'background-color: orange;'}">
                    ${Quantity > 50 ? 'In Stock' : 'Low Stock'}
                </div>
            </div>
        </e-template>
    </e-combobox-templates>
</ejs-combobox>

<style>
    .product-row {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 10px;
    }
    .product-badge {
        padding: 4px 8px;
        color: white;
        border-radius: 4px;
        font-size: 12px;
    }
</style>
```

**Pattern 3: Styled grouping with icons**
```html
<ejs-combobox id="grouped"
              fields-groupBy="Category"
              fields-text="Name">
    <e-combobox-templates>
        <e-template type="GroupTemplate">
            <div class="group-section">
                <span class="category-icon">${GetIcon(Category)}</span>
                <strong>${Category}</strong>
            </div>
        </e-template>
        <e-template type="ItemTemplate">
            <div class="grouped-item">
                <span class="item-dot"></span>
                ${Name}
            </div>
        </e-template>
    </e-combobox-templates>
</ejs-combobox>

<style>
    .group-section {
        display: flex;
        align-items: center;
        gap: 8px;
        padding: 8px 12px;
        background-color: #f0f0f0;
        font-weight: bold;
    }
    .grouped-item {
        display: flex;
        align-items: center;
        gap: 8px;
        padding: 8px 20px;
    }
    .item-dot {
        display: inline-block;
        width: 6px;
        height: 6px;
        background-color: #999;
        border-radius: 50%;
    }
</style>
```
