# API Reference — Syncfusion ASP.NET Core Tooltip

**Source:** [Syncfusion.EJ2.Popups.Tooltip — Official API Docs](https://help.syncfusion.com/cr/aspnetcore-js2/syncfusion.ej2.popups.tooltip.html)  
**Namespace:** `Syncfusion.EJ2.Popups`  
**Assembly:** `Syncfusion.EJ2.dll`  
**Tag Helper:** `<ejs-tooltip>`

> ⚠️ Only use APIs listed in this file. Do not reference undocumented or non-existent properties, methods, or events.

## Table of Contents
- [Properties](#properties)
- [Methods](#methods)
- [Events](#events)
- [Position Values](#position-values)
- [TipPointerPosition Values](#tippointerposition-values)

---

## Properties

| Property | Tag Helper Attr | Type | Default | Description |
|----------|-----------------|------|---------|-------------|
| `Content` | `content` | `string` | — | Tooltip display content (string or HTML) |
| `Position` | `position` | `Position` | `'TopCenter'` | Placement relative to target element (12 values) |
| `Target` | `target` | `string` | — | CSS selector for multi-target tooltips within the container |
| `OpenDelay` | `openDelay` | `number` | `0` | Delay (ms) before opening the tooltip |
| `CloseDelay` | `closeDelay` | `number` | `0` | Delay (ms) before closing the tooltip |
| `OpensOn` | `opensOn` | `string` | `'Auto'` | Trigger mode: `Auto` \| `Hover` \| `Click` \| `Focus` \| `Custom` (combinable with space) |
| `IsSticky` | `isSticky` | `bool` | `false` | Keep tooltip visible until user clicks the close button |
| `MouseTrail` | `mouseTrail` | `bool` | `false` | Tooltip follows mouse pointer movement |
| `ShowTipPointer` | `showTipPointer` | `bool` | `true` | Show or hide the tip arrow pointer |
| `OffsetX` | `offsetX` | `number` | `0` | Horizontal distance (px) between target and tooltip |
| `OffsetY` | `offsetY` | `number` | `0` | Vertical distance (px) between target and tooltip |
| `Width` | `width` | `string \| number` | `'auto'` | Tooltip width; `'auto'` fits content within viewport |
| `Height` | `height` | `string \| number` | `'auto'` | Tooltip height; overflow enables scroll mode |
| `CssClass` | `cssClass` | `string` | `null` | Custom CSS class(es) applied to the tooltip element |
| `EnableHtmlSanitizer` | `enableHtmlSanitizer` | `bool` | `true` | Sanitize untrusted HTML/scripts before rendering |
| `EnableRtl` | `enableRtl` | `bool` | `false` | Render component in right-to-left direction |
| `TipPointerPosition` | `tipPointerPosition` | `TipPointerPosition` | `'Auto'` | Tip arrow position: `Auto` \| `Start` \| `Middle` \| `End` |
| `WindowCollision` | `windowCollision` | `bool` | `false` | Use viewport (window) as collision boundary when `target` is set |

---

## Methods (JavaScript)

After initializing the tooltip, access via JavaScript instance:

```javascript
var tooltipObj = document.getElementById('tooltip').ej2_instances[0];
```

### `open(element?, animation?)`

Shows the tooltip on a specified target element with optional animation settings.

```javascript
tooltipObj.open(targetElement);
```

---

### `close(animation?)`

Hides the tooltip with optional animation settings.

```javascript
tooltipObj.close();
```

---

### `refresh(target?)`

Recalculates and updates the tooltip's content and/or position.

```javascript
tooltipObj.refresh();
```

> Use after programmatically changing `position` or when the target element moves.

---

### `destroy()`

Destroys the tooltip component instance and cleans up event listeners.

```javascript
tooltipObj.destroy();
```

---

### `dataBind()`

Applies all pending property changes to the component immediately.

```javascript
tooltipObj.content = 'New content';
tooltipObj.dataBind();
```

---

## Events

| Event | Type | Description |
|-------|------|-------------|
| `BeforeRender` | `TooltipEventArgs` | Triggered before the tooltip is rendered; allows dynamic content loading |
| `AfterOpen` | `TooltipEventArgs` | Triggered after the tooltip opens |
| `AfterClose` | `TooltipEventArgs` | Triggered after the tooltip closes |
| `Created` | `object` | Triggered after the tooltip component is created |
| `Destroyed` | `object` | Triggered when the tooltip component is destroyed |

---

## Position Values (Position Enum)

| Position | Description |
|----------|-------------|
| `TopLeft` | Tooltip appears top-left of the target |
| `TopCenter` | Tooltip appears top-center of the target (default) |
| `TopRight` | Tooltip appears top-right of the target |
| `BottomLeft` | Tooltip appears bottom-left of the target |
| `BottomCenter` | Tooltip appears bottom-center of the target |
| `BottomRight` | Tooltip appears bottom-right of the target |
| `LeftTop` | Tooltip appears left-top of the target |
| `LeftCenter` | Tooltip appears left-center of the target |
| `LeftBottom` | Tooltip appears left-bottom of the target |
| `RightTop` | Tooltip appears right-top of the target |
| `RightCenter` | Tooltip appears right-center of the target |
| `RightBottom` | Tooltip appears right-bottom of the target |

---

## TipPointerPosition Values

| Value | Description |
|-------|-------------|
| `Auto` | Automatically positions the tip pointer based on tooltip placement (default) |
| `Start` | Positions the tip pointer at the start edge |
| `Middle` | Positions the tip pointer at the middle |
| `End` | Positions the tip pointer at the end edge |
