# Date Formats and Parsing

## Table of Contents
- [Standard Format Patterns](#standard-format-patterns)
- [Custom Format Strings](#custom-format-strings)
- [Input Formats](#input-formats)
- [Locale-Specific Formatting](#locale-specific-formatting)
- [Format Conversion Examples](#format-conversion-examples)
- [Edge Cases](#edge-cases)
- [Date Validation](#date-validation)

## Standard Format Patterns

DatePicker uses .NET date format strings. Common patterns:

| Pattern | Example | Description |
|---------|---------|-------------|
| `yyyy-MM-dd` | 2026-03-19 | ISO 8601 format (international standard) |
| `MM/dd/yyyy` | 03/19/2026 | US format |
| `dd/MM/yyyy` | 19/03/2026 | European format |
| `dd-MM-yyyy` | 19-03-2026 | Alternative European |
| `yyyy-MM-dd HH:mm` | 2026-03-19 14:30 | With time (24-hour) |
| `yyyy-MM-dd hh:mm tt` | 2026-03-19 2:30 PM | With time (12-hour) |
| `MMMM dd, yyyy` | March 19, 2026 | Long format with month name |
| `MMM dd, yyyy` | Mar 19, 2026 | Abbreviated month name |
| `dddd, MMMM dd, yyyy` | Thursday, March 19, 2026 | Full with weekday |

### Format String Components

| Component | Pattern | Example | Description |
|-----------|---------|---------|-------------|
| Year | `yyyy` | 2026 | 4-digit year |
| Year | `yy` | 26 | 2-digit year |
| Month | `MM` | 03 | 2-digit month (01-12) |
| Month | `M` | 3 | Single-digit month (1-12) |
| Month | `MMM` | Mar | Abbreviated month name |
| Month | `MMMM` | March | Full month name |
| Day | `dd` | 19 | 2-digit day (01-31) |
| Day | `d` | 19 | Single-digit day (1-31) |
| Day | `ddd` | Thu | Abbreviated weekday |
| Day | `dddd` | Thursday | Full weekday name |
| Hour (24) | `HH` | 14 | 2-digit hour (00-23) |
| Hour (12) | `hh` | 02 | 2-digit hour (01-12) |
| Minute | `mm` | 30 | 2-digit minute (00-59) |
| Second | `ss` | 45 | 2-digit second (00-59) |
| AM/PM | `tt` | PM | AM or PM indicator |

## Custom Format Strings

### Basic Implementation

**ASP.NET Core:**

```csharp
public class EventModel : PageModel
{
    [BindProperty]
    public DateTime EventDate { get; set; } = DateTime.Today;
}
```

```html
<!-- Display as "03/19/2026" (US format) -->
<ejs-datepicker asp-for="EventDate"
                 format="MM/dd/yyyy"
                 placeholder="MM/DD/YYYY">
</ejs-datepicker>
```

### Multiple Custom Formats

```html
<!-- Display format differs from input formats -->
<ejs-datepicker id="flexibleDate"
                 value="@DateTime.Today"
                 format="yyyy-MM-dd"
                 input-formats="@new[] { 'yyyy-MM-dd', 'dd/MM/yyyy', 'MM-dd-yyyy' }"
                 placeholder="Enter date in any format above">
</ejs-datepicker>
```

User can enter: `2026-03-19` OR `19/03/2026` OR `03-19-2026`
Always displays as: `2026-03-19`

### Long Format Examples

```html
<!-- European style with month name -->
<ejs-datepicker asp-for="ContractDate"
                 format="dd MMMM yyyy"
                 placeholder="19 March 2026">
</ejs-datepicker>

<!-- US style with full weekday -->
<ejs-datepicker asp-for="ConferenceDate"
                 format="dddd, MMMM dd, yyyy"
                 placeholder="Thursday, March 19, 2026">
</ejs-datepicker>

<!-- Compact European -->
<ejs-datepicker asp-for="BirthdayDate"
                 format="dd.MM.yyyy"
                 placeholder="19.03.2026">
</ejs-datepicker>
```

### With Time Component

```html
<!-- Date and time (24-hour) -->
<ejs-datepicker asp-for="MeetingDateTime"
                 format="yyyy-MM-dd HH:mm"
                 placeholder="2026-03-19 14:30">
</ejs-datepicker>

<!-- Date and time (12-hour with AM/PM) -->
<ejs-datepicker asp-for="AppointmentTime"
                 format="MM/dd/yyyy hh:mm tt"
                 placeholder="03/19/2026 2:30 PM">
</ejs-datepicker>
```

## Input Formats

### Accepting Multiple Input Patterns

Allow users to enter dates in different formats while standardizing display:

```csharp
public class FlexibleDateModel : PageModel
{
    [BindProperty]
    public DateTime SubmissionDate { get; set; }
}
```

```html
<ejs-datepicker asp-for="SubmissionDate"
                 format="yyyy-MM-dd"
                 input-formats="@new[] { 
                     'yyyy-MM-dd',
                     'dd/MM/yyyy', 
                     'MM-dd-yyyy',
                     'dd-MMM-yyyy'
                 }"
                 placeholder="2026-03-19 or 19/03/2026 or 03-19-2026">
</ejs-datepicker>
```

**User Input Behavior:**
- Enters: `19/03/2026` → Recognized via `dd/MM/yyyy` format
- Stores as: `2026-03-19` (ISO format)
- Displays as: `2026-03-19` (format attribute)

### Format Parsing Priority

Formats are checked in order. First match wins:

```html
<ejs-datepicker id="priorityTest"
                 format="yyyy-MM-dd"
                 input-formats="@new[] { 
                     'yyyy-MM-dd',   <!-- Checked first -->
                     'dd/MM/yyyy',   <!-- Checked second -->
                     'MM/dd/yyyy'    <!-- Checked third -->
                 }">
</ejs-datepicker>
```

**Edge Case:** Input `01/02/2026` matches first as `yyyy-MM-dd`? No → ISO requires 4-digit year first. Skips to `dd/MM/yyyy` → January 2nd, 2026.

## Locale-Specific Formatting

### Culture-Based Default Format

DatePicker automatically uses culture-based formatting if no format specified:

```html
<!-- No format specified - uses culture default -->
<ejs-datepicker asp-for="EventDate"></ejs-datepicker>
```

**Default Formats by Culture:**
- `en-US`: `M/d/yyyy` (3/19/2026)
- `en-GB`: `dd/MM/yyyy` (19/03/2026)
- `de-DE`: `dd.MM.yyyy` (19.03.2026)
- `fr-FR`: `dd/MM/yyyy` (19/03/2026)
- `ja-JP`: `yyyy/MM/dd` (2026/03/19)

### German Locale

```html
<!-- German format with month name -->
<ejs-datepicker asp-for="DeliveryDate"
                 format="dd. MMMM yyyy"
                 placeholder="19. März 2026">
</ejs-datepicker>
```

### French Locale

```html
<!-- French format -->
<ejs-datepicker asp-for="DateLivraison"
                 format="dd/MM/yyyy"
                 placeholder="19/03/2026">
</ejs-datepicker>
```

## Format Conversion Examples

### Example 1: ISO to US Format

**Requirement:** Store as ISO (2026-03-19), display as US (03/19/2026)

```csharp
public class ProjectModel : PageModel
{
    [BindProperty]
    public DateTime StartDate { get; set; }
}
```

```html
<ejs-datepicker asp-for="StartDate"
                 format="MM/dd/yyyy"
                 input-formats="@new[] { 'MM/dd/yyyy', 'yyyy-MM-dd' }"
                 placeholder="MM/DD/YYYY">
</ejs-datepicker>
```

**Backend Processing:**
```csharp
public void OnPost()
{
    // StartDate is DateTime object
    string isoFormat = StartDate.ToString("yyyy-MM-dd");  // 2026-03-19
    string usFormat = StartDate.ToString("MM/dd/yyyy");   // 03/19/2026
}
```

### Example 2: With Month Names

**Requirement:** Display as "March 19, 2026"

```html
<ejs-datepicker asp-for="ConferenceDate"
                 format="MMMM dd, yyyy"
                 input-formats="@new[] { 'MM/dd/yyyy', 'dd/MM/yyyy', 'MMMM dd, yyyy' }"
                 placeholder="March 19, 2026">
</ejs-datepicker>
```

**User can enter:**
- `03/19/2026` → Parsed and displayed as `March 19, 2026`
- `19/03/2026` → Parsed and displayed as `March 19, 2026`
- `March 19, 2026` → Direct match

### Example 3: Abbreviated Weekday

**Requirement:** Show weekday with date

```html
<ejs-datepicker asp-for="MeetingDate"
                 format="ddd, MMM dd, yyyy"
                 placeholder="Thu, Mar 19, 2026">
</ejs-datepicker>
```

Display: `Thu, Mar 19, 2026` (automatically calculated)

### Example 4: Long Format with Full Weekday

```html
<ejs-datepicker asp-for="EventDate"
                 format="dddd, MMMM dd, yyyy"
                 placeholder="Thursday, March 19, 2026">
</ejs-datepicker>
```

## Edge Cases

### Leap Year Handling

February 29 in leap years:

```csharp
public class LeapYearModel : PageModel
{
    [BindProperty]
    public DateTime LeapDate { get; set; }
}
```

```html
<!-- 2024 is leap year (divisible by 4) -->
<ejs-datepicker asp-for="LeapDate"
                 min="@new DateTime(2024, 2, 1)"
                 max="@new DateTime(2024, 2, 29)"
                 format="yyyy-MM-dd"
                 placeholder="2024-02-29">
</ejs-datepicker>

<!-- 2023 is NOT leap year - max to Feb 28 -->
<ejs-datepicker asp-for="NonLeapDate"
                 min="@new DateTime(2023, 2, 1)"
                 max="@new DateTime(2023, 2, 28)"
                 format="yyyy-MM-dd">
</ejs-datepicker>
```

### Month Boundary Issues

Navigating from Jan 31 to Feb:

```csharp
DateTime janDate = new DateTime(2026, 1, 31);
DateTime nextMonth = janDate.AddMonths(1);  // Feb 28 (automatically adjusted)
```

**DatePicker behavior:** When user navigates month, Feb 31 → Feb 28 (last valid day)

### Century Interpretation

Two-digit years (`yy` format):

```html
<!-- 00-29 interpreted as 2000-2029 -->
<!-- 30-99 interpreted as 1930-1999 -->
<ejs-datepicker format="MM/dd/yy"
                 placeholder="03/19/26 → 2026">
</ejs-datepicker>
```

**Note:** Avoid `yy` format; use `yyyy` for clarity

### Ambiguous Input

```html
<!-- Input: 01/02/2026 -->
<!-- dd/MM/yyyy → February 1, 2026 -->
<!-- MM/dd/yyyy → January 2, 2026 -->
<ejs-datepicker input-formats="@new[] { 'dd/MM/yyyy', 'MM/dd/yyyy' }"></ejs-datepicker>
```

First matching format wins. Order matters!

## Date Validation

### Validating Format

```csharp
public class ValidatedDateModel : PageModel
{
    [BindProperty]
    public DateTime EventDate { get; set; }
    
    public IActionResult OnPost()
    {
        if (!ModelState.IsValid)
        {
            // Invalid date format or value
            return Page();
        }
        
        // Valid date
        return RedirectToPage("Confirmation", new { date = EventDate });
    }
}
```

### Server-Side Format Validation

```csharp
public void OnPost()
{
    string dateStr = Request.Form["eventDate"];
    
    if (DateTime.TryParseExact(dateStr, "yyyy-MM-dd", 
        System.Globalization.CultureInfo.InvariantCulture, 
        System.Globalization.DateTimeStyles.None, 
        out DateTime parsedDate))
    {
        // Valid format
    }
    else
    {
        ModelState.AddModelError("EventDate", "Invalid date format");
    }
}
```

### Invalid Date Handling

```html
<!-- DatePicker with validation -->
<ejs-datepicker asp-for="EventDate"
                 strict-mode="true"
                 format="yyyy-MM-dd"
                 input-formats="@new[] { 'yyyy-MM-dd', 'dd/MM/yyyy' }">
</ejs-datepicker>

<!-- Validation message -->
<span asp-validation-for="EventDate" class="text-danger"></span>
```

**Strict Mode:** Invalid dates rejected, value remains unchanged

---

**Previous:** [Getting Started](../getting-started.md)  
**Next:** [Date Ranges and Validation](../date-ranges-and-validation.md)
