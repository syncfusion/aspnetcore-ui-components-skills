# API Reference

Complete API reference for Syncfusion ASP.NET Core Accumulation Chart component.

## Table of Contents
- [AccumulationChart Class](#accumulationchart-class)
- [AccumulationSeries Class](#accumulationseries-class)
- [Data Label Settings](#data-label-settings)
- [Legend Settings](#legend-settings)
- [Tooltip Settings](#tooltip-settings)
- [Events](#events)
- [Enumerations](#enumerations)
- [Related Classes](#related-classes)
- [Namespace and Assembly](#namespace-and-assembly)
- [Additional Resources](#additional-resources)

---

## AccumulationChart Class

The main accumulation chart component class.

**Namespace:** `Syncfusion.EJ2.Charts`  
**Assembly:** `Syncfusion.EJ2.dll`  
**Inheritance:** `System.Object` → `Syncfusion.EJ2.EJTagHelper` → `AccumulationChart`

### Constructor

```csharp
public AccumulationChart()
```

Creates a new instance of the AccumulationChart class.

### Core Properties

#### Appearance Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `Background` | `string` | null | Background color of the chart (hex or rgba) | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_Background) |
| `BackgroundImage` | `string` | null | Background image URL | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_BackgroundImage) |
| `Border` | `AccumulationChartBorder` | null | Chart border configuration | [AccumulationChartBorder](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChartBorder.html) |
| `Theme` | `AccumulationTheme` | Material | Visual theme (Material, Bootstrap5, Fluent, Tailwind, etc.) | [AccumulationTheme](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationTheme.html) |
| `Height` | `string` | null | Chart height (e.g., "450px", "100%") | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_Height) |
| `Width` | `string` | null | Chart width (e.g., "100%", "800px") | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_Width) |

#### Data Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `DataSource` | `object` | null | Chart data collection (array of objects or DataManager) | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_DataSource) |
| `Series` | `List<AccumulationSeries>` | null | Collection of chart series | [AccumulationSeries](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_Series) |

#### Layout Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `Center` | `AccumulationChartCenter` | null | Center position of pie/doughnut charts | [AccumulationChartCenter](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChartCenter.html) |
| `Margin` | `AccumulationChartMargin` | null | Chart margins (left, right, top, bottom) | [AccumulationChartMargin](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChartMargin.html) |

#### Text Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `Title` | `string` | null | Chart title text | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_Title) |
| `TitleStyle` | `AccumulationChartTitleStyleSettings` | null | Title font and styling | [AccumulationChartTitleStyleSettings](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChartTitleStyleSettings.html) |
| `SubTitle` | `string` | null | Chart subtitle text | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_SubTitle) |
| `SubTitleStyle` | `AccumulationChartSubTitleStyle` | null | Subtitle font and styling | [AccumulationChartSubTitleStyle](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChartSubTitleStyle.html) |

#### Legend Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `LegendSettings` | `AccumulationChartLegendSettings` | null | Legend configuration (position, alignment, styling) | [AccumulationChartLegendSettings](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChartLegendSettings.html) |

#### Interaction Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `HighlightMode` | `AccumulationHighlightMode` | None | Highlight mode when hovering: None or Point | [AccumulationHighlightMode](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationHighlightMode.html) |
| `HighlightColor` | `string` | "" | Color for highlighting data points | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_HighlightColor) |
| `HighlightPattern` | `SelectionPattern` | None | Pattern for highlighting (Chessboard, Dots, Grid, etc.) | [SelectionPattern](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.SelectionPattern.html) |
| `SelectionMode` | `AccumulationSelectionMode` | None | Selection mode: None or Point | [AccumulationSelectionMode](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationSelectionMode.html) |
| `SelectionPattern` | `SelectionPattern` | None | Pattern for selected points | [SelectionPattern](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.SelectionPattern.html) |
| `IsMultiSelect` | `bool` | false | Enable multiple point selection (requires selectionMode=Point) | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_IsMultiSelect) |
| `SelectedDataIndexes` | `object` | null | Initial selected point indexes | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_SelectedDataIndexes) |
| `EnableBorderOnMouseMove` | `bool` | true | Show/hide slice border on mouse hover | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_EnableBorderOnMouseMove) |

#### Tooltip Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `Tooltip` | `AccumulationChartTooltipSettings` | null | Tooltip configuration | [AccumulationChartTooltipSettings](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChartTooltipSettings.html) |

#### Label Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `EnableSmartLabels` | `bool` | true | Auto-arrange labels to prevent overlap | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_EnableSmartLabels) |

#### Annotation Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `Annotations` | `List<AccumulationAnnotationSettings>` | null | Collection of annotations for highlighting data points | [AccumulationAnnotationSettings](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationAnnotationSettings.html) |

#### Accessibility Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `Accessibility` | `AccumulationAccessibility` | null | Accessibility options (WCAG 2.2 compliance) | [AccumulationAccessibility](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationAccessibility.html) |
| `EnableHtmlSanitizer` | `bool` | false | Sanitize untrusted HTML content | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_EnableHtmlSanitizer) |
| `FocusBorderColor` | `string` | - | Focus border color for keyboard navigation | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_FocusBorderColor) |
| `FocusBorderMargin` | `double` | 0 | Focus border margin | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_FocusBorderMargin) |
| `FocusBorderWidth` | `double` | 1.5 | Focus border width | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_FocusBorderWidth) |

