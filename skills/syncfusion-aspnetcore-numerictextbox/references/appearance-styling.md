# Appearance and Styling in ASP.NET Core NumericTextBox

## CSS Structure

### NumericTextBox DOM Structure

```html
<span class="e-input-group e-numeric e-control-wrapper">
    <input class="e-input" type="text" />
    <span class="e-input-group-icon e-numeric-up-icon"></span>
    <span class="e-input-group-icon e-numeric-down-icon"></span>
</span>
```

### Key CSS Classes

| Class | Purpose |
|-------|---------|
| `.e-numeric` | Main container |
| `.e-control-wrapper` | Control wrapper |
| `.e-input-group` | Input group styling |
| `.e-input` | Input field |
| `.e-input-group-icon` | Spinner button |
| `.e-numeric-up-icon` | Up button |
| `.e-numeric-down-icon` | Down button |
| `.e-error` | Error state |
| `.e-warning` | Warning state |
| `.e-success` | Success state |

## Input Styling

### Basic Customization

```css
/* Input field styling */
.e-numeric.e-control-wrapper input.e-input {
    height: 40px;
    font-size: 16px;
    padding: 8px 12px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

/* Focus state */
.e-numeric.e-control-wrapper input.e-input:focus {
    border-color: #0078d4;
    box-shadow: 0 0 0 3px rgba(0, 120, 212, 0.1);
}

/* Placeholder */
.e-numeric.e-control-wrapper input.e-input::placeholder {
    color: #999;
    opacity: 1;
}
```

### Input Sizes

```css
/* Large input -->
.e-numeric.large input.e-input {
    height: 50px;
    font-size: 18px;
    padding: 12px 16px;
}

/* Small input -->
.e-numeric.small input.e-input {
    height: 32px;
    font-size: 13px;
    padding: 4px 8px;
}

/* Compact input -->
.e-numeric.compact input.e-input {
    height: 28px;
    font-size: 12px;
}
```

### Input Colors

```css
/* Green for income/profits -->
.e-numeric.currency-input input.e-input {
    color: #2d8a3e;
    text-align: right;
    font-weight: 500;
}

/* Red for expenses -->
.e-numeric.expense-input input.e-input {
    color: #d32f2f;
    text-align: right;
}

/* Outlined style -->
.e-numeric.outlined input.e-input {
    border: 2px solid #2d3748;
    background-color: #f7f7f7;
    border-radius: 6px;
}
```

## Spinner Button Styling

### Basic Spinner

```css
.e-numeric .e-input-group-icon {
    font-size: 16px;
    width: 32px;
    height: 20px;
    color: #2d3748;
    background-color: #f0f0f0;
    border-left: 1px solid #ccc;
    cursor: pointer;
}

.e-numeric .e-input-group-icon:hover {
    background-color: #e0e0e0;
}

.e-numeric .e-input-group-icon:active {
    background-color: #d0d0d0;
}
```

### Colored Spinners

```css
.e-numeric.success-spinners .e-input-group-icon {
    color: #2d8a3e;
    background-color: #e8f5e9;
}

.e-numeric.danger-spinners .e-input-group-icon {
    color: #d32f2f;
    background-color: #ffebee;
}

.e-numeric.warning-spinners .e-input-group-icon {
    color: #f57c00;
    background-color: #fff3e0;
}
```

### Hide Spinners

```html
<ejs-numerictextbox id="price"
    value="0"
    show-spin-button="false">
</ejs-numerictextbox>
```

Or with CSS:
```css
.e-numeric.no-spinners .e-input-group-icon {
    display: none;
}
```

## Theme Integration

### Available Themes

```html
<!-- Fluent (default) -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/24.1.48/fluent.css" />

<!-- Bootstrap -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/24.1.48/bootstrap.css" />

<!-- Material -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/24.1.48/material.css" />

<!-- Tailwind -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/24.1.48/tailwind.css" />

<!-- High Contrast -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/24.1.48/highcontrast.css" />
```

## Responsive Design

### Mobile-Friendly Sizes

```css
/* Base: mobile sizes */
.e-numeric input.e-input {
    height: 44px;
    font-size: 16px;
}

.e-numeric .e-input-group-icon {
    width: 44px;
    height: 44px;
}

/* Tablet -->
@media (min-width: 768px) {
    .e-numeric input.e-input {
        height: 40px;
        font-size: 14px;
    }
    
    .e-numeric .e-input-group-icon {
        width: 32px;
        height: 20px;
    }
}

/* Desktop -->
@media (min-width: 1024px) {
    .e-numeric input.e-input {
        height: 36px;
        font-size: 13px;
    }
}
```

## Validation States

### CSS Classes

```html
<!-- Success state -->
<ejs-numerictextbox id="salary"
    value="50000"
    css-class="e-success">
</ejs-numerictextbox>

<!-- Error state -->
<ejs-numerictextbox id="age"
    value="150"
    css-class="e-error">
</ejs-numerictextbox>

<!-- Warning state -->
<ejs-numerictextbox id="discount"
    value="150"
    css-class="e-warning">
</ejs-numerictextbox>
```

### State Styling

```css
.e-numeric.e-success input.e-input {
    border-color: #2d8a3e;
    background-color: #f1f8f4;
}

.e-numeric.e-error input.e-input {
    border-color: #d32f2f;
    background-color: #ffebee;
}

.e-numeric.e-warning input.e-input {
    border-color: #f57c00;
    background-color: #fff3e0;
}
```

## Examples

### Financial Dashboard Widget

```html
<div class="dashboard-widget">
    <h6>Key Metrics</h6>
    
    <div class="metrics-grid">
        <div class="metric">
            <label>Revenue</label>
            <ejs-numerictextbox id="revenue"
                value="1500000"
                format="c0"
                css-class="metric-input revenue"
                read-only="true">
            </ejs-numerictextbox>
        </div>

        <div class="metric">
            <label>Growth</label>
            <ejs-numerictextbox id="growth"
                value="0.15"
                format="p1"
                css-class="metric-input growth"
                read-only="true">
            </ejs-numerictextbox>
        </div>
    </div>
</div>

<style>
    .dashboard-widget {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        border-radius: 8px;
        padding: 20px;
        color: white;
    }

    .metrics-grid {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
        gap: 20px;
    }

    .metric {
        background: rgba(255, 255, 255, 0.1);
        padding: 15px;
        border-radius: 6px;
    }

    .metric input.e-input {
        background: transparent;
        border: none;
        color: white;
        font-weight: bold;
    }
</style>
```

### Data Entry Form

```html
<form asp-action="Save" method="post">
    <div class="form-group">
        <label asp-for="ItemPrice"></label>
        <ejs-numerictextbox asp-for="ItemPrice"
            format="c2"
            min="0"
            css-class="finance-input income-input">
        </ejs-numerictextbox>
    </div>

    <div class="form-group">
        <label asp-for="Quantity"></label>
        <ejs-numerictextbox asp-for="Quantity"
            format="n0"
            min="1"
            css-class="compact-input">
        </ejs-numerictextbox>
    </div>

    <button type="submit" class="btn btn-primary">Submit</button>
</form>

<style>
    .finance-input input.e-input {
        font-weight: 500;
        text-align: right;
        font-family: 'Courier New', monospace;
    }

    .income-input input.e-input {
        color: #2d8a3e;
    }

    .compact-input input.e-input {
        height: 32px;
        font-size: 13px;
    }
</style>
```
