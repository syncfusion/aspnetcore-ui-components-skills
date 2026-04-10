# Validation States for ASP.NET Core TextBox

## Table of Contents
- [Validation States Overview](#validation-states-overview)
- [CSS Class-Based Validation](#css-class-based-validation)
- [Server-Side Validation](#server-side-validation)
- [Client-Side Validation](#client-side-validation)
- [Real-World Form Examples](#real-world-form-examples)
- [Accessibility Best Practices](#accessibility-best-practices)
- [Troubleshooting](#troubleshooting)

## Validation States Overview

TextBox supports three visual validation states to provide user feedback:

| State | CSS Class | Use Case | Color |
|-------|-----------|----------|-------|
| **Success** | `.e-success` | Valid input | Green |
| **Error** | `.e-error` | Invalid input | Red |
| **Warning** | `.e-warning` | Needs attention | Orange/Yellow |

### Visual Example

```cshtml
<!-- Success State -->
<ejs-textbox id="successBox" 
              value="john@example.com"
              cssClass="e-success"
              floatLabelType="Auto"
              placeholder="Valid email">
</ejs-textbox>
<small class="text-success">✓ Email is valid</small>

<!-- Error State -->
<ejs-textbox id="errorBox" 
              value="invalid-email"
              cssClass="e-error"
              floatLabelType="Auto"
              placeholder="Invalid email">
</ejs-textbox>
<small class="text-danger">✗ Please enter a valid email</small>

<!-- Warning State -->
<ejs-textbox id="warningBox" 
              value="maybe@test"
              cssClass="e-warning"
              floatLabelType="Auto"
              placeholder="Verify email">
</ejs-textbox>
<small class="text-warning">⚠ Check your email format</small>
```

## CSS Class-Based Validation

### Static Validation

Apply validation states directly to TextBox:

```cshtml
<!-- Hardcoded validation -->
<ejs-textbox cssClass="e-success" 
              value="john@example.com"
              readonly="true">
</ejs-textbox>
```

**Result:** Green-bordered input indicating success

### Dynamic Validation with JavaScript

```cshtml
<ejs-textbox id="emailInput" 
              placeholder="Enter email"
              floatLabelType="Auto">
</ejs-textbox>
<small id="feedbackMessage"></small>

<script>
    const emailInput = document.getElementById('emailInput');
    const feedbackMessage = document.getElementById('feedbackMessage');
    
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    
    emailInput.addEventListener('change', function(e) {
        const value = e.target.value;
        
        // Remove all validation classes
        this.classList.remove('e-success', 'e-error', 'e-warning');
        
        if (value === '') {
            feedbackMessage.textContent = '';
            feedbackMessage.className = '';
        } else if (emailRegex.test(value)) {
            // Valid email
            this.classList.add('e-success');
            feedbackMessage.textContent = '✓ Valid email address';
            feedbackMessage.className = 'text-success';
        } else if (value.includes('@')) {
            // Has @ but invalid format
            this.classList.add('e-warning');
            feedbackMessage.textContent = '⚠ Check your email format';
            feedbackMessage.className = 'text-warning';
        } else {
            // No @ symbol
            this.classList.add('e-error');
            feedbackMessage.textContent = '✗ Invalid email address';
            feedbackMessage.className = 'text-danger';
        }
    });
</script>
```

### Conditional Styling

```cshtml
@model UserProfile

@{
    var emailState = Model.EmailValid ? "e-success" : 
                     Model.EmailWarning ? "e-warning" : 
                     Model.EmailInvalid ? "e-error" : "";
    
    var phoneState = Model.PhoneValid ? "e-success" : 
                     Model.PhoneInvalid ? "e-error" : "";
}

<ejs-textbox id="email" 
              asp-for="Email"
              cssClass="@emailState"
              floatLabelType="Auto">
</ejs-textbox>

<ejs-textbox id="phone" 
              asp-for="Phone"
              cssClass="@phoneState"
              floatLabelType="Auto">
</ejs-textbox>
```

## Server-Side Validation

### ASP.NET Core Data Annotations

**Model:**
```csharp
using System.ComponentModel.DataAnnotations;

public class ContactForm
{
    [Required(ErrorMessage = "Name is required")]
    [StringLength(100, MinimumLength = 2, 
        ErrorMessage = "Name must be between 2 and 100 characters")]
    public string Name { get; set; }

    [Required(ErrorMessage = "Email is required")]
    [EmailAddress(ErrorMessage = "Invalid email format")]
    public string Email { get; set; }

    [Phone(ErrorMessage = "Invalid phone format")]
    public string Phone { get; set; }

    [StringLength(500, ErrorMessage = "Message cannot exceed 500 characters")]
    public string Message { get; set; }
}
```

### View with Validation Display

```cshtml
@model ContactForm

<form asp-action="Submit" method="post">
    <div class="form-group">
        <label asp-for="Name"></label>
        <ejs-textbox asp-for="Name" 
                      placeholder="Enter your name"
                      floatLabelType="Auto"
                      required="true">
        </ejs-textbox>
        <span asp-validation-for="Name" class="text-danger small"></span>
    </div>

    <div class="form-group">
        <label asp-for="Email"></label>
        <ejs-textbox asp-for="Email" 
                      type="email"
                      placeholder="Enter your email"
                      floatLabelType="Auto"
                      required="true">
        </ejs-textbox>
        <span asp-validation-for="Email" class="text-danger small"></span>
    </div>

    <div class="form-group">
        <label asp-for="Phone"></label>
        <ejs-textbox asp-for="Phone" 
                      type="tel"
                      placeholder="Enter your phone"
                      floatLabelType="Auto">
        </ejs-textbox>
        <span asp-validation-for="Phone" class="text-danger small"></span>
    </div>

    <div class="form-group">
        <label asp-for="Message"></label>
        <ejs-textbox asp-for="Message" 
                      placeholder="Enter your message"
                      floatLabelType="Auto"
                      multiline="true"
                      rows="5">
        </ejs-textbox>
        <span asp-validation-for="Message" class="text-danger small"></span>
    </div>

    <button type="submit" class="btn btn-primary">Submit</button>
</form>

<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/jquery-validation/dist/jquery.validate.js"></script>
<script src="~/lib/jquery-validation-unobtrusive/jquery.validate.unobtrusive.js"></script>
```

### Server-Side Handler

```csharp
// Pages/Contact.cshtml.cs
public class ContactModel : PageModel
{
    [BindProperty]
    public ContactForm Form { get; set; }

    public void OnGet()
    {
    }

    public IActionResult OnPost()
    {
        if (!ModelState.IsValid)
        {
            return Page();
        }

        // Process form
        // Send email, save to database, etc.

        return RedirectToPage("Success");
    }
}
```

## Client-Side Validation

### Real-Time Validation

```cshtml
<div class="form-group">
    <label for="username">Username:</label>
    <ejs-textbox id="username" 
                  placeholder="Choose username (3-20 chars)"
                  floatLabelType="Auto"
                  minLength="3"
                  maxLength="20">
    </ejs-textbox>
    <small id="usernameHelp" class="form-text text-muted"></small>
</div>

<script>
    const usernameInput = document.getElementById('username');
    const usernameHelp = document.getElementById('usernameHelp');
    const usernameRegex = /^[a-zA-Z0-9_-]{3,20}$/;

    usernameInput.addEventListener('input', function(e) {
        const value = e.target.value;
        
        // Remove classes
        this.classList.remove('e-success', 'e-error', 'e-warning');
        
        if (value.length === 0) {
            usernameHelp.textContent = '';
            usernameHelp.className = 'form-text text-muted';
        } else if (value.length < 3) {
            this.classList.add('e-warning');
            usernameHelp.textContent = `${value.length}/3 characters minimum`;
            usernameHelp.className = 'form-text text-warning small';
        } else if (usernameRegex.test(value)) {
            this.classList.add('e-success');
            usernameHelp.textContent = '✓ Username is available';
            usernameHelp.className = 'form-text text-success small';
        } else {
            this.classList.add('e-error');
            usernameHelp.textContent = '✗ Only letters, numbers, - and _ allowed';
            usernameHelp.className = 'form-text text-danger small';
        }
    });
</script>
```

### Pattern Validation

```cshtml
<!-- Phone Number Validation -->
<ejs-textbox id="phone" 
              type="tel"
              placeholder="Phone (123-456-7890)"
              pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}"
              floatLabelType="Auto">
</ejs-textbox>

<!-- URL Validation -->
<ejs-textbox id="website" 
              type="url"
              placeholder="https://example.com"
              floatLabelType="Auto">
</ejs-textbox>

<!-- Postal Code Validation -->
<ejs-textbox id="zipcode" 
              placeholder="ZIP Code"
              pattern="[0-9]{5}(-[0-9]{4})?"
              floatLabelType="Auto">
</ejs-textbox>

<script>
    // Validate on form submission
    document.querySelector('form').addEventListener('submit', function(e) {
        const inputs = this.querySelectorAll('input[pattern]');
        let isValid = true;

        inputs.forEach(input => {
            const pattern = new RegExp('^' + input.pattern + '$');
            
            if (input.value && !pattern.test(input.value)) {
                input.classList.add('e-error');
                isValid = false;
            } else if (input.value) {
                input.classList.add('e-success');
            }
        });

        if (!isValid) {
            e.preventDefault();
            alert('Please correct the validation errors');
        }
    });
</script>
```

## Real-World Form Examples

### Example 1: User Registration Form

```cshtml
@model RegistrationForm

<form asp-action="Register" method="post" id="registrationForm" class="form-container">
    <h2>Create Account</h2>

    <!-- Username -->
    <div class="form-group">
        <label for="username">Username:</label>
        <ejs-textbox id="username" 
                      asp-for="Username"
                      placeholder="Choose a username"
                      floatLabelType="Auto"
                      minLength="3"
                      maxLength="20"
                      required="true">
        </ejs-textbox>
        <small id="usernameValidation" class="form-text"></small>
        <span asp-validation-for="Username" class="text-danger small"></span>
    </div>

    <!-- Email -->
    <div class="form-group">
        <label for="email">Email:</label>
        <ejs-textbox id="email" 
                      asp-for="Email"
                      type="email"
                      placeholder="your@email.com"
                      floatLabelType="Auto"
                      required="true">
        </ejs-textbox>
        <small id="emailValidation" class="form-text"></small>
        <span asp-validation-for="Email" class="text-danger small"></span>
    </div>

    <!-- Password -->
    <div class="form-group">
        <label for="password">Password:</label>
        <ejs-textbox id="password" 
                      asp-for="Password"
                      type="password"
                      placeholder="Minimum 8 characters"
                      floatLabelType="Auto"
                      minLength="8"
                      required="true">
        </ejs-textbox>
        <small id="passwordValidation" class="form-text"></small>
        <span asp-validation-for="Password" class="text-danger small"></span>
    </div>

    <!-- Confirm Password -->
    <div class="form-group">
        <label for="confirmPassword">Confirm Password:</label>
        <ejs-textbox id="confirmPassword" 
                      asp-for="ConfirmPassword"
                      type="password"
                      placeholder="Re-enter password"
                      floatLabelType="Auto"
                      required="true">
        </ejs-textbox>
        <small id="confirmValidation" class="form-text"></small>
    </div>

    <button type="submit" class="btn btn-primary">Create Account</button>
</form>

<style>
    .form-container {
        max-width: 400px;
        margin: 2rem auto;
        padding: 2rem;
        border: 1px solid #ddd;
        border-radius: 8px;
    }

    .form-group {
        margin-bottom: 1.5rem;
    }

    label {
        display: block;
        margin-bottom: 0.5rem;
        font-weight: 500;
    }

    .text-success { color: #28a745; }
    .text-danger { color: #dc3545; }
    .text-warning { color: #ffc107; }
</style>

<script>
    const form = document.getElementById('registrationForm');
    const passwordInput = document.getElementById('password');
    const confirmInput = document.getElementById('confirmPassword');

    // Password strength validation
    passwordInput.addEventListener('change', function() {
        validatePassword(this.value);
    });

    // Confirm password match validation
    confirmInput.addEventListener('change', function() {
        validatePasswordMatch();
    });

    function validatePassword(password) {
        const validation = document.getElementById('passwordValidation');
        const minLength = password.length >= 8;
        const hasUpper = /[A-Z]/.test(password);
        const hasLower = /[a-z]/.test(password);
        const hasNumber = /[0-9]/.test(password);

        const isStrong = minLength && hasUpper && hasLower && hasNumber;

        if (isStrong) {
            passwordInput.classList.remove('e-warning', 'e-error');
            passwordInput.classList.add('e-success');
            validation.textContent = '✓ Strong password';
            validation.className = 'form-text text-success small';
        } else {
            passwordInput.classList.remove('e-success', 'e-error');
            passwordInput.classList.add('e-warning');
            validation.textContent = '⚠ Password should have uppercase, lowercase, and numbers';
            validation.className = 'form-text text-warning small';
        }
    }

    function validatePasswordMatch() {
        const validation = document.getElementById('confirmValidation');
        
        if (passwordInput.value === confirmInput.value && passwordInput.value) {
            confirmInput.classList.remove('e-warning', 'e-error');
            confirmInput.classList.add('e-success');
            validation.textContent = '✓ Passwords match';
            validation.className = 'form-text text-success small';
        } else if (passwordInput.value && confirmInput.value) {
            confirmInput.classList.remove('e-success', 'e-warning');
            confirmInput.classList.add('e-error');
            validation.textContent = '✗ Passwords do not match';
            validation.className = 'form-text text-danger small';
        }
    }
</script>
```

## Accessibility Best Practices

### Proper Label Association

```cshtml
<!-- ✓ Good: Label properly associated -->
<label for="email">Email Address:</label>
<ejs-textbox id="email" 
              type="email"
              placeholder="your@email.com">
</ejs-textbox>

<!-- ✗ Poor: No label association -->
<ejs-textbox id="email" 
              placeholder="your@email.com">
</ejs-textbox>
```

### ARIA Attributes for Validation

```cshtml
<!-- With validation message -->
<label for="email">Email:</label>
<ejs-textbox id="email" 
              type="email"
              aria-label="Email address"
              aria-describedby="emailError"
              cssClass="e-error">
</ejs-textbox>
<div id="emailError" class="error-message" role="alert">
    Invalid email format. Please enter a valid email address.
</div>
```

### Screen Reader Support

```cshtml
<!-- Required field indication -->
<label for="name">
    Name
    <span aria-label="required">*</span>
</label>
<ejs-textbox id="name" 
              required="true"
              aria-required="true">
</ejs-textbox>

<!-- Validation status -->
<ejs-textbox id="username" 
              aria-label="Username"
              aria-invalid="true"
              aria-describedby="userError"
              cssClass="e-error">
</ejs-textbox>
<div id="userError" class="error" role="alert">
    Username must be 3-20 characters
</div>
```

## Troubleshooting

### Issue: Validation classes not applying

**Problem:** CSS class `.e-success`, `.e-error`, etc. not visible

**Solution:**
```csharp
// Ensure Syncfusion CSS is loaded
// In _Layout.cshtml, verify:
// <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/fluent.css" />

// Check the CSS is loaded in browser DevTools (F12)
// Network tab → check fluent.css loads with status 200
```

### Issue: Validation messages not showing

**Problem:** Custom validation messages not displayed

**Solution:**
```cshtml
<!-- Verify the small/span element is present -->
<ejs-textbox id="email" 
              type="email"
              cssClass="e-error">
</ejs-textbox>
<small class="text-danger">✗ Invalid email</small>  <!-- This must exist -->
```

### Issue: Form submits with invalid data

**Problem:** Client-side validation not preventing submission

**Solution:**
```cshtml
<!-- Add form validation handler -->
<form onsubmit="return validateForm()">
    <!-- form inputs -->
</form>

<script>
    function validateForm() {
        const email = document.getElementById('email');
        
        if (!email.value.includes('@')) {
            email.classList.add('e-error');
            return false;
        }
        
        return true;
    }
</script>
```

---

**Related References:**
- [tag-helper-syntax.md](tag-helper-syntax.md) - TextBox properties
- [floating-labels.md](floating-labels.md) - Label styling with validation
- [advanced-patterns.md](advanced-patterns.md) - Complex validation scenarios
