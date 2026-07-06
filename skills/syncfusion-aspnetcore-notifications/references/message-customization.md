# Message Customization and Templates

## Table of Contents
- [Content Alignment](#content-alignment)
- [Custom Appearance with css-class](#custom-appearance-with-css-class)
- [CSS-Only Message Rendering](#css-only-message-rendering)
- [Content Templates](#content-templates)
- [RTL Support](#rtl-support)
- [Persistence](#persistence)

---

## Content Alignment

By default, message content aligns to the left. Use built-in CSS classes via the `css-class` attribute to change alignment:

| CSS Class | Effect |
|-----------|--------|
| *(none)* | Left-aligned (default) |
| `e-content-center` | Center-aligned |
| `e-content-right` | Right-aligned |

```cshtml
@* ~/Pages/Index.cshtml *@
@* Left (default) *@
<ejs-message id="msg_left" content="Your license has been activated successfully" severity="Success"></ejs-message>

@* Centered *@
<ejs-message id="msg_center"
             content="The license will expire today"
             css-class="e-content-center"
             severity="Warning">
</ejs-message>

@* Right-aligned *@
<ejs-message id="msg_right"
             content="The license key is invalid"
             css-class="e-content-right"
             severity="Error">
</ejs-message>
```

---

## Custom Appearance with css-class

The `css-class` attribute appends one or more CSS classes to the message's root element. Use this to override default styles, set border-radius, change padding, or apply any custom design:

```cshtml
@* Rounded corners *@
<ejs-message id="msg_rounded" content="The license will expire today"
             css-class="rounded" severity="Warning">
</ejs-message>

@* Square (no border-radius) *@
<ejs-message id="msg_square" content="The license key is invalid"
             css-class="square" severity="Error">
</ejs-message>
```

```css
/* wwwroot/css/site.css */
.rounded {
  border-radius: 20px;
}

.square {
  border-radius: 0;
}
```

Multiple classes are space-separated:

```cshtml
<ejs-message id="msg_combo" css-class="e-content-center rounded"
             severity="Info" content="Centered + rounded">
</ejs-message>
```

---

## CSS-Only Message Rendering

The Message component can be rendered using pure HTML and CSS without any JavaScript initialization. This is ideal for static content, server-rendered HTML, or lightweight scenarios.

### Structure — Content Only

```html
<div class="e-message" role="alert">
  <div class="e-msg-content">Editing is restricted</div>
</div>
```

### Structure — Content with Severity Icon

```html
<div class="e-message" role="alert">
  <span class="e-msg-icon"></span>
  <div class="e-msg-content">Editing is restricted</div>
</div>
```

### Available Predefined CSS Classes

| Class | Description |
|-------|-------------|
| `e-message` | Root message wrapper (required) |
| `e-msg-icon` | Severity type icon |
| `e-msg-content` | Message content container |
| `e-msg-close-icon` | Close icon |
| `e-info` | Info severity styling |
| `e-success` | Success severity styling |
| `e-warning` | Warning severity styling |
| `e-error` | Error severity styling |
| `e-content-center` | Center-align message content |
| `e-content-right` | Right-align message content |

### Full CSS-Only Example

```html
<div class="e-message" role="alert">
  <span class="e-msg-icon"></span>
  <div class="e-msg-content">Editing is restricted</div>
</div>
<div class="e-message e-info" role="alert">
  <span class="e-msg-icon"></span>
  <div class="e-msg-content">Please read the comments carefully</div>
</div>
<div class="e-message e-success" role="alert">
  <span class="e-msg-icon"></span>
  <div class="e-msg-content">Your message has been sent successfully</div>
</div>
<div class="e-message e-warning" role="alert">
  <span class="e-msg-icon"></span>
  <div class="e-msg-content">There was a problem with your network connection</div>
</div>
<div class="e-message e-error" role="alert">
  <span class="e-msg-icon"></span>
  <div class="e-msg-content">A problem occurred while submitting your data</div>
</div>
```

---

## Content Templates

The `<e-content-template>` tag helper enables rich, interactive message bodies. Wrap any HTML markup inside the message tag to embed buttons, links, or other Syncfusion components:

### Static Inner Content

```cshtml
<ejs-message id="msg_static" severity="Info">
    <strong>Note:</strong> This action cannot be undone.
</ejs-message>
```

### Rich Content Template with Buttons

```cshtml
@* ~/Pages/Index.cshtml *@
@{
    var messageVisible = true;
}
<ejs-message id="msg_rich"
             severity="Success"
             visible="@messageVisible"
             closed="onRichClosed">
    <e-content-template>
        <div>
            <h4>Merged pull request</h4>
            <p>Pull request #41 merged after a successful build</p>
            <ejs-button id="viewCommitBtn" content="View commit" css-class="e-link"></ejs-button>
            <ejs-button id="dismissBtn" content="Dismiss" css-class="e-link" click="onRichClosed"></ejs-button>
        </div>
    </e-content-template>
</ejs-message>

<script>
    function onRichClosed() {
        document.getElementById('msg_rich').ej2_instances[0].visible = false;
    }
</script>
```

---

## RTL Support

Enable right-to-left text direction for RTL languages (Arabic, Hebrew, etc.) using `enable-rtl="true"`:

```cshtml
<ejs-message id="msg_rtl" content="مرحبا بالعالم" severity="Info" enable-rtl="true"></ejs-message>
```

---

## Persistence

The `enable-persistence="true"` attribute saves the component's state (including `visible`) across page reloads using browser storage:

```cshtml
<ejs-message id="msg_persist" content="Persistent message" enable-persistence="true"></ejs-message>
```

Use this when you want dismissed messages to stay dismissed after a page reload.
