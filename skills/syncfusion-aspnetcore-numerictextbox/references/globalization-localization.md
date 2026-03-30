# Globalization and Localization in ASP.NET Core NumericTextBox

## Culture-Based Number Formatting

### Setting Culture Application-Wide

**In Program.cs:**

```csharp
var builder = WebApplication.CreateBuilder(args);

// Add services
builder.Services.AddControllersWithViews();

// Configure localization
var supportedCultures = new[]
{
    new CultureInfo("en-US"),
    new CultureInfo("de-DE"),
    new CultureInfo("fr-FR"),
    new CultureInfo("ja-JP")
};

builder.Services.Configure<RequestLocalizationOptions>(options =>
{
    options.DefaultRequestCulture = new RequestCulture("en-US");
    options.SupportedCultures = supportedCultures;
    options.SupportedUICultures = supportedCultures;
});

var app = builder.Build();

// Use localization middleware
app.UseRequestLocalization();

app.Run();
```

### Per-Request Culture

**In Controller:**

```csharp
public IActionResult SetCulture(string culture)
{
    var cultureInfo = new CultureInfo(culture);
    CultureInfo.CurrentCulture = cultureInfo;
    CultureInfo.CurrentUICulture = cultureInfo;
    
    HttpContext.Session.SetString("Culture", culture);
    return RedirectToAction("Index");
}
```

## Locale-Specific Formatting

### Common Culture Codes

| Culture | Code | Currency | Decimal | Format |
|---------|------|----------|---------|--------|
| English (US) | en-US | $ | . | $1,234.56 |
| German | de-DE | € | , | 1.234,56 € |
| French | fr-FR | € | , | 1 234,56 € |
| British | en-GB | £ | . | £1,234.56 |
| Japanese | ja-JP | ¥ | . | ¥1,235 |
| Chinese | zh-CN | ¥ | . | ¥1,234.56 |
| Indian | en-IN | ₹ | . | ₹1,23,456.78 |

### Tag Helper Examples

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

<!-- Indian Format (Lakhs): ₹1,23,456.78 -->
<ejs-numerictextbox id="price-in"
    value="123456.78"
    format="c2"
    locale="en-IN">
</ejs-numerictextbox>
```

## Multi-Currency Support

### Currency Selection with Localization

```html
<div class="row">
    <div class="col-md-6">
        <label>Select Currency</label>
        <select id="currency-select" class="form-control">
            <option value="en-US">🇺🇸 US Dollar ($)</option>
            <option value="de-DE">🇪🇺 Euro (€)</option>
            <option value="en-GB">🇬🇧 British Pound (£)</option>
            <option value="ja-JP">🇯🇵 Japanese Yen (¥)</option>
            <option value="en-IN">🇮🇳 Indian Rupee (₹)</option>
        </select>
    </div>

    <div class="col-md-6">
        <label>Amount</label>
        <ejs-numerictextbox id="amount"
            value="0"
            format="c2"
            min="0"
            step="0.01"
            float-label-type="Auto">
        </ejs-numerictextbox>
    </div>
</div>

<script>
    document.getElementById('currency-select').addEventListener('change', function(e) {
        var locale = e.target.value;
        var amountControl = document.querySelector('input[id="amount"]');
        var amountInstance = amountControl.ej2_instances[0];
        amountInstance.locale = locale;
    });
</script>
```

## RTL Language Support

### Arabic Support

```html
<div dir="rtl" lang="ar">
    <h3>نموذج الدفع</h3>
    
    <div class="form-group">
        <label>المبلغ</label>
        <ejs-numerictextbox id="amount-ar"
            value="1000"
            format="c2"
            locale="ar-SA"
            enable-rtl="true">
        </ejs-numerictextbox>
    </div>

    <div class="form-group">
        <label>الكمية</label>
        <ejs-numerictextbox id="quantity-ar"
            value="1"
            format="n0"
            locale="ar-SA"
            enable-rtl="true">
        </ejs-numerictextbox>
    </div>
</div>

<style>
    [dir="rtl"] .e-numeric input.e-input {
        direction: rtl;
        text-align: right;
    }

    [dir="rtl"] .e-numeric .e-input-group-icon {
        order: -1;
    }
