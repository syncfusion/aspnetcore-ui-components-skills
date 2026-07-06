# API Reference – ASP.NET Core RadioButton

## Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `id` | `string` | - | Unique identifier |
| `label` | `string` | - | Label text |
| `name` | `string` | - | Group name (for mutual exclusivity) |
| `value` | `string` | - | Form submission value |
| `checked` | `bool` | `false` | Initial checked state |
| `disabled` | `bool` | `false` | Disable radio button |
| `labelPosition` | `string` | `Before` | Label position: Before/After |
| `cssClass` | `string` | - | CSS classes |
| `enableRtl` | `bool` | `false` | RTL layout |

## Example

```cshtml
<fieldset>
    <legend>Select Option</legend>
    <ul>
        <li>
            <ejs-radiobutton 
                id="r1" 
                name="options"
                value="opt1"
                label="Option 1"
                checked="true">
            </ejs-radiobutton>
        </li>
        <li>
            <ejs-radiobutton 
                id="r2" 
                name="options"
                value="opt2"
                label="Option 2">
            </ejs-radiobutton>
        </li>
    </ul>
</fieldset>
```

---

## See Also

- [RadioButton Getting Started](radiobutton-getting-started.md)
- [RadioButton Features and State](radiobutton-features-and-state.md)
- [RadioButton Label and Size](radiobutton-label-and-size.md)
