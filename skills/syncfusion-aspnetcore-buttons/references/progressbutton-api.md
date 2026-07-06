# API Reference – ASP.NET Core ProgressButton

## Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `id` | `string` | - | Unique identifier |
| `content` | `string` | - | Button text |
| `enableProgress` | `bool` | `false` | Enable progress display |
| `progress` | `number` | 0 | Current progress value (0-100) |
| `progressText` | `string` | - | Progress text display |
| `hideSpinner` | `bool` | `false` | Hide spinner animation |
| `cssClass` | `string` | - | CSS classes |
| `disabled` | `bool` | `false` | Disabled state |

## Example

```cshtml
<ejs-progressbutton 
    id="progressBtn" 
    content="Upload"
    enableProgress="true"
    cssClass="e-primary">
</ejs-progressbutton>

<script>
    document.getElementById('progressBtn').addEventListener('click', function() {
        const btn = ej2_instances['progressBtn'][0];
        btn.progress = 50;
    });
</script>
```

---

## See Also

- [ProgressButton Getting Started](progressbutton-getting-started.md)
- [ProgressButton Spinner and Progress](progressbutton-spinner-and-progress.md)
- [ProgressButton Style and Appearance](progressbutton-style-and-appearance.md)
