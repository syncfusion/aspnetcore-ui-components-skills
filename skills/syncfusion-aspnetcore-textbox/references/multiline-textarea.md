# Multiline TextBox (Textarea) for ASP.NET Core

## Table of Contents
- [Converting to Textarea](#converting-to-textarea)
- [Multiline Property Configuration](#multiline-property-configuration)
- [Character Counting](#character-counting)
- [Auto-Grow Behavior](#auto-grow-behavior)
- [Placeholder in Multiline](#placeholder-in-multiline)
- [Validation](#validation)
- [Accessibility](#accessibility)
- [Real-World Examples](#real-world-examples)

## Converting to Textarea

### Basic Multiline TextBox

Transform a regular TextBox into a textarea by setting the `multiline` property:

```cshtml
<!-- Single-line TextBox (default) -->
<ejs-textbox id="singleLine" 
              placeholder="Single line input">
</ejs-textbox>

<!-- Multiline TextBox (textarea) -->
<ejs-textbox id="multiLine" 
              placeholder="Enter multiple lines..."
              multiline="true">
</ejs-textbox>
```

**Result:**
- Single-line TextBox: Horizontally expandable input
- Multiline TextBox: Textarea with word wrapping

### Setting Initial Rows

```cshtml
<!-- 4 rows tall -->
<ejs-textbox id="textArea4" 
              placeholder="Enter your message"
              multiline="true"
              rows="4">
</ejs-textbox>

<!-- 8 rows tall -->
<ejs-textbox id="textArea8" 
              placeholder="Enter your comment"
              multiline="true"
              rows="8">
</ejs-textbox>

<!-- 12 rows tall -->
<ejs-textbox id="textArea12" 
              placeholder="Write detailed description"
              multiline="true"
              rows="12">
</ejs-textbox>
```

**Row Heights:**
- `rows="1"` - Minimal, compact
- `rows="3"` - Default message input
- `rows="5-6"` - Comment/feedback field
- `rows="8-10"` - Larger textarea
- `rows="12+"` - Extended content area

## Multiline Property Configuration

### Basic Configuration

```cshtml
<ejs-textbox id="comment" 
              placeholder="Your feedback"
              multiline="true"
              rows="5"
              floatLabelType="Auto"
              value="">
</ejs-textbox>
```

### With Readonly

```cshtml
<!-- Display-only textarea -->
<ejs-textbox id="displayText" 
              placeholder="Content"
              multiline="true"
              rows="6"
              readonly="true"
              value="This is read-only content that cannot be edited by the user.">
</ejs-textbox>
```

### With Disabled

```cshtml
<!-- Completely disabled textarea -->
<ejs-textbox id="disabledText" 
              placeholder="Disabled content"
              multiline="true"
              rows="4"
              disabled="true"
              value="Cannot interact with this field.">
</ejs-textbox>
```

### With Max Length

```cshtml
<!-- Limit text to 500 characters -->
<ejs-textbox id="limitedText" 
              placeholder="Maximum 500 characters"
              multiline="true"
              rows="5"
              maxLength="500"
              floatLabelType="Auto">
</ejs-textbox>
```

## Character Counting

### Basic Character Counter

```cshtml
<div class="character-counter-group">
    <label for="message">Message:</label>
    <ejs-textbox id="message" 
                  placeholder="Enter your message"
                  multiline="true"
                  rows="4"
                  maxLength="500"
                  floatLabelType="Auto">
    </ejs-textbox>
    <small id="charCount" class="char-counter">0 / 500 characters</small>
</div>

<style>
    .character-counter-group {
        max-width: 500px;
    }

    label {
        display: block;
        margin-bottom: 0.5rem;
        font-weight: 500;
    }

    .char-counter {
        display: block;
        margin-top: 0.5rem;
        font-size: 12px;
        color: #666;
    }

    .char-counter.warning {
        color: #ffc107;
        font-weight: 500;
    }

    .char-counter.danger {
        color: #dc3545;
        font-weight: 500;
    }
</style>

<script>
    const messageInput = document.getElementById('message');
    const charCount = document.getElementById('charCount');
    const maxLength = 500;

    messageInput.addEventListener('input', function() {
        const current = this.value.length;
        const remaining = maxLength - current;
        const percentage = (current / maxLength) * 100;

        // Update counter text
        charCount.textContent = `${current} / ${maxLength} characters`;

        // Update color based on usage
        charCount.classList.remove('warning', 'danger');
        if (percentage >= 90) {
            charCount.classList.add('danger');
        } else if (percentage >= 75) {
            charCount.classList.add('warning');
        }
    });
</script>
```

### Advanced Character Counter with Word Count

```cshtml
<div class="advanced-counter">
    <label for="essay">Essay:</label>
    <ejs-textbox id="essay" 
                  placeholder="Write your essay (max 1000 words)"
                  multiline="true"
                  rows="8"
                  floatLabelType="Auto">
    </ejs-textbox>
    
    <div class="counter-stats">
        <span id="wordCount">Words: 0</span>
        <span id="charCount">Characters: 0</span>
        <span id="lineCount">Lines: 1</span>
    </div>
</div>

<style>
    .advanced-counter {
        max-width: 600px;
    }

    label {
        display: block;
        margin-bottom: 0.5rem;
        font-weight: 500;
    }

    .counter-stats {
        display: flex;
        gap: 1rem;
        margin-top: 0.5rem;
        font-size: 12px;
        color: #666;
    }

    .counter-stats span {
        padding: 0.25rem 0.5rem;
        background-color: #f0f0f0;
        border-radius: 4px;
    }
</style>

<script>
    const essayInput = document.getElementById('essay');
    const wordCountSpan = document.getElementById('wordCount');
    const charCountSpan = document.getElementById('charCount');
    const lineCountSpan = document.getElementById('lineCount');

    essayInput.addEventListener('input', function() {
        const text = this.value;
        
        // Character count
        const charCount = text.length;
        charCountSpan.textContent = `Characters: ${charCount}`;

        // Word count
        const words = text.trim().split(/\s+/).filter(w => w.length > 0).length;
        wordCountSpan.textContent = `Words: ${words}`;

        // Line count
        const lines = text.split('\n').length;
        lineCountSpan.textContent = `Lines: ${lines}`;
    });
</script>
```

## Auto-Grow Behavior

### Fixed Size Textarea

```cshtml
<!-- Fixed size, scrollbar appears when text overflows -->
<ejs-textbox id="fixedTextarea" 
              placeholder="Fixed size textarea"
              multiline="true"
              rows="5"
              floatLabelType="Auto">
</ejs-textbox>

<style>
    /* Default behavior - fixed size with scrollbar */
    #fixedTextarea {
        resize: vertical;
        overflow-y: auto;
    }
</style>
```

### Auto-Expanding Textarea

```cshtml
<!-- Auto-expands as user types -->
<ejs-textbox id="autoGrow" 
              placeholder="This textarea will grow as you type..."
              multiline="true"
              rows="3"
              floatLabelType="Auto"
              cssClass="auto-expand">
</ejs-textbox>

<style>
    .auto-expand {
        resize: none;
        overflow: hidden;
    }
</style>

<script>
    const textarea = document.getElementById('autoGrow');
    
    function autoExpand() {
        textarea.style.height = 'auto';
        textarea.style.height = textarea.scrollHeight + 'px';
    }

    textarea.addEventListener('input', autoExpand);
    textarea.addEventListener('keydown', autoExpand);
    
    // Initial size on load
    autoExpand();
</script>
```

### Max-Height Auto-Expand

```cshtml
<!-- Auto-expands up to max-height, then scrolls -->
<ejs-textbox id="limitedAutoGrow" 
              placeholder="Expands until 300px, then scrolls"
              multiline="true"
              rows="4"
              floatLabelType="Auto"
              cssClass="limited-auto-expand">
</ejs-textbox>

<style>
    .limited-auto-expand {
        resize: none;
        overflow: hidden;
        max-height: 300px;
    }
</style>

<script>
    const textarea = document.getElementById('limitedAutoGrow');
    const maxHeight = 300;
    
    function autoExpandLimited() {
        textarea.style.height = 'auto';
        const newHeight = Math.min(textarea.scrollHeight, maxHeight);
        textarea.style.height = newHeight + 'px';
        
        // Show scrollbar if needed
        if (textarea.scrollHeight > maxHeight) {
            textarea.style.overflow = 'auto';
        } else {
            textarea.style.overflow = 'hidden';
        }
    }

    textarea.addEventListener('input', autoExpandLimited);
    textarea.addEventListener('keydown', autoExpandLimited);
    
    autoExpandLimited();
</script>
```

## Placeholder in Multiline

### Simple Placeholder

```cshtml
<!-- Placeholder visible when empty -->
<ejs-textbox id="simpleMultiline" 
              placeholder="Enter your feedback here..."
              multiline="true"
              rows="5">
</ejs-textbox>

<!-- Placeholder disappears when typing starts -->
```

### Placeholder with Instructions

```cshtml
<ejs-textbox id="feedbackForm" 
              placeholder="Please include:&#10;- What you liked&#10;- What needs improvement&#10;- Additional comments"
              multiline="true"
              rows="6"
              floatLabelType="Auto">
</ejs-textbox>

<!-- Multi-line placeholder using Unicode newline (&#10;) -->
```

## Validation

### Server-Side Validation

```csharp
// Model with validation
public class FeedbackForm
{
    [Required(ErrorMessage = "Feedback is required")]
    [StringLength(1000, MinimumLength = 10, 
        ErrorMessage = "Feedback must be between 10 and 1000 characters")]
    public string Message { get; set; }
}
```

```cshtml
@model FeedbackForm

<form asp-action="Submit" method="post">
    <div class="form-group">
        <label for="message">Your Feedback:</label>
        <ejs-textbox asp-for="Message" 
                      placeholder="Share your thoughts..."
                      multiline="true"
                      rows="6"
                      floatLabelType="Auto">
        </ejs-textbox>
        <span asp-validation-for="Message" class="text-danger small"></span>
    </div>

    <button type="submit" class="btn btn-primary">Submit</button>
</form>
```

### Client-Side Validation

```cshtml
<div class="validation-example">
    <label for="feedback">Feedback (10-500 characters):</label>
    <ejs-textbox id="feedback" 
                  placeholder="Enter your feedback"
                  multiline="true"
                  rows="5"
                  minLength="10"
                  maxLength="500"
                  floatLabelType="Auto"
                  cssClass="no-error">
    </ejs-textbox>
    <small id="feedbackError" class="error-message"></small>
    <small id="feedbackHelp" class="help-text">Minimum 10 characters required</small>
</div>

<style>
    .form-group {
        margin-bottom: 1.5rem;
    }

    .error-message {
        display: block;
        color: #dc3545;
        font-size: 12px;
        margin-top: 0.25rem;
        min-height: 16px;
    }

    .help-text {
        display: block;
        color: #666;
        font-size: 12px;
        margin-top: 0.25rem;
    }

    .no-error { }
    .has-error { }
</style>

<script>
    const feedbackInput = document.getElementById('feedback');
    const feedbackError = document.getElementById('feedbackError');
    const minLength = 10;
    const maxLength = 500;

    feedbackInput.addEventListener('change', function() {
        const value = this.value;
        
        this.classList.remove('e-error', 'e-warning', 'e-success');
        feedbackError.textContent = '';

        if (value.length === 0) {
            this.classList.add('e-error');
            feedbackError.textContent = 'Feedback is required';
        } else if (value.length < minLength) {
            this.classList.add('e-warning');
            feedbackError.textContent = `At least ${minLength} characters required (${value.length}/${minLength})`;
        } else if (value.length > maxLength) {
            this.classList.add('e-error');
            feedbackError.textContent = `Maximum ${maxLength} characters allowed (${value.length}/${maxLength})`;
        } else {
            this.classList.add('e-success');
        }
    });
</script>
```

## Accessibility

### Accessible Textarea

```cshtml
<!-- Proper labeling and ARIA attributes -->
<div class="form-group">
    <label for="description">
        Product Description
        <span class="required" aria-label="required">*</span>
    </label>
    <ejs-textbox id="description" 
                  placeholder="Describe the product in detail"
                  multiline="true"
                  rows="6"
                  required="true"
                  aria-required="true"
                  aria-describedby="descriptionHelp"
                  minLength="20"
                  maxLength="1000"
                  floatLabelType="Auto">
    </ejs-textbox>
    <small id="descriptionHelp" class="form-text">
        Provide at least 20 characters for a good product description
    </small>
</div>

<style>
    label {
        display: block;
        margin-bottom: 0.5rem;
        font-weight: 500;
    }

    .required {
        color: #dc3545;
    }

    .form-text {
        display: block;
        margin-top: 0.25rem;
        font-size: 12px;
        color: #666;
    }
</style>
```

## Real-World Examples

### Example 1: Comment Section

```cshtml
<div class="comment-section">
    <h3>Leave a Comment</h3>
    
    <form id="commentForm">
        <div class="form-group">
            <label for="name">Your Name:</label>
            <ejs-textbox id="name" 
                          placeholder="Enter your name"
                          floatLabelType="Auto"
                          required="true">
            </ejs-textbox>
        </div>

        <div class="form-group">
            <label for="comment">Comment:</label>
            <ejs-textbox id="comment" 
                          placeholder="Share your thoughts..."
                          multiline="true"
                          rows="5"
                          maxLength="500"
                          floatLabelType="Auto"
                          cssClass="auto-expand"
                          required="true">
            </ejs-textbox>
            <small id="charCount" class="char-counter">0 / 500</small>
        </div>

        <button type="submit" class="btn btn-primary">Post Comment</button>
    </form>
</div>

<style>
    .comment-section {
        max-width: 500px;
        margin: 2rem auto;
        padding: 2rem;
        border: 1px solid #ddd;
        border-radius: 8px;
    }

    h3 {
        margin-top: 0;
    }

    .form-group {
        margin-bottom: 1.5rem;
    }

    label {
        display: block;
        margin-bottom: 0.5rem;
        font-weight: 500;
    }

    .btn {
        background-color: #007bff;
        color: white;
        border: none;
        padding: 0.5rem 1rem;
        border-radius: 4px;
        cursor: pointer;
        font-weight: 500;
    }

    .btn:hover {
        background-color: #0056b3;
    }

    .char-counter {
        display: block;
        margin-top: 0.25rem;
        font-size: 12px;
        color: #666;
    }

    .auto-expand {
        resize: none;
        overflow: hidden;
    }
</style>

<script>
    const commentForm = document.getElementById('commentForm');
    const commentInput = document.getElementById('comment');
    const charCount = document.getElementById('charCount');
    const maxLength = 500;

    // Auto-expand textarea
    function autoExpand() {
        commentInput.style.height = 'auto';
        commentInput.style.height = commentInput.scrollHeight + 'px';
    }

    commentInput.addEventListener('input', function() {
        autoExpand();
        
        // Update character count
        const current = this.value.length;
        charCount.textContent = `${current} / ${maxLength}`;
    });

    // Form submission
    commentForm.addEventListener('submit', function(e) {
        e.preventDefault();
        
        const name = document.getElementById('name').value;
        const comment = document.getElementById('comment').value;

        if (name && comment) {
            alert(`Comment posted by ${name}`);
            this.reset();
            commentInput.style.height = 'auto';
        }
    });
</script>
```

### Example 2: Issue Report Form

```cshtml
<div class="issue-report">
    <h2>Report an Issue</h2>
    
    <form id="issueForm">
        <div class="form-group">
            <label for="issueTitle">Issue Title:</label>
            <ejs-textbox id="issueTitle" 
                          placeholder="Brief title of the issue"
                          floatLabelType="Auto"
                          maxLength="100"
                          required="true">
            </ejs-textbox>
        </div>

        <div class="form-group">
            <label for="issueDescription">Description:</label>
            <ejs-textbox id="issueDescription" 
                          placeholder="Describe the issue in detail:&#10;- What did you do?&#10;- What happened?&#10;- What did you expect?"
                          multiline="true"
                          rows="8"
                          maxLength="2000"
                          floatLabelType="Auto"
                          required="true">
            </ejs-textbox>
            <small id="descCharCount" class="char-counter">0 / 2000</small>
        </div>

        <div class="form-group">
            <label for="stepsToReproduce">Steps to Reproduce:</label>
            <ejs-textbox id="stepsToReproduce" 
                          placeholder="1. First step&#10;2. Second step&#10;3. Final step"
                          multiline="true"
                          rows="5"
                          maxLength="1000"
                          floatLabelType="Auto">
            </ejs-textbox>
        </div>

        <button type="submit" class="btn btn-danger">Submit Issue Report</button>
    </form>
</div>

<style>
    .issue-report {
        max-width: 600px;
        margin: 2rem auto;
        padding: 2rem;
        background-color: #f8f9fa;
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

    .char-counter {
        display: block;
        margin-top: 0.25rem;
        font-size: 12px;
        color: #666;
    }

    .btn {
        background-color: #dc3545;
        color: white;
        border: none;
        padding: 0.75rem 1.5rem;
        border-radius: 4px;
        cursor: pointer;
        font-weight: 500;
    }

    .btn:hover {
        background-color: #c82333;
    }
</style>

<script>
    const descInput = document.getElementById('issueDescription');
    const descCharCount = document.getElementById('descCharCount');

    descInput.addEventListener('input', function() {
        const current = this.value.length;
        descCharCount.textContent = `${current} / 2000`;
    });

    document.getElementById('issueForm').addEventListener('submit', function(e) {
        e.preventDefault();
        alert('Issue report submitted. Thank you!');
    });
</script>
```

---

**Related References:**
- [tag-helper-syntax.md](tag-helper-syntax.md) - TextBox properties
- [validation-states.md](validation-states.md) - Validation patterns
- [advanced-patterns.md](advanced-patterns.md) - Complex multiline scenarios
