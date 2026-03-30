# Popup Resizing in ComboBox

## Overview

The ComboBox popup can be dynamically resized by users via the `allowResize` property. When enabled, a resize handle appears at the bottom-right corner of the dropdown popup, allowing users to drag and adjust both width and height. The resized dimensions are retained across sessions for consistency.

**Key Features:**
- User-draggable resize handle
- Maintains dimensions across page reloads
- Improves visibility of long lists
- Better control for accessibility
- Responsive resize behavior

## Enable Popup Resize

Use the `allowResize` property to enable resizing.

```html
@{
    var countries = new List<Country>
    {
        new Country { Name = "United States", Code = "US" },
        new Country { Name = "United Kingdom", Code = "UK" },
        new Country { Name = "India", Code = "IN" },
        new Country { Name = "Germany", Code = "DE" }
    };
}

<ejs-combobox id="country"
              dataSource="@countries"
              fields-text="Name"
              fields-value="Code"
              allowResize="true"
              placeholder="Select country (drag popup corner to resize)">
</ejs-combobox>
```

**Visual:**
```
Popup with resize enabled:
┌─────────────────────────┐
│ United States           │
│ United Kingdom          │
│ India                   │
│ Germany                 │
│ (more items below)      │
│                      ⌙  │ ← Resize handle (draggable)
└─────────────────────────┘
```

## Set Initial Popup Size

Define initial width and height for the popup.

```html
<ejs-combobox id="country"
              dataSource="@countries"
              fields-text="Name"
              fields-value="Code"
              allowResize="true"
              popupWidth="400px"
              popupHeight="300px"
              placeholder="Resizable popup">
</ejs-combobox>
```

**Sizing Options:**
- `popupWidth="400px"` - Fixed pixel width
- `popupWidth="100%"` - Match input width
- `popupHeight="250px"` - Fixed pixel height
- `popupHeight="auto"` - Auto-adjust to content

## Programmatic Resize Control

Control popup size via JavaScript.

```html
<ejs-combobox id="country"
              dataSource="@countries"
              allowResize="true">
</ejs-combobox>

<button onclick="resizePopup()">Enlarge Popup</button>
<button onclick="resetPopup()">Reset Size</button>

<script>
function resizePopup() {
    var combobox = document.getElementById('country').ej2_instances[0];
    if (combobox.popup) {
        combobox.popup.height = 400;
        combobox.popup.width = 500;
    }
}

function resetPopup() {
    var combobox = document.getElementById('country').ej2_instances[0];
    if (combobox.popup) {
        combobox.popup.height = 300; // Default
        combobox.popup.width = 300;  // Default
    }
}
</script>
```

## Local Storage Integration

Persist resized popup dimensions across page reloads.

```html
<ejs-combobox id="country"
              dataSource="@countries"
              allowResize="true"
              open="onPopupOpen"
              close="onPopupClose">
</ejs-combobox>

<script>
// Load saved dimensions when popup opens
function onPopupOpen(args) {
    var saved = localStorage.getItem('comboboxPopupSize');
    if (saved) {
        var size = JSON.parse(saved);
        args.popup.height = size.height;
        args.popup.width = size.width;
    }
}

// Save dimensions when popup closes
function onPopupClose(args) {
    var size = {
        height: args.popup.height,
        width: args.popup.width
    };
    localStorage.setItem('comboboxPopupSize', JSON.stringify(size));
    console.log("Popup size saved:", size);
}
</script>
```

## Minimum & Maximum Size Constraints

Limit resize dimensions to reasonable bounds.

```html
<ejs-combobox id="country"
              dataSource="@countries"
              allowResize="true"
              open="onPopupOpen">
</ejs-combobox>

<script>
function onPopupOpen(args) {
    // Set minimum and maximum sizes
    const MIN_WIDTH = 200;
    const MAX_WIDTH = 800;
    const MIN_HEIGHT = 200;
    const MAX_HEIGHT = 600;
    
    // Constrain current size to limits
    args.popup.width = Math.max(
        MIN_WIDTH, 
        Math.min(MAX_WIDTH, args.popup.width || 300)
    );
    args.popup.height = Math.max(
        MIN_HEIGHT, 
        Math.min(MAX_HEIGHT, args.popup.height || 300)
    );
}

// Monitor resize during drag
document.addEventListener('mousemove', function(e) {
    // User is dragging resize handle
    // Validate bounds in real-time
});
</script>
```

## Responsive Resize Behavior

Adjust popup size based on content or screen size.

```html
<ejs-combobox id="product"
              dataSource="@largeDataSet"
              allowResize="true"
              open="onPopupOpen">
</ejs-combobox>

<script>
function onPopupOpen(args) {
    // Responsive sizing based on device/screen
    if (window.innerWidth < 600) {
        // Mobile: smaller popup
        args.popup.width = 250;
        args.popup.height = 200;
    } else if (window.innerWidth < 1200) {
        // Tablet: medium popup
        args.popup.width = 350;
        args.popup.height = 300;
    } else {
        // Desktop: larger popup
        args.popup.width = 500;
        args.popup.height = 400;
    }
}
</script>
```

## Resize with Template Content

Resize behavior works with templated content.

```csharp
public class Product
{
    public int ProductId { get; set; }
    public string ProductName { get; set; }
    public decimal Price { get; set; }
    public string ImageUrl { get; set; }
}
```

