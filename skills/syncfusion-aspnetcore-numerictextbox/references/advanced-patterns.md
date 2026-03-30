# Advanced Patterns in ASP.NET Core NumericTextBox

## Data Binding with Models

### Strong-Typed Model Binding

**Model (Models/Invoice.cs):**

```csharp
using System.ComponentModel.DataAnnotations;

public class Invoice
{
    [Display(Name = "Item Price")]
    [Range(0, 999999.99)]
    public decimal ItemPrice { get; set; }

    [Display(Name = "Quantity")]
    [Range(1, 10000)]
    public int Quantity { get; set; }

    [Display(Name = "Tax Rate")]
    [Range(0, 1)]
    public decimal TaxRate { get; set; }

    public decimal CalculateTotal()
    {
        var subtotal = ItemPrice * Quantity;
        var taxAmount = subtotal * TaxRate;
        return subtotal + taxAmount;
    }
}
```

**View:**

```html
@model Invoice

<form asp-action="Create" method="post">
    <div class="form-group">
        <label asp-for="ItemPrice"></label>
        <ejs-numerictextbox asp-for="ItemPrice"
            format="c2"
            min="0"
            max="999999.99"
            step="0.01"
            float-label-type="Auto"
            change="recalculateTotal">
        </ejs-numerictextbox>
        <span asp-validation-for="ItemPrice" class="text-danger"></span>
    </div>

    <div class="form-group">
        <label asp-for="Quantity"></label>
        <ejs-numerictextbox asp-for="Quantity"
            format="n0"
            min="1"
            max="10000"
            step="1"
            float-label-type="Auto"
            change="recalculateTotal">
        </ejs-numerictextbox>
        <span asp-validation-for="Quantity" class="text-danger"></span>
    </div>

    <div class="form-group">
        <label asp-for="TaxRate"></label>
        <ejs-numerictextbox asp-for="TaxRate"
            format="p2"
            min="0"
            max="1"
            step="0.01"
            float-label-type="Auto"
            change="recalculateTotal">
        </ejs-numerictextbox>
        <span asp-validation-for="TaxRate" class="text-danger"></span>
    </div>

    <div class="alert alert-info">
        <label>Total</label>
        <ejs-numerictextbox id="total"
            format="c2"
            read-only="true">
        </ejs-numerictextbox>
    </div>

    <button type="submit" class="btn btn-primary">Save Invoice</button>
</form>

<script>
    function recalculateTotal() {
        var itemPrice = parseFloat(document.querySelector('input[asp-for="ItemPrice"]').value) || 0;
        var quantity = parseInt(document.querySelector('input[asp-for="Quantity"]').value) || 0;
        var taxRate = parseFloat(document.querySelector('input[asp-for="TaxRate"]').value) || 0;

        var subtotal = itemPrice * quantity;
        var taxAmount = subtotal * taxRate;
        var total = subtotal + taxAmount;

        var totalControl = document.querySelector('input[id="total"]');
        var totalInstance = totalControl.ej2_instances[0];
        totalInstance.value = total;
    }
</script>
```

## Event Handling

### Complete Event Lifecycle

```html
<ejs-numerictextbox id="salary"
    value="50000"
    format="c2"
    created="onCreated"
    focus="onFocus"
    change="onChange"
    blur="onBlur">
</ejs-numerictextbox>

<script>
    function onCreated(args) {
        console.log("NumericTextBox created");
        console.log("Initial value: " + args.value);
    }

    function onFocus(args) {
        console.log("Focus gained, value: " + args.value);
    }

    function onChange(args) {
        console.log("Value changed from " + args.previousValue + " to " + args.value);
        if (args.isInteraction) {
            console.log("User interaction detected");
        }
    }

    function onBlur(args) {
        console.log("Focus lost, final value: " + args.value);
    }
</script>
```

### Event-Driven Calculations

```html
<div class="card">
    <div class="card-header">Commission Calculator</div>
    <div class="card-body">
        
        <div class="form-group">
            <label>Sales Amount</label>
            <ejs-numerictextbox id="sales"
                value="0"
                format="c2"
                min="0"
                change="calculateCommission">
            </ejs-numerictextbox>
        </div>

        <div class="form-group">
            <label>Commission Rate</label>
            <ejs-numerictextbox id="rate"
                value="0.10"
                format="p2"
                min="0"
                max="1"
                change="calculateCommission">
            </ejs-numerictextbox>
        </div>

        <div class="alert alert-info">
            <label>Commission</label>
            <ejs-numerictextbox id="commission"
                format="c2"
                read-only="true">
            </ejs-numerictextbox>
        </div>

    </div>
</div>

<script>
    function calculateCommission() {
        var sales = parseFloat(document.querySelector('input[id="sales"]').value) || 0;
        var rate = parseFloat(document.querySelector('input[id="rate"]').value) || 0;
        var commission = sales * rate;

        var commissionControl = document.querySelector('input[id="commission"]');
        var commissionInstance = commissionControl.ej2_instances[0];
        commissionInstance.value = commission;
    }

    document.addEventListener('DOMContentLoaded', calculateCommission);
</script>
```

## Programmatic Creation

### JavaScript-Based Creation

```html
<div id="container"></div>

<script>
    // Create NumericTextBox programmatically
    var numericInstance = new ej.inputs.NumericTextBox({
        value: 5000,
        format: 'c2',
        min: 0,
        max: 1000000,
        step: 1000,
        placeholder: 'Enter amount',
        floatLabelType: 'Auto',
        change: function(args) {
            console.log('Value changed to: ' + args.value);
        }
    });

    numericInstance.appendTo('#container');

    // Access value
    console.log('Current value: ' + numericInstance.value);

    // Update value
    function updateValue(newValue) {
        numericInstance.value = newValue;
    }
</script>
```