#### Animation Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `EnableAnimation` | `bool` | true | Enable chart animation on load | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_EnableAnimation) |

#### Export & Print Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `EnableExport` | `bool` | true | Enable export (PNG, JPEG, SVG, PDF, XLSX, CSV) | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_EnableExport) |
| `AllowExport` | `bool` | false | Allow export in Blazor | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_AllowExport) |

#### Localization & Persistence Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `Locale` | `string` | "en-US" | Culture/localization override | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_Locale) |
| `EnablePersistence` | `bool` | false | Persist component state across page reloads | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_EnablePersistence) |
| `EnableRtl` | `bool` | false | Enable right-to-left rendering | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_EnableRtl) |
| `UseGroupingSeparator` | `bool` | false | Use thousand separator for numbers | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_UseGroupingSeparator) |

#### Template Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `NoDataTemplate` | `object` | null | Template displayed when chart has no data | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_NoDataTemplate) |
| `CenterLabel` | `AccumulationChartCenterLabel` | null | Center label configuration for doughnut charts | [AccumulationChartCenterLabel](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChartCenterLabel.html) |

#### HTML Attributes

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `HtmlAttributes` | `object` | null | Additional HTML attributes (title, name, etc.) |

---

## AccumulationSeries Class

Represents a data series in the accumulation chart.

**Namespace:** `Syncfusion.EJ2.Charts`  
**API Reference:** [AccumulationSeries](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationSeries.html)

### Properties

#### Basic Data Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `DataSource` | `object[]` | null | Series data collection | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChart.html#Syncfusion_EJ2_Charts_AccumulationChart_DataSource) |
| `XName` | `string` | null | Field name for X values (categories) | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationSeries.html#Syncfusion_EJ2_Charts_AccumulationSeries_XName) |
| `YName` | `string` | null | Field name for Y values (numeric data) | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationSeries.html#Syncfusion_EJ2_Charts_AccumulationSeries_YName) |

#### Chart Type Properties

| Property | Type | Default | Description | Allowed Values |
|----------|------|---------|-------------|-----------------|
| `Type` | `string` | "Pie" | Series type | Pie, Pyramid, Funnel (Note: Doughnut = Pie + innerRadius) |

#### Size & Position Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `Radius` | `string` | "80%" | Chart radius (percentage or pixels) | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationSeries.html#Syncfusion_EJ2_Charts_AccumulationSeries_Radius) |
| `InnerRadius` | `string` | "0%" | Inner radius for doughnut charts | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationSeries.html#Syncfusion_EJ2_Charts_AccumulationSeries_InnerRadius) |
| `StartAngle` | `double` | 0 | Start angle in degrees (0-360) | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationSeries.html#Syncfusion_EJ2_Charts_AccumulationSeries_StartAngle) |
| `EndAngle` | `double` | 360 | End angle in degrees (0-360) | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationSeries.html#Syncfusion_EJ2_Charts_AccumulationSeries_EndAngle) |

