# Getting Started with ASP.NET Core Scheduler

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Setup](#setup)
- [Basic Implementation](#basic-implementation)
- [Populating Appointments](#populating-appointments)
- [Configuring Views](#configuring-views)

## Prerequisites

Before starting, ensure you have:
- Visual Studio 2019 or later
- ASP.NET Core 3.1 or higher
- Basic knowledge of ASP.NET Core and Razor Pages/MVC

## Installation

### Step 1: Create or Use Existing ASP.NET Core Application

You can create a new ASP.NET Core web application using:
- **Microsoft Templates:** Create a Razor Pages Project
- **Syncfusion Extension:** Use Syncfusion ASP.NET Core Extension for automatic setup

### Step 2: Install NuGet Package

Open the NuGet Package Manager Console and install the Syncfusion.EJ2.AspNet.Core package:

```powershell
Install-Package Syncfusion.EJ2.AspNet.Core -Version {{ site.releaseversion }}
```

Alternatively, use the NuGet Package Manager UI (Tools → NuGet Package Manager → Manage NuGet Packages for Solution).

**Dependencies:** This package includes:
- `Newtonsoft.Json` for JSON serialization
- `Syncfusion.Licensing` for license validation

## Setup

### Step 1: Add Tag Helper

Open `~/Pages/_ViewImports.cshtml` and add the Syncfusion EJ2 Tag Helper:

```csharp
@addTagHelper *, Syncfusion.EJ2
```

### Step 2: Add Stylesheet and Script Resources

In `~/Pages/Shared/_Layout.cshtml`, add the CDN references inside the `<head>` tag. When using CDNs, prefer HTTPS and Subresource Integrity (SRI) to prevent tampering:

```html
<head>
    <!-- Syncfusion ASP.NET Core controls styles (HTTPS + SRI example) -->
    <link rel="stylesheet" href="https://cdn.example.com/syncfusion/styles.min.css" integrity="sha384-REPLACE_WITH_ACTUAL_HASH" crossorigin="anonymous">

    <!-- Syncfusion ASP.NET Core controls scripts (HTTPS + SRI example) -->
    <script src="https://cdn.example.com/syncfusion/scripts.min.js" integrity="sha384-REPLACE_WITH_ACTUAL_HASH" crossorigin="anonymous"></script>

    <!-- Fallback: host files locally or use your package manager to avoid CDN exposure if needed -->
</head>

## Configure CORS (Server-side)

When your Scheduler requests data from a different origin, configure CORS on the server instead of relying on client-side flags. Example for ASP.NET Core:

```csharp
// In Program.cs or Startup.cs (ConfigureServices)
services.AddCors(options =>
{
    options.AddPolicy("AllowSpecificOrigin", builder =>
    {
        builder.WithOrigins("https://yourfrontend.example.com")
               .AllowAnyHeader()
               .AllowAnyMethod()
               .AllowCredentials();
    });
});

// In Configure (or app builder)
app.UseCors("AllowSpecificOrigin");
```

Avoid `AllowAnyOrigin` with credentials in production; prefer explicit origins.

## Privacy & Compliance

Add a short note about GDPR/HIPAA when scheduling or storing personal data: annotate which fields are PII, restrict retention, and document how users can request deletion.

## Multi-Tenancy & Data Isolation

If your Scheduler serves multiple customers or users, enforce data isolation at the API level. Recommendations:

- Identify tenant or user claims at authentication time and store them in issued tokens (e.g., `tenant` claim).
- Always scope database queries to `TenantId` or `OwnerUserId` to prevent cross-tenant access.
- Validate authorization at the data layer (e.g., repository methods) and not only at controller level.
- Provide role-based admin endpoints that can operate across tenants, guarded by `Authorize(Roles = "Admin")` and additional checks.
- Include tenant-aware audit logs and tenant-aware retention/erasure APIs for GDPR compliance.

Example: filter queries by tenant from claims before returning data (see `GetData` example in CRUD docs).

## CSRF Protection and Secure Cookies

For POST/PUT/DELETE endpoints, include anti-forgery protection and use secure cookies for authentication where possible. Example for ASP.NET Core MVC/Razor:

```csharp
// In a Razor layout head, emit the antiforgery token into a meta tag
@inject Microsoft.AspNetCore.Antiforgery.IAntiforgery antiforgery
<meta name="request-verification-token" content="@antiforgery.GetAndStoreTokens(HttpContext).RequestToken" />

// Controller: apply ValidateAntiForgeryToken on POST actions
[HttpPost]
[ValidateAntiForgeryToken]
public IActionResult UpdateData(EditParams model) { ... }
```

In AJAX calls, read the token from the meta tag and send it as a header (see data-binding examples). Prefer authentication via secure, HttpOnly cookies with `SameSite=Lax`/`Strict` rather than embedding bearer tokens in client-side code.
```

**Alternative Methods:**
- **NPM Package:** Use `@syncfusion/ej2-asp-core-mvc` package
- **Custom Resource Generator (CRG):** Generate custom theme and script bundles
- **Local Files:** Download and reference files locally

For more details, refer to Adding Script References and Themes.

## Content Security Policy (CSP)

Use a Content-Security-Policy header to reduce XSS risks in templates. Example (set in server configuration or middleware):

```csharp
// Example middleware to add CSP header
app.Use(async (context, next) =>
{
    context.Response.Headers.Add("Content-Security-Policy", "default-src 'self'; script-src 'self' https://cdn.example.com; style-src 'self' https://cdn.example.com; object-src 'none';");
    await next();
});
```

Adjust sources to your CDN and inline-script policies. Combine CSP with HTML encoding and strict template rendering to mitigate XSS.

### Step 3: Register Script Manager

In `~/Pages/Shared/_Layout.cshtml`, add the Syncfusion Script Manager at the end of the `<body>` tag:

```html
<body>
    <!-- Your content here -->
    
    <!-- Syncfusion ASP.NET Core Script Manager -->
    <ejs-scripts></ejs-scripts>
</body>
```

## Basic Implementation

### Adding the Scheduler Control

In your view file (e.g., `~/Pages/Index.cshtml`), add the Scheduler tag helper:

```cshtml
@page
@using Syncfusion.EJ2

<ejs-schedule id="schedule" width="100%" height="550">
</ejs-schedule>
```

**Result:** A blank Scheduler appears displaying the current week by default.

## Populating Appointments

### Using Local Data

Create an appointment data model:

```csharp
public class AppointmentData
{
    public int Id { get; set; }
    public string Subject { get; set; }
    public DateTime StartTime { get; set; }
    public DateTime EndTime { get; set; }
    public string Location { get; set; }
    public string Description { get; set; }
}
```

### Binding Data to Scheduler

In your controller or page handler:

```csharp
public IActionResult Index()
{
    var appointmentData = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Project Review",
            StartTime = new DateTime(2024, 3, 28, 10, 0, 0),
            EndTime = new DateTime(2024, 3, 28, 11, 0, 0),
            Location = "Conference Room A",
            Description = "Review Q1 project status"
        },
        new AppointmentData
        {
            Id = 2,
            Subject = "Team Standup",
            StartTime = new DateTime(2024, 3, 28, 14, 0, 0),
            EndTime = new DateTime(2024, 3, 28, 14, 30, 0),
            Location = "Virtual",
            Description = "Daily standup meeting"
        }
    };
    
    ViewBag.datasource = appointmentData;
    return View();
}
```

In your view, bind the data using the `dataSource` property:

```cshtml
<ejs-schedule id="schedule" width="100%" height="550" selectedDate="new DateTime(2024, 3, 28)">
    <e-schedule-eventsettings dataSource="ViewBag.datasource"></e-schedule-eventsettings>
</ejs-schedule>
```

**Result:** Appointments appear on the calendar on their scheduled dates and times.

## Setting Default Date

To display the Scheduler on a specific date instead of the system date, use the `selectedDate` property:

```cshtml
<ejs-schedule id="schedule" 
    width="100%" 
    height="550" 
    selectedDate="new DateTime(2024, 6, 15)">
    <e-schedule-eventsettings dataSource="ViewBag.datasource"></e-schedule-eventsettings>
</ejs-schedule>
```

## Configuring Views

### Available View Types

The Scheduler supports the following views:
- **Day** - Single day view
- **Week** - 7-day week view (default)
- **WorkWeek** - 5-day working week view
- **Month** - Monthly calendar view
- **Year** - Yearly calendar view
- **Agenda** - List of upcoming appointments
- **MonthAgenda** - Month with agenda list
- **TimelineDay** - Horizontal timeline for a single day
- **TimelineWeek** - Horizontal timeline for a week
- **TimelineWorkWeek** - Horizontal timeline for working week
- **TimelineMonth** - Horizontal timeline for a month
- **TimelineYear** - Horizontal timeline for a year

### Setting Default View

Use the `currentView` property to set the active view:

```cshtml
<ejs-schedule id="schedule" 
    width="100%" 
    height="550" 
    currentView="Month"
    selectedDate="new DateTime(2024, 3, 28)">
    <e-schedule-eventsettings dataSource="ViewBag.datasource"></e-schedule-eventsettings>
</ejs-schedule>
```

### Displaying Specific Views

Use the `e-schedule-views` collection to restrict which views are displayed:

```cshtml
<ejs-schedule id="schedule" 
    width="100%" 
    height="550" 
    currentView="Week"
    selectedDate="new DateTime(2024, 3, 28)">
    <e-schedule-views>
        <e-schedule-view option="Day"></e-schedule-view>
        <e-schedule-view option="Week"></e-schedule-view>
        <e-schedule-view option="Month"></e-schedule-view>
    </e-schedule-views>
    <e-schedule-eventsettings dataSource="ViewBag.datasource">
    </e-schedule-eventsettings>
</ejs-schedule>
```

### View-Specific Configuration

Different views can have different configurations. Configure each view individually:

```cshtml
<ejs-schedule id="schedule" 
    width="100%" 
    height="550" 
    selectedDate="new DateTime(2024, 3, 28)">
    <e-schedule-views>
        <e-schedule-view option="Week"></e-schedule-view>
        <e-schedule-view option="Month"></e-schedule-view>
    </e-schedule-views>
    <e-schedule-eventsettings dataSource="ViewBag.datasource">
    </e-schedule-eventsettings>
</ejs-schedule>
```

## Complete Example

### Controller

```csharp
public class ScheduleController : Controller
{
    public IActionResult Index()
    {
        var appointmentData = new List<AppointmentData>
        {
            new AppointmentData
            {
                Id = 1,
                Subject = "Team Standup",
                StartTime = new DateTime(2024, 3, 28, 9, 0, 0),
                EndTime = new DateTime(2024, 3, 28, 9, 30, 0),
                Location = "Conference Room",
                Description = "Daily team sync"
            },
            new AppointmentData
            {
                Id = 2,
                Subject = "Project Planning",
                StartTime = new DateTime(2024, 3, 28, 10, 0, 0),
                EndTime = new DateTime(2024, 3, 28, 11, 30, 0),
                Location = "Virtual",
                Description = "Q2 project kickoff"
            },
            new AppointmentData
            {
                Id = 3,
                Subject = "Client Meeting",
                StartTime = new DateTime(2024, 3, 29, 14, 0, 0),
                EndTime = new DateTime(2024, 3, 29, 15, 0, 0),
                Location = "Office",
                Description = "Requirements discussion"
            }
        };
        
        ViewBag.datasource = appointmentData;
        return View();
    }
}

public class AppointmentData
{
    public int Id { get; set; }
    public string Subject { get; set; }
    public DateTime StartTime { get; set; }
    public DateTime EndTime { get; set; }
    public string Location { get; set; }
    public string Description { get; set; }
}
```

### View (Index.cshtml)

```cshtml
@{
    ViewData["Title"] = "Scheduler";
}

<ejs-schedule id="schedule" 
    width="100%" 
    height="550" 
    currentView="Week"
    selectedDate="new DateTime(2024, 3, 28)">
    <e-schedule-views>
        <e-schedule-view option="Day"></e-schedule-view>
        <e-schedule-view option="Week"></e-schedule-view>
        <e-schedule-view option="WorkWeek"></e-schedule-view>
        <e-schedule-view option="Month"></e-schedule-view>
        <e-schedule-view option="TimelineDay"></e-schedule-view>
    </e-schedule-views>
    <e-schedule-eventsettings dataSource="ViewBag.datasource">
        <e-eventsettings-fields id="Id">
            <e-field-subject name="Subject"></e-field-subject>
            <e-field-starttime name="StartTime"></e-field-starttime>
            <e-field-endtime name="EndTime"></e-field-endtime>
            <e-field-location name="Location"></e-field-location>
            <e-field-description name="Description"></e-field-description>
        </e-eventsettings-fields>
    </e-schedule-eventsettings>
</ejs-schedule>
```

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Scheduler not displaying | Verify Tag Helper is added to `_ViewImports.cshtml` |
| Appointments not showing | Check `dataSource` property is properly bound |
| CSS not applied | Confirm stylesheet link is in correct location |
| Scripts not loading | Verify `<ejs-scripts>` is at end of `<body>` tag |
| Events not interactive | Ensure JavaScript file is loaded from CDN |