</style>
```

### Hebrew Support

```html
<div dir="rtl" lang="he">
    <ejs-numerictextbox id="price-he"
        value="0"
        format="c2"
        locale="he-IL"
        enable-rtl="true">
    </ejs-numerictextbox>
</div>
```

## Regional Examples

### US English (en-US)

```html
<div class="region-us">
    <h5>US Format</h5>
    
    <div class="form-group">
        <label>Salary</label>
        <ejs-numerictextbox id="salary-us"
            value="75000"
            format="c2"
            locale="en-US">
        </ejs-numerictextbox>
        <!-- Output: $75,000.00 -->
    </div>

    <div class="form-group">
        <label>Tax Rate</label>
        <ejs-numerictextbox id="tax-us"
            value="0.0825"
            format="p2"
            locale="en-US">
        </ejs-numerictextbox>
        <!-- Output: 8.25% -->
    </div>
</div>
```

### German (de-DE)

```html
<div class="region-de">
    <h5>German Format</h5>
    
    <div class="form-group">
        <label>Gehalt</label>
        <ejs-numerictextbox id="salary-de"
            value="75000"
            format="c2"
            locale="de-DE">
        </ejs-numerictextbox>
        <!-- Output: 75.000,00 € -->
    </div>

    <div class="form-group">
        <label>Steuersatz</label>
        <ejs-numerictextbox id="tax-de"
            value="0.19"
            format="p2"
            locale="de-DE">
        </ejs-numerictextbox>
        <!-- Output: 19,00% -->
    </div>
</div>
```

### Indian English (en-IN) with Lakhs

```html
<div class="region-in">
    <h5>Indian Format (Lakhs)</h5>
    
    <div class="form-group">
        <label>Salary</label>
        <ejs-numerictextbox id="salary-in"
            value="1234567"
            format="c2"
            locale="en-IN">
        </ejs-numerictextbox>
        <!-- Output: ₹12,34,567.00 -->
    </div>
</div>
```

### Japanese (ja-JP)

```html
<div class="region-jp">
    <h5>Japanese Format</h5>
    
    <div class="form-group">
        <label>給与</label>
        <ejs-numerictextbox id="salary-jp"
            value="5000000"
            format="c0"
            locale="ja-JP">
        </ejs-numerictextbox>
        <!-- Output: ¥5,000,000 -->
    </div>
</div>
```

## Spinner Localization

### Localized Tooltips

Syncfusion automatically localizes spinner button tooltips based on locale:

```html
<!-- English tooltips: "Increment Value" / "Decrement Value" -->
<ejs-numerictextbox id="qty1"
    value="0"
    locale="en-US">
</ejs-numerictextbox>

<!-- German tooltips: "Wert erhöhen" / "Wert verringern" -->
<ejs-numerictextbox id="qty2"
    value="0"
    locale="de-DE">
</ejs-numerictextbox>

<!-- French tooltips: "Augmenter la valeur" / "Diminuer la valeur" -->
<ejs-numerictextbox id="qty3"
    value="0"
    locale="fr-FR">
</ejs-numerictextbox>
```

## Troubleshooting

### Locale Not Applied

**Solution:** Ensure locale is set before initialization:

```html
<!-- ✓ Correct -->
<ejs-numerictextbox value="1234.56" format="c2" locale="de-DE"></ejs-numerictextbox>

<!-- ✗ Wrong - locale missing -->
<ejs-numerictextbox value="1234.56" format="c2"></ejs-numerictextbox>
```

### Wrong Currency Symbol

**Solution:** Verify locale matches currency:

```html
<!-- USD -->
<ejs-numerictextbox locale="en-US" format="c2"></ejs-numerictextbox>

<!-- EUR -->
<ejs-numerictextbox locale="de-DE" format="c2"></ejs-numerictextbox>

<!-- GBP -->
<ejs-numerictextbox locale="en-GB" format="c2"></ejs-numerictextbox>
```

### RTL Not Working

**Solution:** Ensure dir attribute is set on parent:

```html
<!-- Set dir="rtl" on parent or body -->
<div dir="rtl" lang="ar">
    <ejs-numerictextbox id="price" locale="ar-SA" enable-rtl="true"></ejs-numerictextbox>
</div>
```