**View with templates:**
```html
<ejs-combobox id="product"
              dataSource="@products"
              fields-text="ProductName"
              fields-value="ProductId"
              allowResize="true"
              popupWidth="400px"
              popupHeight="300px">
    <e-combobox-templates>
        <e-template type="ItemTemplate">
            <div class="product-item">
                <img src="${ImageUrl}" alt="${ProductName}" class="product-img" />
                <div class="product-details">
                    <div class="product-name">${ProductName}</div>
                    <div class="product-price">$${Price}</div>
                </div>
            </div>
        </e-template>
    </e-combobox-templates>
</ejs-combobox>

<style>
    .product-item {
        display: flex;
        gap: 10px;
        padding: 10px;
        border-bottom: 1px solid #eee;
    }
    .product-img {
        width: 60px;
        height: 60px;
        border-radius: 4px;
    }
    .product-details {
        flex: 1;
    }
    .product-name { font-weight: bold; }
    .product-price { color: green; }
</style>
```

**Benefit:** Users can resize to see full product images and details.

## Common Patterns

**Pattern 1: Resize for accessibility**
```html
<!-- Allow users with low vision to enlarge popup -->
<ejs-combobox id="accessible"
              dataSource="@data"
              allowResize="true"
              popupHeight="250px"
              aria-label="Resizable dropdown">
</ejs-combobox>

<script>
// Users can also resize via keyboard shortcuts
document.addEventListener('keydown', function(e) {
    if (e.ctrlKey && e.key === '+') {
        // Ctrl++ to enlarge
        enlargeCurrentPopup();
    }
    if (e.ctrlKey && e.key === '-') {
        // Ctrl+- to shrink
        shrinkCurrentPopup();
    }
});
</script>
```

**Pattern 2: Remember user preference per control**
```html
<ejs-combobox id="country" 
              dataSource="@countries"
              allowResize="true"
              close="onCountryClose">
</ejs-combobox>

<ejs-combobox id="city"
              dataSource="@cities"
              allowResize="true"
              close="onCityClose">
</ejs-combobox>

<script>
function onCountryClose(args) {
    localStorage.setItem('countryComboSize', JSON.stringify({
        w: args.popup.width,
        h: args.popup.height
    }));
}

function onCityClose(args) {
    localStorage.setItem('cityComboSize', JSON.stringify({
        w: args.popup.width,
        h: args.popup.height
    }));
}

// On page load, restore sizes
window.addEventListener('load', function() {
    var comboCountry = document.getElementById('country').ej2_instances[0];
    var comboCity = document.getElementById('city').ej2_instances[0];
    
    var countrySize = JSON.parse(localStorage.getItem('countryComboSize') || '{}');
    var citySize = JSON.parse(localStorage.getItem('cityComboSize') || '{}');
    
    if (countrySize.w) comboCountry.popupWidth = countrySize.w;
    if (citySize.w) comboCity.popupWidth = citySize.w;
});
</script>
```

**Pattern 3: Resize for large dataset visibility**
```html
@{
    // 500 items in list
    var largeList = Enumerable.Range(1, 500)
        .Select(i => new { Id = i, Name = $"Item {i:D3}" })
        .ToList();
}

<ejs-combobox id="large"
              dataSource="@largeList"
              fields-text="Name"
              fields-value="Id"
              allowResize="true"
              enableVirtualization="true"
              popupWidth="350px"
              popupHeight="400px"
              allowFiltering="true"
              placeholder="Resizable + virtual scroll">
</ejs-combobox>
```

**Benefits:**
- User can resize to see more items at once
- Virtual scrolling ensures performance
- Combined features for optimal UX

## Styling Resize Handle

Customize the appearance of the resize handle.

```css
/* Resize handle styling */
.e-combobox .e-popup .e-resize-handle {
    background-color: #4CAF50;
    border: 1px solid #2E7D32;
    cursor: se-resize;
    width: 15px;
    height: 15px;
    bottom: 0;
    right: 0;
}

.e-combobox .e-popup .e-resize-handle:hover {
    background-color: #45a049;
}

/* Optional: Add resize icon */
.e-combobox .e-popup .e-resize-handle::after {
    content: '⤡';
    font-size: 12px;
    color: white;
}
```

## Troubleshooting

**Issue: Resize handle not appearing**
- ✓ Verify `allowResize="true"` is set
- ✓ Check browser console for errors
- ✓ Ensure popup has sufficient size (at least 100x100px)
- ✓ Clear browser cache if CSS not updating

**Issue: Resized size not persisting**
- ✓ Implement localStorage code (see Local Storage Integration)
- ✓ Check browser privacy settings (localStorage may be disabled)
- ✓ Verify close event fires when popup closes
- ✓ Test in private/incognito mode (localStorage may be blocked)

**Issue: Resize conflicts with mobile touch**
- ✓ Resize is primarily for mouse/desktop
- ✓ Touch users may accidentally trigger on mobile
- ✓ Disable on mobile: Check `navigator.maxTouchPoints > 0`
- ✓ Or use different UX for mobile (e.g., buttons)

**Issue: Performance degradation with resize**
- ✓ Ensure virtual scrolling is enabled for large lists
- ✓ Monitor memory usage in DevTools
- ✓ Limit maximum popup size to reasonable bounds
- ✓ Avoid excessive re-renders during resize
