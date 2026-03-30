# Advanced Patterns for ASP.NET Core TextBox

## Table of Contents
- [Data Binding Patterns](#data-binding-patterns)
- [Event Handling](#event-handling)
- [Template-Based Rendering](#template-based-rendering)
- [ASP.NET Core Forms Integration](#aspnet-core-forms-integration)
- [Model Binding Examples](#model-binding-examples)
- [Performance Considerations](#performance-considerations)
- [Security Best Practices](#security-best-practices)

## Data Binding Patterns

### One-Way Binding (Display Only)

```cshtml
@{
    var userEmail = "user@example.com";
    var userPhone = "555-123-4567";
}

<!-- Display value without modification possibility -->
<ejs-textbox id="displayEmail" 
              value="@userEmail"
              readonly="true"
              placeholder="Email">
</ejs-textbox>

<ejs-textbox id="displayPhone" 
              value="@userPhone"
              readonly="true"
              placeholder="Phone">
</ejs-textbox>
```

### Two-Way Binding with asp-for

```csharp
public class UserProfile
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Email { get; set; }
    public string Bio { get; set; }
}
```

```cshtml
@model UserProfile

<form asp-action="UpdateProfile" method="post">
    <div class="form-group">
        <label asp-for="FirstName"></label>
        <ejs-textbox asp-for="FirstName" 
                      placeholder="Enter first name"
                      floatLabelType="Auto">
        </ejs-textbox>
        <span asp-validation-for="FirstName"></span>
    </div>

    <div class="form-group">
        <label asp-for="LastName"></label>
        <ejs-textbox asp-for="LastName" 
                      placeholder="Enter last name"
                      floatLabelType="Auto">
        </ejs-textbox>
        <span asp-validation-for="LastName"></span>
    </div>

    <div class="form-group">
        <label asp-for="Email"></label>
        <ejs-textbox asp-for="Email" 
                      type="email"
                      placeholder="Enter email"
                      floatLabelType="Auto">
        </ejs-textbox>
        <span asp-validation-for="Email"></span>
    </div>

    <div class="form-group">
        <label asp-for="Bio"></label>
        <ejs-textbox asp-for="Bio" 
                      placeholder="Tell us about yourself"
                      multiline="true"
                      rows="5"
                      floatLabelType="Auto">
        </ejs-textbox>
        <span asp-validation-for="Bio"></span>
    </div>

    <button type="submit" class="btn btn-primary">Save Profile</button>
</form>
```

**How it works:**
1. Model properties automatically populate TextBox values
2. TextBox `name` attribute matches property name
3. Form submission automatically binds values back to model
4. Validation messages display via `asp-validation-for`

### Dictionary-Based Data Binding

```cshtml
@{
    var formData = new Dictionary<string, object>
    {
        { "Username", "john_doe" },
        { "Email", "john@example.com" },
        { "Department", "Engineering" }
    };
}

<!-- Bind from dictionary -->
<ejs-textbox id="username" 
              value="@(formData["Username"])"
              placeholder="Username"
              readonly="true">
</ejs-textbox>

<ejs-textbox id="email" 
              value="@(formData["Email"])"
              placeholder="Email"
              readonly="true">
</ejs-textbox>

<ejs-textbox id="department" 
              value="@(formData["Department"])"
              placeholder="Department"
              readonly="true">
</ejs-textbox>
```

## Event Handling

### Created Event

```cshtml
<ejs-textbox id="onCreatedBox" 
              placeholder="TextBox Created Event"
              created="onCreated">
</ejs-textbox>

<script>
    function onCreated(args) {
        console.log("TextBox created:", args.element);
        
        // Add custom styles or behaviors after creation
        args.element.style.borderRadius = "8px";
        args.element.style.padding = "12px";
        
        // Add aria-label if not present
        if (!args.element.hasAttribute('aria-label')) {
            args.element.setAttribute('aria-label', 'Text input field');
        }
    }
</script>
```

### Input Event (On Each Keystroke)

```cshtml
<div class="search-live">
    <ejs-textbox id="liveSearch" 
                  placeholder="Type to search...">
    </ejs-textbox>
    <div id="searchResults" class="results"></div>
</div>

<style>
    .search-live {
        max-width: 500px;
    }

    .results {
        margin-top: 1rem;
        border: 1px solid #ddd;
        border-radius: 4px;
        display: none;
    }

    .results.show {
        display: block;
    }

    .result-item {
        padding: 0.5rem 1rem;
        border-bottom: 1px solid #eee;
        cursor: pointer;
    }

    .result-item:hover {
        background-color: #f0f0f0;
    }
</style>

<script>
    const searchInput = document.getElementById('liveSearch');
    const resultsDiv = document.getElementById('searchResults');

    // Simulate product database
    const products = [
        'Apple iPhone',
        'Apple iPad',
        'Android Phone',
        'Samsung Galaxy',
        'Google Pixel'
    ];

    searchInput.addEventListener('input', function(e) {
        const query = e.target.value.toLowerCase();

        if (query.length === 0) {
            resultsDiv.classList.remove('show');
            resultsDiv.innerHTML = '';
            return;
        }

        // Filter results
        const filtered = products.filter(p => 
            p.toLowerCase().includes(query)
        );

        if (filtered.length > 0) {
            resultsDiv.innerHTML = filtered.map(result => 
                `<div class="result-item" onclick="selectResult('${result}')">${result}</div>`
            ).join('');
            resultsDiv.classList.add('show');
        } else {
            resultsDiv.innerHTML = '<div class="result-item">No results found</div>';
            resultsDiv.classList.add('show');
        }
    });

    function selectResult(result) {
        searchInput.value = result;
        resultsDiv.classList.remove('show');
    }
</script>
```

### Change Event (On Value Change Complete)

```cshtml
<div class="form-validation">
    <label for="username">Username:</label>
    <ejs-textbox id="username" 
                  placeholder="3-20 characters"
                  floatLabelType="Auto">
    </ejs-textbox>
    <small id="usernameFeedback"></small>
</div>

<script>
    const usernameInput = document.getElementById('username');
    const feedback = document.getElementById('usernameFeedback');

    usernameInput.addEventListener('change', function(e) {
        const value = this.value;
        const regex = /^[a-zA-Z0-9_-]{3,20}$/;

        this.classList.remove('e-success', 'e-error', 'e-warning');

        if (value === '') {
            feedback.textContent = '';
        } else if (regex.test(value)) {
            this.classList.add('e-success');
            feedback.textContent = '✓ Username is available';
            feedback.className = 'text-success';
        } else {
            this.classList.add('e-error');
            feedback.textContent = '✗ Invalid username format';
            feedback.className = 'text-danger';
        }
    });
</script>
```

### Focus and Blur Events

```cshtml
<div class="focus-blur-demo">
    <ejs-textbox id="focusInput" 
                  placeholder="Focus me to highlight">
    </ejs-textbox>
</div>

<style>
    .focus-blur-demo {
        margin: 2rem 0;
    }

    #focusInput {
        transition: all 0.3s ease;
    }

    #focusInput.focused {
        box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.25);
        background-color: #e7f3ff;
    }
</style>

<script>
    const focusInput = document.getElementById('focusInput');

    focusInput.addEventListener('focus', function() {
        console.log('Input focused');
        this.classList.add('focused');
    });

    focusInput.addEventListener('blur', function() {
        console.log('Input blurred');
        this.classList.remove('focused');
    });
</script>
```

## Template-Based Rendering

### Using Prepend/Append Templates

```cshtml
<div class="template-example">
    <!-- Email with icon and clear button -->
    <ejs-textbox id="templateEmail" 
                  type="email"
                  placeholder="Email"
                  prependTemplate="<span class='email-icon'>✉️</span>"
                  appendTemplate="<button type='button' onclick='clearEmail()' class='clear-btn'>✕</button>"
                  floatLabelType="Auto">
    </ejs-textbox>

    <!-- Currency with symbol and unit -->
    <ejs-textbox id="templatePrice" 
                  type="number"
                  placeholder="0.00"
                  prependTemplate="<span class='currency'>$</span>"
                  appendTemplate="<span class='unit'>USD</span>"
                  floatLabelType="Auto"
                  step="0.01">
    </ejs-textbox>
</div>

<style>
    .template-example {
        margin: 2rem 0;
    }

    .email-icon {
        padding: 0 12px;
        color: #007bff;
    }

    .clear-btn {
        border: none;
        background: transparent;
        cursor: pointer;
        padding: 0 8px;
        color: #999;
    }

    .currency {
        padding: 0 12px;
        font-weight: bold;
        color: #28a745;
        background-color: #f0f0f0;
    }

    .unit {
        padding: 0 8px;
        color: #666;
    }
</style>

<script>
    function clearEmail() {
        document.getElementById('templateEmail').value = '';
    }
</script>
```

## ASP.NET Core Forms Integration

### Reactive Forms with Validation

```csharp
public class ContactFormModel
{
    [Required]
    [StringLength(100)]
    public string Name { get; set; }

    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Phone]
    public string Phone { get; set; }

    [Required]
    [StringLength(1000, MinimumLength = 10)]
    public string Message { get; set; }
}

public class ContactFormModel : PageModel
{
    [BindProperty]
    public ContactFormModel Form { get; set; }

    public void OnGet() { }

    public async Task<IActionResult> OnPostAsync()
    {
        if (!ModelState.IsValid)
        {
            return Page();
        }

        // Send email or save to database
        await SendContactEmailAsync(Form);
        
        return RedirectToPage("Success");
    }

    private async Task SendContactEmailAsync(ContactFormModel form)
    {
        // Implementation here
        await Task.CompletedTask;
    }
}
```

```cshtml
@page
@model ContactFormModel

<form asp-page-handler="Post" method="post" class="contact-form">
    <div asp-validation-summary="ModelOnly" class="alert alert-danger"></div>

    <div class="form-group">
        <label asp-for="Form.Name"></label>
        <ejs-textbox asp-for="Form.Name" 
                      placeholder="Your full name"
                      floatLabelType="Auto"
                      required="true">
        </ejs-textbox>
        <span asp-validation-for="Form.Name" class="text-danger small"></span>
    </div>

    <div class="form-group">
        <label asp-for="Form.Email"></label>
        <ejs-textbox asp-for="Form.Email" 
                      type="email"
                      placeholder="your@email.com"
                      floatLabelType="Auto"
                      required="true">
        </ejs-textbox>
        <span asp-validation-for="Form.Email" class="text-danger small"></span>
    </div>

    <div class="form-group">
        <label asp-for="Form.Phone"></label>
        <ejs-textbox asp-for="Form.Phone" 
                      type="tel"
                      placeholder="(555) 123-4567"
                      floatLabelType="Auto">
        </ejs-textbox>
        <span asp-validation-for="Form.Phone" class="text-danger small"></span>
    </div>

    <div class="form-group">
        <label asp-for="Form.Message"></label>
        <ejs-textbox asp-for="Form.Message" 
                      placeholder="Your message (10-1000 chars)"
                      multiline="true"
                      rows="6"
                      floatLabelType="Auto"
                      required="true">
        </ejs-textbox>
        <span asp-validation-for="Form.Message" class="text-danger small"></span>
    </div>

    <button type="submit" class="btn btn-primary">Send Message</button>
</form>

@section Scripts {
    <partial name="_ValidationScriptsPartial" />
}
```

## Model Binding Examples

### Simple Model Binding

```csharp
public class SearchQuery
{
    public string Query { get; set; }
    public int ResultsPerPage { get; set; } = 10;
}

public class SearchModel : PageModel
{
    [BindProperty]
    public SearchQuery Search { get; set; }

    public List<string> Results { get; set; } = new();

    public void OnPost()
    {
        if (!string.IsNullOrEmpty(Search.Query))
        {
            // Perform search
            Results = PerformSearch(Search.Query);
        }
    }

    private List<string> PerformSearch(string query)
    {
        // Implementation
        return new List<string>();
    }
}
```

```cshtml
@model SearchModel

<form asp-page-handler="Post" method="post">
    <div class="search-group">
        <ejs-textbox asp-for="Search.Query" 
                      placeholder="Enter search query"
                      prependTemplate="<span>🔍</span>"
                      floatLabelType="Never">
        </ejs-textbox>
        <button type="submit" class="btn btn-search">Search</button>
    </div>
</form>

@if (Model.Results.Any())
{
    <div class="results">
        @foreach (var result in Model.Results)
        {
            <div class="result-item">@result</div>
        }
    </div>
}
```

## Performance Considerations

### Lazy Loading Multiple TextBoxes

```cshtml
<div id="formContainer">
    <!-- TextBoxes loaded on demand -->
</div>

<script>
    // Load form fields when needed
    function loadFormFields(count) {
        const container = document.getElementById('formContainer');
        
        for (let i = 0; i < count; i++) {
            const input = document.createElement('input');
            input.id = `field-${i}`;
            input.type = 'text';
            input.placeholder = `Field ${i + 1}`;
            input.className = 'ejs-textbox';
            
            container.appendChild(input);
        }

        // Initialize Syncfusion controls after adding to DOM
        const inputs = container.querySelectorAll('.ejs-textbox');
        inputs.forEach(input => {
            new ej.inputs.TextBox({
                floatLabelType: 'Auto',
                placeholder: input.placeholder
            }, input);
        });
    }

    // Load on page ready
    document.addEventListener('DOMContentLoaded', () => {
        loadFormFields(5);
    });
</script>
```

### Debouncing Input Events

```cshtml
<ejs-textbox id="searchField" 
              placeholder="Search (auto-searches after 500ms)">
</ejs-textbox>

<div id="searchResults"></div>

<script>
    const searchField = document.getElementById('searchField');
    const resultsDiv = document.getElementById('searchResults');
    let searchTimeout;

    function debounce(callback, delay) {
        return function(...args) {
            clearTimeout(searchTimeout);
            searchTimeout = setTimeout(() => callback(...args), delay);
        };
    }

    const performSearch = debounce(async function(query) {
        if (query.length < 2) {
            resultsDiv.innerHTML = '';
            return;
        }

        console.log('Searching for:', query);
        
        // Fetch results from API
        try {
            const response = await fetch(`/api/search?q=${query}`);
            const results = await response.json();
            
            resultsDiv.innerHTML = results
                .map(r => `<div class="result">${r}</div>`)
                .join('');
        } catch (error) {
            console.error('Search error:', error);
        }
    }, 500);

    searchField.addEventListener('input', (e) => {
        performSearch(e.target.value);
    });
</script>
```

## Security Best Practices

### XSS Prevention (Input Sanitization)

```cshtml
<!-- Always encode output -->
<ejs-textbox id="userInput" 
              placeholder="Enter text">
</ejs-textbox>

<div id="output"></div>

<script>
    const userInput = document.getElementById('userInput');
    const output = document.getElementById('output');

    userInput.addEventListener('change', function() {
        // Sanitize input to prevent XSS
        const sanitized = sanitizeHTML(this.value);
        
        // Display safely
        output.textContent = sanitized; // textContent prevents XSS
    });

    function sanitizeHTML(str) {
        const div = document.createElement('div');
        div.textContent = str;
        return div.innerHTML;
    }
</script>
```

### CSRF Protection

```cshtml
<form asp-page-handler="Post" method="post">
    <!-- Automatically included by ASP.NET Core -->
    @Html.AntiForgeryToken()

    <ejs-textbox asp-for="UserInput" 
                  placeholder="Enter text">
    </ejs-textbox>

    <button type="submit">Submit</button>
</form>
```

### Server-Side Validation (Always)

```csharp
public async Task<IActionResult> OnPostAsync()
{
    // ALWAYS validate on server
    if (!ModelState.IsValid)
    {
        return Page();
    }

    // Sanitize before processing
    var sanitizedInput = HtmlEncoder.Default.Encode(Form.UserInput);
    
    // Save safely
    await database.SaveAsync(sanitizedInput);

    return RedirectToPage("Success");
}
```

---

**Related References:**
- [tag-helper-syntax.md](tag-helper-syntax.md) - TextBox properties
- [validation-states.md](validation-states.md) - Form validation
- [getting-started.md](getting-started.md) - Setup and basics