## Form Integration

### Multi-Field Form with Validation

```html
@model EmployeeForm

<form asp-action="SaveEmployee" asp-controller="Employee" method="post" id="employeeForm">
    <div class="container mt-4">
        <h2>Employee Information</h2>

        <div class="form-group">
            <label asp-for="BaseSalary"></label>
            <ejs-numerictextbox asp-for="BaseSalary"
                format="c2"
                min="10000"
                max="500000"
                step="1000"
                float-label-type="Auto"
                blur="validateSalary">
            </ejs-numerictextbox>
            <span asp-validation-for="BaseSalary" class="text-danger"></span>
        </div>

        <div class="form-group">
            <label asp-for="Allowances"></label>
            <ejs-numerictextbox asp-for="Allowances"
                format="c2"
                min="0"
                max="100000"
                step="500"
                float-label-type="Auto"
                blur="validateAllowances">
            </ejs-numerictextbox>
        </div>

        <div class="form-group">
            <label asp-for="Deductions"></label>
            <ejs-numerictextbox asp-for="Deductions"
                format="c2"
                min="0"
                max="50000"
                step="100"
                float-label-type="Auto"
                blur="validateDeductions">
            </ejs-numerictextbox>
        </div>

        <div class="alert alert-info">
            <label>Net Salary</label>
            <ejs-numerictextbox id="net-salary"
                format="c2"
                read-only="true">
            </ejs-numerictextbox>
        </div>

        <button type="submit" class="btn btn-primary">Save Employee</button>
        <button type="reset" class="btn btn-secondary">Clear Form</button>
    </div>
</form>

<script>
    function validateSalary(args) {
        if (args.value < 10000) {
            alert('Salary cannot be less than $10,000');
            return false;
        }
        recalculateNetSalary();
    }

    function validateAllowances(args) {
        if (args.value > 100000) {
            alert('Allowances cannot exceed $100,000');
            return false;
        }
        recalculateNetSalary();
    }

    function validateDeductions(args) {
        if (args.value > 50000) {
            alert('Deductions cannot exceed $50,000');
            return false;
        }
        recalculateNetSalary();
    }

    function recalculateNetSalary() {
        var basic = parseFloat(document.querySelector('input[asp-for="BaseSalary"]').value) || 0;
        var allowances = parseFloat(document.querySelector('input[asp-for="Allowances"]').value) || 0;
        var deductions = parseFloat(document.querySelector('input[asp-for="Deductions"]').value) || 0;
        var net = basic + allowances - deductions;

        var netControl = document.querySelector('input[id="net-salary"]');
        var netInstance = netControl.ej2_instances[0];
        netInstance.value = net;
    }

    // Form submission
    document.getElementById('employeeForm').addEventListener('submit', function(e) {
        var basic = parseFloat(document.querySelector('input[asp-for="BaseSalary"]').value) || 0;
        
        if (basic === 0) {
            e.preventDefault();
            alert('Please enter a valid salary');
            return false;
        }
    });

    // Calculate on page load
    document.addEventListener('DOMContentLoaded', recalculateNetSalary);
</script>
```

## Composite Validation

### Budget Allocation Validation

```html
<div class="card">
    <div class="card-header">Budget Planning</div>
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
            messageDiv.innerHTML = '<div class="alert alert-success">✓ Budget balanced!</div>';
        }
    }

    document.addEventListener('DOMContentLoaded', validateBudget);
</script>
```

## Security

### XSS Prevention

```html
<!-- ASP.NET Core Tag Helper automatically encodes values -->
@{
    var userInput = "<script>alert('XSS')</script>";
}

<!-- Safe - value is encoded -->
<ejs-numerictextbox id="userAmount"
    value="@userInput">
</ejs-numerictextbox>

<!-- Also safe - model properties are encoded by default -->
<form asp-action="Save" method="post">
    <ejs-numerictextbox asp-for="Amount"
        value="@Model.Amount">
    </ejs-numerictextbox>
</form>
```

### CSRF Token in Forms

```html
<form asp-action="Save" asp-controller="Payroll" method="post">
    <!-- CSRF token automatically included by asp-action -->
    
    <div class="form-group">
        <label asp-for="Salary"></label>
        <ejs-numerictextbox asp-for="Salary"
            format="c2"
            min="0">
        </ejs-numerictextbox>
    </div>

    <button type="submit" class="btn btn-primary">Save</button>
</form>

<!-- Controller requires validation -->
@{
    [HttpPost]
    [ValidateAntiForgeryToken]
    public IActionResult Save(PayrollModel model)
    {
        if (ModelState.IsValid)
        {
            // Process safely
        }
        return View(model);
    }
}
```

## Performance

### Lazy Initialization

```html
<!-- Initialize only when needed -->
<button onclick="initializeNumeric()">Initialize NumericTextBox</button>
<div id="numericContainer"></div>

<script>
    function initializeNumeric() {
        if (!document.querySelector('input[id="lazy-numeric"]')) {
            var numeric = new ej.inputs.NumericTextBox({
                value: 0,
                format: 'c2'
            });
            numeric.appendTo('#numericContainer');
        }
    }
</script>
```

### Batch Updates

```html
<script>
    // Avoid multiple renders - batch updates
    var sales = document.querySelector('input[id="sales"]').ej2_instances[0];
    var commission = document.querySelector('input[id="commission"]').ej2_instances[0];

    // ✗ Bad - triggers multiple renders
    sales.value = 100;
    commission.value = 10;

    // ✓ Good - batch updates
    sales.value = 100;
    commission.value = 10;
    // Syncfusion handles re-render efficiently
</script>
```

