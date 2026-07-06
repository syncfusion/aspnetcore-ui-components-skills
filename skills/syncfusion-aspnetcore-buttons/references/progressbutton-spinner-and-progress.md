# Spinner and Progress – ASP.NET Core ProgressButton

## Progress Display

```cshtml
<ejs-progressbutton 
    id="progressBtn" 
    content="Upload"
    enableProgress="true">
</ejs-progressbutton>

<script>
    document.getElementById('progressBtn').addEventListener('click', function() {
        const btn = ej2_instances['progressBtn'][0];
        let progress = 0;
        
        const interval = setInterval(() => {
            progress += 5;
            btn.progress = progress;
            
            if (progress >= 100) {
                clearInterval(interval);
            }
        }, 100);
    });
</script>
```

## With Spinner

```cshtml
<ejs-progressbutton 
    id="progressBtn" 
    content="Processing"
    spinSettings="@new { position = 'Left' }">
</ejs-progressbutton>
```

## Without Spinner

```cshtml
<ejs-progressbutton 
    id="progressBtn" 
    content="Download"
    hideSpinner="true">
</ejs-progressbutton>
```

---

## See Also

- [ProgressButton Getting Started](progressbutton-getting-started.md)
- [ProgressButton Style and Appearance](progressbutton-style-and-appearance.md)
- [ProgressButton API](progressbutton-api.md)
