# Date Ranges and Validation

## Table of Contents
- [Setting Date Ranges](#setting-date-ranges)
- [Min/Max Constraints](#minmax-constraints)
- [Disabled Dates](#disabled-dates)
- [Strict Mode](#strict-mode)
- [Out-of-Range Handling](#out-of-range-handling)
- [Real-World Validation Patterns](#real-world-validation-patterns)
- [Edge Cases](#edge-cases)

## Setting Date Ranges

Date ranges restrict user selection to valid date windows. Common scenarios:
- Birth dates (past only, not older than 150 years)
- Appointment booking (future only, within 90 days)
- Contract dates (between specific start/end dates)
- Availability windows (working days, business hours)

### Basic Min/Max Pattern

**ASP.NET Core:**

```csharp
public class AppointmentModel : PageModel
{
    [BindProperty]
    public DateTime AppointmentDate { get; set; } = DateTime.Today;
    
    public DateTime MinDate { get; set; }
    public DateTime MaxDate { get; set; }
    
    public void OnGet()
    {
        MinDate = DateTime.Today;  // Today
        MaxDate = DateTime.Today.AddDays(30);  // 30 days from now
    }
}
```

```html
<!-- Simple range: today to 30 days ahead -->
<ejs-datepicker asp-for="AppointmentDate"
                 min="@Model.MinDate"
                 max="@Model.MaxDate"
                 format="yyyy-MM-dd"
                 placeholder="Select appointment (next 30 days)">
</ejs-datepicker>
```

## Min/Max Constraints

### Inline Min/Max

```html
<!-- Hard-coded range -->
<ejs-datepicker asp-for="EventDate"
                 min="@new DateTime(2026, 1, 1)"
                 max="@new DateTime(2026, 12, 31)"
                 placeholder="2026 dates only">
</ejs-datepicker>
```

**Result:** User can only select dates within January 1 - December 31, 2026

### Dynamic Min/Max from Properties

```csharp
public class BookingModel : PageModel
{
    [BindProperty]
    public DateTime BookingDate { get; set; }
    
    public DateTime AvailableFromDate { get; set; }
    public DateTime AvailableToDate { get; set; }
    
    public void OnGet()
    {
        AvailableFromDate = DateTime.Today;
        AvailableToDate = DateTime.Today.AddMonths(3);
    }
}
```

```html
<ejs-datepicker asp-for="BookingDate"
                 min="@Model.AvailableFromDate"
                 max="@Model.AvailableToDate"
                 placeholder="Select within available period">
</ejs-datepicker>
```

### Range Validation Rule

**Important:** Min must be ≤ Max

```csharp
DateTime min = DateTime.Today;
DateTime max = DateTime.Today.AddDays(-1);  // ❌ Max < Min - INVALID

// Correct:
DateTime max = DateTime.Today.AddDays(30);  // ✅ Max > Min - VALID
```

### Asymmetric Ranges

**Only Past Dates (Birth Date):**

```html
<ejs-datepicker asp-for="BirthDate"
                 max="@DateTime.Today"
                 placeholder="Birth date (past only)">
</ejs-datepicker>
```

**Only Future Dates (Appointment):**

```html
<ejs-datepicker asp-for="AppointmentDate"
                 min="@DateTime.Today"
                 placeholder="Appointment (future only)">
</ejs-datepicker>
```

**No Weekends (Mon-Fri Only):**

```csharp
public class WorkDayModel : PageModel
{
    [BindProperty]
    public DateTime DeliveryDate { get; set; }
    
    public IEnumerable<DateTime> DisabledDates { get; set; }
    
    public void OnGet()
    {
        // Generate weekend dates for next 90 days
        var disabled = new List<DateTime>();
        for (int i = 0; i < 90; i++)
        {
            DateTime date = DateTime.Today.AddDays(i);
            if (date.DayOfWeek == DayOfWeek.Saturday || 
                date.DayOfWeek == DayOfWeek.Sunday)
            {
                disabled.Add(date);
            }
        }
        DisabledDates = disabled;
    }
}
```

## Disabled Dates

### Disable Specific Dates

Prevent selection of specific dates (holidays, blackout dates):

```html
<!-- Disable specific holiday dates -->
<ejs-datepicker id="holidayPicker"
                 value="@DateTime.Today"
                 format="yyyy-MM-dd">
</ejs-datepicker>

<script>
// After DatePicker is created
var datepicker = document.getElementById('holidayPicker').ej2_instances[0];

// Mark specific dates as disabled
datepicker.disabledDates = [
    new Date(2026, 0, 1),    // Jan 1 - New Year
    new Date(2026, 11, 25),  // Dec 25 - Christmas
    new Date(2026, 11, 26)   // Dec 26 - Boxing Day
];
</script>
```

### Disable Date Ranges

```html
<ejs-datepicker id="rangePicker"
                 value="@DateTime.Today"
                 format="yyyy-MM-dd">
</ejs-datepicker>

<script>
var datepicker = document.getElementById('rangePicker').ej2_instances[0];

// Disable ranges using renderDayCell event
datepicker.renderDayCell = function(args) {
    // Disable maintenance window: March 15-20
    if (args.date >= new Date(2026, 2, 15) && 
        args.date <= new Date(2026, 2, 20)) {
        args.isDisabled = true;
    }
    
    // Disable Sundays
    if (args.date.getDay() === 0) {
        args.isDisabled = true;
    }
};
</script>
```

## Strict Mode

### Strict Mode Enabled (Default False)

Strict mode enforces validation - invalid dates are rejected:

```html
<!-- Strict mode OFF (allows some invalid dates) -->
<ejs-datepicker id="relaxedPicker"
                 format="yyyy-MM-dd"
                 strict-mode="false">
</ejs-datepicker>

<!-- Strict mode ON (rejects invalid dates) -->
<ejs-datepicker id="strictPicker"
                 format="yyyy-MM-dd"
                 strict-mode="true">
</ejs-datepicker>
```

### Strict Mode Behavior

| Action | Strict OFF | Strict ON |
|--------|-----------|-----------|
| Input invalid date | Value set, shows error | Value unchanged |
| Input out-of-range | Value set, shows error | Value unchanged |
| Out-of-range value programmatically | Allowed | Rejected |
| Invalid format | Tries to parse | Strict parse failure |

### Example: Strict Validation

```csharp
public class StrictDateModel : PageModel
{
    [BindProperty]
    public DateTime EventDate { get; set; }
    
    public void OnPost()
    {
        // With strict-mode="true", EventDate is guaranteed valid
        if (EventDate < DateTime.Today || EventDate > DateTime.Today.AddMonths(1))
        {
            // Should never reach here (strict mode prevents this)
            ModelState.AddModelError("EventDate", "Date out of range");
        }
    }
}
```

```html
<ejs-datepicker asp-for="EventDate"
                 min="@DateTime.Today"
                 max="@DateTime.Today.AddMonths(1)"
                 strict-mode="true"
                 format="yyyy-MM-dd">
</ejs-datepicker>
```

## Out-of-Range Handling

### Value When Out of Range

If value is outside min/max, DatePicker behavior:

```csharp
public class OutOfRangeModel : PageModel
{
    [BindProperty]
    public DateTime? SelectedDate { get; set; }
    
    public void OnPost()
    {
        // If user inputs date outside range with strict-mode="true"
        // SelectedDate remains unchanged
        
        // If strict-mode="false"
        // SelectedDate is set but flagged with error state
    }
}
```

### Handling with Validation

```html
<form method="post">
    <ejs-datepicker asp-for="SelectedDate"
                     min="@DateTime.Today"
                     max="@DateTime.Today.AddDays(30)"
                     strict-mode="true"
                     format="yyyy-MM-dd">
    </ejs-datepicker>
    
    <!-- Validation message -->
    <span asp-validation-for="SelectedDate" class="text-danger"></span>
    
    <button type="submit">Submit</button>
</form>
```

**Validation Logic:**

```csharp
public void OnPost()
{
    if (SelectedDate.HasValue)
    {
        if (SelectedDate < DateTime.Today || 
            SelectedDate > DateTime.Today.AddDays(30))
        {
            ModelState.AddModelError("SelectedDate", 
                "Date must be within next 30 days");
            return;
        }
    }
    
    // Valid date - process
}
```

## Real-World Validation Patterns

### Pattern 1: Birth Date Validation

**Requirement:** Users must be 18+ years old

```csharp
public class RegistrationModel : PageModel
{
    [BindProperty]
    public DateTime BirthDate { get; set; }
    
    public void OnPost()
    {
        int age = DateTime.Today.Year - BirthDate.Year;
        if (BirthDate.Date > DateTime.Today.AddYears(-age))
            age--;
        
        if (age < 18)
            ModelState.AddModelError("BirthDate", "Must be 18 or older");
    }
}
```

```html
<!-- Birth date picker -->
<ejs-datepicker asp-for="BirthDate"
                 max="@DateTime.Today.AddYears(-18)"
                 format="MM/dd/yyyy"
                 placeholder="Must be 18+">
</ejs-datepicker>
```

### Pattern 2: Appointment Booking Window

**Requirement:** 
- Not today
- No weekends
- Within 60 days
- Business hours (9 AM - 5 PM)

```csharp
public class AppointmentModel : PageModel
{
    [BindProperty]
    public DateTime AppointmentDate { get; set; }
    
    public DateTime MinAppointment { get; set; }
    public DateTime MaxAppointment { get; set; }
    
    public void OnGet()
    {
        // Next working day
        DateTime start = DateTime.Today.AddDays(1);
        if (start.DayOfWeek == DayOfWeek.Saturday)
            start = start.AddDays(2);
        else if (start.DayOfWeek == DayOfWeek.Sunday)
            start = start.AddDays(1);
        
        MinAppointment = start;
        MaxAppointment = DateTime.Today.AddDays(60);
    }
    
    public void OnPost()
    {
        // Validate not weekend
        if (AppointmentDate.DayOfWeek == DayOfWeek.Saturday ||
            AppointmentDate.DayOfWeek == DayOfWeek.Sunday)
        {
            ModelState.AddModelError("AppointmentDate", 
                "Appointments only on weekdays");
            return;
        }
        
        // Validate range
        if (AppointmentDate < MinAppointment || 
            AppointmentDate > MaxAppointment)
        {
            ModelState.AddModelError("AppointmentDate", 
                "Outside available window");
            return;
        }
    }
}
```

```html
<ejs-datepicker asp-for="AppointmentDate"
                 min="@Model.MinAppointment"
                 max="@Model.MaxAppointment"
                 format="yyyy-MM-dd"
                 placeholder="Choose business day">
</ejs-datepicker>
```

### Pattern 3: Contract Date Range

**Requirement:** End date must be after start date

```csharp
public class ContractModel : PageModel
{
    [BindProperty]
    public DateTime StartDate { get; set; }
    
    [BindProperty]
    public DateTime EndDate { get; set; }
    
    public void OnPost()
    {
        if (EndDate <= StartDate)
        {
            ModelState.AddModelError("EndDate", 
                "End date must be after start date");
            return;
        }
        
        TimeSpan duration = EndDate - StartDate;
        if (duration.TotalDays > 365)
        {
            ModelState.AddModelError("EndDate", 
                "Contract cannot exceed 365 days");
            return;
        }
    }
}
```

```html
<!-- Start date picker -->
<ejs-datepicker asp-for="StartDate"
                 min="@DateTime.Today"
                 format="yyyy-MM-dd">
</ejs-datepicker>

<!-- End date picker (dynamic min = start date) -->
<ejs-datepicker id="endDatePicker"
                 asp-for="EndDate"
                 format="yyyy-MM-dd">
</ejs-datepicker>

<script>
// Update end date min when start date changes
var startPicker = document.querySelector('[asp-for="StartDate"]').ej2_instances[0];
var endPicker = document.getElementById('endDatePicker').ej2_instances[0];

startPicker.change = function(args) {
    if (args.value) {
        var nextDay = new Date(args.value);
        nextDay.setDate(nextDay.getDate() + 1);
        endPicker.min = nextDay;
    }
};
</script>
```

## Edge Cases

### Case 1: Leap Year Boundaries

```csharp
// Check Feb 29 in leap year
DateTime leapDate = new DateTime(2024, 2, 29);  // Valid
DateTime invalidDate = new DateTime(2023, 2, 29);  // Throws exception

// Safe approach:
DateTime februaryDate = new DateTime(2023, 2, 1);
while (februaryDate.Month == 2)
{
    februaryDate = februaryDate.AddDays(1);
}
// Last valid date: 2023-02-28
```

### Case 2: Month Boundary Navigation

```html
<!-- If user selects Jan 31, then navigates to Feb -->
<!-- Auto-adjusts to Feb 28 (last valid day) -->
<ejs-datepicker id="boundaryTest"
                 value="@new DateTime(2026, 1, 31)"
                 format="yyyy-MM-dd">
</ejs-datepicker>
```

### Case 3: Timezone Considerations

```csharp
public class TimezoneModel : PageModel
{
    [BindProperty]
    public DateTime ScheduledDate { get; set; }
    
    public void OnPost()
    {
        // DatePicker provides local date (no timezone info)
        // Convert if necessary:
        DateTime utcDate = ScheduledDate.ToUniversalTime();
        TimeZoneInfo tz = TimeZoneInfo.FindSystemTimeZoneById("Eastern Standard Time");
        DateTime easternDate = TimeZoneInfo.ConvertTime(ScheduledDate, tz);
    }
}
```

---

**Previous:** [Date Formats and Parsing](../date-formats-and-parsing.md)  
**Next:** [Events and Interactions](../events-and-interactions.md)