#### Explosion Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `Explode` | `bool` | false | Enable explosion on click | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationSeries.html#Syncfusion_EJ2_Charts_AccumulationSeries_Explode) |
| `ExplodeIndex` | `double` | null | Index of pre-exploded point | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationSeries.html#Syncfusion_EJ2_Charts_AccumulationSeries_ExplodeIndex) |
| `ExplodeOffset` | `string` | "10%" | Distance exploded slice moves | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationSeries.html#Syncfusion_EJ2_Charts_AccumulationSeries_ExplodeOffset) |

#### Grouping Properties

| Property | Type | Default | Description | Allowed Values |
|----------|------|---------|-------------|-----------------|
| `GroupTo` | `string` | null | Grouping threshold (value or percentage) | Numeric threshold or percentage |
| `GroupMode` | `string` | "Value" | Group mode | Value, Point |
| `GroupName` | `string` | "Others" | Name for grouped points | Any string |

#### Pyramid/Funnel Properties

| Property | Type | Default | Description | Allowed Values |
|----------|------|---------|-------------|-----------------|
| `PyramidMode` | `string` | "Linear" | Pyramid rendering mode | Linear, Surface |
| `FunnelMode` | `string` | "Standard" | Funnel rendering mode | Standard, Trapezoidal |
| `NeckWidth` | `string` | "20%" | Funnel neck width | Percentage |
| `NeckHeight` | `string` | "20%" | Funnel neck height | Percentage |
| `Width` | `string` | "80%" | Pyramid/Funnel width | Percentage |
| `Height` | `string` | "80%" | Pyramid/Funnel height | Percentage |
| `GapRatio` | `double` | 0 | Gap between segments | 0-1 |

#### Color Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `Palettes` | `string[]` | null | Custom color palette (hex colors) | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationSeries.html#Syncfusion_EJ2_Charts_AccumulationSeries_Palettes) |
| `PointColorMapping` | `string` | null | Field name for point colors | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationSeries.html#Syncfusion_EJ2_Charts_AccumulationSeries_PointColorMapping) |

#### Label Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `DataLabel` | `AccumulationDataLabelSettings` | null | Data label configuration | [AccumulationDataLabelSettings](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationDataLabelSettings.html) |

#### Styling Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `Border` | `AccumulationChartBorder` | null | Series border styling | [AccumulationChartBorder](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChartBorder.html) |
| `ConnectorStyle` | `AccumulationChartConnector` | null | Connector line styling | [AccumulationChartConnector](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChartConnector.html) |

#### Data Handling Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `EmptyPointSettings` | `AccumulationChartEmptyPointSettings` | null | Empty/null point handling | [AccumulationChartEmptyPointSettings](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChartEmptyPointSettings.html) |

#### Legend Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `LegendShape` | `LegendShape` | SeriesType | Legend icon shape | [LegendShape](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.LegendShape.html) |

#### Tooltip Properties

