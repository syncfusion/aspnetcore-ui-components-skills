# API Reference – ASP.NET Core Floating Action Button

## Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `id` | `string` | - | Unique identifier |
| `content` | `string` | - | Button text |
| `iconCss` | `string` | - | Icon CSS class |
| `cssClass` | `string` | - | CSS classes |
| `position` | `string` | `BottomRight` | FAB position |
| `target` | `string` | - | Target container |
| `disabled` | `bool` | `false` | Disabled state |
| `visible` | `bool` | `true` | Visibility |

## Example

```cshtml
<ejs-fab 
    id="fab" 
    content="Add"
    iconCss="e-icons e-plus-icon"
    cssClass="e-primary"
    position="BottomRight">
</ejs-fab>
```

---

## See Also

- [FAB Getting Started](floating-action-button-getting-started.md)
- [FAB Positions](floating-action-button-positions.md)
- [FAB Icons](floating-action-button-icons.md)
- [FAB Styles](floating-action-button-styles.md)
