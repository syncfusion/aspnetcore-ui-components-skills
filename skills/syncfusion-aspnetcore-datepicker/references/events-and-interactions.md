# Events and Interactions

## Table of Contents
- [Event Types](#event-types)
- [Created Event](#created-event)
- [Change Event](#change-event)
- [Blur and Focus Events](#blur-and-focus-events)
- [Popup Events](#popup-events)
- [Event Handler Parameters](#event-handler-parameters)
- [Advanced Event Patterns](#advanced-event-patterns)

## Event Types

DatePicker supports these events:

| Event | Triggered | Use Case |
|-------|-----------|----------|
| **Created** | After component initialization | Set default values, initialize related controls |
| **Change** | When user selects a date | Validate selection, update dependent fields |
| **Blur** | When input loses focus | Final validation, formatting |
| **Focus** | When input gains focus | Show hints, prepare validation |
| **Open** | Before popup calendar opens | Customize calendar display |
| **Close** | After popup calendar closes | Cleanup, final processing |

## Created Event

Fired after DatePicker is fully initialized:

```html
<!-- Tag Helper with created event -->
<ejs-datepicker id="createdExample"
                 created="onDatePickerCreated"
                 placeholder="Select date">
</ejs-datepicker>

<script>
function onDatePickerCreated(args) {
    console.log('DatePicker created successfully');
    console.log('Current value:', args.value);
    
    // Set default value
    this.value = new Date(2026, 2, 19);
}
</script>
```

### Practical Use: Initialize Related Fields

```csharp
public class BookingModel : PageModel
{
    [BindProperty]
    public DateTime BookingDate { get; set; }
    
    [BindProperty]
    public string BookingType { get; set; }
}
```

```html
<form method="post">
    <label>Booking Date:</label>
    <ejs-datepicker asp-for="BookingDate"
                     created="onDatePickerCreated"
                     placeholder="Select date">
    </ejs-datepicker>
    
    <label>Booking Type:</label>
    <select asp-for="BookingType">
        <option value="">Select type</option>
        <option value="Standard">Standard</option>
        <option value="Express">Express</option>
    </select>
    
    <button type="submit">Book</button>
</form>

<script>
function onDatePickerCreated(args) {
    // Set today as default when component loads
    if (!args.value) {
        this.value = new Date();
        document.getElementById('bookingType').value = 'Standard';
    }
}
</script>
```

## Change Event

Fired when user selects a date:

```html
<ejs-datepicker id="changeExample"
                 change="onDateChange"
                 placeholder="Select date">
</ejs-datepicker>

<div id="selectedDateDisplay"></div>

<script>
function onDateChange(args) {
    if (args.value) {
        var selectedDate = args.value;
        var formattedDate = selectedDate.toLocaleDateString('en-US');
        document.getElementById('selectedDateDisplay').textContent = 
            'Selected: ' + formattedDate;
    }
}
</script>
```

### Practical Use: Update Related Field

**Scenario:** When user selects appointment date, update available times

```csharp
public class AppointmentModel : PageModel
{
    [BindProperty]
    public DateTime AppointmentDate { get; set; }
    
    [BindProperty]
    public string AppointmentTime { get; set; }
    
    public List<string> AvailableSlots { get; set; }
}
```

```html
<form method="post">
    <label>Date:</label>
    <ejs-datepicker asp-for="AppointmentDate"
                     change="onDateChanged"
                     format="yyyy-MM-dd">
    </ejs-datepicker>
    
    <label>Time Slot:</label>
    <select id="timeSlots" asp-for="AppointmentTime">
        <option value="">Select time</option>
    </select>
    
    <button type="submit">Book</button>
</form>

<script>
var availableSlots = {
    'Monday': ['09:00', '10:00', '11:00', '14:00', '15:00'],
    'Tuesday': ['09:00', '10:00', '11:00', '14:00', '15:00'],
    'Wednesday': ['09:00', '10:00', '14:00', '15:00'],
    'Thursday': ['10:00', '11:00', '14:00', '15:00', '16:00'],
    'Friday': ['09:00', '10:00', '11:00']
};

function onDateChanged(args) {
    if (args.value) {
        var selectedDate = args.value;
        var dayName = selectedDate.toLocaleDateString('en-US', { weekday: 'long' });
        var slots = availableSlots[dayName] || ['No availability'];
        
        var timeSelect = document.getElementById('timeSlots');
        timeSelect.innerHTML = '<option value="">Select time</option>';
        
        slots.forEach(slot => {
            var option = document.createElement('option');
            option.value = slot;
            option.textContent = slot;
            timeSelect.appendChild(option);
        });
    }
}
</script>
```

### Practical Use: Date Validation in Change Event

```html
<ejs-datepicker id="validatedPicker"
                 change="onDateValidate"
                 format="yyyy-MM-dd">
</ejs-datepicker>
<span id="validationMessage" class="text-danger"></span>

<script>
function onDateValidate(args) {
    var msg = document.getElementById('validationMessage');
    
    if (args.value) {
        var selected = args.value;
        var today = new Date();
        today.setHours(0, 0, 0, 0);
        
        if (selected < today) {
            msg.textContent = 'Date cannot be in the past';
            this.value = null;
        } else if (selected.getDay() === 0 || selected.getDay() === 6) {
            msg.textContent = 'Weekends not available';
            this.value = null;
        } else {
            msg.textContent = '';
        }
    }
}
</script>
```

## Blur and Focus Events

### Focus Event

Fired when input gains focus:

```html
<ejs-datepicker id="focusExample"
                 focus="onDateFocus"
                 placeholder="Click to see hint">
</ejs-datepicker>

<script>
function onDateFocus(args) {
    console.log('DatePicker focused');
    // Show helper text, highlight, etc.
}
</script>
```

### Blur Event

Fired when input loses focus:

```html
<ejs-datepicker id="blurExample"
                 blur="onDateBlur"
                 placeholder="Select date">
</ejs-datepicker>

<script>
function onDateBlur(args) {
    console.log('DatePicker blurred');
    console.log('Current value:', args.value);
    
    // Final validation, formatting
    if (args.value) {
        console.log('Date selected:', args.value.toLocaleDateString());
    } else {
        console.log('No date selected');
    }
}
</script>
```

### Practical Use: Multi-Field Validation

```html
<form>
    <label>Start Date:</label>
    <ejs-datepicker id="startDate"
                     blur="onStartDateBlur"
                     format="yyyy-MM-dd">
    </ejs-datepicker>
    
    <label>End Date:</label>
    <ejs-datepicker id="endDate"
                     blur="onEndDateBlur"
                     format="yyyy-MM-dd">
    </ejs-datepicker>
    
    <div id="rangeError" class="text-danger"></div>
</form>

<script>
function onStartDateBlur(args) {
    validateDateRange();
}

function onEndDateBlur(args) {
    validateDateRange();
}

function validateDateRange() {
    var startPicker = document.getElementById('startDate').ej2_instances[0];
    var endPicker = document.getElementById('endDate').ej2_instances[0];
    var errorDiv = document.getElementById('rangeError');
    
    if (startPicker.value && endPicker.value) {
        if (endPicker.value <= startPicker.value) {
            errorDiv.textContent = 'End date must be after start date';
        } else {
            errorDiv.textContent = '';
        }
    }
}
</script>
```

## Popup Events

### Open Event

Fired before calendar popup opens:

```html
<ejs-datepicker id="openExample"
                 open="onPopupOpen"
                 placeholder="Select date">
</ejs-datepicker>

<script>
function onPopupOpen(args) {
    console.log('Calendar popup about to open');
    // Customize calendar before display
}
</script>
```

### Close Event

Fired after calendar popup closes:

```html
<ejs-datepicker id="closeExample"
                 close="onPopupClose"
                 placeholder="Select date">
</ejs-datepicker>

<script>
function onPopupClose(args) {
    console.log('Calendar popup closed');
    console.log('Selected date:', args.value);
    
    // Cleanup, finalize selection
    if (args.value) {
        console.log('Date was selected');
    } else {
        console.log('Popup closed without selection');
    }
}
</script>
```

### Preventing Popup Close

Stop popup from closing after selection:

```html
<ejs-datepicker id="stickyPopup"
                 close="onPreventClose"
                 placeholder="Select date">
</ejs-datepicker>

<script>
var allowClose = false;

function onPreventClose(args) {
    if (!allowClose && args.value) {
        // Prevent default close behavior
        args.preventDefault();
        
        console.log('Popup stays open after selection');
        
        // Allow close on second interaction
        setTimeout(() => {
            allowClose = true;
        }, 1000);
    }
}
</script>
```

## Event Handler Parameters

### Change Event Args

```typescript
interface DatePickerEventArgs {
    value: Date;              // Selected date
    isInteracted: boolean;    // User-initiated vs programmatic
    element: HTMLElement;     // DatePicker element
    event?: Event;            // Original event
    isDate: boolean;          // Is valid date
    type: string;             // Event type ('Change', etc.)
}
```

**Usage:**

```html
<ejs-datepicker id="argsExample"
                 change="onChangeArgs"
                 placeholder="Select date">
</ejs-datepicker>

<script>
function onChangeArgs(args) {
    console.log('Date:', args.value);
    console.log('Is user interaction:', args.isInteracted);
    console.log('Is valid date:', args.isDate);
    console.log('Event type:', args.type);
    
    if (args.isInteracted && args.isDate) {
        console.log('User selected a valid date');
    }
}
</script>
```

## Advanced Event Patterns

### Pattern 1: Dependent DatePickers

**Scenario:** End date must be after start date

```html
<form>
    <label>Project Start Date:</label>
    <ejs-datepicker id="startDatePicker"
                     change="onStartDateChange"
                     format="yyyy-MM-dd">
    </ejs-datepicker>
    
    <label>Project End Date:</label>
    <ejs-datepicker id="endDatePicker"
                     format="yyyy-MM-dd">
    </ejs-datepicker>
</form>

<script>
function onStartDateChange(args) {
    if (args.value && args.isInteracted) {
        var endPicker = document.getElementById('endDatePicker').ej2_instances[0];
        
        // Set min for end date to start date + 1 day
        var minDate = new Date(args.value);
        minDate.setDate(minDate.getDate() + 1);
        
        endPicker.min = minDate;
        
        // Clear end date if it's before new min
        if (endPicker.value && endPicker.value < minDate) {
            endPicker.value = null;
        }
    }
}
</script>
```

### Pattern 2: Conditional Event Handling

```html
<ejs-datepicker id="conditionExample"
                 change="onConditionalChange"
                 placeholder="Select date">
</ejs-datepicker>
<div id="feedback"></div>

<script>
function onConditionalChange(args) {
    var feedback = document.getElementById('feedback');
    var selected = args.value;
    
    if (!selected) {
        feedback.textContent = '';
        return;
    }
    
    var today = new Date();
    today.setHours(0, 0, 0, 0);
    
    if (selected.getTime() === today.getTime()) {
        feedback.textContent = '✓ Today';
        feedback.className = 'text-info';
    } else if (selected < today) {
        feedback.textContent = '✗ Past date not allowed';
        feedback.className = 'text-danger';
        this.value = null;
    } else {
        var daysAhead = Math.ceil((selected - today) / (1000 * 60 * 60 * 24));
        feedback.textContent = '✓ In ' + daysAhead + ' days';
        feedback.className = 'text-success';
    }
}
</script>
```

### Pattern 3: Real-Time Validation with Change Event

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
            ModelState.AddModelError("EndDate", "Must be after start date");
    }
}
```

```html
<form method="post">
    <label>Contract Start:</label>
    <ejs-datepicker asp-for="StartDate"
                     change="onContractDateChange"
                     format="yyyy-MM-dd">
    </ejs-datepicker>
    
    <label>Contract End:</label>
    <ejs-datepicker id="endDatePicker"
                     asp-for="EndDate"
                     change="onContractDateChange"
                     format="yyyy-MM-dd">
    </ejs-datepicker>
    
    <div id="durationInfo"></div>
    <button type="submit">Save</button>
</form>

<script>
function onContractDateChange(args) {
    var startPicker = document.querySelector('[asp-for="StartDate"]').ej2_instances[0];
    var endPicker = document.getElementById('endDatePicker').ej2_instances[0];
    var infoDiv = document.getElementById('durationInfo');
    
    if (startPicker.value && endPicker.value) {
        if (endPicker.value <= startPicker.value) {
            infoDiv.textContent = '⚠️ End date must be after start date';
            infoDiv.className = 'text-warning';
        } else {
            var days = Math.ceil(
                (endPicker.value - startPicker.value) / (1000 * 60 * 60 * 24)
            );
            infoDiv.textContent = '✓ Duration: ' + days + ' days';
            infoDiv.className = 'text-success';
        }
    } else {
        infoDiv.textContent = '';
    }
}
</script>
```

---

**Previous:** [Date Ranges and Validation](../date-ranges-and-validation.md)  
**Next:** [Globalization and Localization](../globalization-localization.md)
