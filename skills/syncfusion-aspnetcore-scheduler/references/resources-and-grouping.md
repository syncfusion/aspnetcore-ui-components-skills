# Resources and Grouping in ASP.NET Core Scheduler

## Table of Contents
- [Single Resource Scheduling](#single-resource-scheduling)
- [Multiple Resources](#multiple-resources)
- [Resource Grouping](#resource-grouping)
- [Custom Resource Fields](#custom-resource-fields)
- [Resource Data Binding](#resource-data-binding)
- [Timeline Views with Resources](#timeline-views-with-resources)

## Single Resource Scheduling

### Basic Resource Setup

Assign appointments to individual resources (users, rooms, equipment):

```csharp
public class ResourceData {
    public int Id { get; set; }
    public string Text { get; set; }
    public string Color { get; set; }
}

public class ScheduleEventData {
    public int Id { get; set; }
    public string Subject { get; set; }
    public DateTime StartTime { get; set; }
    public DateTime EndTime { get; set; }
    public int ResourceId { get; set; }  // Link to resource
}
```

Configure in view:

```cshtml
<ejs-schedule id="schedule" width="100%" height="550">
    <e-schedule-group resources="new string[] { 'Rooms' }"></e-schedule-group>
    
    <e-schedule-resources>
        <e-schedule-resource field="ResourceId" 
            title="Room" 
            name="Rooms"
            dataSource="ViewBag.resourceData"
            idField="Id"
            textField="Text"
            colorField="Color">
        </e-schedule-resource>
    </e-schedule-resources>
    
    <e-schedule-eventsettings dataSource="ViewBag.datasource">
    </e-schedule-eventsettings>
</ejs-schedule>
```

Controller implementation:

```csharp
public IActionResult Index() {
    ViewBag.resourceData = new List<ResourceData> {
        new ResourceData { Id = 1, Text = "Meeting Room A", Color = "#FF6B6B" },
        new ResourceData { Id = 2, Text = "Meeting Room B", Color = "#4ECDC4" },
        new ResourceData { Id = 3, Text = "Conference Hall", Color = "#FFE66D" }
    };
    
    ViewBag.datasource = new List<ScheduleEventData> {
        new ScheduleEventData {
            Id = 1,
            Subject = "Team Standup",
            StartTime = new DateTime(2024, 3, 28, 10, 0, 0),
            EndTime = new DateTime(2024, 3, 28, 10, 30, 0),
            ResourceId = 1
        },
        new ScheduleEventData {
            Id = 2,
            Subject = "Board Meeting",
            StartTime = new DateTime(2024, 3, 28, 14, 0, 0),
            EndTime = new DateTime(2024, 3, 28, 16, 0, 0),
            ResourceId = 3
        }
    };
    
    return View();
}
```

## Multiple Resources

### Multi-Level Grouping

Group appointments by multiple resource types:

```cshtml
<ejs-schedule id="schedule" width="100%" height="550">
    <!-- Group by Department then by Employee -->
    <e-schedule-group resources="new string[] { 'Departments', 'Employees' }"></e-schedule-group>
    
    <e-schedule-resources>
        <!-- First grouping level -->
        <e-schedule-resource field="DepartmentId" 
            title="Department" 
            name="Departments"
            dataSource="ViewBag.departmentData"
            idField="Id"
            textField="Text">
        </e-schedule-resource>
        
        <!-- Second grouping level -->
        <e-schedule-resource field="EmployeeId" 
            title="Employee" 
            name="Employees"
            dataSource="ViewBag.employeeData"
            idField="Id"
            textField="Text"
            groupIDField="DepartmentId"
            colorField="Color">
        </e-schedule-resource>
    </e-schedule-resources>
    
    <e-schedule-eventsettings dataSource="ViewBag.datasource">
    </e-schedule-eventsettings>
</ejs-schedule>
```

Controller:

```csharp
public IActionResult Index() {
    // Department resources
    ViewBag.departmentData = new List<ResourceData> {
        new ResourceData { Id = 1, Text = "Sales" },
        new ResourceData { Id = 2, Text = "Engineering" },
        new ResourceData { Id = 3, Text = "Marketing" }
    };
    
    // Employee resources grouped by department
    ViewBag.employeeData = new List<object> {
        // Sales department
        new { Id = 101, Text = "John Smith", DepartmentId = 1, Color = "#FF6B6B" },
        new { Id = 102, Text = "Sarah Johnson", DepartmentId = 1, Color = "#FF8C42" },
        
        // Engineering department
        new { Id = 201, Text = "Mike Davis", DepartmentId = 2, Color = "#4ECDC4" },
        new { Id = 202, Text = "Emily Brown", DepartmentId = 2, Color = "#45B7D1" },
        
        // Marketing department
        new { Id = 301, Text = "Lisa Wilson", DepartmentId = 3, Color = "#FFE66D" },
        new { Id = 302, Text = "David Lee", DepartmentId = 3, Color = "#95E1D3" }
    };
    
    ViewBag.datasource = new List<ScheduleEventData> {
        new ScheduleEventData {
            Id = 1,
            Subject = "Client Meeting",
            StartTime = new DateTime(2024, 3, 28, 10, 0, 0),
            EndTime = new DateTime(2024, 3, 28, 11, 0, 0),
            EmployeeId = 101,  // John Smith
            DepartmentId = 1   // Sales
        }
    };
    
    return View();
}
```

## Resource Grouping

### Allow Multiple Resource Selection

Enable users to select multiple resources per appointment:

```cshtml
<ejs-schedule id="schedule" width="100%" height="550">
    <e-schedule-group resources="new string[] { 'Employees' }"></e-schedule-group>
    
    <e-schedule-resources>
        <e-schedule-resource field="EmployeeIds" 
            title="Employees" 
            name="Employees"
            allowMultiple="true"
            dataSource="ViewBag.employeeData"
            idField="Id"
            textField="Text"
            colorField="Color">
        </e-schedule-resource>
    </e-schedule-resources>
    
    <e-schedule-eventsettings dataSource="ViewBag.datasource">
    </e-schedule-eventsettings>
</ejs-schedule>
```

Data model with multiple resources:

```csharp
public class ScheduleEventData {
    public int Id { get; set; }
    public string Subject { get; set; }
    public DateTime StartTime { get; set; }
    public DateTime EndTime { get; set; }
    public int[] EmployeeIds { get; set; }  // Array of employee IDs
}
```

Example data:

```csharp
ViewBag.datasource = new List<ScheduleEventData> {
    new ScheduleEventData {
        Id = 1,
        Subject = "Project Planning",
        StartTime = new DateTime(2024, 3, 28, 10, 0, 0),
        EndTime = new DateTime(2024, 3, 28, 12, 0, 0),
        EmployeeIds = new int[] { 101, 102, 103 }  // Multiple employees
    }
};
```

## Custom Resource Fields

### Define Resource-Specific Working Hours

```csharp
public class ResourceData {
    public int Id { get; set; }
    public string Text { get; set; }
    public string StartHour { get; set; }    // e.g., "09:00"
    public string EndHour { get; set; }      // e.g., "17:00"
    public int[] WorkDays { get; set; }      // e.g., new int[] {1,2,3,4,5}
    public string Color { get; set; }
}
```

Configure in view:

```cshtml
<ejs-schedule id="schedule" width="100%" height="550">
    <e-schedule-group resources="new string[] { 'Employees' }"></e-schedule-group>
    
    <e-schedule-resources>
        <e-schedule-resource field="EmployeeId" 
            title="Employee" 
            name="Employees"
            dataSource="ViewBag.employeeData"
            idField="Id"
            textField="Text"
            startHourField="StartHour"
            endHourField="EndHour"
            workDaysField="WorkDays"
            colorField="Color">
        </e-schedule-resource>
    </e-schedule-resources>
    
    <e-schedule-eventsettings dataSource="ViewBag.datasource">
    </e-schedule-eventsettings>
</ejs-schedule>
```

Controller:

```csharp
ViewBag.employeeData = new List<object> {
    new {
        Id = 101,
        Text = "John Smith",
        StartHour = "09:00",
        EndHour = "17:00",
        WorkDays = new int[] {1, 2, 3, 4, 5},  // Monday-Friday
        Color = "#FF6B6B"
    },
    new {
        Id = 102,
        Text = "Sarah Johnson",
        StartHour = "10:00",
        EndHour = "18:00",
        WorkDays = new int[] {1, 2, 3, 4, 5},  // Different hours
        Color = "#FF8C42"
    },
    new {
        Id = 103,
        Text = "Mike Davis",
        StartHour = "08:00",
        EndHour = "16:00",
        WorkDays = new int[] {1, 2, 3, 4},     // Monday-Thursday only
        Color = "#4ECDC4"
    }
};
```

### Resource-Specific Colors

Apply color coding to distinguish resources:

```cshtml
<ejs-schedule id="schedule" width="100%" height="550" eventRendered="onEventRendered">
    <e-schedule-group resources="new string[] { 'Employees' }"></e-schedule-group>
    
    <e-schedule-resources>
        <e-schedule-resource field="EmployeeId" 
            title="Employee" 
            name="Employees"
            dataSource="ViewBag.employeeData"
            idField="Id"
            textField="Text"
            colorField="Color">
        </e-schedule-resource>
    </e-schedule-resources>
    
    <e-schedule-eventsettings dataSource="ViewBag.datasource">
    </e-schedule-eventsettings>
</ejs-schedule>

<script>
function onEventRendered(args) {
    // Resource color is automatically applied
    // Custom styling can be added here
    args.element.style.fontWeight = 'bold';
}
</script>
```

## Resource Data Binding

### Local Resource Data

Bind resources from in-memory collection:

```cshtml
<ejs-schedule id="schedule" width="100%" height="550">
    <e-schedule-group resources="new string[] { 'Rooms' }"></e-schedule-group>
    
    <e-schedule-resources>
        <e-schedule-resource field="RoomId" 
            title="Room" 
            name="Rooms"
            dataSource="ViewBag.resourceData"
            idField="Id"
            textField="Text">
        </e-schedule-resource>
    </e-schedule-resources>
    
    <e-schedule-eventsettings dataSource="ViewBag.datasource">
    </e-schedule-eventsettings>
</ejs-schedule>
```

### Remote Resource Data

Load resources from server endpoint:

```cshtml
<ejs-schedule id="schedule" width="100%" height="550">
    <e-schedule-group resources="new string[] { 'Employees' }"></e-schedule-group>
    
    <e-schedule-resources>
        <e-schedule-resource field="EmployeeId" 
            title="Employee" 
            name="Employees"
            dataSource="new DataManager { Url = '/api/resources/employees', Adaptor = typeof(UrlAdaptor) }"
            idField="Id"
            textField="Text"
            colorField="Color">
        </e-schedule-resource>
    </e-schedule-resources>
    
    <e-schedule-eventsettings dataSource="ViewBag.datasource">
    </e-schedule-eventsettings>
</ejs-schedule>
```

Controller endpoint:

```csharp
[ApiController]
[Route("api/resources")]
public class ResourcesController : ControllerBase {
    [HttpGet("employees")]
    public IActionResult GetEmployees() {
        var employees = new List<object> {
            new { Id = 101, Text = "John Smith", Color = "#FF6B6B" },
            new { Id = 102, Text = "Sarah Johnson", Color = "#FF8C42" },
            new { Id = 103, Text = "Mike Davis", Color = "#4ECDC4" }
        };
        return Ok(employees);
    }
}
```

### OData Resource Data

Fetch resources from OData service:

```cshtml
<ejs-schedule id="schedule" width="100%" height="550">
    <e-schedule-group resources="new string[] { 'Employees' }"></e-schedule-group>
    
    <e-schedule-resources>
        <e-schedule-resource field="EmployeeId" 
            title="Employee" 
            name="Employees"
            dataSource="new DataManager { 
                Url = ':url', 
                Adaptor = typeof(ODataV4Adaptor) 
            }"
            idField="Id"
            textField="Name">
        </e-schedule-resource>
    </e-schedule-resources>
    
    <e-schedule-eventsettings dataSource="ViewBag.datasource">
    </e-schedule-eventsettings>
</ejs-schedule>
```

## Timeline Views with Resources

### Timeline Day with Resources

Display resources as columns in timeline view:

```cshtml
<ejs-schedule id="schedule" width="100%" height="550" currentView="TimelineDay">
    <e-schedule-group resources="new string[] { 'Rooms' }"></e-schedule-group>
    
    <e-schedule-views>
        <e-schedule-view option="TimelineDay"></e-schedule-view>
        <e-schedule-view option="TimelineWeek"></e-schedule-view>
        <e-schedule-view option="TimelineMonth"></e-schedule-view>
    </e-schedule-views>
    
    <e-schedule-resources>
        <e-schedule-resource field="RoomId" 
            title="Meeting Room" 
            name="Rooms"
            dataSource="ViewBag.resourceData"
            idField="Id"
            textField="Text"
            colorField="Color">
        </e-schedule-resource>
    </e-schedule-resources>
    
    <e-schedule-eventsettings dataSource="ViewBag.datasource">
    </e-schedule-eventsettings>
</ejs-schedule>
```

### Timeline Week with Grouped Resources

```cshtml
<ejs-schedule id="schedule" width="100%" height="550" currentView="TimelineWeek">
    <e-schedule-group resources="new string[] { 'Departments', 'Employees' }"></e-schedule-group>
    
    <e-schedule-views>
        <e-schedule-view option="TimelineWeek"></e-schedule-view>
    </e-schedule-views>
    
    <e-schedule-resources>
        <e-schedule-resource field="DepartmentId" 
            title="Department" 
            name="Departments"
            dataSource="ViewBag.departmentData"
            idField="Id"
            textField="Text">
        </e-schedule-resource>
        
        <e-schedule-resource field="EmployeeId" 
            title="Employee" 
            name="Employees"
            dataSource="ViewBag.employeeData"
            idField="Id"
            textField="Text"
            groupIDField="DepartmentId"
            colorField="Color">
        </e-schedule-resource>
    </e-schedule-resources>
    
    <e-schedule-eventsettings dataSource="ViewBag.datasource">
    </e-schedule-eventsettings>
</ejs-schedule>
```

### Resource Header Template

Customize resource column headers:

```cshtml
<ejs-schedule id="schedule" width="100%" height="550" currentView="TimelineDay">
    <e-schedule-group resources="new string[] { 'Employees' }"></e-schedule-group>
    
    <e-schedule-views>
        <e-schedule-view option="TimelineDay">
            <e-resource-header-template>
                <div class="resource-header">
                    <div class="header-icon">${ResourceData.Icon}</div>
                    <div class="header-text">
                        <div class="header-name">${ResourceData.Text}</div>
                        <div class="header-email">${ResourceData.Email}</div>
                    </div>
                </div>
            </e-resource-header-template>
        </e-schedule-view>
    </e-schedule-views>
    
    <e-schedule-resources>
        <e-schedule-resource field="EmployeeId" 
            title="Employee" 
            name="Employees"
            dataSource="ViewBag.employeeData"
            idField="Id"
            textField="Text">
        </e-schedule-resource>
    </e-schedule-resources>
    
    <e-schedule-eventsettings dataSource="ViewBag.datasource">
    </e-schedule-eventsettings>
</ejs-schedule>

<style>
.resource-header {
    display: flex;
    align-items: center;
    padding: 10px;
}

.header-icon {
    width: 40px;
    height: 40px;
    margin-right: 10px;
    border-radius: 50%;
    background-size: cover;
}

.header-text {
    flex: 1;
}

.header-name {
    font-weight: bold;
    font-size: 13px;
}

.header-email {
    font-size: 11px;
    color: #999;
}
</style>
```

## Complete Resource Scheduling Example

```cshtml
<ejs-schedule id="schedule" 
    width="100%" 
    height="550"
    currentView="TimelineWeek"
    selectedDate="new DateTime(2024, 3, 28)">
    
    <!-- Group by Department then Employee -->
    <e-schedule-group resources="new string[] { 'Departments', 'Employees' }"></e-schedule-group>
    
    <e-schedule-views>
        <e-schedule-view option="TimelineDay"></e-schedule-view>
        <e-schedule-view option="TimelineWeek"></e-schedule-view>
        <e-schedule-view option="TimelineMonth"></e-schedule-view>
    </e-schedule-views>
    
    <e-schedule-resources>
        <!-- Level 1: Departments -->
        <e-schedule-resource field="DepartmentId" 
            title="Department" 
            name="Departments"
            dataSource="ViewBag.departmentData"
            idField="Id"
            textField="Text">
        </e-schedule-resource>
        
        <!-- Level 2: Employees grouped by department -->
        <e-schedule-resource field="EmployeeId" 
            title="Employee" 
            name="Employees"
            dataSource="ViewBag.employeeData"
            idField="Id"
            textField="Text"
            groupIDField="DepartmentId"
            colorField="Color"
            startHourField="StartHour"
            endHourField="EndHour"
            workDaysField="WorkDays">
        </e-schedule-resource>
    </e-schedule-resources>
    
    <e-schedule-eventsettings 
        dataSource="ViewBag.datasource">
    </e-schedule-eventsettings>
</ejs-schedule>
```

## Best Practices

1. **Color Coding** - Use colors to distinguish resources
2. **Hierarchical Grouping** - Organize resources in logical hierarchy
3. **Resource Availability** - Set working hours per resource
4. **Load Performance** - Limit visible resources in calendar
5. **Multi-Select UI** - Make multiple selection intuitive
6. **Resource Search** - Implement search for large resource lists
