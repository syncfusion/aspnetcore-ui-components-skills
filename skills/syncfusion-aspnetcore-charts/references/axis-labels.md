# Axis Labels Configuration

This guide covers comprehensive axis label configuration in Syncfusion ASP.NET Core Charts, including smart labels, multilevel labels, formatting, templates, and edge label handling.

## Table of Contents
- [Smart Label Management](#smart-label-management)
- [Label Formatting](#label-formatting)
- [Multilevel Labels](#multilevel-labels)
- [Label Templates](#label-templates)
- [Edge Label Handling](#edge-label-handling)
- [Label Rotation and Alignment](#label-rotation-and-alignment)
- [Custom Label Placement](#custom-label-placement)
- [When to Use](#when-to-use)

## Smart Label Management

Smart labels automatically adjust to prevent overlapping using trim, wrap, rotate, or hide strategies.

### Trim Labels

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category" maximumLabelWidth="50">
    </e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.LongLabelData" xName="Category" yName="Value" type="@Syncfusion.EJ2.Charts.ChartSeriesType.Bar"></e-series>
    </e-series-collection>
</ejs-chart>
```

```csharp
// Controller
ViewBag.LongLabelData = new[]
{
    new { Category = "Very Long Category Name 1", Value = 45 },
    new { Category = "Very Long Category Name 2", Value = 60 },
    new { Category = "Very Long Category Name 3", Value = 38 }
};
```

### Wrap Labels

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category" enableTrim="false" maximumLabelWidth="60">
    </e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.LongLabelData" xName="Category" yName="Value" type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column"></e-series>
    </e-series-collection>
</ejs-chart>
```

### Auto Rotation

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category" labelRotation="0" labelIntersectAction="@Syncfusion.EJ2.Charts.LabelIntersectAction.Rotate45">
    </e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" xName="Month" yName="Sales" type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column"></e-series>
    </e-series-collection>
</ejs-chart>
```

### Hide Overlapping Labels

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category" labelIntersectAction="@Syncfusion.EJ2.Charts.LabelIntersectAction.Hide">
    </e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" xName="Month" yName="Sales" type="@Syncfusion.EJ2.Charts.ChartSeriesType.Line"></e-series>
    </e-series-collection>
</ejs-chart>
```

## Label Formatting

### Numeric Format

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryyaxis labelFormat="C" minimum="0" maximum="100000" interval="20000">
    </e-chart-primaryyaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.RevenueData" xName="Month" yName="Revenue" type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column"></e-series>
    </e-series-collection>
</ejs-chart>
```

```csharp
// Controller
ViewBag.RevenueData = new[]
{
    new { Month = "Jan", Revenue = 35000 },
    new { Month = "Feb", Revenue = 28000 },
    new { Month = "Mar", Revenue = 42000 },
    new { Month = "Apr", Revenue = 51000 }
};
```

### Percentage Format

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryyaxis labelFormat="P0" minimum="0" maximum="1" interval="0.2">
    </e-chart-primaryyaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.PercentageData" xName="Quarter" yName="Growth" type="@Syncfusion.EJ2.Charts.ChartSeriesType.Line"></e-series>
    </e-series-collection>
</ejs-chart>
```

### Custom Format

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryyaxis labelFormat="{value}K">
    </e-chart-primaryyaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" xName="Category" yName="Value" type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column"></e-series>
    </e-series-collection>
</ejs-chart>
```

### Date Format

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.DateTime" labelFormat="MMM yyyy" intervalType="@Syncfusion.EJ2.Charts.IntervalType.Months">
    </e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.DateData" xName="Date" yName="Sales" type="@Syncfusion.EJ2.Charts.ChartSeriesType.Line"></e-series>
    </e-series-collection>
</ejs-chart>
```

```csharp
// Controller
ViewBag.DateData = new[]
{
    new { Date = new DateTime(2024, 1, 1), Sales = 35 },
    new { Date = new DateTime(2024, 2, 1), Sales = 42 },
    new { Date = new DateTime(2024, 3, 1), Sales = 38 }
};
```

## Multilevel Labels

Multilevel labels group axis labels into hierarchical categories.

Observe the [Multilevel label](https://ej2.syncfusion.com/aspnetcore/chart/multilevellabel#/fluent2) examples in this Sample Browser.

## Label Templates

### Custom Label Template

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category" labelTemplate="<div style='text-align:center'><img src='/images/${value}.png' style='width:30px;height:30px'/><br/>${value}</div>">
    </e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.ProductData" xName="Product" yName="Sales" type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column"></e-series>
    </e-series-collection>
</ejs-chart>
```

### Styled Template

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category" labelTemplate="<div style='background:#f0f0f0;padding:5px;border-radius:3px;font-weight:bold'>${value}</div>">
    </e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" xName="Month" yName="Sales" type="@Syncfusion.EJ2.Charts.ChartSeriesType.Bar"></e-series>
    </e-series-collection>
</ejs-chart>
```

## Edge Label Handling

### Edge Label Placement

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Double" edgeLabelPlacement="@Syncfusion.EJ2.Charts.EdgeLabelPlacement.Shift">
    </e-chart-primaryxaxis>
    <e-chart-primaryyaxis edgeLabelPlacement="@Syncfusion.EJ2.Charts.EdgeLabelPlacement.Shift">
    </e-chart-primaryyaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.NumericData" xName="X" yName="Y" type="@Syncfusion.EJ2.Charts.ChartSeriesType.Scatter"></e-series>
    </e-series-collection>
</ejs-chart>
```

### Hide Edge Labels

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.DateTime" edgeLabelPlacement="@Syncfusion.EJ2.Charts.EdgeLabelPlacement.Hide" intervalType="@Syncfusion.EJ2.Charts.IntervalType.Years">
    </e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.DateData" xName="Date" yName="Value" type="@Syncfusion.EJ2.Charts.ChartSeriesType.Line"></e-series>
    </e-series-collection>
</ejs-chart>
```

## Label Rotation and Alignment

### Label Rotation

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category" labelRotation="-45">
    </e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" xName="Category" yName="Value" type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column"></e-series>
    </e-series-collection>
</ejs-chart>
```

### Label Alignment

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category" labelRotation="-90" labelPosition="@Syncfusion.EJ2.Charts.AxisPosition.Inside">
    </e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" xName="Month" yName="Sales" type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column"></e-series>
    </e-series-collection>
</ejs-chart>
```

### Axis label Rotation

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category" labelRotation="45">
    </e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" xName="Category" yName="Value" type="@Syncfusion.EJ2.Charts.ChartSeriesType.Line"></e-series>
    </e-series-collection>
</ejs-chart>
```

## Custom Label Placement

### Inside Labels

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category" labelPosition="@Syncfusion.EJ2.Charts.AxisPosition.Inside" opposedPosition="true">
    </e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" xName="Month" yName="Sales" type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column"></e-series>
    </e-series-collection>
</ejs-chart>
```

### Opposed Position

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category" opposedPosition="true">
    </e-chart-primaryxaxis>
    <e-chart-primaryyaxis opposedPosition="true">
    </e-chart-primaryyaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" xName="Category" yName="Value" type="@Syncfusion.EJ2.Charts.ChartSeriesType.Line"></e-series>
    </e-series-collection>
</ejs-chart>
```

### Label Padding

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category" labelPadding="15">
    </e-chart-primaryxaxis>
    <e-chart-primaryyaxis labelPadding="10">
    </e-chart-primaryyaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" xName="Month" yName="Sales" type="@Syncfusion.EJ2.Charts.ChartSeriesType.Bar"></e-series>
    </e-series-collection>
</ejs-chart>
```

## When to Use

**Smart Label Management**: Use when dealing with long category names or many data points that cause label overlapping.

**Label Formatting**: Use to display axis values in specific formats like currency, percentage, or custom units for better readability.

**Multilevel Labels**: Use to show hierarchical relationships in data, such as grouping months into quarters or organizing products by category.

**Label Templates**: Use when you need to display custom content like icons, images, or rich HTML formatting in axis labels.

**Edge Label Handling**: Use to manage labels at chart boundaries, preventing cutoff or overlapping with the chart edges.

**Label Rotation**: Use when horizontal labels overlap or when vertical alignment improves readability, especially with long text.

**Custom Placement**: Use to position labels inside the chart area, on opposite sides, or with custom padding for unique layout requirements.

Choose the appropriate label configuration based on your data density, label length, available space, and visual hierarchy needs.
