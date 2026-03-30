# Customization and Templates in ASP.NET Core Scheduler

## Table of Contents
- [Event Templates](#event-templates)
- [Cell Templates](#cell-templates)
- [Date Header Templates](#date-header-templates)
- [Tooltips](#tooltips)
- [Event Rendering](#event-rendering)
- [CSS Customization](#css-customization)

## Event Templates

### Basic Event Template

Customize appointment appearance using the `eventTemplate` property (defined as a template string):

```csharp
@{
    var eventTemplate = "<div class='event-template'><div class='event-title'>${Subject}</div><div class='event-location'>${Location}</div></div>";
}
```

```cshtml
<ejs-schedule id="schedule" width="100%" height="550" eventTemplate="@eventTemplate">
    <e-schedule-eventsettings dataSource="ViewBag.datasource">
    </e-schedule-eventsettings>
</ejs-schedule>

<style>
.event-template {
    padding: 5px;
    font-size: 12px;
}

.event-title {
    font-weight: bold;
    color: #333;
    margin-bottom: 3px;
}

.event-location {
    color: #666;
    font-size: 11px;
    margin-bottom: 3px;
}

.event-time {
    color: #999;
    font-size: 10px;
}
</style>
```

**Note:** The `eventTemplate` property accepts a template string defined in the `@{ }` block, not nested `<e-template>` tags.

### Template with Conditional Formatting

```csharp
@{
    var eventTemplate = "<div class='event-template ${Priority}'><div class='event-icon'>${if(Priority === 'High')}<span class='icon-urgent'>!</span>${/if}</div><div class='event-content'><div class='event-subject'>${Subject}</div><div class='event-status'>${Status}</div></div></div>";
}
```

```cshtml
<ejs-schedule id="schedule" width="100%" height="550" eventTemplate="@eventTemplate">
    <e-schedule-eventsettings dataSource="ViewBag.datasource">
    </e-schedule-eventsettings>
</ejs-schedule>

<style>
.event-template.High {
    background-color: #ff6b6b;
    color: white;
}

.event-template.Medium {
    background-color: #ffd43b;
    color: black;
}

.event-template.Low {
    background-color: #51cf66;
    color: white;
}

.icon-urgent {
    font-weight: bold;
    font-size: 16px;
}
</style>
```

### View-Specific Templates

For view-specific event templates, use the `eventTemplate` property with conditional logic to detect the current view:

```csharp
@{
    var viewEventTemplate = "<div class='event-wrapper'><div class='event-title'>${Subject}</div><div class='event-location'>${Location}</div></div>";
}
```

```cshtml
<ejs-schedule id="schedule" width="100%" height="550" selectedDate="new DateTime(2024, 3, 28)" eventTemplate="@viewEventTemplate">
    <e-schedule-views>
        <e-schedule-view option="Week"></e-schedule-view>
        <e-schedule-view option="Month"></e-schedule-view>
        <e-schedule-view option="Day"></e-schedule-view>
    </e-schedule-views>
    <e-schedule-eventsettings dataSource="ViewBag.datasource">
    </e-schedule-eventsettings>
</ejs-schedule>

<style>
.event-wrapper {
    padding: 5px;
}

.event-title {
    font-weight: bold;
    margin-bottom: 2px;
}

.event-location {
    font-size: 11px;
    color: #666;
}
</style>
```

**Note:** View-specific logic can be added via `eventRendered` event to apply different styles per view.

## Cell Templates

### Customizing Work Cells

Customize individual time slots using the `cellTemplate` property:

```csharp
@{
    var cellTemplate = "${if(type === 'workCells')}<div class='cell-template'><div class='time-slot'>${#data.date.toLocaleTimeString()}</div></div>${/if}";
}
```

```cshtml
<ejs-schedule id="schedule" width="100%" height="550" cellTemplate="@cellTemplate">
    <e-schedule-views>
        <e-schedule-view option="Week"></e-schedule-view>
    </e-schedule-views>
    <e-schedule-eventsettings dataSource="ViewBag.datasource">
    </e-schedule-eventsettings>
</ejs-schedule>

<style>
.cell-template {
    background-color: #f5f5f5;
    padding: 5px;
    border-radius: 3px;
    min-height: 50px;
}

.time-slot {
    font-size: 12px;
    color: #666;
}
</style>
```

**Note:** The `cellTemplate` accepts a template string and checks the `type` parameter to differentiate cell types.

### Highlighting Specific Cells

Use the `renderCell` event to highlight specific cells based on conditions:

```cshtml
<ejs-schedule id="schedule" width="100%" height="550" renderCell="onRenderCell">
    <e-schedule-views>
        <e-schedule-view option="Day"></e-schedule-view>
    </e-schedule-views>
    <e-schedule-eventsettings dataSource="ViewBag.datasource">
    </e-schedule-eventsettings>
</ejs-schedule>

<style>
.day-cell {
    padding: 10px;
    background-color: white;
}

.day-cell.meeting-time {
    background-color: #e3f2fd;
    border-left: 3px solid #2196f3;
}
</style>

<script>
function onRenderCell(args) {
    // Check if this is a work cell
    if (args.elementType === 'workCells') {
        // Highlight specific hours as meeting time
        if (args.date && args.date.getHours() >= 10 && args.date.getHours() <= 12) {
            args.element.classList.add('meeting-time');
        }
    }
}
</script>
```

## Date Header Templates

### Custom Date Header Display

Use the `cellHeaderTemplate` property to customize date header appearance:

```csharp
@{
    var dateHeaderTemplate = "<div class='date-header-template'><div class='date-value'>${#data.date.getDate()}</div><div class='day-name'>${#data.date.toLocaleDateString('en-US', {weekday: 'short'})}</div></div>";
}
```

```cshtml
<ejs-schedule id="schedule" width="100%" height="550" cellHeaderTemplate="@dateHeaderTemplate">
    <e-schedule-views>
        <e-schedule-view option="Week"></e-schedule-view>
    </e-schedule-views>
    <e-schedule-eventsettings dataSource="ViewBag.datasource">
    </e-schedule-eventsettings>
</ejs-schedule>

<style>
.date-header-template {
    text-align: center;
    padding: 10px 5px;
}

.date-value {
    font-size: 18px;
    font-weight: bold;
    color: #333;
}

.day-name {
    font-size: 12px;
    color: #999;
    margin-top: 5px;
}
</style>
```

**Note:** The `cellHeaderTemplate` accepts a template string with access to the date through `#data.date`.

## Tooltips

### Enabling Tooltips

Display appointment information on hover:

```cshtml
<ejs-schedule id="schedule" width="100%" height="550">
    <e-schedule-eventsettings 
        dataSource="ViewBag.datasource"
        enableTooltip="true">
    </e-schedule-eventsettings>
</ejs-schedule>
```

### Custom Tooltip Template

Use the `tooltipTemplate` property to customize tooltips:

```csharp
@{
    var tooltipTemplate = "<div class='custom-tooltip'><div class='tooltip-title'>${Subject}</div><div class='tooltip-location'><strong>Location:</strong> ${Location}</div><div class='tooltip-time'><strong>Time:</strong> ${StartTime.toLocaleTimeString()} - ${EndTime.toLocaleTimeString()}</div><div class='tooltip-description'><strong>Description:</strong> ${Description}</div></div>";
}
```

```cshtml
<ejs-schedule id="schedule" width="100%" height="550" tooltipTemplate="@tooltipTemplate">
    <e-schedule-eventsettings 
        dataSource="ViewBag.datasource"
        enableTooltip="true">
    </e-schedule-eventsettings>
</ejs-schedule>

<style>
.custom-tooltip {
    background-color: #fff;
    border: 1px solid #ddd;
    border-radius: 4px;
    padding: 10px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    max-width: 250px;
}

.tooltip-title {
    font-weight: bold;
    color: #333;
    margin-bottom: 8px;
    font-size: 14px;
}

.tooltip-location,
.tooltip-time,
.tooltip-description {
    font-size: 12px;
    margin-bottom: 5px;
    color: #666;
}
</style>
```

**Note:** The `tooltipTemplate` property accepts a template string with event data accessible via field names.

### Preventing Tooltip on Specific Events

```cshtml
<ejs-schedule id="schedule" 
    width="100%" 
    height="550"
    tooltipOpen="onTooltipOpen">
    <e-schedule-eventsettings 
        dataSource="ViewBag.datasource"
        enableTooltip="true">
    </e-schedule-eventsettings>
</ejs-schedule>

<script>
function onTooltipOpen(args) {
    // Prevent tooltip for read-only or blocked events
    if (args.data && (args.data.IsReadonly || args.data.IsBlock)) {
        args.cancel = true;
    }
}
</script>
```

## Event Rendering

### eventRendered Event

Customize appointments before rendering:

```cshtml
<ejs-schedule id="schedule" 
    width="100%" 
    height="550"
    eventRendered="onEventRendered">
    <e-schedule-eventsettings dataSource="ViewBag.datasource"></e-schedule-eventsettings>
</ejs-schedule>

<script>
function onEventRendered(args) {
    // Color code by priority
    if (args.data.Priority === 'High') {
        args.element.style.backgroundColor = '#ff6b6b';
        args.element.style.color = 'white';
    } else if (args.data.Priority === 'Medium') {
        args.element.style.backgroundColor = '#ffd43b';
    } else if (args.data.Priority === 'Low') {
        args.element.style.backgroundColor = '#51cf66';
    }
    
    // Add custom class
    args.element.classList.add('priority-' + args.data.Priority);
}
</script>

<style>
.priority-High {
    font-weight: bold;
}

.priority-Medium {
    font-style: italic;
}
</style>
```

### Minimum Height for Short Events

```cshtml
<ejs-schedule id="schedule" 
    width="100%" 
    height="550"
    eventRendered="onEventRendered">
    <e-schedule-eventsettings dataSource="ViewBag.datasource"></e-schedule-eventsettings>
</ejs-schedule>

<script>
function onEventRendered(args) {
    var duration = (args.data.EndTime - args.data.StartTime) / (1000 * 60);  // minutes
    
    // Apply minimum height for short appointments
    if (duration < 30) {
        args.element.style.minHeight = '40px';
    }
}
</script>
```

### Differentiate Past Events

```cshtml
<ejs-schedule id="schedule" 
    width="100%" 
    height="550"
    eventRendered="onEventRendered">
    <e-schedule-eventsettings dataSource="ViewBag.datasource"></e-schedule-eventsettings>
</ejs-schedule>

<script>
function onEventRendered(args) {
    var now = new Date();
    var eventEnd = new Date(args.data.EndTime);
    
    // Gray out past events
    if (eventEnd < now) {
        args.element.style.opacity = '0.5';
        args.element.style.backgroundColor = '#e0e0e0';
        args.element.style.color = '#999';
    }
}
</script>
```

## CSS Customization

### Styling with CSS Classes

```cshtml
<ejs-schedule id="schedule" 
    width="100%" 
    height="550"
    cssClass="custom-schedule">
    <e-schedule-eventsettings dataSource="ViewBag.datasource"></e-schedule-eventsettings>
</ejs-schedule>

<style>
/* Override default colors */
.custom-schedule .e-appointment {
    background-color: #4CAF50;
    border-left: 4px solid #2E7D32;
}

.custom-schedule .e-appointment:hover {
    background-color: #45a049;
    box-shadow: 0 2px 8px rgba(0,0,0,0.2);
}

/* Customize header */
.custom-schedule .e-schedule .e-header {
    background-color: #1976d2;
    color: white;
}

/* Customize time cells */
.custom-schedule .e-timecells {
    background-color: #f9f9f9;
}

/* Customize weekend cells */
.custom-schedule .e-weekend {
    background-color: #f5f5f5;
}

/* All-day row */
.custom-schedule .e-all-day-cells {
    background-color: #e3f2fd;
}
</style>
```

### Dynamic Theming

```cshtml
<select id="themeSelector" onchange="changeTheme(this.value)">
    <option value="">Default</option>
    <option value="dark">Dark</option>
    <option value="light">Light</option>
    <option value="high-contrast">High Contrast</option>
</select>

<ejs-schedule id="schedule" width="100%" height="550" cssClass="theme-default">
    <e-schedule-eventsettings dataSource="ViewBag.datasource"></e-schedule-eventsettings>
</ejs-schedule>

<script>
function changeTheme(theme) {
    var schedule = document.getElementById('schedule');
    
    // Remove all theme classes
    schedule.classList.remove('theme-default', 'theme-dark', 'theme-light', 'theme-high-contrast');
    
    // Add new theme class
    if (theme) {
        schedule.classList.add('theme-' + theme);
    } else {
        schedule.classList.add('theme-default');
    }
}
</script>

<style>
/* Dark theme */
.theme-dark .e-appointment {
    background-color: #333;
    color: #fff;
}

.theme-dark .e-schedule {
    background-color: #1e1e1e;
    color: #fff;
}

/* Light theme */
.theme-light .e-appointment {
    background-color: #fff;
    color: #333;
    border: 1px solid #ddd;
}

/* High contrast */
.theme-high-contrast .e-appointment {
    background-color: #000;
    color: #fff;
    font-weight: bold;
}

.theme-high-contrast .e-schedule {
    background-color: #fff;
    color: #000;
}
</style>
```

### Overlapping Event Prevention

```cshtml
<ejs-schedule id="schedule" 
    width="100%" 
    height="550"
    allowOverlap="false">
    <e-schedule-eventsettings dataSource="ViewBag.datasource"></e-schedule-eventsettings>
</ejs-schedule>
```

When overlapping events are detected:
- Initial load prioritizes longer durations
- New/updated overlaps show alert
- Use `actionBegin` event for custom validation

```cshtml
<ejs-schedule id="schedule" 
    width="100%" 
    height="550"
    allowOverlap="false"
    actionBegin="onActionBegin">
    <e-schedule-eventsettings dataSource="ViewBag.datasource"></e-schedule-eventsettings>
</ejs-schedule>

<script>
function onActionBegin(args) {
    if ((args.requestType === 'eventCreate' || args.requestType === 'eventChange') 
        && args.data) {
        var scheduleObj = document.getElementById('schedule').ej2_instances[0];
        
        // Check for overlaps in entire date range
        var isAvailable = scheduleObj.isSlotAvailable(
            args.data.StartTime, 
            args.data.EndTime
        );
        
        if (!isAvailable) {
            args.cancel = true;
            scheduleObj.openOverlapAlert();
        }
    }
}
</script>
```

## Complete Customization Example

```csharp
@{
    var eventTemplate = "<div class='event-card'><div class='event-priority ${Priority}'></div><div class='event-body'><div class='event-title'>${Subject}</div><div class='event-meta'>${StartTime.toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'})} - ${EndTime.toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'})}</div></div></div>";
    var tooltipTemplate = "<div class='event-tooltip'><strong>${Subject}</strong><br/>Location: ${Location}<br/>Status: ${Status}</div>";
}
```

```cshtml
<ejs-schedule id="schedule" 
    width="100%" 
    height="550"
    currentView="Week"
    selectedDate="new DateTime(2024, 3, 28)"
    cssClass="custom-company-schedule"
    eventTemplate="@eventTemplate"
    tooltipTemplate="@tooltipTemplate"
    eventRendered="onEventRendered"
    tooltipOpen="onTooltipOpen">
    
    <e-schedule-views>
        <e-schedule-view option="Week"></e-schedule-view>
        <e-schedule-view option="Day"></e-schedule-view>
        <e-schedule-view option="Month"></e-schedule-view>
    </e-schedule-views>
    
    <e-schedule-eventsettings 
        dataSource="ViewBag.datasource"
        enableTooltip="true">
    </e-schedule-eventsettings>
</ejs-schedule>

<script>
function onEventRendered(args) {
    if (args.data.IsReadonly) {
        args.element.classList.add('read-only-event');
    }
}

function onTooltipOpen(args) {
    if (args.data && args.data.IsBlock) {
        args.cancel = true;
    }
}
</script>

<style>
.custom-company-schedule .e-appointment {
    border-radius: 4px;
    box-shadow: 0 1px 3px rgba(0,0,0,0.1);
}

.event-card {
    display: flex;
    padding: 5px;
    height: 100%;
}

.event-priority {
    width: 3px;
    background-color: #ccc;
    margin-right: 5px;
}

.event-priority.High {
    background-color: #ff6b6b;
}

.event-priority.Medium {
    background-color: #ffd43b;
}

.event-body {
    flex: 1;
}

.event-title {
    font-weight: 500;
    font-size: 13px;
    margin-bottom: 2px;
}

.event-meta {
    font-size: 11px;
    color: #666;
}

.read-only-event {
    opacity: 0.7;
    pointer-events: none;
}

.event-tooltip {
    padding: 10px;
    font-size: 12px;
}
</style>
```

## Best Practices

1. **Keep templates lean** - Complex templates impact performance
2. **Use CSS classes** - Easier to maintain than inline styles
3. **Test responsiveness** - Ensure customizations work on mobile
4. **Validate data** - Check for null/undefined in templates
5. **Cache templates** - Pre-compile for better performance
