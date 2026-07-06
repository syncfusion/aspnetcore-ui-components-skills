# Items – ASP.NET Core Speed Dial

## Basic Items

```cshtml
@{
    List<object> items = new List<object>();
    items.Add(new { text = "Edit" });
    items.Add(new { text = "Delete" });
    items.Add(new { text = "Add" });
}

<ejs-speeddial 
    id="speeddial"
    content="Action"
    items="items"
    target="#container">
</ejs-speeddial>
```

## Items with Icons

```cshtml
@{
    List<object> items = new List<object>();
    items.Add(new { text = "Edit", iconCss = "e-icons e-edit-icon" });
    items.Add(new { text = "Delete", iconCss = "e-icons e-delete-icon" });
    items.Add(new { text = "Add", iconCss = "e-icons e-plus-icon" });
}

<ejs-speeddial 
    id="speeddial"
    content="Action"
    items="items"
    iconCss="e-icons e-share-icon"
    target="#container">
</ejs-speeddial>
```

## Items with Titles

```cshtml
@{
    List<object> items = new List<object>();
    items.Add(new { text = "Cut", title = "Cut selected text" });
    items.Add(new { text = "Copy", title = "Copy selected text" });
    items.Add(new { text = "Paste", title = "Paste from clipboard" });
}

<ejs-speeddial 
    id="speeddial"
    items="items"
    target="#container">
</ejs-speeddial>
```

---

## See Also

- [Speed Dial Getting Started](speeddial-getting-started.md)
- [Speed Dial Display Modes](speeddial-display-modes.md)
- [Speed Dial Positions](speeddial-positions.md)
- [Speed Dial API](speeddial-api.md)
