# Number Formats in ASP.NET Core NumericTextBox

## Table of Contents
- [Standard Format Specifiers](#standard-format-specifiers)
- [Currency Formatting](#currency-formatting)
- [Percentage Formatting](#percentage-formatting)
- [Custom Number Formats](#custom-number-formats)
- [Locale-Specific Formatting](#locale-specific-formatting)
- [Tag Helper Examples](#tag-helper-examples)

## Standard Format Specifiers

| Specifier | Name | Example | Output |
|-----------|------|---------|--------|
| `c` or `C` | Currency | format="c" | $1,234.50 |
| `c2` | Currency, 2 decimals | format="c2" | $1,234.50 |
| `p` or `P` | Percent | format="p" | 25.00 % |
| `p2` | Percent, 2 decimals | format="p2" | 25.00% |
| `n` | Number | format="n" | 1,234.50 |
| `n0` | Number, 0 decimals | format="n0" | 1,235 |
| `f` | Fixed-point | format="f2" | 1234.50 |
| `e` | Scientific notation | format="e2" | 1.23e+003 |

## Currency Formatting

### Basic Currency Format

```html
<!-- US Format: $1,234.56 -->
<ejs-numerictextbox id="price-us"
    value="1234.56"
    format="c2"
    locale="en-US">
</ejs-numerictextbox>

<!-- German Format: 1.234,56 € -->
<ejs-numerictextbox id="price-de"
    value="1234.56"
    format="c2"
    locale="de-DE">
</ejs-numerictextbox>

<!-- French Format: 1 234,56 € -->
<ejs-numerictextbox id="price-fr"
    value="1234.56"
    format="c2"
    locale="fr-FR">
</ejs-numerictextbox>
```

### Currency with No Decimals

```html
<ejs-numerictextbox id="price"
    value="1234"
    format="c0"
    locale="en-US">
</ejs-numerictextbox>
<!-- Output: $1,234 -->
```

### Multi-Currency Example

```html
<div class="row">
    <div class="col-md-6">
        <label>Price (USD)</label>
        <ejs-numerictextbox id="price-usd"
            value="100"
            format="c2"
            locale="en-US">
        </ejs-numerictextbox>
    </div>

    <div class="col-md-6">
        <label>Price (EUR)</label>
        <ejs-numerictextbox id="price-eur"
            value="100"
            format="c2"
            locale="de-DE">
        </ejs-numerictextbox>
    </div>
</div>
```

## Percentage Formatting

### Basic Percentage Format

```html
<!-- Percentage format multiplies by 100 for display -->
<ejs-numerictextbox id="tax-rate"
    value="0.15"
    format="p2"
    locale="en-US">
</ejs-numerictextbox>
<!-- Displays: 15.00% -->
```

### Percentage Input Form

```html
<div class="form-group">
    <label>Tax Rate</label>
    <ejs-numerictextbox id="tax-rate"
        value="0.08"
        format="p2"
        min="0"
        max="1"
        step="0.01"
        float-label-type="Auto">
    </ejs-numerictextbox>
    <small class="form-text">Enter as decimal (0.08 = 8%)</small>
</div>

<div class="form-group">
    <label>Discount Percentage</label>
    <ejs-numerictextbox id="discount"
        value="0.10"
        format="p2"
        min="0"
        max="1"
        change="applyDiscount">
    </ejs-numerictextbox>
</div>

<div class="form-group">
    <label>Final Amount</label>
    <ejs-numerictextbox id="final-amount"
        format="c2"
        read-only="true">
    </ejs-numerictextbox>
</div>

<script>
    function applyDiscount() {
        var baseAmount = 100;
        var discountPercent = document.querySelector('input[id="discount"]').ej2_instances[0].value || 0;
        var finalAmount = baseAmount * (1 - discountPercent);
        
        var finalControl = document.querySelector('input[id="final-amount"]');
        var finalInstance = finalControl.ej2_instances[0];
        finalInstance.value = finalAmount;
    }
</script>
```

## Custom Number Formats

### Custom Format Patterns

```html
<!-- Leading zeros -->
<ejs-numerictextbox id="inventory"
    value="25"
    format="000000">
</ejs-numerictextbox>
<!-- Displays: 000025 -->

<!-- Thousands with decimals -->
<ejs-numerictextbox id="amount"
    value="1234567.89"
    format="#,##0.00">
</ejs-numerictextbox>
<!-- Displays: 1,234,567.89 -->

<!-- Accounting format for negatives -->
<ejs-numerictextbox id="balance"
    value="-1234.50"
    format="#,##0.00;(#,##0.00)">
</ejs-numerictextbox>
<!-- Displays: (1,234.50) -->
```

## Locale-Specific Formatting

### Culture Mapping to Locales

```csharp
// In Controller
public class PaymentController : Controller
{
    public IActionResult Create()
    {
        var cultures = new Dictionary<string, string>
        {
            { "en-US", "English (US)" },
            { "de-DE", "Deutsch (Germany)" },
            { "fr-FR", "Français (France)" },
            { "ja-JP", "日本語 (Japan)" },
            { "en-IN", "English (India)" }
        };
        
        return View(cultures);
    }
}
```

### Per-NumericTextBox Locale

```html
<!-- English (US) -->
<ejs-numerictextbox id="num1"
    value="1234.50"
    format="n2"
    locale="en-US">
</ejs-numerictextbox>
<!-- Output: 1,234.50 -->

<!-- German: 1.234,50 -->
<ejs-numerictextbox id="num2"
    value="1234.50"
    format="n2"
    locale="de-DE">
</ejs-numerictextbox>
<!-- Output: 1.234,50 -->

<!-- Indian English: supports lakhs -->
<ejs-numerictextbox id="num3"
    value="123456.78"
    format="n2"
    locale="en-IN">
</ejs-numerictextbox>
<!-- Output: 1,23,456.78 -->
```

## Tag Helper Examples

### Employee Salary with Multiple Formats

```html
@model EmployeeModel

<form asp-action="Save" method="post">
    <!-- Salary in currency -->
    <div class="form-group">
        <label asp-for="BaseSalary"></label>
        <ejs-numerictextbox asp-for="BaseSalary"
            format="c2"
            min="20000"
            max="500000"
            step="1000"
            locale="en-US">
        </ejs-numerictextbox>
    </div>

    <!-- Bonus as percentage -->
    <div class="form-group">
        <label asp-for="BonusPercent"></label>
        <ejs-numerictextbox asp-for="BonusPercent"
            format="p2"
            min="0"
            max="1"
            step="0.05">
        </ejs-numerictextbox>
    </div>

    <!-- Commission as number -->
    <div class="form-group">
        <label asp-for="Commission"></label>
        <ejs-numerictextbox asp-for="Commission"
            format="n2"
            min="0">
        </ejs-numerictextbox>
    </div>

    <button type="submit" class="btn btn-primary">Save</button>
</form>
```

### Financial Report Display

```html
<table class="table table-bordered">
    <tr>
        <td>Revenue</td>
        <td>
            <ejs-numerictextbox id="revenue"
                value="150000"
                format="c2"
                read-only="true">
            </ejs-numerictextbox>
        </td>
    </tr>
    <tr>
        <td>Expenses</td>
        <td>
            <ejs-numerictextbox id="expenses"
                value="75000"
                format="c2"
                read-only="true">
            </ejs-numerictextbox>
        </td>
    </tr>
    <tr>
        <td>Profit Margin</td>
        <td>
            <ejs-numerictextbox id="margin"
                value="0.50"
                format="p2"
                read-only="true">
            </ejs-numerictextbox>
        </td>
    </tr>
</table>
```

## Troubleshooting

### Format Not Applied
**Solution:** Format applies on blur. Ensure control loses focus to see formatting.

### Wrong Decimal Separator
**Solution:** Set locale to match region:
```html
<!-- Germany uses comma as decimal: 1.234,56 -->
<ejs-numerictextbox format="c2" locale="de-DE"></ejs-numerictextbox>
```

### Currency Symbol Wrong
**Solution:** Locale determines currency symbol:
```html
<!-- USD -->
<ejs-numerictextbox locale="en-US" format="c2"></ejs-numerictextbox>
<!-- EUR -->
<ejs-numerictextbox locale="de-DE" format="c2"></ejs-numerictextbox>
```
