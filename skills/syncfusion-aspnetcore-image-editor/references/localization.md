# Localization and Multi-Language Support

## Table of Contents
- [Locale Property](#locale-property)
- [Setting Culture](#setting-culture)
- [Supported Languages](#supported-languages)
- [Custom Translations](#custom-translations)
- [Translation Reference](#translation-reference)

## Locale Property

Control the language of Image Editor UI using the `Locale` property:

```html
<!-- English (default) -->
<ejs-imageeditor id="imageEditor" locale="en"></ejs-imageeditor>

<!-- Spanish -->
<ejs-imageeditor id="imageEditor" locale="es"></ejs-imageeditor>

<!-- French -->
<ejs-imageeditor id="imageEditor" locale="fr"></ejs-imageeditor>

<!-- Arabic -->
<ejs-imageeditor id="imageEditor" locale="ar"></ejs-imageeditor>

<!-- German -->
<ejs-imageeditor id="imageEditor" locale="de"></ejs-imageeditor>
```

## Setting Culture

### From Razor Page

```html
@{
    var culture = System.Globalization.CultureInfo.CurrentCulture.TwoLetterISOLanguageName;
}

<ejs-imageeditor id="imageEditor" locale="@culture"></ejs-imageeditor>
```

### From User Settings

```csharp
// In Controller
public IActionResult Index()
{
    var userCulture = HttpContext.Session.GetString("UserLanguage") ?? "en";
    ViewBag.Language = userCulture;
    return View();
}
```

```html
<!-- In View -->
<ejs-imageeditor id="imageEditor" locale="@ViewBag.Language"></ejs-imageeditor>
```

### Dynamically Change Language

```javascript
function changeLanguage(locale) {
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.locale = locale;
}

// Usage:
// changeLanguage('es'); // Switch to Spanish
// changeLanguage('fr'); // Switch to French
```

## Supported Languages

Image Editor supports these locales:

| Locale Code | Language | Region |
|-------------|----------|--------|
| `en` | English | United States |
| `es` | Spanish | Spain |
| `fr` | French | France |
| `de` | German | Germany |
| `it` | Italian | Italy |
| `pt` | Portuguese | Portugal |
| `ru` | Russian | Russia |
| `ar` | Arabic | Saudi Arabia |
| `ja` | Japanese | Japan |
| `ko` | Korean | Korea |
| `zh` | Chinese | China |
| `zh-tw` | Chinese Traditional | Taiwan |
| `tr` | Turkish | Turkey |
| `sv` | Swedish | Sweden |
| `pl` | Polish | Poland |
| `nl` | Dutch | Netherlands |
| `nb` | Norwegian Bokmål | Norway |
| `hu` | Hungarian | Hungary |
| `fa` | Persian/Farsi | Iran |

## Custom Translations

### Define Custom Locale

```html
<script>
// Define Spanish custom locale
var customES = {
    imageeditor: {
        'Crop': 'Recortar',
        'ZoomIn': 'Zoom Entrada',
        'ZoomOut': 'Zoom Salida',
        'Undo': 'Deshacer',
        'Redo': 'Rehacer',
        'Open': 'Abrir',
        'Save': 'Guardar',
        'Rotate': 'Girar',
        'Flip': 'Voltear',
        'Straighten': 'Enderezar'
    }
};

// Register custom locale
ej.base.setCulture('es-custom', customES);
</script>

<ejs-imageeditor id="imageEditor" locale="es-custom"></ejs-imageeditor>
```

### Add Missing Strings

```javascript
function setupCustomTranslations() {
    // Extend existing locale with custom strings
    var customLocale = {
        imageeditor: {
            'MyCustomTool': 'Herramienta Personal',
            'UploadImage': 'Cargar Imagen',
            'ApplyChanges': 'Aplicar Cambios'
        }
    };
    
    ej.base.setCulture('es-extended', customLocale);
}
```

## Translation Reference

Complete list of localizable Image Editor UI strings:

### Toolbar Items

```
'Crop': Crop tool
'ZoomIn': Increase zoom level
'ZoomOut': Decrease zoom level
'Open': Open image
'Save': Save image
'Undo': Undo last action
'Redo': Redo last action
'Rotate': Rotate image
'FlipHorizontal': Flip left-right
'FlipVertical': Flip top-bottom
'Straighten': Straighten image
'Annotate': Add annotations
'Finetune': Adjust brightness/contrast
'Filter': Apply filters
'Frame': Add frame
'Resize': Resize image
'Redact': Blur/pixelate areas
'Reset': Reset to original
'Transform': Rotation/flip
'SelectRectangle': Select rectangular area
'SelectCircle': Select circular area
```

### Selection Aspect Ratios

```
'2:3': Portrait (2:3)
'3:2': Landscape (3:2)
'3:4': Portrait (3:4)
'4:3': Landscape (4:3)
'4:5': Portrait (4:5)
'5:4': Landscape (5:4)
'5:7': Portrait (5:7)
'7:5': Landscape (7:5)
'9:16': Portrait (9:16)
'16:9': Landscape (16:9)
'Free': Free selection
```

### Transform Operations

```
'RotateLeft': Rotate 90° left
'RotateRight': Rotate 90° right
'FlipHorizontal': Mirror horizontally
'FlipVertical': Mirror vertically
```

### Annotation Tools

```
'Annotate': Add annotations
'Text': Add text
'Rectangle': Draw rectangle
'Ellipse': Draw circle
'Arrow': Draw arrow
'Line': Draw line
'Path': Draw freehand
'Pen': Freehand drawing
```

### Fine-Tune Options

```
'Brightness': Adjust brightness
'Contrast': Adjust contrast
'Saturation': Adjust saturation
'Hue': Adjust hue
'Exposure': Adjust exposure
'Blur': Apply blur
'Opacity': Adjust opacity
```

### Filters

```
'Filter': Apply filter
'Cold': Cold tone
'Warm': Warm tone
'Chrome': Chrome effect
'Sepia': Sepia tone
'Invert': Invert colors
'Grayscale': Black & white
'BlackAndWhite': High contrast B&W
'Default': Remove filter
```

### Frames

```
'Frame': Add frame
'Mat': Mat frame
'Bevel': Bevel frame
'Line': Line frame
'Hook': Hook frame
'Inset': Inset frame
```

### Dialogs and Messages

```
'Ok': OK button
'Cancel': Cancel button
'Close': Close
'Apply': Apply changes
'Delete': Delete
'Edit': Edit
'Duplicate': Duplicate
'Quality': Image quality
'FileSize': File size
'Uploaded': Upload complete
'Uploading': Uploading...
'Loading': Loading...
'Error': Error occurred
'Success': Operation successful
'Warning': Warning message
'Information': Information
```

### Redaction Options

```
'Redact': Redact areas
'Blur': Blur redaction
'Pixelate': Pixelate redaction
```

### Z-Order Operations

```
'BringForward': Bring forward
'SendBackward': Send backward
'BringToFront': Bring to front
'SendToBack': Send to back
```

### Undo/Redo

```
'Undo': Undo
'Redo': Redo
'UndoAll': Undo all
'RedoAll': Redo all
```

### Additional UI Strings

```
'Color': Color
'StrokeWidth': Stroke width
'Font': Font
'FontFamily': Font family
'FontSize': Font size
'Bold': Bold
'Italic': Italic
'Underline': Underline
'Strikethrough': Strikethrough
'Quick Access': Quick access toolbar
'Tooltip': Tooltip text
'Property': Property
'Value': Value
'Settings': Settings
'Options': Options
```

## Complete Example

### Multi-Language Editor

```html
<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" href="~/lib/bootstrap.css">
    <link rel="stylesheet" href="~/lib/ej2/material.css">
</head>
<body>
    <!-- Language Selector -->
    <div class="mb-3">
        <label>Select Language:</label>
        <select id="languageSelect" onchange="changeLanguage()">
            <option value="en">English</option>
            <option value="es">Español</option>
            <option value="fr">Français</option>
            <option value="de">Deutsch</option>
            <option value="ja">日本語</option>
            <option value="ar">العربية</option>
        </select>
    </div>

    <!-- Image Editor -->
    <ejs-imageeditor id="imageEditor" 
        width="100%" height="500px"
        toolbar="['Open', 'ZoomIn', 'ZoomOut', 'Crop', 'Save']"
        locale="en">
    </ejs-imageeditor>

    <script>
        function changeLanguage() {
            var select = document.getElementById('languageSelect');
            var locale = select.value;
            
            var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
            imageEditor.locale = locale;
            
            // Store preference
            localStorage.setItem('preferredLanguage', locale);
        }

        // Load saved preference
        window.addEventListener('DOMContentLoaded', function() {
            var saved = localStorage.getItem('preferredLanguage');
            if (saved) {
                document.getElementById('languageSelect').value = saved;
                changeLanguage();
            }
        });
    </script>
</body>
</html>
```

### Server-Side Localization

```csharp
// In Controller
public class ImageEditorController : Controller
{
    public IActionResult Index()
    {
        var cultureInfo = CultureInfo.CurrentCulture;
        var locale = cultureInfo.TwoLetterISOLanguageName.ToLower();
        
        ViewBag.Locale = locale;
        return View();
    }
}
```

```html
<!-- In Razor View -->
@{
    var userLocale = ViewBag.Locale ?? "en";
}

<ejs-imageeditor id="imageEditor" locale="@userLocale"></ejs-imageeditor>
```

## Right-to-Left (RTL) Languages

For RTL languages like Arabic and Persian, enable RTL mode:

```html
<ejs-imageeditor id="imageEditor" 
    locale="ar"
    enableRtl="true">
</ejs-imageeditor>
```

RTL automatically adjusts:
- Toolbar button order
- Text direction
- Menu alignment
- Dialog positioning

## Best Practices

### 1. **Set Locale Early**

```javascript
// ✅ Set on initialization
<ejs-imageeditor id="imageEditor" locale="es"></ejs-imageeditor>

// ❌ Avoid setting after creation
// This may not refresh all UI elements
```

### 2. **Match Application Culture**

```csharp
public IActionResult Index()
{
    var cultureName = HttpContext.Session.GetString("Culture") ?? "en";
    
    CultureInfo.CurrentCulture = new CultureInfo(cultureName);
    CultureInfo.CurrentUICulture = new CultureInfo(cultureName);
    
    ViewBag.Locale = cultureName;
    return View();
}
```

### 3. **Persist User Selection**

```javascript
function setUserLanguage(locale) {
    // Save to database
    fetch('/api/user/language', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ language: locale })
    });
    
    // Update UI
    var imageEditor = document.getElementById('imageEditor').ej2_instances[0];
    imageEditor.locale = locale;
}
```

### 4. **Provide Language Switcher**

```html
<div class="language-switcher">
    <button onclick="changeLocale('en')">English</button>
    <button onclick="changeLocale('es')">Español</button>
    <button onclick="changeLocale('fr')">Français</button>
    <button onclick="changeLocale('ar')">العربية</button>
</div>

<script>
function changeLocale(locale) {
    document.getElementById('imageEditor').ej2_instances[0].locale = locale;
}
</script>
```

### 5. **Test All Supported Languages**

Before deploying, verify:
- ✓ All toolbar items display correctly
- ✓ Dialogs show proper translations
- ✓ Tooltips are readable
- ✓ RTL languages render properly
- ✓ Font supports character set

## Troubleshooting

**Issue:** Locale not changing
- **Solution:** Verify locale code matches supported list
- **Solution:** Ensure EJ2 localization library is loaded

**Issue:** Partial translations showing
- **Solution:** Custom locale missing strings - add to definition
- **Solution:** Check browser console for warnings

**Issue:** RTL not working
- **Solution:** Set `enableRtl="true"` in addition to locale
- **Solution:** Verify CSS supports RTL layout

**Issue:** Font not displaying characters
- **Solution:** Include Unicode-compatible font
- **Solution:** Add @font-face for language-specific fonts

```css
@font-face {
    font-family: 'Arial Unicode MS';
    src: url('arialuni.ttf');
}

.ej2-imageeditor {
    font-family: 'Arial Unicode MS', Arial, sans-serif;
}
```
