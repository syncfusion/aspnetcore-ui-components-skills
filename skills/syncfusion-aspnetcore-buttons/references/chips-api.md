# ChipList Component API Reference

## Table of Contents
- [Import](#import)
- [Properties](#properties)
- [Methods](#methods)
- [Events](#events)
- [ChipModel Interface](#chipmodel-interface)
- [Event Argument Types](#event-argument-types)

---

## Import

Add the Chips component to your Razor view:

```razor
@using Syncfusion.EJ2.Buttons
```

Or via Tag Helper:

```razor
@addTagHelper *, Syncfusion.EJ2
```

---

## Properties

### `Text` — `string`
Specifies the text content for a single chip.

- **Default:** `''`
- **Use when:** Rendering a single `ChipList` without `ChipItems`.

```razor
<ejs-chiplist id="single-chip" text="Janet Leverling"></ejs-chiplist>
```

---

### `Chips` — `List<ChipModel>` or `IEnumerable<ChipModel>`
Provides chip data programmatically as a collection. Alternative to using `ChipItems`.

- **Default:** `new List<ChipModel>()`

```razor
@{
    var chipsData = new List<ChipModel>
    {
        new ChipModel { Text = "React" },
        new ChipModel { Text = "Angular" }
    };
}

<ejs-chiplist id="chip-list" chips="chipsData"></ejs-chiplist>
```

C# in Controller:
```csharp
public IActionResult Index()
{
    var chipsData = new List<ChipModel>
    {
        new ChipModel { Text = "React", CssClass = "e-primary" },
        new ChipModel { Text = "Angular", CssClass = "e-success" },
        new ChipModel { Text = "Vue", CssClass = "e-info" }
    };
    
    return View(chipsData);
}
```

---

### `Selection` — `string` (`"None"` | `"Single"` | `"Multiple"`)
Defines the selection behavior of the chip list.

- **Default:** `"None"`
- `"None"` — No selection (use for action chips)
- `"Single"` — One chip selected at a time (choice chips)
- `"Multiple"` — Multiple chips can be selected (filter chips)

```razor
<ejs-chiplist id="multi-chip" selection="Multiple">
    <e-chips>
        <e-chip text="React"></e-chip>
        <e-chip text="Angular"></e-chip>
    </e-chips>
</ejs-chiplist>
```

---

### `SelectedChips` — `List<int>` or `List<string>`
Pre-selects chips by index or text value.

- **Default:** `new List<int>()`
- Requires `Selection` to be `"Single"` or `"Multiple"` to be visible.

```razor
<ejs-chiplist id="chip-list" selection="Multiple" selected-chips="@new List<int> { 0, 2 }">
    <e-chips>
        <e-chip text="React"></e-chip>
        <e-chip text="Angular"></e-chip>
        <e-chip text="Vue"></e-chip>
    </e-chips>
</ejs-chiplist>
```

---

### `EnableDelete` — `bool`
Shows a delete (×) icon on each chip, allowing removal.

- **Default:** `false`

```razor
<ejs-chiplist id="deletable-chips" enableDelete="true">
    <e-chips>
        <e-chip text="JavaScript"></e-chip>
        <e-chip text="TypeScript"></e-chip>
    </e-chips>
</ejs-chiplist>
```

---

### `CssClass` — `string`
Applies custom CSS class(es) to the chip list or individual chip elements.

- **Default:** `''`
- Common values: `'e-outline'`, `'e-primary'`, `'e-success'`, `'e-info'`, `'e-warning'`, `'e-danger'`

```razor
<ejs-chiplist id="styled-chips" cssClass="e-outline">
    <e-chips>
        <e-chip text="Outlined"></e-chip>
    </e-chips>
</ejs-chiplist>
```

---

### `Enabled` — `bool`
Enables or disables the entire chip list.

- **Default:** `true`
- When `false`, chips are visible but not interactive; `aria-disabled="true"` is applied.

```razor
<ejs-chiplist id="disabled-chips" enabled="false">
    <e-chips>
        <e-chip text="Disabled Chip"></e-chip>
    </e-chips>
</ejs-chiplist>
```

---

### `AllowDragAndDrop` — `bool`
Enables drag-and-drop reordering of chips within or across containers.

- **Default:** `false`

```razor
<ejs-chiplist id="draggable-chips" allowDragAndDrop="true">
    <e-chips>
        <e-chip text="Task 1"></e-chip>
        <e-chip text="Task 2"></e-chip>
    </e-chips>
</ejs-chiplist>
```

---

### `DragArea` — `string` or `HTMLElement`
Restricts the draggable chip's movement to a specific container. Accepts a CSS selector string or an `HTMLElement`.

- **Default:** `null` (no restriction — full page)

```razor
<div id="my-container" style="border: 2px dashed #ccc; padding: 16px;">
    <ejs-chiplist id="bounded-chips" allowDragAndDrop="true" dragArea="#my-container">
        <e-chips>
            <e-chip text="Task 1"></e-chip>
            <e-chip text="Task 2"></e-chip>
        </e-chips>
    </ejs-chiplist>
</div>
```

---

### `LeadingIconCss` — `string`
Specifies the CSS class for the leading (left) icon on a single chip.

- **Default:** `''`

```razor
<ejs-chiplist id="leading-icon-chip" text="Janet" leadingIconCss="janet-icon"></ejs-chiplist>
```

CSS:
```css
.janet-icon {
    background-image: url('/images/janet.png');
    background-size: cover;
}
```

---

### `LeadingIconUrl` — `string`
Specifies a direct image URL for the leading icon.

- **Default:** `''`

```razor
<ejs-chiplist id="icon-url-chip" text="Profile" leadingIconUrl="https://example.com/images/profile.png"></ejs-chiplist>
```

---

### `AvatarIconCss` — `string`
Specifies the CSS class for the avatar icon on a single chip.

- **Default:** `''`

```razor
<ejs-chiplist id="avatar-chip" text="Andrew" avatarIconCss="andrew-avatar"></ejs-chiplist>
```

---

### `AvatarText` — `string`
Displays text initials in a circular avatar.

- **Default:** `''`

```razor
<ejs-chiplist id="avatar-text-chip" text="Andrew" avatarText="A"></ejs-chiplist>
```

---

### `TrailingIconCss` — `string`
Specifies the CSS class for the trailing (right) icon on a single chip.

- **Default:** `''`

```razor
<ejs-chiplist id="trailing-icon-chip" text="Close" trailingIconCss="close-icon"></ejs-chiplist>
```

---

### `TrailingIconUrl` — `string`
Specifies a direct image URL for the trailing icon.

- **Default:** `''`

```razor
<ejs-chiplist id="trailing-url-chip" text="Remove" trailingIconUrl="https://example.com/images/remove.png"></ejs-chiplist>
```

---

### `EnableRtl` — `bool`
Enables right-to-left (RTL) rendering for Arabic, Hebrew, and other RTL languages.

- **Default:** `false`

```razor
<ejs-chiplist id="rtl-chips" enableRtl="true" selection="Multiple">
    <e-chips>
        <e-chip text="واجهة"></e-chip>
        <e-chip text="ويب"></e-chip>
    </e-chips>
</ejs-chiplist>
```

---

### `HtmlAttributes` — `Dictionary<string, object>`
Passes custom HTML attributes to the chip list wrapper element.

- **Default:** `null`

```razor
@{
    var htmlAttributes = new Dictionary<string, object>
    {
        { "data-custom", "value" },
        { "aria-label", "Filter by category" }
    };
}

<ejs-chiplist id="custom-chips" html-attributes="htmlAttributes"></ejs-chiplist>
```

---

## Methods

### `GetSelectedChips()`
Returns the list of currently selected chips as indices.

**JavaScript:**
```javascript
// In your view's <script> section
document.getElementById('chip-list').ej2_instances[0].getSelectedChips().then(selectedIndices => {
    console.log('Selected:', selectedIndices.join(', '));
});
```

---

### `Select(int index)`
Selects a chip by its index.

```javascript
// Selects the second chip
document.getElementById('chip-list').ej2_instances[0].select(1);
```

---

### `Unselect(int index)`
Deselects a chip by its index.

```javascript
// Deselects the first chip
document.getElementById('chip-list').ej2_instances[0].unselect(0);
```

---

### `Delete(int index)`
Removes a chip by its index.

```javascript
// Removes the third chip
document.getElementById('chip-list').ej2_instances[0].delete(2);
```

---

### `Refresh()`
Re-renders the chip list (useful after data changes).

```javascript
// Re-renders the chip list
document.getElementById('chip-list').ej2_instances[0].refresh();
```

---

## Events

### `OnCreated`
Fires when the Chips component is created and initialized.

```razor
<ejs-chiplist id="event-chips" created="onChipsCreated">
    <e-chips>
        <e-chip text="React"></e-chip>
    </e-chips>
</ejs-chiplist>

<script>
    function onChipsCreated() {
        console.log('Chips component created!');
    }
</script>
```

---

### `OnClick`
Fires when a chip is clicked.

**Event Args:**
- `ChipModel` — the clicked chip data
- `Index` — index of the clicked chip
- `Text` — text content of the chip

```razor
<ejs-chiplist id="click-chips" clicked="onChipClicked">
    <e-chips>
        <e-chip text="Send"></e-chip>
        <e-chip text="Delete"></e-chip>
    </e-chips>
</ejs-chiplist>

<script>
    function onChipClicked(args) {
        console.log('Clicked: ' + args.text + ' at index ' + args.index);
    }
</script>
```

---

### `OnDelete`
Fires when a chip is deleted (when `EnableDelete="true"`).

```razor
<ejs-chiplist id="delete-chips" enableDelete="true" onDelete="onChipDeleted">
    <e-chips>
        <e-chip text="Item 1"></e-chip>
        <e-chip text="Item 2"></e-chip>
    </e-chips>
</ejs-chiplist>

<script>
    function onChipDeleted(args) {
        console.log('Deleted chip: ' + args.text);
    }
</script>
```

---

### `OnSelect`
Fires when a chip selection state changes.

```razor
<ejs-chiplist id="select-chips" selection="Multiple" onSelect="onChipSelected">
    <e-chips>
        <e-chip text="React"></e-chip>
        <e-chip text="Angular"></e-chip>
    </e-chips>
</ejs-chiplist>

<script>
    function onChipSelected(args) {
        console.log('Selected: ' + args.isSelected + ' for chip ' + args.text);
    }
</script>
```

---

### `OnDragStart`
Fires when a chip drag operation begins (if `AllowDragAndDrop="true"`).

```razor
<ejs-chiplist id="drag-chips" allowDragAndDrop="true" onDragStart="onDragStarted">
    <e-chips>
        <e-chip text="Item 1"></e-chip>
    </e-chips>
</ejs-chiplist>

<script>
    function onDragStarted(args) {
        console.log('Started dragging: ' + args.text);
    }
</script>
```

---

### `OnDragEnd`
Fires when a chip drag operation completes.

```javascript
function onDragEnded(args) {
    console.log('Finished dragging: ' + args.text);
}
```

---

## ChipModel Interface

The `ChipModel` interface represents a single chip's data:

```csharp
public class ChipModel
{
    public string Text { get; set; }
    public string CssClass { get; set; }
    public string AvatarText { get; set; }
    public string AvatarIconCss { get; set; }
    public string LeadingIconCss { get; set; }
    public string LeadingIconUrl { get; set; }
    public string TrailingIconCss { get; set; }
    public string TrailingIconUrl { get; set; }
    public bool Enabled { get; set; } = true;
    public Dictionary<string, object> HtmlAttributes { get; set; }
}
```

---

## Event Argument Types

### `ChipEventArgs`
```csharp
public class ChipEventArgs
{
    public int Index { get; set; }
    public string Text { get; set; }
    public ChipModel ChipModel { get; set; }
}
```

### `ChipSelectEventArgs`
```csharp
public class ChipSelectEventArgs : ChipEventArgs
{
    public bool IsSelected { get; set; }
}
```

### `ChipDragEventArgs`
```csharp
public class ChipDragEventArgs : ChipEventArgs
{
    public string DropArea { get; set; }
    public int OldIndex { get; set; }
    public int NewIndex { get; set; }
}
```

---

## Summary

This API reference covers all essential properties, methods, and events for the Syncfusion ASP.NET Core Chips component. Refer to specific sections for implementation details and code examples.
