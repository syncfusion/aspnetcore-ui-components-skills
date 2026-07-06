# Button Types and Styles – ASP.NET Core

## Table of Contents
1. [Button Styles (Color Variants)](#button-styles-color-variants)
2. [Flat Button](#flat-button)
3. [Outline Button](#outline-button)
4. [Round Button](#round-button)
5. [Toggle Button](#toggle-button)
6. [Basic HTML Button Types](#basic-html-button-types)
7. [Icons](#icons)
   - [Font Icons](#font-icons)
   - [SVG Icons](#svg-icons)
   - [Icon Position](#icon-position)
8. [Button Sizes](#button-sizes)

---

## Button Styles (Color Variants)

Apply predefined semantic color styles using the `cssClass` attribute. These convey intent visually but do not alter functional behavior — always use meaningful button text for assistive technology users.

| `cssClass` value | Visual meaning |
|---|---|
| `e-primary` | Primary action |
| `e-success` | Positive / confirmatory action |
| `e-info` | Informative action |
| `e-warning` | Cautionary action |
| `e-danger` | Destructive / negative action |
| `e-link` | Hyperlink appearance |

**Basic Color Variants:**

```cshtml
<div class="button-group">
    <ejs-button id="button1" cssClass="e-primary">Primary</ejs-button>
    <ejs-button id="button2" cssClass="e-success">Success</ejs-button>
    <ejs-button id="button3" cssClass="e-info">Info</ejs-button>
    <ejs-button id="button4" cssClass="e-warning">Warning</ejs-button>
    <ejs-button id="button5" cssClass="e-danger">Danger</ejs-button>
    <ejs-button id="button6" cssClass="e-link">Link</ejs-button>
</div>
```

Multiple classes can be combined (space-separated):

```cshtml
<ejs-button id="button" cssClass="e-small e-primary">Small Primary Button</ejs-button>
```

---

## Flat Button

A flat button has no background color — useful for secondary or low-emphasis actions.

```cshtml
<ejs-button id="flatButton" cssClass="e-flat">Flat Button</ejs-button>
<ejs-button id="flatButtonPrimary" cssClass="e-flat e-primary">Flat Primary</ejs-button>
```

---

## Outline Button

An outline button has a visible border with a transparent background.

```cshtml
<ejs-button id="outlineButton" cssClass="e-outline">Outline Button</ejs-button>
<ejs-button id="outlineButtonPrimary" cssClass="e-outline e-primary">Outline Primary</ejs-button>
```

---

## Round Button

A round (circular) button typically contains only an icon. Set `cssClass="e-round"` and supply an icon via `iconCss`.

```cshtml
<ejs-button id="roundButton" cssClass="e-round e-primary" iconCss="e-icons e-plus-icon"></ejs-button>
<ejs-button id="roundButtonSmall" cssClass="e-round e-small" iconCss="e-icons e-edit-icon"></ejs-button>
```

> Round buttons are often used as floating action buttons or compact icon buttons.

---

## Toggle Button

A toggle button switches between two states (normal ↔ active). Set `isToggle="true"`. When active, the `e-active` CSS class is applied.

**View (`~/Pages/Index.cshtml`):**
```cshtml
<ejs-button id="toggleButton" isToggle="true" cssClass="e-flat" iconCss="e-icons e-play-icon">Play</ejs-button>

<script>
    let isPlaying = false;

    document.getElementById('toggleButton').addEventListener('click', function() {
        isPlaying = !isPlaying;
        const btn = ej2_instances['toggleButton'][0];
        
        if (isPlaying) {
            btn.iconCss = 'e-icons e-pause-icon';
            btn.content = 'Pause';
        } else {
            btn.iconCss = 'e-icons e-play-icon';
            btn.content = 'Play';
        }
    });
</script>
```

---

## Basic HTML Button Types

Use the native HTML `type` attribute to control form submission behavior:

| Type | Description |
|---|---|
| `submit` | Submits the enclosing form (default) |
| `reset` | Resets form fields to default values |
| `button` | No default action; used for custom JavaScript |

```cshtml
<form method="post">
    <ejs-button id="submitBtn" type="submit">Submit</ejs-button>
    <ejs-button id="resetBtn" type="reset">Reset</ejs-button>
    <ejs-button id="customBtn" type="button" onclick="customAction()">Custom Action</ejs-button>
</form>

<script>
    function customAction() {
        alert('Custom button clicked!');
    }
</script>
```

---

## Icons

### Font Icons

Use Syncfusion's built-in icon library via the `iconCss` attribute. Icon classes follow the format `e-icons e-[icon-name]-icon`:

```cshtml
<ejs-button id="saveBtn" iconCss="e-icons e-save-icon">Save</ejs-button>
<ejs-button id="deleteBtn" iconCss="e-icons e-delete-icon">Delete</ejs-button>
<ejs-button id="editBtn" iconCss="e-icons e-edit-icon">Edit</ejs-button>
<ejs-button id="closeBtn" iconCss="e-icons e-close-icon">Close</ejs-button>
```

**Common icon names:**
- `e-plus-icon` — Plus/Add
- `e-minus-icon` — Minus/Remove
- `e-search-icon` — Search
- `e-download-icon` — Download
- `e-upload-icon` — Upload
- `e-refresh-icon` — Refresh
- `e-play-icon` — Play
- `e-pause-icon` — Pause

Browse all icons: https://ej2.syncfusion.com/aspnetcore/documentation/appearance/icons

---

### SVG Icons

Use custom SVG or external icon libraries (Font Awesome, Bootstrap Icons, etc.):

```cshtml
<!-- Bootstrap Icons -->
<ejs-button id="svgBtn" iconCss="bi bi-heart">Like</ejs-button>

<!-- Font Awesome -->
<ejs-button id="faBtn" iconCss="fa-solid fa-star">Star</ejs-button>
```

Include the necessary CSS library in your `_Layout.cshtml`:

```cshtml
<!-- Bootstrap Icons -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@latest/font/bootstrap-icons.css" />

<!-- Font Awesome -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" />
```

---

### Icon Position

By default, icons appear **before** the button text. Control positioning with the `iconPosition` attribute:

```cshtml
<!-- Icon on the left (default) -->
<ejs-button id="btnLeft" iconCss="e-icons e-save-icon">Save</ejs-button>

<!-- Icon on the right -->
<ejs-button id="btnRight" iconCss="e-icons e-arrow-right-icon" iconPosition="Right">Next</ejs-button>
```

---

## Button Sizes

### Default Size

Buttons render with default height and padding:

```cshtml
<ejs-button id="defaultBtn">Default Button</ejs-button>
```

### Small Size

Use `cssClass="e-small"` for compact buttons (useful in toolbars, dense UIs):

```cshtml
<ejs-button id="smallBtn" cssClass="e-small">Small Button</ejs-button>
<ejs-button id="smallBtnIcon" cssClass="e-small" iconCss="e-icons e-plus-icon"></ejs-button>
```

### Block (Full-Width) Button

Use CSS to make a button fill its container width:

```cshtml
<div style="max-width: 300px;">
    <ejs-button id="blockBtn" style="width: 100%;">Full Width Button</ejs-button>
</div>
```

Or use a CSS class:

```css
/* In your stylesheet */
.e-btn-block {
    width: 100%;
}
```

```cshtml
<ejs-button id="blockBtn" cssClass="e-btn-block">Full Width Button</ejs-button>
```

---

## Complete Example

```cshtml
<div class="button-demo">
    <!-- Color variants -->
    <section>
        <h2>Color Variants</h2>
        <ejs-button id="btn1" cssClass="e-primary">Primary</ejs-button>
        <ejs-button id="btn2" cssClass="e-success">Success</ejs-button>
        <ejs-button id="btn3" cssClass="e-danger">Danger</ejs-button>
    </section>

    <!-- Button types -->
    <section>
        <h2>Button Types</h2>
        <ejs-button id="btn4" cssClass="e-flat">Flat</ejs-button>
        <ejs-button id="btn5" cssClass="e-outline">Outline</ejs-button>
        <ejs-button id="btn6" cssClass="e-round" iconCss="e-icons e-plus-icon"></ejs-button>
    </section>

    <!-- With icons -->
    <section>
        <h2>With Icons</h2>
        <ejs-button id="btn7" iconCss="e-icons e-save-icon">Save</ejs-button>
        <ejs-button id="btn8" iconCss="e-icons e-delete-icon" cssClass="e-danger">Delete</ejs-button>
    </section>

    <!-- Sizes -->
    <section>
        <h2>Sizes</h2>
        <ejs-button id="btn9">Normal</ejs-button>
        <ejs-button id="btn10" cssClass="e-small">Small</ejs-button>
    </section>
</div>

<style>
    .button-demo section {
        margin: 20px 0;
    }
    .button-demo button {
        margin: 5px;
    }
</style>
```

---

## See Also

- [Button Getting Started](button-getting-started.md)
- [Button How-To Patterns](button-how-to.md)
- [Button API Reference](button-api.md)
- [Syncfusion Icon Gallery](https://ej2.syncfusion.com/aspnetcore/documentation/appearance/icons)
