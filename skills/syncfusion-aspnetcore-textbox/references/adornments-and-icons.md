# Adornments and Icons for ASP.NET Core TextBox

## Table of Contents
- [Introduction](#introduction)
- [Prepend Template](#prepend-template)
- [Append Template](#append-template)
- [Icon Integration](#icon-integration)
- [Password Visibility Toggle](#password-visibility-toggle)
- [Clear Button Functionality](#clear-button-functionality)
- [Unit Indicators](#unit-indicators)
- [Complex Patterns](#complex-patterns)
- [Accessibility](#accessibility)

## Introduction

Adornments are custom elements (icons, buttons, text) that are placed before or after the TextBox input. They enhance usability by providing visual context, interactive controls, and validation indicators.

### Benefits

- **Visual Context**: Display icons indicating input type (email, phone, user)
- **Functional Controls**: Password visibility toggle, clear button
- **Validation Feedback**: Show status icons (success, error, warning)
- **Unit Indicators**: Display currency symbols, measurements, domains
- **Improved UX**: Guide users on expected input format

## Prepend Template

Prepend templates add content **before** the input field.

### Basic Prepend Icon

```cshtml
<!-- Search box with magnifying glass -->
<ejs-textbox id="search" 
              placeholder="Search..."
              prependTemplate="<span class='e-icon-prepend'>🔍</span>"
              floatLabelType="Auto">
</ejs-textbox>

<style>
    .e-icon-prepend {
        display: inline-flex;
        align-items: center;
        padding: 0 8px;
        color: #666;
        font-size: 18px;
    }
</style>
```

### Email Icon Example

```cshtml
<ejs-textbox id="email" 
              type="email"
              placeholder="Email address"
              prependTemplate="<span class='e-input-icon'>✉️</span>"
              floatLabelType="Auto">
</ejs-textbox>

<style>
    .e-input-icon {
        display: inline-flex;
        align-items: center;
        justify-content: center;
        width: 40px;
        color: #007bff;
    }
</style>
```

### Text Label (Currency Symbol)

```cshtml
<!-- Price input with dollar sign -->
<ejs-textbox id="price" 
              type="number"
              placeholder="0.00"
              prependTemplate="<span class='currency-label'>$</span>"
              floatLabelType="Always">
</ejs-textbox>

<style>
    .currency-label {
        display: inline-flex;
        align-items: center;
        padding: 0 12px;
        font-weight: bold;
        color: #28a745;
        background-color: #f0f0f0;
    }
</style>
```

## Append Template

Append templates add content **after** the input field.

### Basic Append Button

```cshtml
<!-- Input with action button -->
<ejs-textbox id="invite" 
              placeholder="Enter email to invite"
              appendTemplate="<button type='button' class='btn-invite' onclick='inviteUser()'>Invite</button>"
              floatLabelType="Auto">
</ejs-textbox>

<style>
    .btn-invite {
        border: none;
        background-color: #007bff;
        color: white;
        padding: 6px 12px;
        margin: 0 4px;
        border-radius: 4px;
        cursor: pointer;
        font-weight: 500;
    }

    .btn-invite:hover {
        background-color: #0056b3;
    }
</style>

<script>
    function inviteUser() {
        const email = document.getElementById('invite').value;
        alert('Invite sent to: ' + email);
    }
</script>
```

### Append Status Icon

```cshtml
<!-- Input with validation icon -->
<ejs-textbox id="domain" 
              placeholder="example.com"
              value="mysite.com"
              appendTemplate="<span class='status-icon valid'>✓</span>"
              readonly="true"
              floatLabelType="Auto">
</ejs-textbox>

<style>
    .status-icon {
        display: inline-flex;
        align-items: center;
        padding: 0 8px;
        font-weight: bold;
        font-size: 18px;
    }

    .status-icon.valid {
        color: #28a745;
    }

    .status-icon.invalid {
        color: #dc3545;
    }

    .status-icon.warning {
        color: #ffc107;
    }
</style>
```

## Icon Integration

### Using Font Awesome Icons

```cshtml
<!-- Add Font Awesome CDN -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />

<!-- TextBox with Font Awesome icons -->
<div class="icon-examples">
    <h3>Font Awesome Icons</h3>

    <!-- User Icon -->
    <div class="form-group">
        <ejs-textbox id="username" 
                      placeholder="Username"
                      prependTemplate="<i class='fas fa-user'></i>"
                      floatLabelType="Auto">
        </ejs-textbox>
    </div>

    <!-- Email Icon -->
    <div class="form-group">
        <ejs-textbox id="email" 
                      type="email"
                      placeholder="Email"
                      prependTemplate="<i class='fas fa-envelope'></i>"
                      floatLabelType="Auto">
        </ejs-textbox>
    </div>

    <!-- Phone Icon -->
    <div class="form-group">
        <ejs-textbox id="phone" 
                      type="tel"
                      placeholder="Phone"
                      prependTemplate="<i class='fas fa-phone'></i>"
                      floatLabelType="Auto">
        </ejs-textbox>
    </div>

    <!-- Location Icon -->
    <div class="form-group">
        <ejs-textbox id="address" 
                      placeholder="Address"
                      prependTemplate="<i class='fas fa-map-marker-alt'></i>"
                      floatLabelType="Auto">
        </ejs-textbox>
    </div>
</div>

<style>
    .icon-examples .form-group {
        margin-bottom: 1.5rem;
    }

    .icon-examples i {
        width: 40px;
        display: inline-flex;
        align-items: center;
        justify-content: center;
        color: #007bff;
    }
</style>
```

### Using Material Icons

```cshtml
<!-- Add Material Icons CDN -->
<link rel="stylesheet" href="https://fonts.googleapis.com/icon?family=Material+Icons" />

<!-- TextBox with Material icons -->
<div class="material-examples">
    <div class="form-group">
        <ejs-textbox placeholder="Search" 
                      prependTemplate="<span class='material-icons'>search</span>"
                      floatLabelType="Auto">
        </ejs-textbox>
    </div>

    <div class="form-group">
        <ejs-textbox placeholder="Favorite" 
                      appendTemplate="<span class='material-icons'>favorite_border</span>"
                      floatLabelType="Auto">
        </ejs-textbox>
    </div>

    <div class="form-group">
        <ejs-textbox placeholder="Settings" 
                      prependTemplate="<span class='material-icons'>settings</span>"
                      floatLabelType="Auto">
        </ejs-textbox>
    </div>
</div>

<style>
    .material-examples .material-icons {
        display: inline-flex;
        align-items: center;
        vertical-align: middle;
        width: 40px;
        justify-content: center;
        color: #666;
    }
</style>
```

## Password Visibility Toggle

### Complete Password Toggle Implementation

```cshtml
<div class="password-container">
    <label for="password">Password:</label>
    <ejs-textbox id="password" 
                  type="password"
                  placeholder="Enter password"
                  floatLabelType="Auto"
                  appendTemplate="<button type='button' class='toggle-password' onclick='togglePasswordVisibility()' title='Toggle password visibility'>👁️</button>">
    </ejs-textbox>
</div>

<style>
    .password-container {
        max-width: 400px;
        margin: 2rem auto;
    }

    label {
        display: block;
        margin-bottom: 0.5rem;
        font-weight: 500;
    }

    .toggle-password {
        border: none;
        background: transparent;
        cursor: pointer;
        font-size: 18px;
        padding: 4px 8px;
        color: #666;
        transition: color 0.2s ease;
    }

    .toggle-password:hover {
        color: #333;
    }

    .toggle-password.show {
        color: #007bff;
    }
</style>

<script>
    function togglePasswordVisibility() {
        const input = document.getElementById('password');
        const button = document.querySelector('.toggle-password');

        if (input.type === 'password') {
            input.type = 'text';
            button.textContent = '🙈';
            button.classList.add('show');
        } else {
            input.type = 'password';
            button.textContent = '👁️';
            button.classList.remove('show');
        }
    }
</script>
```

### Password Strength Indicator with Toggle

```cshtml
<div class="password-form">
    <div class="form-group">
        <label for="password">Password:</label>
        <ejs-textbox id="password" 
                      type="password"
                      placeholder="Minimum 8 characters"
                      floatLabelType="Auto"
                      appendTemplate="<button type='button' class='toggle-pwd'>👁️</button>">
        </ejs-textbox>
        <div class="strength-meter">
            <div class="strength-bar" id="strengthBar"></div>
        </div>
        <small id="strengthText">Password strength: None</small>
    </div>

    <div class="form-group">
        <label for="confirmPassword">Confirm Password:</label>
        <ejs-textbox id="confirmPassword" 
                      type="password"
                      placeholder="Re-enter password"
                      floatLabelType="Auto"
                      appendTemplate="<button type='button' class='toggle-pwd'>👁️</button>">
        </ejs-textbox>
    </div>
</div>

<style>
    .password-form {
        max-width: 400px;
        margin: 2rem auto;
    }

    .form-group {
        margin-bottom: 1.5rem;
    }

    label {
        display: block;
        margin-bottom: 0.5rem;
        font-weight: 500;
    }

    .toggle-pwd {
        border: none;
        background: transparent;
        cursor: pointer;
        font-size: 18px;
        padding: 4px 8px;
        color: #666;
    }

    .strength-meter {
        height: 6px;
        background-color: #e9ecef;
        border-radius: 3px;
        margin-top: 0.5rem;
        overflow: hidden;
    }

    .strength-bar {
        height: 100%;
        width: 0%;
        transition: width 0.3s ease, background-color 0.3s ease;
        background-color: #dc3545;
    }

    .strength-bar.weak { width: 33%; background-color: #dc3545; }
    .strength-bar.fair { width: 66%; background-color: #ffc107; }
    .strength-bar.strong { width: 100%; background-color: #28a745; }

    small {
        display: block;
        margin-top: 0.25rem;
        font-size: 12px;
        color: #666;
    }
</style>

<script>
    const passwordInput = document.getElementById('password');
    const confirmInput = document.getElementById('confirmPassword');
    const strengthBar = document.getElementById('strengthBar');
    const strengthText = document.getElementById('strengthText');

    // Toggle buttons
    document.querySelectorAll('.toggle-pwd').forEach(btn => {
        btn.addEventListener('click', function(e) {
            e.preventDefault();
            const input = this.closest('.form-group').querySelector('input');
            input.type = input.type === 'password' ? 'text' : 'password';
            this.textContent = input.type === 'password' ? '👁️' : '🙈';
        });
    });

    // Password strength checker
    passwordInput.addEventListener('input', function() {
        const password = this.value;
        const strength = calculatePasswordStrength(password);

        strengthBar.className = 'strength-bar';
        
        if (password.length === 0) {
            strengthText.textContent = 'Password strength: None';
            strengthBar.style.width = '0%';
        } else if (strength < 0.4) {
            strengthBar.classList.add('weak');
            strengthText.textContent = 'Password strength: Weak';
        } else if (strength < 0.7) {
            strengthBar.classList.add('fair');
            strengthText.textContent = 'Password strength: Fair';
        } else {
            strengthBar.classList.add('strong');
            strengthText.textContent = 'Password strength: Strong';
        }
    });

    function calculatePasswordStrength(password) {
        let strength = 0;

        if (password.length >= 8) strength += 0.2;
        if (password.length >= 12) strength += 0.2;
        if (/[a-z]/.test(password)) strength += 0.15;
        if (/[A-Z]/.test(password)) strength += 0.15;
        if (/[0-9]/.test(password)) strength += 0.15;
        if (/[!@#$%^&*]/.test(password)) strength += 0.15;

        return Math.min(strength, 1);
    }
</script>
```

## Clear Button Functionality

### Clear Button Implementation

```cshtml
<div class="search-with-clear">
    <ejs-textbox id="searchInput" 
                  placeholder="Search products..."
                  appendTemplate="<button type='button' class='btn-clear' onclick='clearSearch()' title='Clear search'>✕</button>"
                  floatLabelType="Never">
    </ejs-textbox>
</div>

<style>
    .search-with-clear {
        max-width: 500px;
        margin: 2rem auto;
    }

    .btn-clear {
        border: none;
        background: transparent;
        cursor: pointer;
        font-size: 18px;
        padding: 4px 8px;
        color: #999;
        transition: color 0.2s ease;
        display: none;
    }

    .btn-clear.visible {
        display: inline;
    }

    .btn-clear:hover {
        color: #333;
    }
</style>

<script>
    const searchInput = document.getElementById('searchInput');
    const clearBtn = searchInput.querySelector('.btn-clear');

    searchInput.addEventListener('input', function() {
        if (this.value.length > 0) {
            clearBtn.classList.add('visible');
        } else {
            clearBtn.classList.remove('visible');
        }
    });

    function clearSearch() {
        searchInput.value = '';
        clearBtn.classList.remove('visible');
        searchInput.focus();
        searchInput.dispatchEvent(new Event('change', { bubbles: true }));
    }
</script>
```

## Unit Indicators

### Currency Input

```cshtml
<div class="currency-input">
    <label>Price (USD)</label>
    <ejs-textbox id="price" 
                  type="number"
                  placeholder="0.00"
                  step="0.01"
                  min="0"
                  prependTemplate="<span class='currency-symbol'>$</span>"
                  floatLabelType="Always">
    </ejs-textbox>
</div>

<style>
    .currency-input {
        max-width: 300px;
    }

    label {
        display: block;
        margin-bottom: 0.5rem;
        font-weight: 500;
    }

    .currency-symbol {
        display: inline-flex;
        align-items: center;
        padding: 0 12px;
        font-weight: bold;
        color: #28a745;
        background-color: #f0f0f0;
    }
</style>
```

### Temperature Unit Indicator

```cshtml
<div class="temperature-input">
    <ejs-textbox id="temperature" 
                  type="number"
                  placeholder="Enter temperature"
                  appendTemplate="<span class='unit-indicator'>°C</span>"
                  floatLabelType="Always">
    </ejs-textbox>
</div>

<style>
    .temperature-input {
        max-width: 250px;
    }

    .unit-indicator {
        display: inline-flex;
        align-items: center;
        padding: 0 8px;
        font-weight: bold;
        color: #666;
    }
</style>
```

## Complex Patterns

### Search Input with Recent Searches

```cshtml
<div class="search-advanced">
    <ejs-textbox id="advancedSearch" 
                  placeholder="Search products, categories..."
                  prependTemplate="<span class='search-icon'>🔍</span>"
                  appendTemplate="<button type='button' class='btn-filter' title='Advanced filters'>⚙️</button>"
                  floatLabelType="Never">
    </ejs-textbox>
    <div id="recentSearches" class="recent-searches" style="display: none;">
        <h4>Recent Searches:</h4>
        <ul id="searchList"></ul>
    </div>
</div>

<style>
    .search-advanced {
        max-width: 500px;
        margin: 2rem auto;
    }

    .search-icon {
        display: inline-flex;
        align-items: center;
        padding: 0 12px;
        color: #007bff;
        font-size: 18px;
    }

    .btn-filter {
        border: none;
        background: transparent;
        cursor: pointer;
        font-size: 18px;
        padding: 4px 12px;
        color: #666;
    }

    .btn-filter:hover {
        color: #333;
    }

    .recent-searches {
        background-color: #f8f9fa;
        border: 1px solid #ddd;
        border-radius: 4px;
        margin-top: 0.5rem;
        padding: 1rem;
    }

    .recent-searches h4 {
        margin: 0 0 0.5rem 0;
        font-size: 0.875rem;
        color: #666;
    }

    .recent-searches ul {
        list-style: none;
        padding: 0;
        margin: 0;
    }

    .recent-searches li {
        padding: 0.25rem 0;
    }

    .recent-searches a {
        color: #007bff;
        text-decoration: none;
        font-size: 0.875rem;
    }

    .recent-searches a:hover {
        text-decoration: underline;
    }
</style>

<script>
    const searchInput = document.getElementById('advancedSearch');
    const recentSearches = document.getElementById('recentSearches');
    const searchList = document.getElementById('searchList');
    let searches = JSON.parse(localStorage.getItem('searches')) || [];

    searchInput.addEventListener('focus', function() {
        if (searches.length > 0) {
            updateRecentSearches();
            recentSearches.style.display = 'block';
        }
    });

    searchInput.addEventListener('blur', function() {
        setTimeout(() => {
            recentSearches.style.display = 'none';
        }, 200);
    });

    searchInput.addEventListener('keypress', function(e) {
        if (e.key === 'Enter' && this.value.trim()) {
            addToSearchHistory(this.value);
            console.log('Searching for:', this.value);
        }
    });

    function addToSearchHistory(query) {
        searches = searches.filter(s => s !== query);
        searches.unshift(query);
        searches = searches.slice(0, 5);
        localStorage.setItem('searches', JSON.stringify(searches));
    }

    function updateRecentSearches() {
        searchList.innerHTML = searches.map(s => 
            `<li><a href="#" onclick="selectSearch('${s}'); return false;">${s}</a></li>`
        ).join('');
    }

    function selectSearch(query) {
        searchInput.value = query;
        searchInput.focus();
    }
</script>
```

## Accessibility

### Accessible Adornments

```cshtml
<!-- Button adornment with accessibility -->
<div class="form-group">
    <label for="email">Email Address:</label>
    <ejs-textbox id="email" 
                  type="email"
                  placeholder="your@email.com"
                  appendTemplate="<button type='button' class='verify-btn' aria-label='Verify email address'>Verify</button>"
                  aria-describedby="emailHelp"
                  floatLabelType="Auto">
    </ejs-textbox>
    <small id="emailHelp">We'll verify your email address</small>
</div>

<!-- Icon adornment with aria-label -->
<div class="form-group">
    <label for="password">Password:</label>
    <ejs-textbox id="password" 
                  type="password"
                  placeholder="Enter password"
                  prependTemplate="<span class='lock-icon' aria-label='Secure field'>🔒</span>"
                  floatLabelType="Auto">
    </ejs-textbox>
</div>
```

---

**Related References:**
- [tag-helper-syntax.md](tag-helper-syntax.md) - TextBox properties
- [floating-labels.md](floating-labels.md) - Label styling
- [advanced-patterns.md](advanced-patterns.md) - Complex interactions
