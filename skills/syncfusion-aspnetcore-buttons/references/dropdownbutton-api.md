# API Reference – ASP.NET Core DropDownButton

## Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `id` | `string` | - | Unique identifier |
| `content` | `string` | - | Button text |
| `items` | `List<object>` | - | Dropdown items |
| `cssClass` | `string` | - | CSS classes |
| `disabled` | `bool` | `false` | Disabled state |
| `iconCss` | `string` | - | Icon CSS class |
| `iconPosition` | `string` | `Left` | Icon position |

## Item Structure

| Property | Type | Description |
|----------|------|-------------|
| `text` | `string` | Item label |
| `iconCss` | `string` | Item icon |
| `disabled` | `bool` | Disable item |
| `separator` | `bool` | Separator line |
| `url` | `string` | Navigation URL |

## Example

```cshtml
@{
    List<object> items = new List<object>();
    items.Add(new { text = "Open", iconCss = "e-icons e-open-icon" });
    items.Add(new { text = "Save", iconCss = "e-icons e-save-icon" });
}

<ejs-dropdownbutton 
    id="ddb1" 
    content="File" 
    items="items"
    cssClass="e-primary">
</ejs-dropdownbutton>
```

---

## See Also

- [DropDownButton Getting Started](dropdownbutton-getting-started.md)
- [DropDownButton Popup Items](dropdownbutton-popup-items.md)
- [DropDownButton Events](dropdownbutton-events-and-interactivity.md)
