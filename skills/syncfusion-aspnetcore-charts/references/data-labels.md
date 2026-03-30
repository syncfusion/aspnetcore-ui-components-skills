# Data Labels Configuration

This guide covers comprehensive data label configuration in Syncfusion ASP.NET Core Charts, including positioning, templates, formatting, smart arrangement, and customization options.

## Table of Contents
- [Basic Data Labels](#basic-data-labels)
- [Label Positioning](#label-positioning)
- [Label Templates](#label-templates)
- [Label Formatting](#label-formatting)
- [Text Mapping](#text-mapping)
- [Smart Label Arrangement](#smart-label-arrangement)
- [Label Customization](#label-customization)
- [Connector Lines](#connector-lines)
- [When to Use](#when-to-use)

## Basic Data Labels

### Enable Data Labels

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="@ViewBag.ChartData" 
                  xName="Month" 
                  yName="Sales" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
            <e-series-marker>
                <e-series-datalabel visible="true"></e-series-datalabel>
            </e-series-marker>
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

### Data Labels for Multiple Series

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="@ViewBag.Sales2023" 
                  xName="Month" 
                  yName="Sales" 
                  name="2023"
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
            <e-series-marker>
                <e-series-datalabel visible="true"></e-series-datalabel>
            </e-series-marker>
        </e-series>
        <e-series dataSource="@ViewBag.Sales2024" 
                  xName="Month" 
                  yName="Sales" 
                  name="2024"
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
            <e-series-marker>
                <e-series-datalabel visible="true"></e-series-datalabel>
            </e-series-marker>
        </e-series>
    </e-series-collection>
</ejs-chart>
```

## Label Positioning

### Top Position

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="@ViewBag.ChartData" 
                  xName="Category" 
                  yName="Value" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
            <e-series-marker>
                <e-series-datalabel visible="true" position="@Syncfusion.EJ2.Charts.LabelPosition.Top"></e-series-datalabel>
            </e-series-marker>
        </e-series>
    </e-series-collection>
</ejs-chart>
```

### Middle Position

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="@ViewBag.ChartData" 
                  xName="Category" 
                  yName="Value" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
            <e-series-marker>
                <e-series-datalabel visible="true" position="@Syncfusion.EJ2.Charts.LabelPosition.Middle">
                    <e-font color="#ffffff" fontweight="600"></e-font>
                </e-series-datalabel>
            </e-series-marker>
        </e-series>
    </e-series-collection>
</ejs-chart>
```

### Bottom Position

```cshtml
@{
    var margin = new { left = 15, right = 15, bottom = 15, top = 15 };
 }
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="@ViewBag.ChartData" 
                  xName="Category" 
                  yName="Value" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
            <e-series-marker>
                <e-series-datalabel visible="true" position="@Syncfusion.EJ2.Charts.LabelPosition.Bottom" margin="margin">
                </e-series-datalabel>
            </e-series-marker>
        </e-series>
    </e-series-collection>
</ejs-chart>
```

### Outer Position for Pie Chart

```cshtml
<ejs-accumulationchart id="pie-chart">
    <e-accumulation-series-collection>
        <e-accumulation-series dataSource="ViewBag.PieData" 
                              xName="Category" 
                              yName="Value">
            <e-accumulationseries-datalabel visible="true" position="@Syncfusion.EJ2.Charts.LabelPosition.Outside" name="Text">
            </e-accumulationseries-datalabel>
        </e-accumulation-series>
    </e-accumulation-series-collection>
</ejs-accumulationchart>
```

```csharp
// Controller
ViewBag.PieData = new[]
{
    new { Category = "Product A", Value = 35, Text = "Product A: 35%" },
    new { Category = "Product B", Value = 28, Text = "Product B: 28%" },
    new { Category = "Product C", Value = 37, Text = "Product C: 37%" }
};
```

### Custom Alignment

```cshtml
@{
    var margin = new { top = 5 };
 }
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" 
                  xName="Month" 
                  yName="Sales" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
            <e-series-marker>
                <e-datalabel visible="true" position="@Syncfusion.EJ2.Charts.LabelPosition.Top" alignment="@Syncfusion.EJ2.Charts.Alignment.Center" margin="margin">
                </e-datalabel>
            </e-series-marker>
        </e-series>
    </e-series-collection>
</ejs-chart>
```

## Label Templates

### Basic Template

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" 
                  xName="Month" 
                  yName="Sales" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
            <e-series-marker>
                <e-datalabel visible="true" template="#template">
                </e-datalabel>
            </e-series-marker>
        </e-series>
    </e-series-collection>
</ejs-chart>
<script id="template">
    <div style="background:#f5f5f5; border: 1px solid black; padding: 3px 3px 3px 3px">
        <div>${point.x}</div>
        <div>${point.y}</div>
    </div>
</script>
```

### Rich Template with Icons

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" 
                  xName="Product" 
                  yName="Sales" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
            <e-series-marker>
                <e-datalabel visible="true" template="#template">
                </e-datalabel>
            </e-series-marker>
        </e-series>
    </e-series-collection>
</ejs-chart>
<script id="template">
    <div style='text-align:center'><img src='/images/${point.x}.png' style='width:20px;height:20px'/><br/><b>${point.y}</b></div>
</script>
```

### Conditional Template

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" 
                  xName="Month" 
                  yName="Sales" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
            <e-series-marker>
                <e-datalabel visible="true" template="#template">
                </e-datalabel>
            </e-series-marker>
        </e-series>
    </e-series-collection>
</ejs-chart>
<script id="template">
    <div style='background:${point.y > 35 ? '#107C10' : '#E74856'};color:white;padding:3px 8px;border-radius:3px;font-weight:bold'>${point.y}</div>
</script>
```

### Multi-Line Template

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.DetailedData" 
                  xName="Month" 
                  yName="Sales" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
            <e-series-marker>
                <e-datalabel visible="true" template="#template">
                </e-datalabel>
            </e-series-marker>
        </e-series>
    </e-series-collection>
</ejs-chart>
<script id="template">
    <div style='text-align:center;background:#f8f9fa;padding:5px;border:1px solid #dee2e6;border-radius:3px'><div style='font-weight:bold;color:#0078D4'>${point.y}</div><div style='font-size:10px;color:#666'>${point.text}</div></div>
</script>
```

```csharp
// Controller
ViewBag.DetailedData = new[]
{
    new { Month = "Jan", Sales = 35, text = "Target: 40" },
    new { Month = "Feb", Sales = 42, text = "Target: 40" },
    new { Month = "Mar", Sales = 38, text = "Target: 40" }
};
```

## Label Formatting

### Currency Format

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.RevenueData" 
                  xName="Month" 
                  yName="Revenue" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
            <e-series-marker>
                <e-datalabel visible="true" format="c1"></e-datalabel>
            </e-series-marker>
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
    <e-series-collection>
        <e-series dataSource="ViewBag.GrowthData" 
                  xName="Quarter" 
                  yName="Growth" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Line">
            <e-series-marker visible="true" width="10" height="10">
                <e-datalabel visible="true" format="p1"></e-datalabel>
            </e-series-marker>
        </e-series>
    </e-series-collection>
</ejs-chart>
```

### Custom Format

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" 
                  xName="Product" 
                  yName="Units" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
            <e-series-marker>
                <e-datalabel visible="true" format="{value} units"></e-datalabel>
            </e-series-marker>
        </e-series>
    </e-series-collection>
</ejs-chart>
```

### Decimal Format

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.RatingData" 
                  xName="Product" 
                  yName="Rating" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
            <e-series-marker>
                <e-datalabel visible="true" format="n2"></e-datalabel>
            </e-series-marker>
        </e-series>
    </e-series-collection>
</ejs-chart>
```

## Text Mapping

### Map Custom Text

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.MappedData" 
                  xName="Month" 
                  yName="Sales" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
            <e-series-marker>
                <e-datalabel visible="true" name="Label"></e-datalabel>
            </e-series-marker>
        </e-series>
    </e-series-collection>
</ejs-chart>
```

```csharp
// Controller
ViewBag.MappedData = new[]
{
    new { Month = "Jan", Sales = 35, Label = "Good" },
    new { Month = "Feb", Sales = 28, Label = "Fair" },
    new { Month = "Mar", Sales = 42, Label = "Excellent" },
    new { Month = "Apr", Sales = 32, Label = "Good" }
};
```

## Smart Label Arrangement

### Auto Smart Labels

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.DenseData" 
                  xName="X" 
                  yName="Y" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
            <e-series-marker>
                <e-datalabel visible="true" enableRotation="true" angle="-45"></e-datalabel>
            </e-series-marker>
        </e-series>
    </e-series-collection>
</ejs-chart>
```

### Prevent Label Overlap

```cshtml
<ejs-accumulationchart id="pie-chart" enableSmartLabels="true">
    <e-accumulation-series-collection>
        <e-accumulation-series dataSource="ViewBag.PieData" 
                              xName="Category" 
                              yName="Value">
            <e-accumulationseries-datalabel visible="true" position="@Syncfusion.EJ2.Charts.LabelPosition.Outside">
            </e-accumulationseries-datalabel>
        </e-accumulation-series>
    </e-accumulation-series-collection>
</ejs-accumulationchart>
```

## Label Customization

### Font Customization

```cshtml
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" 
                  xName="Month" 
                  yName="Sales" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
            <e-series-marker>
                <e-datalabel visible="true">
                    <e-font fontFamily="Arial" size="14px" fontWeight="600" color="#0078D4"></e-font>
                </e-datalabel>
            </e-series-marker>
        </e-series>
    </e-series-collection>
</ejs-chart>
```

### Background and Border

```cshtml
@{
    var border = new { width = 2, color = "red" };
    var margin = new { left = 15, right = 15, bottom = 15, top = 15 };
 }
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" 
                  xName="Category" 
                  yName="Value" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
            <e-series-marker>
                <e-datalabel visible="true" fill="#FFE5B4" border="border" margin="margin"></e-datalabel>
            </e-series-marker>
        </e-series>
    </e-series-collection>
</ejs-chart>
```

### Rounded Corners

```cshtml
@{
    var margin = new { left = 15, right = 15, bottom = 15, top = 15 };
 }
<ejs-chart id="chart">
    <e-chart-primaryxaxis valueType="@Syncfusion.EJ2.Charts.ValueType.Category"></e-chart-primaryxaxis>
    <e-series-collection>
        <e-series dataSource="ViewBag.ChartData" 
                  xName="Month" 
                  yName="Sales" 
                  type="@Syncfusion.EJ2.Charts.ChartSeriesType.Column">
            <e-series-marker>
                <e-datalabel visible="true" fill="#0078D4" rx="5" ry="5" margin="margin">
                    <e-font color="#ffffff" fontWeight="600"></e-font>
                </e-datalabel>
            </e-series-marker>
        </e-series>
    </e-series-collection>
</ejs-chart>
```

## Connector Lines

### Basic Connectors

```cshtml
<ejs-accumulationchart id="pie-chart">
    <e-accumulation-series-collection>
        <e-accumulation-series dataSource="ViewBag.PieData" 
                              xName="Category" 
                              yName="Value">
            <e-accumulationseries-datalabel visible="true" position="@Syncfusion.EJ2.Charts.LabelPosition.Outside">
                <e-connectorstyle type="@Syncfusion.EJ2.Charts.ConnectorType.Curve" length="20px"></e-connectorstyle>
            </e-accumulationseries-datalabel>
        </e-accumulation-series>
    </e-accumulation-series-collection>
</ejs-accumulationchart>
```

### Styled Connectors

```cshtml
<ejs-accumulationchart id="pie-chart">
    <e-accumulation-series-collection>
        <e-accumulation-series dataSource="ViewBag.PieData" 
                              xName="Category" 
                              yName="Value">
            <e-accumulationseries-datalabel visible="true" position="@Syncfusion.EJ2.Charts.LabelPosition.Outside">
                <e-connectorstyle type="@Syncfusion.EJ2.Charts.ConnectorType.Line" length="20px" width="2" color="#0078D4" dashArray="5,3"></e-connectorstyle>
            </e-accumulationseries-datalabel>
        </e-accumulation-series>
    </e-accumulation-series-collection>
</ejs-accumulationchart>
```

### Curved Connectors

```cshtml
<ejs-accumulationchart id="pie-chart">
    <e-accumulation-series-collection>
        <e-accumulation-series dataSource="ViewBag.PieData" 
                              xName="Category" 
                              yName="Value">
            <e-accumulationseries-datalabel visible="true" position="@Syncfusion.EJ2.Charts.LabelPosition.Outside">
                <e-connector type="@Syncfusion.EJ2.Charts.ConnectorType.Curve" length="20px" width="1.5" color="#666"></e-connector>
            </e-accumulationseries-datalabel>
        </e-accumulation-series>
    </e-accumulation-series-collection>
</ejs-accumulationchart>
```

## When to Use

**Basic Data Labels**: Use to display exact values on chart points when precision is more important than visual estimation.

**Label Positioning**: Use appropriate positions based on chart type and data density - Top for positive values, Middle for better visibility in columns.

**Label Templates**: Use when you need rich formatting, icons, images, or complex HTML layouts beyond simple text labels.

**Label Formatting**: Use to format numbers, currency, percentages, or dates according to locale and business requirements.

**Text Mapping**: Use to display custom descriptive text instead of raw numeric values for better context and readability.

**Smart Arrangement**: Use when dealing with many data points or overlapping labels to automatically adjust label positions and prevent collisions.

**Customization**: Use to match brand guidelines, create visual hierarchy, or highlight important data points with colors and borders.

**Connector Lines**: Use in pie/doughnut charts when labels are positioned outside to clearly connect labels to their respective segments.

Choose data label configuration based on data density, chart type, readability requirements, and the level of detail your audience needs.
