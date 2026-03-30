# Accessibility and Localization in QueryBuilder for ASP.NET MVC

## Table of Contents
- [Accessibility Overview](#accessibility-overview)
- [WCAG 2.1 Compliance](#wcag-21-compliance)
- [ARIA Attributes](#aria-attributes)
- [Keyboard Navigation](#keyboard-navigation)
- [Screen Reader Support](#screen-reader-support)
- [Localization Setup](#localization-setup)
- [Translated Strings](#translated-strings)
- [Culture-Specific Formatting](#culture-specific-formatting)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Accessibility Overview

QueryBuilder supports accessibility standards to ensure usability for all users, including those with disabilities.

**Key Standards:**
- **WCAG 2.1** Level AA compliance (Web Content Accessibility Guidelines)
- **ARIA** roles and attributes for screen readers
- **Keyboard navigation** for non-mouse users
- **Color contrast** for low-vision users
- **Focus management** for keyboard users

## WCAG 2.1 Compliance

QueryBuilder meets WCAG Level AA standards across:

| Criterion | Support | Notes |
|-----------|---------|-------|
| **1.4.3 Contrast (Minimum)** | ✅ Yes | Text/background contrast ratio ≥ 4.5:1 |
| **2.1.1 Keyboard** | ✅ Yes | All features accessible via keyboard |
| **2.1.2 No Keyboard Trap** | ✅ Yes | Users can navigate away from all elements |
| **2.4.3 Focus Order** | ✅ Yes | Logical, predictable focus sequence |
| **2.4.7 Focus Visible** | ✅ Yes | Visible indicator for focused elements |
| **3.2.1 On Focus** | ✅ Yes | No unexpected context changes |
| **4.1.2 Name, Role, Value** | ✅ Yes | ARIA labels and roles defined |
| **4.1.3 Status Messages** | ✅ Yes | Live regions for announcements |

## ARIA Attributes

QueryBuilder automatically includes ARIA attributes. Enhance with custom labels:

### Example: Add Custom ARIA Labels

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(data)
    .Render()

<script>
var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];

// Add ARIA label to main container
var qbElement = document.getElementById("querybuilder");
qbElement.setAttribute("role", "form");
qbElement.setAttribute("aria-label", "Filter Rules Builder");

// Add ARIA description
var desc = document.createElement("div");
desc.id = "qb-description";
desc.textContent = "Use this form to create filter rules. Add conditions using fields, operators, and values. Combine multiple rules with AND/OR logic.";
qbElement.setAttribute("aria-describedby", "qb-description");
</script>
```

### Common ARIA Roles and Attributes

| ARIA | Purpose | Example |
|------|---------|---------|
| `role="form"` | Identifies container as form | Main QueryBuilder |
| `role="group"` | Groups related elements | Filter group |
| `aria-label` | Provides accessible name | "Department Filter" |
| `aria-describedby` | Provides description | Links to help text |
| `aria-required` | Indicates required field | Input field |
| `aria-invalid` | Indicates validation error | Invalid value |
| `aria-live="polite"` | Announces updates | Rule added message |

## Keyboard Navigation

### Standard Keyboard Controls

| Key | Action |
|-----|--------|
| **Tab** | Move to next focusable element |
| **Shift+Tab** | Move to previous focusable element |
| **Enter** | Activate button or select option |
| **Space** | Toggle checkbox or expand/collapse |
| **Arrow Up/Down** | Navigate dropdown options |
| **Escape** | Close dropdown or cancel action |

### Example: Keyboard Navigation Setup

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(data)
    .AllowKeyboard(true)
    .Render()

<script>
var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];

// All elements are keyboard accessible:
// 1. Field selector: Tab to focus, Arrow to navigate, Enter to select
// 2. Operator selector: Same as field selector
// 3. Value input: Tab to focus, Type value
// 4. Add/Delete buttons: Tab to focus, Enter to activate
// 5. Groups: Tab between group headers, use keyboard to manage

// Custom keyboard event handler
document.addEventListener('keydown', function(e) {
    if (e.key === 'Enter' && e.ctrlKey) {
        // Ctrl+Enter to apply filters
        console.log("Applying filters...");
    }
    if (e.key === 'Delete' && e.shiftKey) {
        // Shift+Delete to clear all filters
        queryBuilder.setRule({ condition: 'and', rules: [] });
    }
});
</script>
```

## Screen Reader Support

Screen readers announce QueryBuilder elements to visually impaired users.

### Example: Optimized for Screen Readers

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(data)
    .Render()

<script>
var queryBuilder = document.getElementById("querybuilder").ej2_instances[0];

// Announce when rules are added
queryBuilder.addEventListener('ruleChange', function(args) {
    if (args.type === 'insert-rule') {
        announceToScreenReader(`New rule added: ${args.data.label} ${args.data.operator} ${args.data.value}`);
    }
});

function announceToScreenReader(message) {
    var announcement = document.createElement('div');
    announcement.setAttribute('role', 'status');
    announcement.setAttribute('aria-live', 'polite');
    announcement.setAttribute('aria-atomic', 'true');
    announcement.style.display = 'none';
    announcement.textContent = message;
    document.body.appendChild(announcement);
    
    // Remove after announcement
    setTimeout(() => announcement.remove(), 1000);
}

// Example announcements:
// "New rule added: First Name equal Nancy"
// "Rule deleted: Salary greater than 50000"
// "Group created with AND condition"
</script>
```

### Screen Reader Tested With

- ✅ NVDA (Windows)
- ✅ JAWS (Windows)
- ✅ VoiceOver (macOS/iOS)
- ✅ TalkBack (Android)

## Localization Setup

Enable multiple language support in QueryBuilder.

### Example: Set Language/Locale

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(data)
    .Locale("es-ES") // Spanish (Spain)
    .Render()
```

### Supported Locales

```
en-US    English (United States)
en-GB    English (United Kingdom)
es-ES    Spanish (Spain)
fr-FR    French (France)
de-DE    German (Germany)
it-IT    Italian (Italy)
pt-BR    Portuguese (Brazil)
ru-RU    Russian
ja-JP    Japanese
zh-CN    Chinese (Simplified)
zh-TW    Chinese (Traditional)
ar-SA    Arabic (Saudi Arabia)
he-IL    Hebrew
hi-IN    Hindi
```

### Example: Multi-Language Application

```csharp
// Controller
public ActionResult Index(string lang = "en-US")
{
    var locale = lang; // en-US, es-ES, fr-FR, etc.
    ViewBag.Locale = locale;
    
    List<QueryBuilderColumn> columns = new List<QueryBuilderColumn>
    {
        new QueryBuilderColumn { Field = "FirstName", Label = "First Name", Type = "string" },
        new QueryBuilderColumn { Field = "Salary", Label = "Salary", Type = "number" }
    };
    
    ViewBag.Columns = columns;
    return View();
}
```

```html
<!-- View -->
<div id="languageSelector">
    <select onchange="changeLanguage(this.value)">
        <option value="en-US">English</option>
        <option value="es-ES">Español</option>
        <option value="fr-FR">Français</option>
        <option value="de-DE">Deutsch</option>
        <option value="ja-JP">日本語</option>
        <option value="zh-CN">中文(简体)</option>
        <option value="ar-SA">العربية</option>
    </select>
</div>

@Html.EJS().QueryBuilder("querybuilder")
    .Columns((IEnumerable<QueryBuilderColumn>)ViewBag.Columns)
    .DataSource((IEnumerable<object>)ViewBag.Data)
    .Locale((string)ViewBag.Locale)
    .Render()

<script>
function changeLanguage(locale) {
    // Reload page with new language
    window.location.href = `?lang=${locale}`;
}
</script>
```

## Translated Strings

Customize text strings for different languages.

### Example: Custom Localization Strings

```html
<script>
// Define custom translations
var translations = {
    'es-ES': {
        'addGroup': 'Agregar Grupo',
        'addCondition': 'Agregar Condición',
        'delete': 'Eliminar',
        'and': 'Y',
        'or': 'O',
        'validationMessage': 'Regla inválida'
    },
    'fr-FR': {
        'addGroup': 'Ajouter un Groupe',
        'addCondition': 'Ajouter une Condition',
        'delete': 'Supprimer',
        'and': 'ET',
        'or': 'OU',
        'validationMessage': 'Règle invalide'
    }
};

// Apply translations
function setLocalizationStrings(locale) {
    if (translations[locale]) {
        var localeStrings = translations[locale];
        
        // Update UI elements
        document.querySelectorAll('[data-i18n]').forEach(el => {
            var key = el.getAttribute('data-i18n');
            if (localeStrings[key]) {
                el.textContent = localeStrings[key];
            }
        });
    }
}
</script>

<button data-i18n="addGroup">Add Group</button>
<button data-i18n="delete">Delete</button>
```

## Culture-Specific Formatting

Format dates and numbers according to user's culture.

### Example: Date and Number Formatting

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(new List<QueryBuilderColumn>
    {
        new QueryBuilderColumn 
        { 
            Field = "HireDate", 
            Label = "Hire Date", 
            Type = "date",
            Format = "MM/dd/yyyy"  // US format
        },
        new QueryBuilderColumn 
        { 
            Field = "Salary", 
            Label = "Salary", 
            Type = "number",
            Format = "C2"  // Currency format
        }
    })
    .DataSource(data)
    .Locale("en-US")
    .Render()
```

### Format Examples by Locale

| Locale | Date Format | Number Format | Currency |
|--------|-------------|---------------|----------|
| en-US | 12/25/2023 | 1,234.56 | $1,234.56 |
| de-DE | 25.12.2023 | 1.234,56 | 1.234,56 € |
| fr-FR | 25/12/2023 | 1 234,56 | 1 234,56 € |
| ja-JP | 2023/12/25 | 1,234.56 | ￥1,234 |
| ar-SA | 25/12/2023 (RTL) | ١٬٢٣٤٫٥٦ | ﷼ ١٬٢٣٤ |

### Code: Apply Locale-Specific Formatting

```csharp
public ActionResult Index(string culture = "en-US")
{
    // Map culture to date/number format
    var formatMap = new Dictionary<string, object>
    {
        { "en-US", new { dateFormat = "MM/dd/yyyy", numberFormat = "N2" } },
        { "de-DE", new { dateFormat = "dd.MM.yyyy", numberFormat = "N2" } },
        { "fr-FR", new { dateFormat = "dd/MM/yyyy", numberFormat = "N2" } },
        { "ja-JP", new { dateFormat = "yyyy/MM/dd", numberFormat = "N2" } }
    };

    var format = formatMap.ContainsKey(culture) ? formatMap[culture] : formatMap["en-US"];

    List<QueryBuilderColumn> columns = new List<QueryBuilderColumn>
    {
        new QueryBuilderColumn
        {
            Field = "HireDate",
            Label = "Hire Date",
            Type = "date",
            Format = (string)((dynamic)format).dateFormat
        },
        new QueryBuilderColumn
        {
            Field = "Salary",
            Label = "Salary",
            Type = "number",
            Format = (string)((dynamic)format).numberFormat
        }
    };

    ViewBag.Columns = columns;
    ViewBag.Culture = culture;
    return View();
}
```

## Common Patterns

### Pattern 1: Fully Accessible QueryBuilder

```html
<div id="qb-container" role="application" aria-label="Advanced Filter Builder">
    <h2 id="qb-title">Filter Results</h2>
    <p id="qb-description">
        Build custom filters by selecting fields, operators, and values. 
        Combine multiple conditions with AND/OR logic.
    </p>
    
    @Html.EJS().QueryBuilder("querybuilder")
        .Columns(columns)
        .DataSource(data)
        .Locale("en-US")
        .AllowKeyboard(true)
        .Render()
    
    <div role="status" aria-live="polite" aria-atomic="true" id="qb-status"></div>
</div>

<style>
#qb-title {
    font-size: 24px;
    margin: 20px 0 10px;
    color: #333;
}

#qb-description {
    font-size: 14px;
    color: #666;
    margin-bottom: 20px;
}

#qb-container .e-querybuilder *:focus {
    outline: 3px solid #007bff;
    outline-offset: 2px;
}
</style>
```

### Pattern 2: Right-to-Left (RTL) with Localization

```html
@Html.EJS().QueryBuilder("querybuilder")
    .Columns(columns)
    .DataSource(data)
    .Locale("ar-SA")
    .EnableRtl(true)
    .Render()
```

## Troubleshooting

### Issue: "Screen reader not announcing rules"
**Cause:** ARIA live regions not configured  
**Solution:**
1. Set `aria-live="polite"` on status container
2. Use `aria-atomic="true"` for complete announcements
3. Clear status region between announcements

### Issue: "Keyboard navigation skips elements"
**Cause:** Incorrect tab index or focus management  
**Solution:**
1. Verify all interactive elements have `tabindex >= 0`
2. Remove `tabindex > 0` (creates tab order problems)
3. Use `tabindex="-1"` for programmatically focused elements

### Issue: "Date format incorrect for locale"
**Cause:** Format string doesn't match locale convention  
**Solution:**
1. Use standard format codes: `MM/dd/yyyy`, `dd.MM.yyyy`, `dd/MM/yyyy`
2. Test with actual locale settings
3. Verify browser language matches application locale

### Issue: "RTL text alignment issues"
**Cause:** `dir="rtl"` not set or CSS not updated  
**Solution:**
1. Set `dir="rtl"` on parent container
2. Use CSS flexbox/grid for proper alignment
3. Set `EnableRtl(true)` in QueryBuilder
4. Test with actual RTL language content

---

## Next Steps

- Review [Data Binding](data-binding.md) for multi-language data sources
- Explore [Styling and Appearance](styling-and-appearance.md) for accessible colors/contrast
- Check [Advanced Features](advanced-features.md) for import/export with i18n
