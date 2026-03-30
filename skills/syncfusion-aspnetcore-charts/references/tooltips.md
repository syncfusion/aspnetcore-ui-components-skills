# Tooltips Configuration

This guide covers comprehensive tooltip configuration in Syncfusion ASP.NET Core Charts, including enabling, customizing, templates, shared tooltips, formatting, styling, and animations.

## Table of Contents
- [Basic Tooltips](#basic-tooltips)
- [Tooltip Customization](#tooltip-customization)
- [Tooltip Templates](#tooltip-templates)
- [Shared Tooltips](#shared-tooltips)
- [Format and Styling](#format-and-styling)
- [Tooltip Animations](#tooltip-animations)
- [Advanced Features](#advanced-features)
- [When to Use](#when-to-use)

## Basic Tooltips

### Enable Basic Tooltip

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true"></e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" 
                  xName="Month" 
                  yName="Sales" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
        </e-series>
    </e-series-collection>
</ejs-chart>
```

```csharp
// Controller
public class ChartController : Controller
{
    public IActionResult Index()
    {
        ViewBag.ChartData = new[]
        {
            new { Month = "Jan", Sales = 35 },
            new { Month = "Feb", Sales = 28 },
            new { Month = "Mar", Sales = 34 },
            new { Month = "Apr", Sales = 32 },
            new { Month = "May", Sales = 40 }
        };
        return View();
    }
}
```

### Tooltip for Multiple Series

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true"></e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.Sales2023" 
                  xName="Month" 
                  yName="Sales" 
                  name="2023"
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
        </e-series>
        <e-series dataSource="ViewBag.Sales2024" 
                  xName="Month" 
                  yName="Sales" 
                  name="2024"
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
        </e-series>
    </e-series-collection>
</ejs-chart>
```

### Series-Specific Tooltip

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.ProductA" 
                  xName="Month" 
                  yName="Sales" 
                  name="Product A"
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Line">
            <e-series-tooltip enable="true" format="Product A<br/>${point.x} : <b>${point.y}</b>"></e-series-tooltip>
        </e-series>
        <e-series dataSource="ViewBag.ProductB" 
                  xName="Month" 
                  yName="Sales" 
                  name="Product B"
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Line">
            <e-series-tooltip enable="true" format="Product B<br/>${point.x} : <b>${point.y}</b>"></e-series-tooltip>
        </e-series>
    </e-series-collection>
</ejs-chart>
```

## Tooltip Customization

### Custom Fill Color

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true" fill="#0078D4" opacity="0.9">
        <e-tooltip-textstyle color="#ffffff"></e-tooltip-textstyle>
    </e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" 
                  xName="Month" 
                  yName="Sales" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
        </e-series>
    </e-series-collection>
</ejs-chart>
```

### Border and Shadow

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true" fill="#ffffff">
        <e-tooltip-border width="2" color="#0078D4"></e-tooltip-border>
        <e-tooltip-textstyle color="#333" fontSize="14px" fontWeight="500"></e-tooltip-textstyle>
    </e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" 
                  xName="Month" 
                  yName="Sales" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Line">
        </e-series>
    </e-series-collection>
</ejs-chart>
```

### Rounded Corners

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true" 
                   fill="#107C10" 
                   opacity="0.9"
                   rx="10"
                   ry="10">
        <e-tooltip-textstyle color="#ffffff" fontWeight="600"></e-tooltip-textstyle>
    </e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" 
                  xName="Category" 
                  yName="Value" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Area">
        </e-series>
    </e-series-collection>
</ejs-chart>
```

### Opacity and Fade

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true" 
                   fill="#FFB900" 
                   opacity="0.8"
                   fadeOutDuration="1000">
        <e-tooltip-textstyle color="#333"></e-tooltip-textstyle>
    </e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" 
                  xName="Month" 
                  yName="Sales" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Spline">
        </e-series>
    </e-series-collection>
</ejs-chart>
```

## Tooltip Templates

### Basic Template

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true" 
                   template="<div style='background:#0078D4;color:white;padding:10px;border-radius:5px'><b>${point.x}</b><br/>Sales: <b>${point.y}</b></div>">
    </e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" 
                  xName="Month" 
                  yName="Sales" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
        </e-series>
    </e-series-collection>
</ejs-chart>
```

### Rich Template with Icons

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true" 
                   template="<div style='background:#f5f5f5;padding:12px;border:2px solid #0078D4;border-radius:8px;box-shadow:0 2px 4px rgba(0,0,0,0.1)'><div style='display:flex;align-items:center;margin-bottom:5px'><div style='width:12px;height:12px;background:${series.fill};margin-right:8px;border-radius:50%'></div><span style='font-weight:bold;color:#0078D4'>${series.name}</span></div><div style='color:#666'>${point.x}: <b style='color:#333'>${point.y}</b></div></div>">
    </e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.Sales" 
                  xName="Month" 
                  yName="Amount" 
                  name="Sales"
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Line">
        </e-series>
    </e-series-collection>
</ejs-chart>
```

### Multi-Value Template

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true" 
                   template="<div style='background:white;padding:15px;border:2px solid #0078D4;border-radius:5px'><div style='font-weight:bold;color:#0078D4;margin-bottom:10px'>${point.x}</div><table style='font-size:12px'><tr><td>Sales:</td><td style='text-align:right;padding-left:10px'><b>${point.y}</b></td></tr><tr><td>Target:</td><td style='text-align:right;padding-left:10px'><b>${point.target}</b></td></tr><tr><td>Achievement:</td><td style='text-align:right;padding-left:10px'><b>${point.achievement}%</b></td></tr></table></div>">
    </e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.DetailedData" 
                  xName="Month" 
                  yName="Sales" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
        </e-series>
    </e-series-collection>
</ejs-chart>
```

```csharp
// Controller
ViewBag.DetailedData = new[]
{
    new { Month = "Jan", Sales = 35, target = 40, achievement = 87.5 },
    new { Month = "Feb", Sales = 42, target = 40, achievement = 105.0 },
    new { Month = "Mar", Sales = 38, target = 40, achievement = 95.0 }
};
```

### Image in Template

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true" 
                   template="<div style='background:white;padding:10px;border:1px solid #ddd;border-radius:5px;text-align:center'><img src='/images/${point.x}.png' style='width:40px;height:40px'/><div style='margin-top:5px;font-weight:bold'>${point.x}</div><div style='color:#0078D4;font-weight:bold'>${point.y} units</div></div>">
    </e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.ProductData" 
                  xName="Product" 
                  yName="Sales" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Bar">
        </e-series>
    </e-series-collection>
</ejs-chart>
```

## Shared Tooltips

### Basic Shared Tooltip

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true" shared="true"></e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.Sales2023" 
                  xName="Month" 
                  yName="Sales" 
                  name="2023"
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Line">
        </e-series>
        <e-series dataSource="ViewBag.Sales2024" 
                  xName="Month" 
                  yName="Sales" 
                  name="2024"
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Line">
        </e-series>
    </e-series-collection>
</ejs-chart>
```

### Shared Tooltip with Header

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true" 
                   shared="true"
                   header="${point.x}">
    </e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.ProductA" 
                  xName="Month" 
                  yName="Sales" 
                  name="Product A"
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
        </e-series>
        <e-series dataSource="ViewBag.ProductB" 
                  xName="Month" 
                  yName="Sales" 
                  name="Product B"
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
        </e-series>
        <e-series dataSource="ViewBag.ProductC" 
                  xName="Month" 
                  yName="Sales" 
                  name="Product C"
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
        </e-series>
    </e-series-collection>
</ejs-chart>
```

### Shared Tooltip Template

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.DateTime" ></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true" 
                   shared="true"
                   template="<div style='background:#f8f9fa;padding:12px;border-radius:5px;border:1px solid #dee2e6'><div style='font-weight:bold;margin-bottom:8px;padding-bottom:5px;border-bottom:2px solid #0078D4'>${point.x}</div><div style='margin-top:5px'><span style='color:${series.fill};font-weight:bold'>\u25CF</span> ${series.name}: <b>${point.y}</b></div></div>">
    </e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.Sales" 
                  xName="Date" 
                  yName="Amount" 
                  name="Sales"
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Line">
        </e-series>
        <e-series dataSource="ViewBag.Profit" 
                  xName="Date" 
                  yName="Amount" 
                  name="Profit"
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Line">
        </e-series>
    </e-series-collection>
</ejs-chart>
```

## Format and Styling

### Currency Format

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true" format="${point.x} : ${point.y}"></e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.RevenueData" 
                  xName="Month" 
                  yName="Revenue" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
        </e-series>
    </e-series-collection>
</ejs-chart>
```

```csharp
// Controller
ViewBag.RevenueData = new[]
{
    new { Month = "Jan", Revenue = 35000 },
    new { Month = "Feb", Revenue = 42000 },
    new { Month = "Mar", Revenue = 38000 }
};
```

### Percentage Format

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true" format="${point.x} : ${point.y}%"></e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.GrowthData" 
                  xName="Quarter" 
                  yName="Growth" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Line">
        </e-series>
    </e-series-collection>
</ejs-chart>
```

### Font Styling

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true" fill="#0078D4">
        <e-tooltip-textstyle fontFamily="Arial" 
                           fontSize="14px" 
                           fontWeight="600" 
                           color="#ffffff">
        </e-tooltip-textstyle>
    </e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" 
                  xName="Month" 
                  yName="Sales" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Area">
        </e-series>
    </e-series-collection>
</ejs-chart>
```

### Multi-Line Format

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true" 
                   format="<b>${point.x}</b><br/>Sales: ${point.y}<br/>Target: 40">
    </e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" 
                  xName="Month" 
                  yName="Sales" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
        </e-series>
    </e-series-collection>
</ejs-chart>
```

## Tooltip Animations

### Enable Animation

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true" 
                   enableAnimation="true"
                   duration="500">
    </e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" 
                  xName="Month" 
                  yName="Sales" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Line">
        </e-series>
    </e-series-collection>
</ejs-chart>
```

### Fade Duration

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true" 
                   enableAnimation="true"
                   fadeOutDuration="2000">
    </e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" 
                  xName="Category" 
                  yName="Value" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Spline">
        </e-series>
    </e-series-collection>
</ejs-chart>
```

## Advanced Features

### Tooltip Events

```cshtml
<ejs-chart id="chart" tooltipRender="tooltipRender">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true"></e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" 
                  xName="Month" 
                  yName="Sales" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
        </e-series>
    </e-series-collection>
</ejs-chart>

<script>
    function tooltipRender(args) {
        // Customize tooltip based on value
        if (args.point.y > 35) {
            args.tooltip.fill = '#107C10';
            args.tooltip.textStyle.color = '#ffffff';
        } else {
            args.tooltip.fill = '#E74856';
            args.tooltip.textStyle.color = '#ffffff';
        }
    }
</script>
```

### Conditional Tooltips

```cshtml
<ejs-chart id="chart" tooltipRender="conditionalTooltip">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true"></e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" 
                  xName="Month" 
                  yName="Sales" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Line">
        </e-series>
    </e-series-collection>
</ejs-chart>

<script>
    function conditionalTooltip(args) {
        // Show different content based on conditions
        if (args.point.y > 40) {
            args.tooltip.text = [args.point.x + ': ' + args.point.y + ' (Excellent!)'];
        } else if (args.point.y < 30) {
            args.tooltip.text = [args.point.x + ': ' + args.point.y + ' (Needs Improvement)'];
        }
    }
</script>
```

### Position Control

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true" 
                   location-x="100"
                   location-y="50"
                   enableMarker="true">
    </e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" 
                  xName="Month" 
                  yName="Sales" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Area">
        </e-series>
    </e-series-collection>
</ejs-chart>
```

### Marker in Tooltip

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-chart-tooltip enable="true" enableMarker="true"></e-chart-tooltip>
    <e-series-collection>
        <e-series dataSource="ViewBag.ProductA" 
                  xName="Month" 
                  yName="Sales" 
                  name="Product A"
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Line">
        </e-series>
        <e-series dataSource="ViewBag.ProductB" 
                  xName="Month" 
                  yName="Sales" 
                  name="Product B"
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Line">
        </e-series>
    </e-series-collection>
</ejs-chart>
```

## When to Use

**Basic Tooltips**: Use to provide additional information about data points without cluttering the chart visualization.

**Custom Styling**: Use to match tooltips with your brand guidelines, theme, or to improve visibility against chart backgrounds.

**Templates**: Use when you need to display complex information, multiple values, images, or rich HTML content in tooltips.

**Shared Tooltips**: Use with multiple series to compare values across all series at the same X-axis position simultaneously.

**Format Strings**: Use to display values in specific formats like currency, percentage, or custom units for better readability.

**Animations**: Use to create smooth, professional transitions when tooltips appear and disappear, enhancing user experience.

**Event Handling**: Use when you need dynamic tooltip behavior based on data values, conditions, or user interactions.

**Conditional Tooltips**: Use to show different tooltip content or styling based on data thresholds, targets, or business rules.

Choose tooltip configuration based on the complexity of information to display, number of series, design requirements, and level of interactivity needed.
