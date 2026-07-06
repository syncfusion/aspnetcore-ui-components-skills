# Animation — Syncfusion ASP.NET Core Tooltip

## Table of Contents
- [Overview](#overview)
- [Basic Animation](#basic-animation)
- [Animation Effects](#animation-effects)
- [Duration and Delay](#duration-and-delay)
- [Disable Animation](#disable-animation)
- [Programmatic Animation Control](#programmatic-animation-control)

---

## Overview

The `<e-tooltip-animationsettings>` child element configures open and close animations for the tooltip. Each animation can have:
- `effect` — animation effect name (e.g., `'FadeIn'`, `'ZoomIn'`, `'SlideUp'`, etc.)
- `duration` — duration in milliseconds
- `delay` — delay before animation starts in milliseconds

---

## Basic Animation

Configure animation settings with opening and closing effects:

```csharp
<ejs-tooltip content="Animated Tooltip" position="TopCenter">
    <e-tooltip-animationsettings>
        <e-tooltip-opensettings effect="FadeIn" duration="300"></e-tooltip-opensettings>
        <e-tooltip-closesettings effect="FadeOut" duration="300"></e-tooltip-closesettings>
    </e-tooltip-animationsettings>
    <button class="e-btn">Show Tooltip</button>
</ejs-tooltip>
```

---

## Animation Effects

Available animation effects:

| Effect | Description |
|--------|-------------|
| `FadeIn` / `FadeOut` | Fade in/out smoothly |
| `ZoomIn` / `ZoomOut` | Zoom in/out from center |
| `FlipX` / `FlipY` | Flip horizontally/vertically |
| `SlideUp` / `SlideDown` | Slide up/down |
| `SlideLeft` / `SlideRight` | Slide left/right |
| `ScaleUp` / `ScaleDown` | Scale up/down |
| `RotateIn` / `RotateOut` | Rotate in/out |

```csharp
@* Zoom effect *@
<ejs-tooltip content="Zoom Animation" position="TopCenter">
    <e-tooltip-animationsettings>
        <e-tooltip-opensettings effect="ZoomIn" duration="400"></e-tooltip-opensettings>
        <e-tooltip-closesettings effect="ZoomOut" duration="400"></e-tooltip-closesettings>
    </e-tooltip-animationsettings>
    <button class="e-btn">Zoom</button>
</ejs-tooltip>

@* Slide effect *@
<ejs-tooltip content="Slide Animation" position="TopCenter">
    <e-tooltip-animationsettings>
        <e-tooltip-opensettings effect="SlideUp" duration="300"></e-tooltip-opensettings>
        <e-tooltip-closesettings effect="SlideDown" duration="300"></e-tooltip-closesettings>
    </e-tooltip-animationsettings>
    <button class="e-btn">Slide</button>
</ejs-tooltip>

@* Flip effect *@
<ejs-tooltip content="Flip Animation" position="TopCenter">
    <e-tooltip-animationsettings>
        <e-tooltip-opensettings effect="FlipX" duration="500"></e-tooltip-opensettings>
        <e-tooltip-closesettings effect="FlipX" duration="500"></e-tooltip-closesettings>
    </e-tooltip-animationsettings>
    <button class="e-btn">Flip</button>
</ejs-tooltip>
```

---

## Duration and Delay

Control how fast the animation plays and when it starts:

```csharp
@* Quick animation (200ms) *@
<ejs-tooltip content="Fast Animation" position="TopCenter">
    <e-tooltip-animationsettings>
        <e-tooltip-opensettings effect="FadeIn" duration="200"></e-tooltip-opensettings>
        <e-tooltip-closesettings effect="FadeOut" duration="200"></e-tooltip-closesettings>
    </e-tooltip-animationsettings>
    <button class="e-btn">Fast</button>
</ejs-tooltip>

@* Slow animation (600ms) with delay *@
<ejs-tooltip content="Slow Animation with Delay" position="TopCenter">
    <e-tooltip-animationsettings>
        <e-tooltip-opensettings effect="FadeIn" duration="600" delay="200"></e-tooltip-opensettings>
        <e-tooltip-closesettings effect="FadeOut" duration="600"></e-tooltip-closesettings>
    </e-tooltip-animationsettings>
    <button class="e-btn">Slow</button>
</ejs-tooltip>
```

---

## Disable Animation

Set `effect` to `'None'` to disable animation:

```csharp
<ejs-tooltip content="No Animation" position="TopCenter">
    <e-tooltip-animationsettings>
        <e-tooltip-opensettings effect="None"></e-tooltip-opensettings>
        <e-tooltip-closesettings effect="None"></e-tooltip-closesettings>
    </e-tooltip-animationsettings>
    <button class="e-btn">Instant</button>
</ejs-tooltip>
```

---

## Programmatic Animation Control

Override animation settings when calling `.open()` or `.close()` methods:

```csharp
<ejs-tooltip id="tooltip" content="Programmatic Animation">
    <button class="e-btn" onclick="openWithAnimation()">Open with Zoom</button>
    <button class="e-btn" onclick="closeWithAnimation()">Close with Slide</button>
</ejs-tooltip>

<script>
    function openWithAnimation() {
        var tooltipObj = document.getElementById('tooltip').ej2_instances[0];
        var btn = document.querySelector('.e-btn');
        tooltipObj.open(btn, { effect: 'ZoomIn', duration: 500 });
    }
    
    function closeWithAnimation() {
        var tooltipObj = document.getElementById('tooltip').ej2_instances[0];
        tooltipObj.close({ effect: 'SlideDown', duration: 400 });
    }
</script>
```
