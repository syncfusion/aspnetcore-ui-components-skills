# Markers

## Table of Contents
- [Overview](#overview)
- [Adding Markers](#adding-markers)
- [Marker Types](#marker-types)
- [Marker Customization](#marker-customization)
- [Template-Based Markers](#template-based-markers)
- [Multiple Marker Sets](#multiple-marker-sets)
- [Marker Events](#marker-events)
- [Marker Tooltips](#marker-tooltips)
- [Marker Legend](#marker-legend)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

Markers are visual indicators placed at specific geographical locations on a map. They're perfect for marking points of interest, locations, cities, landmarks, or any coordinate-based data.

**When to Use Markers:**
- Mark specific locations (stores, offices, landmarks)
- Show event locations
- Display cities or points of interest
- Indicate data points at specific coordinates
- Create interactive location-based interfaces

**Markers vs Bubbles:**
- **Markers** - Fixed size, precise locations, good for points of interest
- **Bubbles** - Size represents data magnitude, better for quantitative visualization

## Adding Markers

### Basic Marker Setup

Markers require a data source with `latitude` and `longitude` properties:

```cshtml
@using Syncfusion.EJ2.Maps
@using Newtonsoft.Json

@{
    var worldMap = JsonConvert.DeserializeObject(System.IO.File.ReadAllText("wwwroot/maps/world.json"));
    
    var cities = new[] {
        new { Name = "New York", Latitude = 40.7128, Longitude = -74.0060 },
        new { Name = "London", Latitude = 51.5074, Longitude = -0.1278 },
        new { Name = "Tokyo", Latitude = 35.6762, Longitude = 139.6503 },
        new { Name = "Sydney", Latitude = -33.8688, Longitude = 151.2093 }
    };
}

<ejs-maps id="maps" height="600px">
    <e-maps-layers>
        <e-maps-layer shapeData="worldMap">
            <e-layersettings-shapesettings fill="#C3E6ED"></e-layersettings-shapesettings>
            
            <e-layersettings-markersettings>
                <e-layersettings-markersetting 
                    visible="true" 
                    dataSource="cities"
                    shape="Circle"
                    fill="#FF6347"
                    height="15"
                    width="15">
                </e-layersettings-markersetting>
            </e-layersettings-markersettings>
        </e-maps-layer>
    </e-maps-layers>
</ejs-maps>
```

**Required Properties:**
- `visible="true"` - Enable marker display
- `dataSource` - Array of objects with latitude/longitude
- Latitude/Longitude - Coordinate properties in data source (case-sensitive: must be "Latitude" and "Longitude")

### Marker Data Structure

```csharp
// Minimum required structure
var markerData = new[] {
    new { Latitude = 40.7128, Longitude = -74.0060 }
};

// Recommended structure with additional data
var markerData = new[] {
    new { 
        Name = "Store #1",
        Latitude = 40.7128, 
        Longitude = -74.0060,
        Type = "Retail",
        Revenue = 150000,
        Status = "Active"
    }
};
```

## Marker Types

Syncfusion Maps supports 12 built-in marker shapes:

### Available Shapes

- **Circle** - Simple circular marker (default)
- **Rectangle** - Square marker
- **Diamond** - Diamond-shaped marker
- **Star** - Star-shaped marker
- **Triangle** - Triangular marker
- **Pentagon** - Five-sided marker
- **Balloon** - Speech bubble marker
- **Cross** - Cross/plus sign marker
- **HorizontalLine** - Horizontal line marker
- **VerticalLine** - Vertical line marker
- **Image** - Custom image marker
- **InvertedTriangle** - Upside-down triangle

### Using Different Shapes

```cshtml
<e-layersettings-markersettings>
    <!-- Circle markers -->
    <e-layersettings-markersetting 
        visible="true"
        dataSource="@officeLocations"
        shape="Circle"
        fill="#1E90FF"
        height="20"
        width="20">
    </e-layersettings-markersetting>
    
    <!-- Star markers for featured locations -->
    <e-layersettings-markersetting 
        visible="true"
        dataSource="@featuredLocations"
        shape="Star"
        fill="#FFD700"
        height="25"
        width="25">
    </e-layersettings-markersetting>
    
    <!-- Diamond markers for warehouses -->
    <e-layersettings-markersetting 
        visible="true"
        dataSource="@warehouses"
        shape="Diamond"
        fill="#32CD32"
        height="20"
        width="20">
    </e-layersettings-markersetting>
</e-layersettings-markersettings>
```

### Image Markers

Use custom images as markers:

```cshtml
@{
    var storeLocations = new[] {
        new { 
            Latitude = 34.0522, 
            Longitude = -118.2437,
            ImagePath = "/images/markers/store-icon.png"
        }
    };
}

<e-layersettings-markersetting 
    visible="true"
    dataSource="storeLocations"
    shape="Image"
    imageUrl="/images/markers/store-icon.png"
    height="30"
    width="30">
</e-layersettings-markersetting>
```

**Image Best Practices:**
- Use PNG with transparency
- Recommended size: 20x20 to 40x40 pixels
- Optimize file size for performance
- Use SVG for scalable markers

## Marker Customization

### Size and Dimensions

```cshtml
<e-layersettings-markersetting 
    height="25"           <!-- Height in pixels -->
    width="25"            <!-- Width in pixels -->
    visible="true"
    dataSource="locations">
</e-layersettings-markersetting>
```

### Colors and Fill

```cshtml
<e-layersettings-markersetting 
    fill="#FF6347"        <!-- Fill color -->
    opacity="0.8"         <!-- Transparency (0-1) -->
    visible="true"
    dataSource="locations">
</e-layersettings-markersetting>
```

### Border Styling

```cshtml
<e-layersettings-markersetting 
    visible="true"
    dataSource="locations"
    shape="Circle"
    fill="#FF6347"
    height="20"
    width="20">
    <e-markersettings-border 
        color="#000000"   <!-- Border color -->
        width="2">        <!-- Border width -->
    </e-markersettings-border>
</e-layersettings-markersetting>
```

### Animation

```cshtml
<e-layersettings-markersetting 
    animationDuration="1000"    <!-- Animation duration in ms -->
    animationDelay="0"          <!-- Delay before animation starts -->
    visible="true"
    dataSource="locations">
</e-layersettings-markersetting>
```

### Offset Positioning

Fine-tune marker position relative to coordinates:

```cshtml
@{    
    var offset = new { x = 35, y = -10 };
}
<e-layersettings-markersetting 
    visible="true"
    dataSource="locations" offset="offset">
</e-layersettings-markersetting>
```

**Offset Use Cases:**
- Align marker with label
- Prevent overlap with other markers
- Position marker above/below coordinate point

## Template-Based Markers

Create custom HTML markers for advanced visualizations:

### Basic Template

```cshtml
@{
    var cities = new[] {
        new { Name = "New York", Latitude = 40.7128, Longitude = -74.0060, Population = "8.4M" }
    };
}

<e-layersettings-markersetting 
    visible="true"
    dataSource="cities" template="template">
</e-layersettings-markersetting>

<div id="template" style="background: #FF6347; color: white; padding: 5px 10px; border-radius: 5px; font-size: 12px;">
    <strong>${Name}</strong><br/>
    Pop: ${Population}
</div>
```

### Advanced Template with Conditional Styling

```cshtml
@{
    var stores = new[] {
        new { Name = "Store A", Latitude = 34.05, Longitude = -118.24, Status = "Active", Sales = 150000 },
        new { Name = "Store B", Latitude = 40.71, Longitude = -74.00, Status = "Closed", Sales = 50000 }
    };
}

<e-layersettings-markersetting 
    visible="true"
    dataSource="stores" template="template">
</e-layersettings-markersetting>

<div id="template" style="background: ${Status === 'Active' ? '#28a745' : '#dc3545'}; 
            color: white; 
            padding: 8px; 
            border-radius: 8px; 
            box-shadow: 0 2px 6px rgba(0,0,0,0.3);
            min-width: 120px;">
    <div style="font-weight: bold; font-size: 14px;">${Name}</div>
    <div style="font-size: 11px;">Status: ${Status}</div>
    <div style="font-size: 11px;">Sales: $${Sales.toLocaleString()}</div>
</div>
```

### Template with Icons

```cshtml
<e-layersettings-markersetting visible="true" dataSource="stores" template="template">
</e-layersettings-markersetting>

<div id="template" style="text-align: center;">
    <i class="fas fa-map-marker-alt" style="font-size: 30px; color: #FF6347;"></i>
    <div style="background: white; padding: 4px 8px; border-radius: 3px; margin-top: 5px; font-size: 11px; font-weight: bold;">
        ${Name}
    </div>
</div>
```

**Template Variables:**
- Access data properties using `${PropertyName}`
- Use JavaScript expressions: `${Value > 100 ? 'High' : 'Low'}`
- Apply conditional styling with ternary operators

## Multiple Marker Sets

Display different types of markers on the same map:

```cshtml
@{
    var worldMap = JsonConvert.DeserializeObject(System.IO.File.ReadAllText("wwwroot/maps/world.json"));
    
    var offices = new[] {
        new { Name = "HQ", Latitude = 40.7128, Longitude = -74.0060, Type = "Headquarters" },
        new { Name = "Branch 1", Latitude = 34.0522, Longitude = -118.2437, Type = "Branch" }
    };
    
    var warehouses = new[] {
        new { Name = "Warehouse A", Latitude = 41.8781, Longitude = -87.6298 },
        new { Name = "Warehouse B", Latitude = 29.7604, Longitude = -95.3698 }
    };
    
    var distributionCenters = new[] {
        new { Name = "DC 1", Latitude = 33.4484, Longitude = -112.0740 },
        new { Name = "DC 2", Latitude = 39.7392, Longitude = -104.9903 }
    };
}

<ejs-maps id="maps" height="600px">
    <e-maps-layers>
        <e-maps-layer shapeData="worldMap">
            <e-layersettings-shapesettings fill="#E8E8E8"></e-layersettings-shapesettings>
            
            <e-layersettings-markersettings>
                <!-- Office markers (stars) -->
                <e-layersettings-markersetting 
                    visible="true"
                    dataSource="offices"
                    shape="Star"
                    fill="#FFD700"
                    height="25"
                    width="25">
                    <e-markersettings-tooltipsettings 
                        visible="true" 
                        valuePath="Name">
                    </e-markersettings-tooltipsettings>
                </e-layersettings-markersetting>
                
                <!-- Warehouse markers (rectangles) -->
                <e-layersettings-markersetting 
                    visible="true"
                    dataSource="warehouses"
                    shape="Rectangle"
                    fill="#32CD32"
                    height="20"
                    width="20">
                    <e-markersettings-tooltipsettings 
                        visible="true" 
                        valuePath="Name">
                    </e-markersettings-tooltipsettings>
                </e-layersettings-markersetting>
                
                <!-- Distribution center markers (diamonds) -->
                <e-layersettings-markersetting 
                    visible="true"
                    dataSource="distributionCenters"
                    shape="Diamond"
                    fill="#1E90FF"
                    height="20"
                    width="20">
                    <e-markersettings-tooltipsettings 
                        visible="true" 
                        valuePath="Name">
                    </e-markersettings-tooltipsettings>
                </e-layersettings-markersetting>
            </e-layersettings-markersettings>
        </e-maps-layer>
    </e-maps-layers>
</ejs-maps>
```

## Marker Events

### Click Event

```cshtml
<ejs-maps id="maps" markerClick="onMarkerClick">
    <e-maps-layers>
        <e-maps-layer shapeData="worldMap">
            <e-layersettings-markersettings>
                <e-layersettings-markersetting 
                    visible="true"
                    dataSource="locations">
                </e-layersettings-markersetting>
            </e-layersettings-markersettings>
        </e-maps-layer>
    </e-maps-layers>
</ejs-maps>

<script>
    function onMarkerClick(args) {
        console.log('Marker clicked:', args.data);
        alert('Location: ' + args.data.Name);
        
        // Access marker data
        var latitude = args.data.Latitude;
        var longitude = args.data.Longitude;
        
        // Perform custom action (e.g., show details panel)
        showLocationDetails(args.data);
    }
</script>
```

### Mouse Events

```cshtml
<ejs-maps id="maps" 
          markerClick="onMarkerClick"
          markerMouseMove="onMarkerHover">
</ejs-maps>

<script>
    function onMarkerHover(args) {
        // Change cursor or highlight
        args.target.style.cursor = 'pointer';
    }
</script>
```

## Marker Tooltips

### Basic Tooltip

```cshtml
<e-layersettings-markersetting 
    visible="true"
    dataSource="cities">
    <e-markersettings-tooltipsettings 
        visible="true" 
        valuePath="Name">
    </e-markersettings-tooltipsettings>
</e-layersettings-markersetting>
```

### Custom Tooltip Template

```cshtml
<e-layersettings-markersetting 
    visible="true"
    dataSource="cities">
    <e-markersettings-tooltipsettings visible="true" template="template">
    </e-markersettings-tooltipsettings>
</e-layersettings-markersetting>

<div id="template" style="padding: 10px; background: white; border-radius: 5px;">
    <h4 style="margin: 0 0 5px 0;">${Name}</h4>
    <div><strong>Population:</strong> ${Population}</div>
    <div><strong>Country:</strong> ${Country}</div>
    <div><strong>Coordinates:</strong> ${Latitude}, ${Longitude}</div>
</div>
```

### Styled Tooltip

```cshtml
<e-markersettings-tooltipsettings visible="true">
    <e-marker-tooltip-textstyle 
        fontFamily="Arial"
        size="14px"
        color="#FFFFFF">
    </e-marker-tooltip-textstyle>
    <e-marker-tooltip-border 
        color="#FF6347"
        width="2">
    </e-marker-tooltip-border>
</e-markersettings-tooltipsettings>
```

## Marker Legend

Add legend for different marker types:

```cshtml
<ejs-maps id="maps">
    <e-maps-legendsettings 
        visible="true"
        position="Bottom"
        type="Markers">
    </e-maps-legendsettings>
    
    <e-maps-layers>
        <e-maps-layer shapeData="worldMap">
            <e-layersettings-markersettings>
                <e-layersettings-markersetting 
                    visible="true"
                    dataSource="offices"
                    shape="Star"
                    fill="#FFD700"
                    legendText="Offices">
                </e-layersettings-markersetting>
                
                <e-layersettings-markersetting 
                    visible="true"
                    dataSource="warehouses"
                    shape="Rectangle"
                    fill="#32CD32"
                    legendText="Warehouses">
                </e-layersettings-markersetting>
            </e-layersettings-markersettings>
        </e-maps-layer>
    </e-maps-layers>
</ejs-maps>
```

## Complete Examples

### Example 1: Store Locator Map

```cshtml
@page
@using Syncfusion.EJ2.Maps
@using Newtonsoft.Json

@{
    var usaMap = JsonConvert.DeserializeObject(System.IO.File.ReadAllText("wwwroot/maps/usa.json"));
    
    var stores = new[] {
        new { Name = "New York Store", Latitude = 40.7128, Longitude = -74.0060, Status = "Open", Sales = 250000 },
        new { Name = "LA Store", Latitude = 34.0522, Longitude = -118.2437, Status = "Open", Sales = 320000 },
        new { Name = "Chicago Store", Latitude = 41.8781, Longitude = -87.6298, Status = "Closed", Sales = 150000 },
        new { Name = "Houston Store", Latitude = 29.7604, Longitude = -95.3698, Status = "Open", Sales = 280000 }
    };
}

<div style="padding: 20px;">
    <h2>Store Locations</h2>
    
    <ejs-maps id="storeMap" height="600px">
        <e-maps-zoomsettings enable="true" zoomFactor="3"></e-maps-zoomsettings>
        
        <e-maps-layers>
            <e-maps-layer shapeData="usaMap">
                <e-layersettings-shapesettings fill="#E8E8E8">
                    <e-layers-shapesettings-border color="#FFFFFF" width="1"></e-layers-shapesettings-border>
                </e-layersettings-shapesettings>
                
                <e-layersettings-markersettings>
                    <e-layersettings-markersetting 
                        visible="true"
                        dataSource="stores"
                        shape="Circle"
                        height="20"
                        width="20" template="markerTemplate">
                        <e-markersettings-tooltipsettings visible="true" template="tooltipTemplate">
                        </e-markersettings-tooltipsettings>
                    </e-layersettings-markersetting>
                </e-layersettings-markersettings>
            </e-maps-layer>
        </e-maps-layers>
    </ejs-maps>
</div>

<div id="markerTemplate" style="background: ${Status === 'Open' ? '#28a745' : '#dc3545'}; 
            color: white; 
            padding: 8px 12px; 
            border-radius: 20px; 
            font-size: 12px;
            font-weight: bold;
            box-shadow: 0 2px 8px rgba(0,0,0,0.3);">
    ${Name}
</div>


<div id="tooltipTemplate" style="padding: 10px;">
    <h4 style="margin: 0 0 8px 0;">${Name}</h4>
    <div><strong>Status:</strong> <span style="color: ${Status === 'Open' ? '#28a745' : '#dc3545'};">${Status}</span></div>
    <div><strong>Annual Sales:</strong> $${Sales.toLocaleString()}</div>
</div>
```

### Example 2: Event Map with Custom Icons

```cshtml
@{
    var worldMap = JsonConvert.DeserializeObject(System.IO.File.ReadAllText("wwwroot/maps/world.json"));
    
    var events = new[] {
        new { City = "Paris", Latitude = 48.8566, Longitude = 2.3522, Type = "Conference" },
        new { City = "Berlin", Latitude = 52.5200, Longitude = 13.4050, Type = "Workshop" },
        new { City = "London", Latitude = 51.5074, Longitude = -0.1278, Type = "Meetup" }
    };
}

<ejs-maps id="eventMap" height="600px">
    <e-maps-layers>
        <e-maps-layer shapeData="worldMap">
            <e-layersettings-markersettings>
                <e-layersettings-markersetting 
                    visible="true"
                    dataSource="events"
                    shape="Balloon"
                    fill="#FF6347"
                    height="25"
                    width="25">
                    <e-markersettings-tooltipsettings 
                        visible="true" 
                        valuePath="City" template="tooltipTemplate">
                    </e-markersettings-tooltipsettings>
                </e-layersettings-markersetting>
            </e-layersettings-markersettings>
        </e-maps-layer>
    </e-maps-layers>
</ejs-maps>

<div id="tooltipTemplate" style="padding: 8px;">
    <strong>${City}</strong><br/>
    Type: ${Type}
</div>
```

## Best Practices

### 1. Limit Marker Count
- Keep markers under 50-100 for optimal performance
- Use marker clustering for large datasets
- Filter markers based on zoom level

### 2. Use Appropriate Shapes
- Circle - Generic locations
- Star - Featured/important locations
- Diamond - Special categories
- Image - Brand-specific markers

### 3. Consistent Sizing
- Use uniform sizes within marker categories
- Reserve larger sizes for important markers
- Scale marker size with map zoom if needed

### 4. Color Coding
- Use distinct colors for different categories
- Ensure sufficient contrast with map background
- Consider color-blind users (avoid red/green only)

### 5. Tooltip Best Practices
- Keep tooltip content concise
- Include only essential information
- Use clear formatting and spacing

## Troubleshooting

### Markers Not Appearing

**Problem:** Markers configured but not visible.

**Solutions:**
1. Verify `visible="true"` is set
2. Check latitude/longitude values are correct
3. Ensure data source has "Latitude" and "Longitude" properties (case-sensitive)
4. Verify coordinates are within map bounds
5. Check fill color isn't transparent

### Markers in Wrong Location

**Problem:** Markers appear at incorrect positions.

**Solutions:**
1. Verify latitude/longitude are not swapped
2. Check coordinate format (decimal degrees, not DMS)
3. Ensure correct coordinate system (WGS84)
4. Verify map projection matches coordinate system

### Template Markers Not Rendering

**Problem:** Template shows as blank or error.

**Solutions:**
1. Check template syntax for errors
2. Verify property names in `${PropertyName}` match data
3. Escape special characters in template
4. Test template with simple HTML first

### Performance Issues with Many Markers

**Problem:** Map is slow with many markers.

**Solutions:**
1. Implement marker clustering
2. Filter markers by zoom level
3. Use simpler shapes instead of templates
4. Lazy load markers as user zooms/pans
5. Reduce marker animation duration

### Tooltip Not Showing

**Problem:** Tooltip configured but not appearing.

**Solutions:**
1. Verify `visible="true"` in tooltip settings
2. Check `valuePath` property exists in data
3. Ensure tooltip isn't hidden behind other elements
4. Test with simple tooltip before custom template
