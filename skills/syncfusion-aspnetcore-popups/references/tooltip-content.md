# Content — Syncfusion ASP.NET Core Tooltip

## Table of Contents
- [String Content](#string-content)
- [Template Content](#template-content)
- [Dynamic Content via beforeRender](#dynamic-content-via-beforerender)
- [HTML Content](#html-content)
- [Updating Content Programmatically](#updating-content-programmatically)
- [HTML Sanitizer](#html-sanitizer)

---

## String Content

The simplest form — pass any string (including basic HTML tags) to `content`:

```csharp
<ejs-tooltip content="Simple tooltip text">
    <button class="e-btn">Hover Me</button>
</ejs-tooltip>
```

To include HTML markup (bold, italic, etc.):

```csharp
<ejs-tooltip content="<b>Bold</b> and <i>italic</i> text">
    <button class="e-btn">Hover Me</button>
</ejs-tooltip>
```

---

## Template Content

Use the `<e-content-template>` child element to define rich HTML content:

```csharp
<ejs-tooltip isSticky="true" position="RightCenter">
    <a href="#" style="display: inline-block; padding: 5px;">
        <u>environmentally friendly</u>
    </a>
    <e-content-template>
        <span>
            <p>
                <strong>Environmentally friendly</strong> or 
                <strong>environment-friendly</strong>,
                <i>(also referred to as eco-friendly, nature-friendly, and green)</i> 
                are marketing and sustainability terms referring to goods and services 
                that inflict reduced, minimal, or no harm upon ecosystems.
            </p>
        </span>
    </e-content-template>
</ejs-tooltip>
```

> `isSticky="true"` keeps the tooltip open until the user clicks the close icon — useful for rich template content.

---

## Dynamic Content via beforeRender

Load tooltip content asynchronously in the `beforeRender` event. The pattern:
1. Set a "Loading…" placeholder immediately.
2. Fetch real data, **validate and sanitize each value**, then assign content.

```csharp
@* View *@
<ejs-tooltip id="tooltip" 
    target=".target" 
    position="RightCenter" 
    beforeRender="onBeforeRender">
    <ul>
        <li class="target" data-content="1"><span>Australia</span></li>
        <li class="target" data-content="2"><span>Bhutan</span></li>
        <li class="target" data-content="3"><span>China</span></li>
        <li class="target" data-content="4"><span>India</span></li>
    </ul>
</ejs-tooltip>

<script>
    function onBeforeRender(args) {
        var tooltipObj = document.getElementById('tooltip').ej2_instances[0];
        tooltipObj.content = 'Loading...';
        tooltipObj.dataBind();

        // Fetch data from your ASP.NET Core endpoint
        fetch('/api/tooltip-data')
            .then(response => response.json())
            .then(result => {
                for (let i = 0; i < result.length; i++) {
                    if (result[i].id === args.target.getAttribute('data-content')) {
                        // Sanitize the value before embedding in HTML to prevent XSS
                        var safeText = result[i].description
                            .replace(/&/g, '&amp;')
                            .replace(/</g, '&lt;')
                            .replace(/>/g, '&gt;');
                        tooltipObj.content = '<div class="contentWrap">' + safeText + '</div>';
                    }
                }
                tooltipObj.dataBind();
            })
            .catch(error => {
                // Never expose raw error messages as HTML content
                tooltipObj.content = 'Failed to load content.';
                tooltipObj.dataBind();
            });
    }
</script>
```

---

## HTML Content

Pass HTML strings directly to `content` to render embedded content:

```csharp
<ejs-tooltip content="<h4>Help</h4><p>This is a tooltip with HTML content.</p>">
    <button class="e-btn">Help</button>
</ejs-tooltip>
```

> HTML content is sanitized by default (`enableHtmlSanitizer="true"`) to prevent XSS attacks.

---

## Updating Content Programmatically

Change tooltip content after initialization using JavaScript:

```csharp
<ejs-tooltip id="tooltip" content="Initial Content">
    <button class="e-btn" onclick="updateContent()">Update Tooltip</button>
</ejs-tooltip>

<script>
    function updateContent() {
        var tooltipObj = document.getElementById('tooltip').ej2_instances[0];
        tooltipObj.content = 'Updated Content';
        tooltipObj.dataBind(); // Apply changes immediately
    }
</script>
```

---

## HTML Sanitizer

By default, `enableHtmlSanitizer="true"` sanitizes untrusted HTML and scripts before rendering. To allow raw HTML rendering (only from trusted sources):

```csharp
<ejs-tooltip content="<b>Bold</b> and <i>italic</i>" enableHtmlSanitizer="false">
    <button class="e-btn">Show Tooltip</button>
</ejs-tooltip>
```

> **⚠️ Security Warning:** Only set `enableHtmlSanitizer="false"` for content you fully control and trust. Never set this for user-supplied or third-party content, as it exposes your application to XSS attacks.
