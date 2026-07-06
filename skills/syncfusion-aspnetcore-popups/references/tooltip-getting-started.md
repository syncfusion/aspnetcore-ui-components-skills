# Getting Started — Syncfusion ASP.NET Core Tooltip

## Table of Contents
- [Installation](#installation)
- [Setup](#setup)
- [Basic Tooltip](#basic-tooltip)
- [Tooltip on Multiple Targets](#tooltip-on-multiple-targets)
- [Using title Attribute as Content](#using-title-attribute-as-content)
- [Running the Application](#running-the-application)

---

## Installation

### Step 1: Install NuGet Package

Open the NuGet Package Manager Console and run:

```bash
Install-Package Syncfusion.EJ2.AspNet.Core -Version {{ site.ej2version }}
```

Or use the .NET CLI:

```bash
dotnet add package Syncfusion.EJ2.AspNet.Core --version {{ site.ej2version }}
```

### Step 2: Add Tag Helper Reference

Open `~/Pages/_ViewImports.cshtml` and add:

```csharp
@addTagHelper *, Syncfusion.EJ2
```

---

## Setup

### Reference Styles and Scripts

In your `~/Pages/Shared/_Layout.cshtml` file, add the Syncfusion CSS and JavaScript in the `<head>` and `<body>` tags:

```html
<head>
    ...
    <!-- Syncfusion CSS -->
    <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/fluent.css" />
</head>
<body>
    ...
    <!-- Syncfusion JavaScript -->
    <script src="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/dist/ej2.min.js"></script>
    <!-- Script Manager -->
    <ejs-scripts></ejs-scripts>
</body>
```

> Available themes: `fluent.css`, `bootstrap5.css`, `material3.css`, `tailwind3.css`. Replace `fluent.css` with your preferred theme.

---

## Basic Tooltip

The simplest usage wraps any element with `<ejs-tooltip>` Tag Helper and provides a `content` attribute:

```csharp
<ejs-tooltip content="Tooltip Content">
    <button class="e-btn" style="margin: 60px;">Show Tooltip</button>
</ejs-tooltip>
```

To place the tooltip on a specific child element rather than the wrapper itself, use the `target` and `position` attributes:

```csharp
<div id="container">
    <ejs-tooltip position="TopCenter" content="Tooltip Content" target="#target">
        <button class="e-btn" id="target">Show Tooltip</button>
    </ejs-tooltip>
</div>
```

- `target` — CSS selector identifying which child element triggers the tooltip.
- `position` — one of 12 placement values (default `TopCenter`).

---

## Tooltip on Multiple Targets

A single `<ejs-tooltip>` can serve multiple targets within a container. Set `target` to a shared CSS selector and the tooltip reads each element's `title` attribute as its content.

```csharp
<div id="container">
    <ejs-tooltip target=".e-info" position="RightCenter">
        <form>
            <table>
                <tbody>
                    <tr>
                        <td>User Name</td>
                        <td>
                            <input
                                type="text"
                                class="e-info"
                                title="Please enter your name"
                            />
                        </td>
                    </tr>
                    <tr>
                        <td>Email Address</td>
                        <td>
                            <input
                                type="email"
                                class="e-info"
                                title="Enter a valid email address"
                            />
                        </td>
                    </tr>
                    <tr>
                        <td>Password</td>
                        <td>
                            <input
                                type="password"
                                class="e-info"
                                title="Be at least 8 characters length"
                            />
                        </td>
                    </tr>
                </tbody>
            </table>
        </form>
    </ejs-tooltip>
</div>
```

> Each matched `.e-info` element's `title` attribute becomes that element's tooltip content. The `title` attribute is used as fallback when no `content` attribute is supplied.

---

## Using title Attribute as Content

When no `content` attribute is provided, the tooltip reads each target element's `title` attribute:

```csharp
<div id="container">
    <ejs-tooltip target=".info-icon" position="TopCenter">
        <button class="e-btn info-icon" title="Information">Info 1</button>
        <button class="e-btn info-icon" title="Details">Info 2</button>
        <button class="e-btn info-icon" title="Help">Info 3</button>
    </ejs-tooltip>
</div>
```

---

## Next Steps

Explore the [API Reference](tooltip-api.md) for all available options, or learn about specific features:
- [Content](tooltip-content.md) — string, HTML, template, or dynamic content
- [Positioning](tooltip-positioning.md) — 12 static positions and dynamic placement
- [Open Modes](tooltip-open-mode.md) — Hover, Click, Focus, or Custom triggers
- [Animation](tooltip-animation.md) — smooth open/close transitions
- [Customization](tooltip-customization.md) — styling, themes, and appearance
- [Accessibility](tooltip-accessibility.md) — WCAG compliance and keyboard support