| Property | Type | Default | Description | API Link |
|----------|------|---------|-------------|----------|
| `TooltipMappingName` | `string` | null | Field for custom tooltip content | [Reference](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationSeries.html#Syncfusion_EJ2_Charts_AccumulationSeries_TooltipMappingName) |

#### Event Properties

| Event | Type | Description |
|-------|------|-------------|
| `PointRender` | `string` | Event triggered before each point renders |

---

## Data Label Settings

### AccumulationDataLabelSettings Class

**API Reference:** [AccumulationDataLabelSettings](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationDataLabelSettings.html)

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Visible` | `bool` | false | Show/hide data labels |
| `Position` | `string` | "Outside" | Label position: Inside, Outside |
| `Name` | `string` | null | Field name for label text |
| `Template` | `string` | null | HTML template for labels |
| `Format` | `string` | null | Number format (e.g., "p1", "n2", "c2") |
| `TextWrap` | `string` | "Normal" | Text wrapping: Normal, Wrap, AnyWhere |
| `MaxWidth` | `double` | null | Max label width (pixels) |
| `Font` | `object` | null | Font configuration |
| `Border` | `object` | null | Label border configuration |
| `ConnectorStyle` | `string` | "Line" | Connector type: Line, Curve |
| `Rx` | `double` | 0 | Rotation angle |
| `Ry` | `double` | 0 | Rotation point offset Y |

---

## Legend Settings

### AccumulationChartLegendSettings Class

**API Reference:** [AccumulationChartLegendSettings](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChartLegendSettings.html)

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Visible` | `bool` | false | Show/hide legend |
| `Position` | `string` | "Right" | Position: Top, Bottom, Left, Right, Custom |
| `Alignment` | `string` | "Center" | Alignment: Near, Center, Far |
| `Width` | `string` | "0" | Legend width |
| `Height` | `string` | "0" | Legend height |
| `Reverse` | `bool` | false | Reverse legend item order |
| `Layout` | `string` | "Vertical" | Layout: Vertical, Horizontal |
| `MaximumColumns` | `double` | null | Max columns in horizontal layout |
| `ShapeWidth` | `double` | 15 | Legend shape width |
| `ShapeHeight` | `double` | 15 | Legend shape height |
| `Title` | `object` | null | Legend title |
| `Template` | `string` | null | Custom HTML template |
| `TextWrap` | `string` | "Normal" | Text wrapping: Normal, Wrap |
| `MaximumLabelWidth` | `double` | null | Max legend item label width |
| `EnablePages` | `bool` | false | Enable paging |
| `ToggleVisibility` | `bool` | true | Toggle visibility on click |

---

## Tooltip Settings

### AccumulationChartTooltipSettings Class

**API Reference:** [AccumulationChartTooltipSettings](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChartTooltipSettings.html)

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Enable` | `bool` | false | Enable/disable tooltips |
| `Header` | `string` | null | Custom tooltip header |
| `Format` | `string` | null | Tooltip text format |
| `Template` | `string` | null | HTML template for tooltips |
| `Fill` | `string` | null | Tooltip background color |
| `Border` | `object` | null | Tooltip border |
| `TextStyle` | `object` | null | Tooltip text styling |
| `Location` | `object` | null | Fixed tooltip position |
| `Opacity` | `double` | 1 | Tooltip opacity |
| `Shared` | `bool` | false | Shared tooltip mode |

---

## Events

All events are triggered at different stages of chart lifecycle:

| Event | Triggered When | Handler Signature |
|-------|---|---|
| `Load` | Before chart loads | `function(args)` |
| `Loaded` | After chart loads | `function(args)` |
| `AnimationComplete` | After animation completes | `function(args)` |
| `PointRender` | Before each point renders | `function(args)` |
| `SeriesRender` | Before series renders | `function(args)` |
| `LegendRender` | Before legend renders | `function(args)` |
| `AnnotationRender` | Before annotation renders | `function(args)` |
| `TextRender` | Before data label renders | `function(args)` |
| `TooltipRender` | Before tooltip renders | `function(args)` |
| `PointClick` | When point is clicked | `function(args)` |
| `PointMove` | When point is hovered | `function(args)` |
| `LegendClick` | After legend click | `function(args)` |
| `ChartMouseClick` | When chart is clicked | `function(args)` |
| `ChartMouseMove` | When mouse moves over chart | `function(args)` |
| `ChartMouseDown` | On mouse down | `function(args)` |
| `ChartMouseUp` | On mouse up | `function(args)` |
| `ChartMouseLeave` | When mouse leaves chart | `function(args)` |
| `ChartDoubleClick` | On double-click | `function(args)` |
| `BeforeExport` | Before export starts | `function(args)` |
| `AfterExport` | After export completes | `function(args)` |
| `BeforePrint` | Before print starts | `function(args)` |
| `BeforeResize` | Before resize event | `function(args)` |
| `Resized` | After resize completes | `function(args)` |
| `SelectionComplete` | After selection completes | `function(args)` |

---

## Enumerations

### AccumulationTheme Enum

**API Reference:** [AccumulationTheme](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationTheme.html)

Available themes for visual styling:

- `Fabric`
- `FabricDark`
- `Bootstrap4`
- `Bootstrap`
- `BootstrapDark`
- `HighContrastLight`
- `HighContrast`
- `Tailwind`
- `TailwindDark`
- `Bootstrap5`
- `Bootstrap5Dark`
- `Fluent`
- `FluentDark`
- `Fluent2`
- `Fluent2Dark`
- `Fluent2HighContrast`
- `Material3`
- `Material3Dark`
- `Material`
- `MaterialDark`

### AccumulationHighlightMode Enum

**API Reference:** [AccumulationHighlightMode](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationHighlightMode.html)

- `None` - No highlighting
- `Point` - Highlight individual point on hover

### AccumulationSelectionMode Enum

**API Reference:** [AccumulationSelectionMode](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationSelectionMode.html)

- `None` - No selection
- `Point` - Enable point selection

### SelectionPattern Enum

**API Reference:** [SelectionPattern](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.SelectionPattern.html)

Patterns for highlighting/selection:

- `None`
- `Chessboard`
- `Dots`
- `DiagonalForward`
- `Crosshatch`
- `Pacman`
- `DiagonalBackward`
- `Grid`
- `Turquoise`
- `Star`
- `Triangle`
- `Circle`
- `Tile`
- `HorizontalDash`
- `VerticalDash`
- `Rectangle`
- `Box`
- `VerticalStripe`
- `HorizontalStripe`
- `Bubble`

### LegendShape Enum

**API Reference:** [LegendShape](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.LegendShape.html)

Legend icon shapes:

- `Circle`
- `Rectangle`
- `Triangle`
- `Diamond`
- `Cross`
- `HorizontalLine`
- `VerticalLine`
- `Pentagon`
- `InvertedTriangle`
- `SeriesType` (default - uses series shape)

### LegendPosition Enum

**API Reference:** [LegendPosition](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.LegendPosition.html)

- `Top`
- `Bottom`
- `Left`
- `Right`
- `Custom`

### Alignment Enum

**API Reference:** [Alignment](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.Alignment.html)

- `Near`
- `Center`
- `Far`

---

## Related Classes

### Border Classes

| Class | Description | API Reference |
|-------|-------------|---|
| `AccumulationChartBorder` | Configure chart/series border appearance | [Link](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChartBorder.html) |

### Layout Classes

| Class | Description | API Reference |
|-------|-------------|---|
| `AccumulationChartCenter` | Define pie/doughnut center position | [Link](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChartCenter.html) |
| `AccumulationChartMargin` | Define chart margins | [Link](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChartMargin.html) |

### Style Classes

| Class | Description | API Reference |
|-------|-------------|---|
| `AccumulationChartTitleStyleSettings` | Title styling | [Link](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChartTitleStyleSettings.html) |
| `AccumulationChartSubTitleStyle` | Subtitle styling | [Link](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChartSubTitleStyle.html) |

### Special Features Classes

| Class | Description | API Reference |
|-------|-------------|---|
| `AccumulationChartConnector` | Connector line styling for labels | [Link](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChartConnector.html) |
| `AccumulationChartEmptyPointSettings` | Handle empty/null data points | [Link](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChartEmptyPointSettings.html) |
| `AccumulationChartCenterLabel` | Center label for doughnut charts | [Link](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationChartCenterLabel.html) |
| `AccumulationAnnotationSettings` | Annotations configuration | [Link](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationAnnotationSettings.html) |
| `AccumulationAccessibility` | Accessibility features | [Link](https://help.syncfusion.com/cr/aspnetcore-js2/Syncfusion.EJ2.Charts.AccumulationAccessibility.html) |

---

## Namespace and Assembly

**Namespace:** `Syncfusion.EJ2.Charts`

**Assembly:** `Syncfusion.EJ2.dll`

**Version:** Compatible with Syncfusion EJ2 ASP.NET Core (v18.4.0 and later)

### Using Statement

```csharp
using Syncfusion.EJ2.Charts;
```

### NuGet Package

```powershell
Install-Package Syncfusion.EJ2.AspNet.Core -Version 33.1.44
```

---

## Additional Resources

- [Official Documentation](https://ej2.syncfusion.com/aspnetcore/documentation/accumulation-chart/getting-started)
- [Live Demos](https://ej2.syncfusion.com/aspnetcore/chart/pie#/)
- [GitHub Examples](https://github.com/SyncfusionExamples/ASP-NET-Core-Getting-Started-Examples/tree/main/AccumulationChart)
