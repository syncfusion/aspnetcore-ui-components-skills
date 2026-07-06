# Positioning — Syncfusion ASP.NET Core Tooltip

## Table of Contents
- [12 Static Positions](#12-static-positions)
- [Tip Pointer Positioning](#tip-pointer-positioning)
- [Dynamic Positioning](#dynamic-positioning)
- [Offset Values](#offset-values)
- [Collision Handling](#collision-handling)

---

## 12 Static Positions

The `position` attribute specifies where the tooltip appears relative to the target element. There are 12 preset position values:

```csharp
@* Top Row *@
<ejs-tooltip content="Top Left" position="TopLeft">
    <button class="e-btn">Top Left</button>
</ejs-tooltip>

<ejs-tooltip content="Top Center" position="TopCenter">
    <button class="e-btn">Top Center</button>
</ejs-tooltip>

<ejs-tooltip content="Top Right" position="TopRight">
    <button class="e-btn">Top Right</button>
</ejs-tooltip>

@* Bottom Row *@
<ejs-tooltip content="Bottom Left" position="BottomLeft">
    <button class="e-btn">Bottom Left</button>
</ejs-tooltip>

<ejs-tooltip content="Bottom Center" position="BottomCenter">
    <button class="e-btn">Bottom Center</button>
</ejs-tooltip>

<ejs-tooltip content="Bottom Right" position="BottomRight">
    <button class="e-btn">Bottom Right</button>
</ejs-tooltip>

@* Left Column *@
<ejs-tooltip content="Left Top" position="LeftTop">
    <button class="e-btn">Left Top</button>
</ejs-tooltip>

<ejs-tooltip content="Left Center" position="LeftCenter">
    <button class="e-btn">Left Center</button>
</ejs-tooltip>

<ejs-tooltip content="Left Bottom" position="LeftBottom">
    <button class="e-btn">Left Bottom</button>
</ejs-tooltip>

@* Right Column *@
<ejs-tooltip content="Right Top" position="RightTop">
    <button class="e-btn">Right Top</button>
</ejs-tooltip>

<ejs-tooltip content="Right Center" position="RightCenter">
    <button class="e-btn">Right Center</button>
</ejs-tooltip>

<ejs-tooltip content="Right Bottom" position="RightBottom">
    <button class="e-btn">Right Bottom</button>
</ejs-tooltip>
```

---

## Tip Pointer Positioning

The `tipPointerPosition` attribute controls where the arrow tip points on the tooltip edge:

| Value | Description |
|-------|-------------|
| `Auto` | Automatically positions the tip pointer based on tooltip placement (default) |
| `Start` | Positions the tip pointer at the start edge |
| `Middle` | Positions the tip pointer at the middle |
| `End` | Positions the tip pointer at the end edge |

```csharp
<ejs-tooltip content="Tip at Start" 
    position="RightCenter" 
    tipPointerPosition="Start">
    <button class="e-btn">Start Tip</button>
</ejs-tooltip>

<ejs-tooltip content="Tip at Middle" 
    position="RightCenter" 
    tipPointerPosition="Middle">
    <button class="e-btn">Middle Tip</button>
</ejs-tooltip>

<ejs-tooltip content="Tip at End" 
    position="RightCenter" 
    tipPointerPosition="End">
    <button class="e-btn">End Tip</button>
</ejs-tooltip>
```

---

## Dynamic Positioning

Use the `refresh()` method to recalculate tooltip position after the target element moves or the viewport changes:

```csharp
<ejs-tooltip id="tooltip" content="Dynamic Tooltip">
    <button id="targetBtn" class="e-btn" onclick="moveButton()">Move Me</button>
</ejs-tooltip>

<script>
    function moveButton() {
        var btn = document.getElementById('targetBtn');
        btn.style.marginLeft = '200px';
        
        // Refresh tooltip position after target moves
        var tooltipObj = document.getElementById('tooltip').ej2_instances[0];
        tooltipObj.refresh(btn);
    }
</script>
```

---

## Offset Values

Use `offsetX` and `offsetY` to adjust the distance between the target and the tooltip:

```csharp
@* No offset (default) *@
<ejs-tooltip content="No Offset" position="TopCenter">
    <button class="e-btn">Default</button>
</ejs-tooltip>

@* 10px horizontal offset *@
<ejs-tooltip content="10px Right Offset" 
    position="RightCenter" 
    offsetX="10">
    <button class="e-btn">Offset X</button>
</ejs-tooltip>

@* 20px vertical offset *@
<ejs-tooltip content="20px Down Offset" 
    position="TopCenter" 
    offsetY="20">
    <button class="e-btn">Offset Y</button>
</ejs-tooltip>

@* Both offsets *@
<ejs-tooltip content="Combined Offset" 
    position="RightCenter" 
    offsetX="10" 
    offsetY="10">
    <button class="e-btn">Both Offsets</button>
</ejs-tooltip>
```

---

## Collision Handling

The `windowCollision` attribute controls whether the tooltip adjusts position to stay within the viewport when it would otherwise extend beyond the window edges:

```csharp
@* Auto-flip behavior enabled *@
<ejs-tooltip content="Stays in viewport" 
    position="TopCenter" 
    windowCollision="true">
    <button class="e-btn">Near Edge</button>
</ejs-tooltip>

@* Allow tooltip to extend beyond viewport *@
<ejs-tooltip content="May go off-screen" 
    position="TopCenter" 
    windowCollision="false">
    <button class="e-btn">Allow Overflow</button>
</ejs-tooltip>
```
