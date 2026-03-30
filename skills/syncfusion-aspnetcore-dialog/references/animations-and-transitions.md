# Animations & Transitions — Dialog (ASP.NET Core)

## Table of Contents
- [Animation Basics](#animation-basics)
- [Built-in Effects](#built-in-effects)
- [Animation Duration](#animation-duration)
- [Custom Animation](#custom-animation)
- [Disabling Animations](#disabling-animations)
- [Performance Considerations](#performance-considerations)

## Animation Basics

The Dialog supports smooth animations when opening and closing. Animations enhance user experience by providing visual feedback.

### Animation Configuration

Animations are configured via the `animation` property, which accepts:
- **effect:** Animation type (Zoom, FadeIn, FadeOut, SlideIn, SlideOut, etc.)
- **duration:** Time in milliseconds
- **delay:** Delay before animation starts

```csharp
<ejs-dialog id="animatedDialog" 
    header="Animated Dialog" 
    width="500px">
    <e-animation-settings effect="Zoom" duration="500"></e-animation-settings>
    <e-content-template>
        <p>This dialog animates with a Zoom effect.</p>
    </e-content-template>
</ejs-dialog>
```

## Built-in Effects

### Zoom Effect

Smoothly scales the dialog in/out:

```csharp
<ejs-dialog id="zoomDialog" 
    header="Zoom Animation" 
    width="500px">
    <e-animation-settings effect="Zoom" duration="400"></e-animation-settings>
    <e-content-template>
        <p>Dialog zooms in when opening and zooms out when closing.</p>
    </e-content-template>
</ejs-dialog>

<button class="e-btn" onclick="showZoomDialog()">Show Zoom</button>

<script>
    function showZoomDialog() {
        document.getElementById('zoomDialog').ej2_instances[0].show();
    }
</script>
```

**Use case:** Creates a professional, modern feel. Good for confirmations and alerts.

### FadeIn / FadeOut

Gradually appears or disappears:

```csharp
<ejs-dialog id="fadeDialog" 
    header="Fade Animation" 
    width="500px">
    <e-animation-settings effect="FadeIn" duration="600"></e-animation-settings>
    <e-content-template>
        <p>Dialog fades in smoothly when opened.</p>
    </e-content-template>
</ejs-dialog>

<button class="e-btn" onclick="showFadeDialog()">Show Fade</button>

<script>
    function showFadeDialog() {
        document.getElementById('fadeDialog').ej2_instances[0].show();
    }
</script>
```

**Use case:** Subtle, elegant transition for informational dialogs.

### SlideIn / SlideOut

Dialog slides from edge into view:

```csharp
<ejs-dialog id="slideDialog" 
    header="Slide Animation" 
    width="500px" 
    position-x="right">
    <e-animation-settings effect="SlideDown" duration="500"></e-animation-settings>
    <e-content-template>
        <p>Dialog slides down when opening.</p>
    </e-content-template>
</ejs-dialog>

<button class="e-btn" onclick="showSlideDialog()">Show Slide</button>

<script>
    function showSlideDialog() {
        document.getElementById('slideDialog').ej2_instances[0].show();
    }
</script>
```

**Variants:** `SlideUp`, `SlideDown`, `SlideLeft`, `SlideRight`

### Bounce Effect

Dialog bounces in for a playful feel:

```csharp
<ejs-dialog id="bounceDialog" 
    header="Bounce Animation" 
    width="500px">
    <e-animation-settings effect="Bounce" duration="700"></e-animation-settings>
    <e-content-template>
        <p>Dialog bounces in with energy!</p>
    </e-content-template>
</ejs-dialog>
```

**Use case:** Attention-grabbing for important notifications or game-like interfaces.

### Flip Effect

Dialog flips into view:

```csharp
<ejs-dialog id="flipDialog" 
    header="Flip Animation" 
    width="500px">
    <e-animation-settings effect="Flip" duration="600"></e-animation-settings>
    <e-content-template>
        <p>Dialog flips in for a dynamic presentation.</p>
    </e-content-template>
</ejs-dialog>
```

**Use case:** Modern, technical interfaces.

## Animation Duration

### Fast Animation (200-300ms)

Quick, snappy transitions:

```csharp
<ejs-dialog id="fastDialog" 
    header="Fast Animation" 
    width="500px">
    <e-animation-settings effect="Zoom" duration="200"></e-animation-settings>
    <e-content-template>
        <p>This animation is quick and responsive.</p>
    </e-content-template>
</ejs-dialog>
```

**Good for:** Frequent interactions, quick feedback.

### Standard Animation (400-500ms)

Balanced duration:

```csharp
<ejs-dialog id="standardDialog" 
    header="Standard Animation" 
    width="500px">
    <e-animation-settings effect="Zoom" duration="400"></e-animation-settings>
    <e-content-template>
        <p>This is the recommended animation duration.</p>
    </e-content-template>
</ejs-dialog>
```

**Good for:** Most use cases, professional feel.

### Slow Animation (600-800ms)

Graceful, deliberate transitions:

```csharp
<ejs-dialog id="slowDialog" 
    header="Slow Animation" 
    width="500px">
    <e-animation-settings effect="FadeIn" duration="800"></e-animation-settings>
    <e-content-template>
        <p>Slow animations give time to perceive the change.</p>
    </e-content-template>
</ejs-dialog>
```

**Good for:** High-importance messages, accessibility.

## Custom Animation

### Using CSS Animations

Create custom animations with CSS:

```csharp
<style>
    @@keyframes slideInRotate {
        from {
            opacity: 0;
            transform: translateX(-100px) rotate(-5deg);
        }
        to {
            opacity: 1;
            transform: translateX(0) rotate(0deg);
        }
    }
    
    .custom-animate {
        animation: slideInRotate 0.5s ease-out;
    }
</style>

<ejs-dialog id="customDialog" 
    header="Custom Animation" 
    width="500px"
    css-class="custom-animate">
    <e-content-template>
        <p>This dialog uses a custom CSS animation.</p>
    </e-content-template>
</ejs-dialog>
```

### Event-Triggered Animation

Apply animations on specific events:

```csharp
<ejs-dialog id="eventDialog" 
    header="Event Animation" 
    width="500px"
    on-opening="onDialogOpening"
    on-opened="onDialogOpened">
    <e-content-template>
        <p>Animation occurs when the dialog opens.</p>
    </e-content-template>
</ejs-dialog>

<script>
    function onDialogOpening(args) {
        console.log('Dialog is about to open');
        // Add animation classes here
    }
    
    function onDialogOpened(args) {
        console.log('Dialog has finished opening');
    }
</script>
```

## Disabling Animations

### No Animation

Remove all animations for instant display:

```csharp
<ejs-dialog id="noAnimDialog" 
    header="No Animation" 
    width="500px">
    <e-animation-settings effect="None"></e-animation-settings>
    <e-content-template>
        <p>This dialog appears instantly with no animation.</p>
    </e-content-template>
</ejs-dialog>
```

**Use case:** Accessibility, performance optimization, or accessibility preferences.

### Animation Off via JavaScript

Disable animation at runtime:

```csharp
<ejs-dialog id="toggleAnimDialog" 
    header="Toggle Animation" 
    width="500px">
    <e-animation-settings effect="Zoom" duration="400"></e-animation-settings>
    <e-content-template>
        <p>Animation can be toggled on/off.</p>
    </e-content-template>
</ejs-dialog>

<button class="e-btn" onclick="toggleAnimation()">Toggle Animation</button>

<script>
    var animationEnabled = true;
    
    function toggleAnimation() {
        var dialog = document.getElementById('toggleAnimDialog').ej2_instances[0];
        
        if (animationEnabled) {
            dialog.animation = {effect: 'None'};
            animationEnabled = false;
        } else {
            dialog.animation = {effect: 'Zoom', duration: 400};
            animationEnabled = true;
        }
    }
</script>
```

## Performance Considerations

### Respecting prefers-reduced-motion

Respect user's accessibility preferences:

```csharp
<ejs-dialog id="a11yDialog" 
    header="Accessible Animation" 
    width="500px">
    <e-animation-settings effect="Zoom" duration="400" id="animSettings"></e-animation-settings>
    <e-content-template>
        <p>This dialog respects user motion preferences.</p>
    </e-content-template>
</ejs-dialog>

<script>
    // Check if user prefers reduced motion
    const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
    
    if (prefersReducedMotion) {
        var dialog = document.getElementById('a11yDialog').ej2_instances[0];
        dialog.animation = {effect: 'None'};
    }
</script>
```

### Animation on Low-Performance Devices

Disable animation on slower devices:

```javascript
// Detect device performance
function getDevicePerformance() {
    if (navigator.deviceMemory && navigator.deviceMemory < 4) {
        return 'low';
    }
    return 'normal';
}

var performance = getDevicePerformance();

var animEffect = performance === 'low' ? 'None' : 'Zoom';
// Use animEffect in dialog configuration
```

### Multiple Dialogs

If many dialogs with animations exist, reduce duration or disable for less important ones:

```csharp
<!-- Important dialog: full animation -->
<ejs-dialog id="importantDialog" header="Important">
    <e-animation-settings effect="Zoom" duration="500"></e-animation-settings>
    <e-content-template><p>Critical information</p></e-content-template>
</ejs-dialog>

<!-- Secondary dialog: no animation -->
<ejs-dialog id="secondaryDialog" header="Secondary">
    <e-animation-settings effect="None"></e-animation-settings>
    <e-content-template><p>Additional info</p></e-content-template>
</ejs-dialog>
```

## Examples Summary

| Effect | Duration | Use Case |
|--------|----------|----------|
| Zoom | 400ms | Confirmations, alerts |
| FadeIn | 500ms | Information, subtle |
| SlideDown | 500ms | Important messages |
| Bounce | 600ms | Attention-grabbing |
| Flip | 600ms | Modern, technical |
| None | 0ms | Accessibility, performance |

