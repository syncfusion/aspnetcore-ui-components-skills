# API Reference – ASP.NET Core Speed Dial

## Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `id` | `string` | - | Unique identifier |
| `content` | `string` | - | Button text |
| `items` | `List<object>` | - | Menu items |
| `target` | `string` | - | Target container selector |
| `position` | `string` | `BottomRight` | Position of Speed Dial |
| `direction` | `string` | `Up` | Direction items open |
| `mode` | `string` | `Linear` | Display mode: Linear or Radial |
| `iconCss` | `string` | - | Icon CSS class |
| `cssClass` | `string` | - | Custom CSS classes |
| `disabled` | `bool` | `false` | Disabled state |

## Item Structure

| Property | Type | Description |
|----------|------|-------------|
| `text` | `string` | Item label |
| `iconCss` | `string` | Item icon |
| `title` | `string` | Tooltip text |
| `disabled` | `bool` | Disable item |

## Example

```cshtml
@{
    List<object> items = new List<object>();
    items.Add(new { text = "Edit", iconCss = "e-icons e-edit-icon", title = "Edit Item" });
    items.Add(new { text = "Delete", iconCss = "e-icons e-delete-icon", title = "Delete Item" });
}

<ejs-speeddial 
    id="speeddial"
    content="Action"
    items="items"
    direction="Up"
    position="BottomRight"
    target="#container"
    mode="Linear">
</ejs-speeddial>
```

---

## See Also

- [Speed Dial Getting Started](speeddial-getting-started.md)
- [Speed Dial Items](speeddial-items.md)
- [Speed Dial Display Modes](speeddial-display-modes.md)
- [Speed Dial Positions](speeddial-positions.md)
