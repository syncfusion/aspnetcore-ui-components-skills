# Data Binding in Smith Chart

## Table of Contents
- [Understanding Smith Chart Data Structure](#understanding-smith-chart-data-structure)
- [Data Binding Methods](#data-binding-methods)
  - [Method 1: Using dataSource Property](#method-1-using-datasource-property)
  - [Method 2: Using points Property](#method-2-using-points-property)
- [Multiple Series Data Binding](#multiple-smithchart-smithchartseries-data-binding)
- [Common Data Patterns](#common-data-patterns)
  - [RF Circuit Analysis](#rf-circuit-analysis)
  - [Transmission Line Matching](#transmission-line-matching)
  - [Multiple Load Conditions](#multiple-load-conditions)
- [Data Value Ranges](#data-value-ranges)
- [Dynamic Data Updates](#dynamic-data-updates)
- [Data Validation Considerations](#data-validation-considerations)
- [Complete Data Binding Example](#complete-data-binding-example)


## Understanding Smith Chart Data Structure

Smith Chart visualizations require specific data formats to properly render transmission line parameters. Your data must contain two essential fields:
- **Resistance** - The real part of impedance/admittance (horizontal axis component)
- **Reactance** - The imaginary part of impedance/admittance (circular component)

## Data Binding Methods

Smith Chart supports two primary methods for binding data:

### Method 1: Using dataSource Property

The `dataSource` property binds an entire collection of data objects to a series. This approach is ideal when you have data available in your page model or retrieved from a service.

```cshtml
<ejs-smithchart id="smithchart">
    <e-smithchart-smithchartseriescollection>
        <e-smithchart-smithchartseries dataSource="TransmissionLineData" name="TL1">
        </e-smithchart-smithchartseries>
    </e-smithchart-smithchartseriescollection>
</ejs-smithchart>
```

In your page model:

```csharp
public List<SmithChartData> TransmissionLineData { get; set; }

public void OnGet()
{
    TransmissionLineData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 20, Reactance = 50 },
        new SmithChartData { Resistance = 30, Reactance = 75 }
    };
}

public class SmithChartData
{
    public double Resistance { get; set; }
    public double Reactance { get; set; }
}
```

### Method 2: Using points Property

The `points` property allows you to specify individual points directly in markup or programmatically. Use this when you have a smaller set of data or prefer inline configuration:

```cshtml
<ejs-smithchart id="smithchart">
    <e-smithchart-smithchartseriescollection>
        <e-smithchart-smithchartseries name="Transmission 1">
        </e-smithchart-smithchartseries>
    </e-smithchart-smithchartseriescollection>
</ejs-smithchart>

<script>
    function loaded(args) {
        window.smithchart = args.smithchart;
        args.smithchart.series[0].marker = {
            dataLabel: {
                visible: true,
            }
        };
        args.smithchart.series[0].points = [
            { resistance: 10, reactance: 25 }, { resistance: 20, reactance: 50 },
            { resistance: 30, reactance: 75 }
        ];
        args.smithchart.loaded = null;
        args.smithchart.refresh();
    }
</script>
```

## Multiple Series Data Binding

Smith Charts can display multiple series simultaneously, allowing comparison of different transmission lines or circuit configurations:

```cshtml
<ejs-smithchart id="smithchart">
    <e-smithchart-smithchartseriescollection>
        <e-smithchart-smithchartseries dataSource="TransmissionLine1Data" name="Line A">
        </e-smithchart-smithchartseries>
        <e-smithchart-smithchartseries dataSource="TransmissionLine2Data" name="Line B">
        </e-smithchart-smithchartseries>
        <e-smithchart-smithchartseries dataSource="TransmissionLine3Data" name="Line C">
        </e-smithchart-smithchartseries>
    </e-smithchart-smithchartseriescollection>
</ejs-smithchart>
```

In your page model:

```csharp
public List<SmithChartData> TransmissionLine1Data { get; set; }
public List<SmithChartData> TransmissionLine2Data { get; set; }
public List<SmithChartData> TransmissionLine3Data { get; set; }

public void OnGet()
{
    TransmissionLine1Data = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 20, Reactance = 50 },
        new SmithChartData { Resistance = 30, Reactance = 75 }
    };

    TransmissionLine2Data = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 5, Reactance = 15 },
        new SmithChartData { Resistance = 15, Reactance = 45 },
        new SmithChartData { Resistance = 25, Reactance = 65 }
    };

    TransmissionLine3Data = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 15, Reactance = 35 },
        new SmithChartData { Resistance = 25, Reactance = 55 },
        new SmithChartData { Resistance = 35, Reactance = 70 }
    };
}
```

## Common Data Patterns

### RF Circuit Analysis

For RF circuit measurements, resistance typically represents impedance components and reactance represents reactive components:

```csharp
public class RFMeasurement
{
    public double Resistance { get; set; }  // Real impedance (Ohms)
    public double Reactance { get; set; }   // Imaginary impedance (Ohms)
}

var rfData = new List<RFMeasurement>
{
    new RFMeasurement { Resistance = 50, Reactance = 0 },     // Matched load
    new RFMeasurement { Resistance = 75, Reactance = 25 },    // Mismatched
    new RFMeasurement { Resistance = 100, Reactance = -50 }   // Reactive load
};
```

### Transmission Line Matching

When visualizing impedance matching along a transmission line:

```csharp
var matchingData = new List<SmithChartData>
{
    new SmithChartData { Resistance = 50, Reactance = 0 },      // Start point
    new SmithChartData { Resistance = 60, Reactance = 20 },     // Quarter wave
    new SmithChartData { Resistance = 100, Reactance = 50 },    // Half wave
    new SmithChartData { Resistance = 50, Reactance = 0 }       // Full wave (match)
};
```

### Multiple Load Conditions

Comparing different load impedances:

```csharp
var openCircuit = new SmithChartData { Resistance = 0, Reactance = 0 };      // Left edge
var shortCircuit = new SmithChartData { Resistance = 0, Reactance = 0 };     // Left edge
var matchedLoad = new SmithChartData { Resistance = 50, Reactance = 0 };     // Center
var capacitiveLoad = new SmithChartData { Resistance = 50, Reactance = -25 };  // Below center
var inductiveLoad = new SmithChartData { Resistance = 50, Reactance = 25 };    // Above center
```

## Data Value Ranges

Smith Chart coordinates work with normalized impedance values (typically 0 to infinity on resistance axis, with reactance ranging from -∞ to +∞). However, practical values typically range:

- **Resistance:** 0 to 200+ Ohms (for 50 Ohm systems)
- **Reactance:** -200 to +200 Ohms

Values outside typical ranges will still plot correctly but may appear at extreme positions on the chart.

## Dynamic Data Updates

If you need to update chart data dynamically, you can modify the dataSource in your page model and re-render:

```csharp
public void OnPost()
{
    // Update data based on form submission or calculation
    TransmissionLineData = CalculateImpedanceValues();
}

private List<SmithChartData> CalculateImpedanceValues()
{
    var data = new List<SmithChartData>();
    // Your calculation logic here
    return data;
}
```

Then re-render the page with the updated `TransmissionLineData`.

## Data Validation Considerations

When preparing data for Smith Chart:

1. **Ensure Numeric Values** - Both Resistance and Reactance must be numeric (not strings)
2. **Handle Null Values** - Remove or handle null entries to prevent rendering errors
3. **Check Field Names** - Field names must exactly match "Resistance" and "Reactance" (case-sensitive in some contexts)
4. **Validate Data Range** - While any numeric values will plot, ensure your values make sense for your circuit analysis
5. **Sort Data Logically** - Order series points in a logical sequence (e.g., chronologically or by position) for meaningful line rendering

## Complete Data Binding Example

```csharp
public class IndexModel : PageModel
{
    public List<SmithChartData> Series1Data { get; set; }
    public List<SmithChartData> Series2Data { get; set; }

    public void OnGet()
    {
        Series1Data = new List<SmithChartData>
        {
            new SmithChartData { Resistance = 10, Reactance = 25 },
            new SmithChartData { Resistance = 20, Reactance = 50 },
            new SmithChartData { Resistance = 30, Reactance = 75 },
            new SmithChartData { Resistance = 40, Reactance = 60 },
            new SmithChartData { Resistance = 50, Reactance = 30 }
        };

        Series2Data = new List<SmithChartData>
        {
            new SmithChartData { Resistance = 5, Reactance = 15 },
            new SmithChartData { Resistance = 15, Reactance = 45 },
            new SmithChartData { Resistance = 25, Reactance = 65 },
            new SmithChartData { Resistance = 35, Reactance = 55 },
            new SmithChartData { Resistance = 45, Reactance = 25 }
        };
    }
}

public class SmithChartData
{
    public double Resistance { get; set; }
    public double Reactance { get; set; }
}
```

```cshtml
<ejs-smithchart id="smithchart">
    <e-smithchart-smithchartseriescollection>
        <e-smithchart-smithchartseries dataSource="Model.Series1Data" name="Series 1">
        </e-smithchart-smithchartseries>
        <e-smithchart-smithchartseries dataSource="Model.Series2Data" name="Series 2">
        </e-smithchart-smithchartseries>
    </e-smithchart-smithchartseriescollection>
</ejs-smithchart>
```

This approach ensures clean, maintainable data binding for Smith Chart visualizations.
