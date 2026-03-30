# Range Validation in ASP.NET Core NumericTextBox

## Table of Contents
- [Min and Max Constraints](#min-and-max-constraints)
- [Decimal Precision](#decimal-precision)
- [Step Values](#step-values)
- [Validation Patterns](#validation-patterns)
- [Real-World Examples](#real-world-examples)

## Min and Max Constraints

### Basic Min/Max with Tag Helpers

```html
<!-- Age validation: 18-100 -->
<ejs-numerictextbox id="age"
    value="25"
    min="18"
    max="100">
</ejs-numerictextbox>

<!-- Salary range -->
<ejs-numerictextbox id="salary"
    value="50000"
    format="c2"
    min="20000"
    max="500000"
    step="1000">
</ejs-numerictextbox>
```

### With ASP.NET Core Model Binding

```csharp
public class UserProfile
{
    [Range(18, 100)]
    public int Age { get; set; }

    [Range(0, 1000000)]
    public decimal Salary { get; set; }
}
```

```html
<form asp-action="Save" method="post">
    <div class="form-group">
        <label asp-for="Age"></label>
        <ejs-numerictextbox asp-for="Age"
            min="18"
            max="100">
        </ejs-numerictextbox>
        <span asp-validation-for="Age" class="text-danger"></span>
    </div>

    <div class="form-group">
        <label asp-for="Salary"></label>
        <ejs-numerictextbox asp-for="Salary"
            format="c2"
            min="0"
            max="1000000">
        </ejs-numerictextbox>
        <span asp-validation-for="Salary" class="text-danger"></span>
    </div>

    <button type="submit" class="btn btn-primary">Save</button>
</form>
```

## Decimal Precision

### Fixed Decimal Places

```html
<!-- 2 decimal places for currency -->
<ejs-numerictextbox id="price"
    value="100.50"
    format="n2">
</ejs-numerictextbox>

<!-- 3 decimal places for scientific -->
<ejs-numerictextbox id="measurement"
    value="100.500"
    format="n3">
</ejs-numerictextbox>

<!-- 4 decimals for exchange rates -->
<ejs-numerictextbox id="rate"
    value="1.2345"
    format="n4">
</ejs-numerictextbox>
```

## Step Values

### Spinner Increment Configuration

```html
<!-- Step by 1 -->
<ejs-numerictextbox id="quantity"
    value="1"
    step="1"
    min="0"
    max="100">
</ejs-numerictextbox>

<!-- Step by 5 -->
<ejs-numerictextbox id="score"
    value="0"
    step="5"
    min="0"
    max="100">
</ejs-numerictextbox>

<!-- Step by 0.01 for currency -->
<ejs-numerictextbox id="price"
    value="0"
    format="c2"
    step="0.01">
</ejs-numerictextbox>

<!-- Step by 10 for large quantities -->
<ejs-numerictextbox id="stock"
    value="0"
    step="10"
    min="0"
    max="10000">
</ejs-numerictextbox>
```

## Validation Patterns

### Age Validation

```html
<div class="form-group">
    <label>Age</label>
    <ejs-numerictextbox id="age"
        value="25"
        min="1"
        max="150"
        float-label-type="Auto"
        blur="validateAge">
    </ejs-numerictextbox>
    <div id="ageError" class="text-danger"></div>
</div>

<script>
    function validateAge(args) {
        var age = parseInt(args.value);
        var errorDiv = document.getElementById('ageError');
        
        if (age < 18) {
            errorDiv.textContent = 'Must be 18 or older';
        } else if (age > 100) {
            errorDiv.textContent = 'Please check age';
        } else {
            errorDiv.textContent = '';
        }
    }
</script>
```

### Price Range Validation

```html
<div class="form-group">
    <label>Sale Price</label>
    <ejs-numerictextbox id="sale-price"
        format="c2"
        min="0"
        max="10000"
        step="0.01"
        change="validatePrice">
    </ejs-numerictextbox>
    <small id="priceWarning" class="text-warning"></small>
</div>

<script>
    function validatePrice(args) {
        var price = args.value || 0;
        var warning = document.getElementById('priceWarning');
        
        if (price > 5000) {
            warning.textContent = '⚠️ High price - verify before saving';
        } else if (price < 10) {
            warning.textContent = '⚠️ Very low price - check for errors';
        } else {
            warning.textContent = '';
        }
    }
</script>
```

### Inventory Quantity Validation

```html
<div class="form-group">
    <label asp-for="Quantity"></label>
    <ejs-numerictextbox asp-for="Quantity"
        min="1"
        max="100"
        step="1"
        float-label-type="Auto"
        change="checkInventory">
    </ejs-numerictextbox>
    <small>Available: 100 units</small>
</div>

<script>
    function checkInventory(args) {
        var quantity = args.value;
        var available = 100;
        
        if (quantity > available) {
            document.querySelector('input[asp-for="Quantity"]').classList.add('is-invalid');
        } else {
            document.querySelector('input[asp-for="Quantity"]').classList.remove('is-invalid');
        }
    }
</script>
```

### Percentage Validation (0-100)

```html
<div class="form-group">
    <label>Tax Rate (%)</label>
    <ejs-numerictextbox id="tax-rate"
        value="0"
        min="0"
        max="100"
        step="0.1"
        format="n2"
        float-label-type="Auto">
    </ejs-numerictextbox>
    <small class="form-text text-muted">Valid range: 0-100%</small>
</div>

<div class="form-group">
    <label>Discount (%)</label>
    <ejs-numerictextbox id="discount"
        value="0"
        min="0"
        max="100"
        step="5"
        format="n0">
    </ejs-numerictextbox>
</div>
```

## Real-World Examples

### Employee Payroll with Validation

```html
<form asp-action="SavePayroll" method="post">
    <div class="card">
        <div class="card-header">Payroll Information</div>
        <div class="card-body">
            
            <div class="form-group">
                <label asp-for="BaseSalary"></label>
                <ejs-numerictextbox asp-for="BaseSalary"
                    format="c2"
                    min="0"
                    max="500000"
                    step="1000"
                    float-label-type="Auto"
                    change="calculateTotalCompensation">
                </ejs-numerictextbox>
            </div>

            <div class="form-group">
                <label asp-for="Bonus"></label>
                <ejs-numerictextbox asp-for="Bonus"
                    format="c2"
                    min="0"
                    max="100000"
                    step="500"
                    float-label-type="Auto"
                    change="calculateTotalCompensation">
                </ejs-numerictextbox>
            </div>

            <div class="form-group">
                <label asp-for="Benefits"></label>
                <ejs-numerictextbox asp-for="Benefits"
                    format="c2"
                    min="0"
                    max="50000"
                    step="100"
                    float-label-type="Auto"
                    change="calculateTotalCompensation">
                </ejs-numerictextbox>
            </div>

            <hr />

            <div class="alert alert-info">
                <label>Total Annual Compensation</label>
                <ejs-numerictextbox id="total-compensation"
                    format="c2"
                    read-only="true">
                </ejs-numerictextbox>
            </div>

            <button type="submit" class="btn btn-primary">Save Payroll</button>
        </div>
    </div>
</form>

<script>
    function calculateTotalCompensation() {
        var salary = parseFloat(document.querySelector('input[asp-for="BaseSalary"]').value) || 0;
        var bonus = parseFloat(document.querySelector('input[asp-for="Bonus"]').value) || 0;
        var benefits = parseFloat(document.querySelector('input[asp-for="Benefits"]').value) || 0;
        
        var total = salary + bonus - benefits;
        
        var totalControl = document.querySelector('input[id="total-compensation"]');
        var totalInstance = totalControl.ej2_instances[0];
        totalInstance.value = total;
    }
</script>
```

### Budget Planning with Range Checks

```html
<div class="card">
    <div class="card-header">Budget Allocation</div>
    <div class="card-body">
        
        <div class="form-group">
            <label>Total Budget</label>
            <ejs-numerictextbox id="total-budget"
                value="100000"
                format="c2"
                min="0"
                change="validateBudget">
            </ejs-numerictextbox>
        </div>

        <div class="form-group">
            <label>Marketing</label>
            <ejs-numerictextbox id="marketing"
                value="30000"
                format="c2"
                min="0"
                change="validateBudget">
            </ejs-numerictextbox>
        </div>

        <div class="form-group">
            <label>Operations</label>
            <ejs-numerictextbox id="operations"
                value="50000"
                format="c2"
                min="0"
                change="validateBudget">
            </ejs-numerictextbox>
        </div>

        <div class="form-group">
            <label>R&D</label>
            <ejs-numerictextbox id="research"
                value="20000"
                format="c2"
                min="0"
                change="validateBudget">
            </ejs-numerictextbox>
        </div>

        <div id="validation-message"></div>

        <div class="alert alert-info">
            <label>Remaining Budget</label>
            <ejs-numerictextbox id="remaining"
                format="c2"
                read-only="true">
            </ejs-numerictextbox>
        </div>

    </div>
</div>

<script>
    function validateBudget() {
        var total = parseFloat(document.querySelector('input[id="total-budget"]').value) || 0;
        var marketing = parseFloat(document.querySelector('input[id="marketing"]').value) || 0;
        var operations = parseFloat(document.querySelector('input[id="operations"]').value) || 0;
        var research = parseFloat(document.querySelector('input[id="research"]').value) || 0;

        var allocated = marketing + operations + research;
        var remaining = total - allocated;

        var remainingControl = document.querySelector('input[id="remaining"]');
        var remainingInstance = remainingControl.ej2_instances[0];
        remainingInstance.value = remaining;

        var messageDiv = document.getElementById('validation-message');
        if (allocated > total) {
            messageDiv.innerHTML = '<div class="alert alert-danger">❌ Over-budget!</div>';
        } else if (allocated < total) {
            messageDiv.innerHTML = '<div class="alert alert-warning">⚠️ Under-budget!</div>';
        } else {
            messageDiv.innerHTML = '<div class="alert alert-success">✓ Budget perfectly allocated!</div>';
        }
    }

    document.addEventListener('DOMContentLoaded', validateBudget);
</script>
```

## Troubleshooting

### Min/Max Not Enforced on Input
**Solution:** Values outside range are corrected on blur, not during typing.

### Step Size Too Large/Small
**Solution:** Match step to value scale:
- Currency: step="0.01"
- Age: step="1"
- Large quantities: step="10"

### Decimal Places Inconsistent
**Solution:** Use matching types in all properties:
```html
<!-- ✓ Correct -->
<ejs-numerictextbox value="500.50" min="0.00" max="1000.00" format="c2"></ejs-numerictextbox>

<!-- ✗ Problematic -->
<ejs-numerictextbox value="500.50" min="0" max="1000" format="c2"></ejs-numerictextbox>
```
