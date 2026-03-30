# Globalization and Localization

## Table of Contents
- [Culture-Specific Date Formatting](#culture-specific-date-formatting)
- [CLDR and Locale Configuration](#cldr-and-locale-configuration)
- [RTL Support](#rtl-support)
- [Multiple Language Display](#multiple-language-display)
- [Regional Calendar Considerations](#regional-calendar-considerations)
- [Timezone Handling](#timezone-handling)
- [Dynamic Culture Switching](#dynamic-culture-switching)

## Culture-Specific Date Formatting

DatePicker automatically formats dates based on culture/locale:

### Common Culture Formats

| Culture | Locale ID | Default Format | Example |
|---------|-----------|-----------------|---------|
| US English | en-US | M/d/yyyy | 3/19/2026 |
| GB English | en-GB | dd/MM/yyyy | 19/03/2026 |
| German | de-DE | dd.MM.yyyy | 19.03.2026 |
| French | fr-FR | dd/MM/yyyy | 19/03/2026 |
| Spanish | es-ES | d/M/yyyy | 19/3/2026 |
| Italian | it-IT | dd/MM/yyyy | 19/03/2026 |
| Japanese | ja-JP | yyyy/MM/dd | 2026/03/19 |
| Dutch | nl-NL | d-M-yyyy | 19-3-2026 |

### Setting Culture in ASP.NET Core

**Global Culture in Program.cs:**

```csharp
var builder = WebApplication.CreateBuilder(args);

// Add localization services
builder.Services.AddLocalization(options => 
    options.ResourcesPath = "Resources");

var app = builder.Build();

// Set default culture
var supportedCultures = new[] { 
    new CultureInfo("en-US"),
    new CultureInfo("de-DE"),
    new CultureInfo("fr-FR"),
    new CultureInfo("ja-JP")
};

app.UseRequestLocalization(new RequestLocalizationOptions
{
    DefaultRequestCulture = new RequestCulture("en-US"),
    SupportedCultures = supportedCultures,
    SupportedUICultures = supportedCultures
});

app.Run();
```

### Culture-Based DatePicker Format

```csharp
public class LocalizedModel : PageModel
{
    [BindProperty]
    public DateTime EventDate { get; set; }
    
    public string CultureSpecificFormat { get; set; }
    
    public void OnGet()
    {
        var culture = CultureInfo.CurrentCulture;
        CultureSpecificFormat = culture.DateTimeFormat.ShortDatePattern;
        
        // Examples:
        // en-US: "M/d/yyyy"
        // de-DE: "dd.MM.yyyy"
        // ja-JP: "yyyy/MM/dd"
    }
}
```

```html
<!-- DatePicker uses culture-specific format automatically -->
<ejs-datepicker asp-for="EventDate"
                 placeholder="Culture-aware format">
</ejs-datepicker>
```

### Explicit Culture Override

```html
<!-- Force specific culture format regardless of app culture -->
<ejs-datepicker id="germanDatePicker"
                 value="@DateTime.Today"
                 format="dd.MM.yyyy"
                 placeholder="19.03.2026">
</ejs-datepicker>

<!-- Force US format -->
<ejs-datepicker id="usDatePicker"
                 value="@DateTime.Today"
                 format="MM/dd/yyyy"
                 placeholder="03/19/2026">
</ejs-datepicker>
```

## CLDR and Locale Configuration

### CLDR (Common Locale Data Repository)

Syncfusion uses CLDR for comprehensive locale data (date formats, month names, etc.):

```csharp
using Syncfusion.EJ2;

public class CLDRModel : PageModel
{
    public void OnGet()
    {
        // CLDR provides standardized locale formats
        // en-US, en-GB, de-DE, ja-JP, etc.
        // Automatically applied to DatePicker
    }
}
```

### Locale-Specific Month Names

DatePicker automatically displays month names in the selected locale:

```html
<!-- English: January, February, March ... -->
<ejs-datepicker id="enPicker"
                 format="MMMM yyyy"
                 placeholder="March 2026">
</ejs-datepicker>

<!-- German: Januar, Februar, März ... -->
<ejs-datepicker id="dePicker"
                 format="MMMM yyyy"
                 placeholder="März 2026">
</ejs-datepicker>

<!-- French: janvier, février, mars ... -->
<ejs-datepicker id="frPicker"
                 format="MMMM yyyy"
                 placeholder="mars 2026">
</ejs-datepicker>

<!-- Japanese: 1月, 2月, 3月 ... -->
<ejs-datepicker id="jaPicker"
                 format="yyyy年MM月"
                 placeholder="2026年3月">
</ejs-datepicker>
```

### Locale-Specific Day Names

```html
<!-- Monday in different languages (Calendar popup shows day headers) -->
<ejs-datepicker id="localeDay"
                 format="dddd, MMMM dd, yyyy"
                 placeholder="Show weekday name in locale">
</ejs-datepicker>

<!-- English: Thursday, March 19, 2026 -->
<!-- German: Donnerstag, 19. März 2026 -->
<!-- French: jeudi 19 mars 2026 -->
<!-- Spanish: jueves, 19 de marzo de 2026 -->
```

## RTL Support

Right-to-left language support for Arabic, Hebrew, Persian, Urdu:

### RTL in HTML

```html
<!-- Add dir="rtl" to enable RTL layout -->
<div dir="rtl">
    <ejs-datepicker id="arabicPicker"
                     format="yyyy-MM-dd"
                     placeholder="التاريخ">
    </ejs-datepicker>
</div>
```

### RTL in CSS

```css
/* CSS RTL support */
[dir="rtl"] .e-datepicker {
    direction: rtl;
    text-align: right;
}

[dir="rtl"] .e-input {
    text-align: right;
}

[dir="rtl"] .e-calendar {
    direction: rtl;
}
```

### RTL with Different Locales

**Arabic (ar-SA):**
```html
<div dir="rtl" lang="ar">
    <ejs-datepicker id="arabicDatePicker"
                     format="yyyy-MM-dd"
                     placeholder="اختر التاريخ">
    </ejs-datepicker>
</div>
```

**Hebrew (he-IL):**
```html
<div dir="rtl" lang="he">
    <ejs-datepicker id="hebrewDatePicker"
                     format="yyyy-MM-dd"
                     placeholder="בחר תאריך">
    </ejs-datepicker>
</div>
```

**Persian (fa-IR):**
```html
<div dir="rtl" lang="fa">
    <ejs-datepicker id="persianDatePicker"
                     format="yyyy-MM-dd"
                     placeholder="تاریخ را انتخاب کنید">
    </ejs-datepicker>
</div>
```

### RTL Page Layout

```html
<!DOCTYPE html>
<html dir="rtl" lang="ar">
<head>
    <meta charset="utf-8" />
    <title>تطبيق اختيار التاريخ</title>
    <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/24.1.48/fluent.css" />
</head>
<body>
    <h1>حجز الموعد</h1>
    
    <form method="post">
        <label>اختر التاريخ:</label>
        <ejs-datepicker asp-for="AppointmentDate"
                         format="yyyy-MM-dd"
                         placeholder="التاريخ">
        </ejs-datepicker>
        
        <button type="submit">حفظ</button>
    </form>
    
    <script src="https://cdn.syncfusion.com/ej2/24.1.48/dist/ej2.min.js"></script>
    <ejs-scripts></ejs-scripts>
</body>
</html>
```

## Multiple Language Display

### Language Switching

```csharp
public class MultiLanguageModel : PageModel
{
    [BindProperty]
    public DateTime EventDate { get; set; }
    
    [BindProperty]
    public string CurrentLanguage { get; set; } = "en";
    
    public void OnGet(string lang = "en")
    {
        CurrentLanguage = lang;
        Thread.CurrentThread.CurrentCulture = new CultureInfo(GetCultureCode(lang));
        Thread.CurrentThread.CurrentUICulture = new CultureInfo(GetCultureCode(lang));
    }
    
    private string GetCultureCode(string lang)
    {
        return lang switch
        {
            "de" => "de-DE",
            "fr" => "fr-FR",
            "es" => "es-ES",
            "ja" => "ja-JP",
            _ => "en-US"
        };
    }
}
```

```html
<!-- Language selector -->
<div class="language-selector">
    <a asp-route-lang="en">English</a>
    <a asp-route-lang="de">Deutsch</a>
    <a asp-route-lang="fr">Français</a>
    <a asp-route-lang="ja">日本語</a>
</div>

<!-- DatePicker with current culture -->
<ejs-datepicker asp-for="EventDate"
                 placeholder="Select date in current language">
</ejs-datepicker>
```

**Behavior by Language:**
- English (en-US): `3/19/2026` | Month names: January, February
- German (de-DE): `19.03.2026` | Month names: Januar, Februar
- French (fr-FR): `19/03/2026` | Month names: janvier, février
- Japanese (ja-JP): `2026/03/19` | Month names: 1月, 2月

## Regional Calendar Considerations

### Different Calendar Systems

Some cultures use alternative calendar systems:

```csharp
// Gregorian calendar (most common)
var gregCulture = new CultureInfo("en-US");

// Islamic calendar (hijri) - Middle East, North Africa
var hijriCulture = new CultureInfo("ar-SA");
hijriCulture.DateTimeFormat.Calendar = new HijriCalendar();

// Hebrew calendar - Israel
var hebrewCulture = new CultureInfo("he-IL");
hebrewCulture.DateTimeFormat.Calendar = new HebrewCalendar();
```

### Calendar Implementation

```csharp
public class CalendarAwareModel : PageModel
{
    public DateTime GetCurrentDate()
    {
        // ASP.NET Core uses Gregorian by default
        // CLDR handles calendar presentation
        var culture = CultureInfo.CurrentCulture;
        return DateTime.Now;
    }
}
```

```html
<!-- DatePicker works with all calendar systems via CLDR -->
<ejs-datepicker asp-for="EventDate"
                 placeholder="Calendar adapts to culture">
</ejs-datepicker>

<!-- For Islamic regions, displays in Hijri calendar format -->
<!-- For Hebrew regions, displays in Hebrew calendar format -->
```

## Timezone Handling

### Basic Timezone Support

```csharp
public class TimezoneModel : PageModel
{
    [BindProperty]
    public DateTime ScheduledDate { get; set; }
    
    public void OnPost()
    {
        // DatePicker returns local DateTime (no timezone info embedded)
        var local = ScheduledDate;
        
        // Convert to specific timezone
        TimeZoneInfo tz = TimeZoneInfo.FindSystemTimeZoneById("Eastern Standard Time");
        DateTime eastern = TimeZoneInfo.ConvertTime(ScheduledDate, tz);
        
        // Store in database as UTC
        DateTime utc = ScheduledDate.ToUniversalTime();
    }
}
```

### Display in Multiple Timezones

```csharp
public class MultiTimezoneModel : PageModel
{
    [BindProperty]
    public DateTime ConferenceDate { get; set; }
    
    public Dictionary<string, DateTime> TimesByZone { get; set; }
    
    public void OnGet()
    {
        ConferenceDate = new DateTime(2026, 3, 19, 14, 0, 0);
        TimesByZone = new();
        
        var timezones = new[] 
        {
            "Pacific Standard Time",
            "Eastern Standard Time",
            "UTC",
            "Central European Standard Time",
            "Japan Standard Time"
        };
        
        foreach (var tzName in timezones)
        {
            TimeZoneInfo tz = TimeZoneInfo.FindSystemTimeZoneById(tzName);
            TimesByZone[tzName] = TimeZoneInfo.ConvertTime(ConferenceDate, tz);
        }
    }
}
```

```html
<div>
    <h2>Conference Date: @Model.ConferenceDate.ToString("yyyy-MM-dd HH:mm")</h2>
    
    <h3>Local Times by Timezone:</h3>
    <ul>
        @foreach (var tz in Model.TimesByZone)
        {
            <li>@tz.Key: @tz.Value.ToString("HH:mm")</li>
        }
    </ul>
</div>
```

### Timezone-Aware Selection

```html
<form method="post">
    <label>Select conference date (UTC):</label>
    <ejs-datepicker asp-for="ConferenceDate"
                     format="yyyy-MM-dd HH:mm"
                     placeholder="2026-03-19 14:00 UTC">
    </ejs-datepicker>
    
    <p>Local times will be calculated automatically</p>
    
    <button type="submit">Schedule</button>
</form>

<script>
document.getElementById('conferenceDate').addEventListener('change', function(e) {
    var date = this.ej2_instances[0].value;
    
    if (date) {
        // Timezone info can be appended to ensure UTC handling
        console.log('UTC date:', date);
    }
});
</script>
```

## Dynamic Culture Switching

### Runtime Culture Change

```csharp
public class DynamicCultureModel : PageModel
{
    [BindProperty]
    public DateTime EventDate { get; set; }
    
    [BindProperty]
    public string SelectedCulture { get; set; } = "en-US";
    
    public List<SelectListItem> AvailableCultures { get; set; }
    
    public void OnGet()
    {
        AvailableCultures = new()
        {
            new SelectListItem { Text = "English (US)", Value = "en-US" },
            new SelectListItem { Text = "German", Value = "de-DE" },
            new SelectListItem { Text = "French", Value = "fr-FR" },
            new SelectListItem { Text = "Japanese", Value = "ja-JP" },
            new SelectListItem { Text = "Arabic", Value = "ar-SA" }
        };
    }
    
    public IActionResult OnPostChangeCulture()
    {
        Response.Cookies.Append(
            ".AspNetCore.Culture",
            $"c={SelectedCulture}|uic={SelectedCulture}");
        
        return RedirectToPage();
    }
}
```

```html
<form method="post" asp-page-handler="ChangeCulture">
    <label>Select Language:</label>
    <select asp-for="SelectedCulture" 
            asp-items="Model.AvailableCultures">
    </select>
    <button type="submit">Change</button>
</form>

<hr />

<form method="post">
    <label>Select Date:</label>
    <ejs-datepicker asp-for="EventDate"
                     placeholder="Date format adapts to language">
    </ejs-datepicker>
    
    <button type="submit">Submit</button>
</form>

<p>Current Culture: @CultureInfo.CurrentCulture.DisplayName</p>
```

### JavaScript-Side Culture Detection

```html
<script>
// Detect browser language and auto-set culture
function getUICulture() {
    var userLang = navigator.language || navigator.userLanguage;
    
    var cultureMap = {
        'de': 'de-DE',
        'fr': 'fr-FR',
        'ja': 'ja-JP',
        'ar': 'ar-SA',
        'en': 'en-US'
    };
    
    var lang = userLang.split('-')[0];
    return cultureMap[lang] || 'en-US';
}

console.log('Detected Culture:', getUICulture());
</script>
```

---

**Previous:** [Events and Interactions](../events-and-interactions.md)  
**Next:** [Styling and Appearance](../styling-and-appearance.md)
