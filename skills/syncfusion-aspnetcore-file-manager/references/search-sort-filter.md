# Search, Sorting, and Filtering Guide

## Table of Contents
- [Search Functionality](#search-functionality)
- [Search Configuration](#search-configuration)
- [Search Events](#search-events)
- [Sorting Files](#sorting-files)
- [Sort Order Configuration](#sort-order-configuration)
- [Filtering Operations](#filtering-operations)
- [Advanced Filtering](#advanced-filtering)
- [Case Sensitivity](#case-sensitivity)
- [Practical Examples](#practical-examples)

## Search Functionality

### Enable Search

**View Code (Index.cshtml)**:
```html
<ejs-filemanager id="filemanager"
    allowSearching="true">
    <e-filemanager-searchsettings
        allowSearchInside="true"
        ignoreCase="true">
    </e-filemanager-searchsettings>
    <e-filemanager-ajaxsettings url="/FileManager/FileManager">
    </e-filemanager-ajaxsettings>
</ejs-filemanager>
```

### Search Bar Configuration

**View Code (Index.cshtml)**:
```html
<div id="searchResults" style="padding: 10px; background-color: #f5f5f5; margin-bottom: 10px;"></div>

<ejs-filemanager id="filemanager"
    allowSearching="true"
    searchComplete="onSearchComplete">
    <e-filemanager-searchsettings
        allowSearchInside="true"
        ignoreCase="true"
        filterType="startsWith">
    </e-filemanager-searchsettings>
    <e-filemanager-ajaxsettings url="/FileManager/FileManager">
    </e-filemanager-ajaxsettings>
</ejs-filemanager>

<script>
    function onSearchComplete(args) {
        console.log('Search results:', args.files);
        
        var resultsDiv = document.getElementById('searchResults');
        var html = '<strong>Search Results: ' + args.files.length + ' files found</strong><br/>';
        args.files.forEach(function(file) {
            html += '<div>• ' + file.name + ' (' + formatBytes(file.size) + ')</div>';
        });
        resultsDiv.innerHTML = html;
    }
</script>
```

### Search Methods

**View Code (Index.cshtml)**:
```html
<input type="text" id="searchQuery" placeholder="Search for files" 
    style="padding: 8px; width: 300px; margin-right: 5px;" />
<button type="button" onclick="performSearch()" style="padding: 8px 16px; margin-bottom: 10px;">
    Search
</button>

<script>
    function performSearch() {
        var fileManager = document.getElementById('filemanager').ej2_instances[0];
        var query = document.getElementById('searchQuery').value;
        
        if (fileManager && query) {
            fileManager.search(query);
            console.log('Searching for: ' + query);
        }
    }
</script>
```

### Search Results Handling

**View Code (Index.cshtml)**:
```html
<ejs-filemanager id="filemanager"
    allowSearching="true"
    beforeSearch="validateSearch"
    searchComplete="displayResults">
    <e-filemanager-searchsettings ignoreCase="true">
    </e-filemanager-searchsettings>
    <e-filemanager-ajaxsettings url="/FileManager/FileManager">
    </e-filemanager-ajaxsettings>
</ejs-filemanager>

<script>
    function validateSearch(args) {
        console.log('Search query:', args.searchString);
        
        // Require minimum 2 characters
        if (args.searchString.length < 2) {
            args.cancel = true;
            alert('Please enter at least 2 characters');
        }
    }
    
    function displayResults(args) {
        console.log('Found files:', args.files.length);
        args.files.forEach(function(file) {
            console.log('- ' + file.name + ' (' + formatBytes(file.size) + ')');
        });
    }
</script>
```

## Search Configuration

### Filter Type Options

**View Code (Index.cshtml)**:
```html
<label style="margin-right: 10px;">Filter Type:</label>
<select id="filterType" onchange="updateFilterType()" style="padding: 8px; margin-right: 10px;">
    <option value="startsWith">Starts With</option>
    <option value="endsWith">Ends With</option>
    <option value="contains">Contains</option>
</select>

<ejs-filemanager id="filemanager"
    allowSearching="true">
    <e-filemanager-searchsettings
        filterType="startsWith"
        ignoreCase="true">
    </e-filemanager-searchsettings>
    <e-filemanager-ajaxsettings url="/FileManager/FileManager">
    </e-filemanager-ajaxsettings>
</ejs-filemanager>

<script>
    function updateFilterType() {
        var fileManager = document.getElementById('filemanager').ej2_instances[0];
        var filterType = document.getElementById('filterType').value;
        fileManager.searchSettings.filterType = filterType;
        console.log('Filter type changed to: ' + filterType);
    }
</script>

**Filter Options:**
- **startsWith**: Query "doc" matches doc.pdf, document.txt (not "word doc")
- **endsWith**: Query ".pdf" matches file.pdf, document.pdf (not "pdf-reader")
- **contains**: Query "doc" matches document, word doc, mydoc.pdf (anywhere in name)
```

### Case Sensitivity

**View Code (Index.cshtml)**:
```html
<label style="margin-right: 10px;">Case Sensitivity:</label>
<input type="radio" name="caseSensitive" value="false" checked="checked" onchange="setCaseSensitivity()" /> 
Ignore Case (Case-insensitive)
<input type="radio" name="caseSensitive" value="true" onchange="setCaseSensitivity()" /> 
Match Case (Case-sensitive)

<ejs-filemanager id="filemanager"
    allowSearching="true">
    <e-filemanager-searchsettings ignoreCase="true">
    </e-filemanager-searchsettings>
    <e-filemanager-ajaxsettings url="/FileManager/FileManager">
    </e-filemanager-ajaxsettings>
</ejs-filemanager>

<script>
    function setCaseSensitivity() {
        var fileManager = document.getElementById('filemanager').ej2_instances[0];
        var caseSensitive = document.querySelector('input[name="caseSensitive"]:checked').value === 'true';
        fileManager.searchSettings.ignoreCase = !caseSensitive;
        console.log('Case sensitive: ' + caseSensitive);
    }
</script>
```

### Search Scope

**View Code (Index.cshtml)**:
```html
<label style="margin-right: 10px;">Search Scope:</label>
<input type="radio" name="searchScope" value="inside" checked="checked" onchange="setSearchScope()" /> 
Current Folder Only
<input type="radio" name="searchScope" value="all" onchange="setSearchScope()" /> 
All Subfolders

<ejs-filemanager id="filemanager"
    allowSearching="true">
    <e-filemanager-searchsettings allowSearchInside="true">
    </e-filemanager-searchsettings>
    <e-filemanager-ajaxsettings url="/FileManager/FileManager">
    </e-filemanager-ajaxsettings>
</ejs-filemanager>

<script>
    function setSearchScope() {
        var fileManager = document.getElementById('filemanager').ej2_instances[0];
        var searchInside = document.querySelector('input[name="searchScope"]:checked').value === 'inside';
        fileManager.searchSettings.allowSearchInside = searchInside;
        console.log('Search inside: ' + searchInside);
    }
</script>
```

## Search Events

### Before Search Event

**View Code (Index.cshtml)**:
```html
<ejs-filemanager id="filemanager"
    allowSearching="true"
    beforeSearch="validateSearchInput">
    <e-filemanager-ajaxsettings url="/FileManager/FileManager">
    </e-filemanager-ajaxsettings>
</ejs-filemanager>

<script>
    function validateSearchInput(args) {
        console.log('Search initiated');
        console.log('Query:', args.searchString);
        
        // Validate search term
        if (args.searchString.length < 2) {
            alert('Minimum 2 characters required');
            args.cancel = true;
        }
        
        // Track search analytics
        logSearchEvent(args.searchString);
    }
    
    function logSearchEvent(query) {
        // Send to server or track locally
        console.log('Search tracked:', query);
    }
</script>
```

### Search Complete Event

**View Code (Index.cshtml)**:
```html
<div id="searchMessage" style="padding: 10px; background-color: #f5f5f5; margin-bottom: 10px;"></div>

<ejs-filemanager id="filemanager"
    allowSearching="true"
    searchComplete="onSearchComplete">
    <e-filemanager-ajaxsettings url="/FileManager/FileManager">
    </e-filemanager-ajaxsettings>
</ejs-filemanager>

<script>
    function onSearchComplete(args) {
        console.log('Found ' + args.files.length + ' results');
        
        var messageDiv = document.getElementById('searchMessage');
        if (args.files.length === 0) {
            messageDiv.innerHTML = '<strong style="color: orange;">No files match your search</strong>';
        } else {
            var html = '<strong style="color: green;">Found ' + args.files.length + ' file(s):</strong><br/>';
            args.files.slice(0, 10).forEach(function(file) {
                html += '<div>• ' + file.name + '</div>';
            });
            if (args.files.length > 10) {
                html += '<div style="color: #666;">... and ' + (args.files.length - 10) + ' more</div>';
            }
            messageDiv.innerHTML = html;
        }
    }
</script>
```

## Sorting Files

### Sort Order Configuration

**View Code (Index.cshtml)**:
```html
<ejs-filemanager id="filemanager"
    sortOrder="Ascending"
    sortBy="name">
    <e-filemanager-ajaxsettings url="/FileManager/FileManager">
    </e-filemanager-ajaxsettings>
</ejs-filemanager>

<script>
    // Sort Order: 'Ascending' or 'Descending'
    // Sort By: 'name', 'modified', 'size', 'type'
</script>
```

### Sort Options

**View Code (Index.cshtml)**:
```html
<style>
    .sort-options {
        padding: 10px;
        background-color: #f5f5f5;
        margin-bottom: 10px;
    }
    .sort-options button {
        padding: 6px 12px;
        margin: 3px;
        cursor: pointer;
    }
    .sort-options button.active {
        background-color: #2196f3;
        color: white;
    }
</style>

<div class="sort-options">
    <button type="button" onclick="setSortOrder('name', 'Ascending')">Name A-Z</button>
    <button type="button" onclick="setSortOrder('name', 'Descending')">Name Z-A</button>
    <button type="button" onclick="setSortOrder('modified', 'Descending')">Newest First</button>
    <button type="button" onclick="setSortOrder('modified', 'Ascending')">Oldest First</button>
    <button type="button" onclick="setSortOrder('size', 'Descending')">Largest First</button>
    <button type="button" onclick="setSortOrder('type', 'Ascending')">By Type</button>
</div>

<script>
    function setSortOrder(sortBy, sortOrder) {
        var fileManager = document.getElementById('filemanager').ej2_instances[0];
        if (fileManager) {
            fileManager.sortBy = sortBy;
            fileManager.sortOrder = sortOrder;
            fileManager.refresh();
            console.log('Sort changed: ' + sortBy + ' (' + sortOrder + ')');
        }
    }
</script>
```

### Dynamic Sort

**View Code (Index.cshtml)**:
```html
<label style="margin-right: 10px;">Sort By:</label>
<select id="sortBySelect" onchange="updateSort()" style="padding: 8px; margin-right: 10px;">
    <option value="name">Name</option>
    <option value="modified">Date Modified</option>
    <option value="size">Size</option>
    <option value="type">Type</option>
</select>

<label style="margin-right: 10px;">Order:</label>
<select id="sortOrderSelect" onchange="updateSort()" style="padding: 8px; margin-bottom: 10px;">
    <option value="Ascending">Ascending</option>
    <option value="Descending">Descending</option>
</select>

<ejs-filemanager id="filemanager"
    sortBy="name"
    sortOrder="Ascending">
    <e-filemanager-ajaxsettings url="/FileManager/FileManager">
    </e-filemanager-ajaxsettings>
</ejs-filemanager>

<script>
    function updateSort() {
        var fileManager = document.getElementById('filemanager').ej2_instances[0];
        var sortBy = document.getElementById('sortBySelect').value;
        var sortOrder = document.getElementById('sortOrderSelect').value;
        
        if (fileManager) {
            fileManager.sortBy = sortBy;
            fileManager.sortOrder = sortOrder;
            fileManager.refresh();
            console.log('Sort: ' + sortBy + ' (' + sortOrder + ')');
        }
    }
</script>
```

## Filtering Operations

### Filter by File Extension

**View Code (Index.cshtml)**:
```html
<button type="button" onclick="filterByExtension(['pdf', 'doc', 'docx'])" style="padding: 8px 16px; margin: 3px;">
    Show Documents (PDF, DOC, DOCX)
</button>
<button type="button" onclick="filterByExtension(['jpg', 'png', 'gif'])" style="padding: 8px 16px; margin: 3px;">
    Show Images
</button>

<div id="filterResults" style="padding: 10px; background-color: #f5f5f5; margin-top: 10px;"></div>

<script>
    function filterByExtension(extensions) {
        var fileManager = document.getElementById('filemanager').ej2_instances[0];
        if (fileManager) {
            var fileDetails = fileManager.fileDetails;
            
            var filtered = fileDetails.filter(function(file) {
                if (!file.isFile) return true;
                var ext = file.name.split('.').pop().toLowerCase();
                return extensions.indexOf(ext) !== -1;
            });
            
            displayFilterResults(filtered, 'Extension: ' + extensions.join(', '));
        }
    }
    
    function displayFilterResults(files, filterName) {
        var html = '<strong>' + filterName + '</strong><br/>Found: ' + files.length + ' files<br/>';
        files.slice(0, 5).forEach(function(f) {
            html += '• ' + f.name + '<br/>';
        });
        if (files.length > 5) {
            html += '... and ' + (files.length - 5) + ' more';
        }
        document.getElementById('filterResults').innerHTML = html;
    }
</script>
```

### Filter by File Size

**View Code (Index.cshtml)**:
```html
<button type="button" onclick="filterBySize(0, 1048576)" style="padding: 8px 16px; margin: 3px;">
    &lt; 1MB
</button>
<button type="button" onclick="filterBySize(1048576, 104857600)" style="padding: 8px 16px; margin: 3px;">
    1-100MB
</button>
<button type="button" onclick="filterBySize(104857600, Infinity)" style="padding: 8px 16px; margin: 3px;">
    &gt; 100MB
</button>

<script>
    function filterBySize(minBytes, maxBytes) {
        var fileManager = document.getElementById('filemanager').ej2_instances[0];
        if (fileManager) {
            var fileDetails = fileManager.fileDetails;
            
            var filtered = fileDetails.filter(function(file) {
                var size = file.size || 0;
                return size >= minBytes && size <= maxBytes;
            });
            
            var sizeRange = formatBytes(minBytes) + ' - ' + 
                (maxBytes === Infinity ? '∞' : formatBytes(maxBytes));
            displayFilterResults(filtered, 'Size: ' + sizeRange);
        }
    }
</script>
```

### Filter by Date Range

**View Code (Index.cshtml)**:
```html
<button type="button" onclick="filterByDateRange(7)" style="padding: 8px 16px; margin: 3px;">
    Last 7 Days
</button>
<button type="button" onclick="filterByDateRange(30)" style="padding: 8px 16px; margin: 3px;">
    Last 30 Days
</button>
<button type="button" onclick="filterByDateRange(365)" style="padding: 8px 16px; margin: 3px;">
    Last Year
</button>

<script>
    function filterByDateRange(daysBack) {
        var fileManager = document.getElementById('filemanager').ej2_instances[0];
        if (fileManager) {
            var fileDetails = fileManager.fileDetails;
            var cutoffDate = new Date();
            cutoffDate.setDate(cutoffDate.getDate() - daysBack);
            
            var filtered = fileDetails.filter(function(file) {
                var fileDate = new Date(file.dateModified);
                return fileDate >= cutoffDate;
            });
            
            displayFilterResults(filtered, 'Modified: Last ' + daysBack + ' days');
        }
    }
</script>
```

### Filter by Name Pattern

**View Code (Index.cshtml)**:
```html
<input type="text" id="patternInput" placeholder="Enter pattern (regex)" 
    style="padding: 8px; width: 250px; margin-right: 5px;" />
<button type="button" onclick="filterByPattern()" style="padding: 8px 16px; margin: 3px;">
    Filter by Pattern
</button>

<div style="margin-top: 10px; font-size: 12px;">
    Examples: <code>^[A-M].*</code> | <code>.*backup.*</code> | <code>\\d{4}-\\d{2}.*</code>
</div>

<script>
    function filterByPattern() {
        var fileManager = document.getElementById('filemanager').ej2_instances[0];
        var pattern = document.getElementById('patternInput').value;
        
        if (fileManager && pattern) {
            var fileDetails = fileManager.fileDetails;
            var regex = new RegExp(pattern, 'i');
            
            var filtered = fileDetails.filter(function(file) {
                return regex.test(file.name);
            });
            
            displayFilterResults(filtered, 'Pattern: ' + pattern);
        }
    }
</script>
```

## Advanced Filtering

### Compound Filtering

**View Code (Index.cshtml)**:
```html
<div style="border: 1px solid #ddd; padding: 15px; margin: 10px 0; background-color: #fafafa;">
    <label>File Extensions: </label>
    <input type="text" id="compoundExt" placeholder="pdf,doc,docx" style="width: 150px; padding: 5px;" />
    
    <label style="margin-left: 20px;">Min Size (bytes): </label>
    <input type="number" id="compoundMinSize" placeholder="1048576" style="width: 120px; padding: 5px;" />
    
    <label style="margin-left: 20px;">Max Size (bytes): </label>
    <input type="number" id="compoundMaxSize" placeholder="104857600" style="width: 120px; padding: 5px;" />
    
    <label style="margin-left: 20px;">Pattern: </label>
    <input type="text" id="compoundPattern" placeholder=".*report.*" style="width: 150px; padding: 5px;" />
    
    <button type="button" onclick="applyCompoundFilter()" style="padding: 8px 16px; margin-left: 10px;">
        Apply Filter
    </button>
</div>

<script>
    function applyCompoundFilter() {
        var fileManager = document.getElementById('filemanager').ej2_instances[0];
        if (fileManager) {
            var ext = document.getElementById('compoundExt').value;
            var minSize = parseInt(document.getElementById('compoundMinSize').value) || 0;
            var maxSize = parseInt(document.getElementById('compoundMaxSize').value) || Infinity;
            var pattern = document.getElementById('compoundPattern').value;
            
            var fileDetails = fileManager.fileDetails;
            var extArray = ext ? ext.split(',').map(function(e) { 
                return e.trim().toLowerCase(); 
            }) : [];
            
            var filtered = fileDetails.filter(function(file) {
                // Extension check
                if (extArray.length > 0) {
                    var fileExt = file.name.split('.').pop().toLowerCase();
                    if (extArray.indexOf(fileExt) === -1) return false;
                }
                
                // Size check
                var size = file.size || 0;
                if (size < minSize || size > maxSize) return false;
                
                // Pattern check
                if (pattern) {
                    var regex = new RegExp(pattern, 'i');
                    if (!regex.test(file.name)) return false;
                }
                
                return true;
            });
            
            var criteria = [];
            if (extArray.length > 0) criteria.push('Extensions: ' + ext);
            if (minSize > 0) criteria.push('Size > ' + formatBytes(minSize));
            if (maxSize !== Infinity) criteria.push('Size < ' + formatBytes(maxSize));
            if (pattern) criteria.push('Pattern: ' + pattern);
            
            displayFilterResults(filtered, 'Compound: ' + criteria.join(' AND '));
        }
    }
</script>
```

### Smart Filter Engine

**Controller Code (FileManagerController.cs)**:
```csharp
// Advanced filter engine for backend operations
public class FileFilterEngine
{
    private List<FileInfo> _fileDetails;
    private List<(string name, Func<FileInfo, bool> predicate)> _filters;

    public FileFilterEngine(List<FileInfo> fileDetails)
    {
        _fileDetails = fileDetails ?? new List<FileInfo>();
        _filters = new List<(string, Func<FileInfo, bool>)>();
    }

    public FileFilterEngine AddFilter(string name, Func<FileInfo, bool> predicate)
    {
        _filters.Add((name, predicate));
        return this;
    }

    public List<FileInfo> Apply()
    {
        return _fileDetails.Where(file => 
            _filters.All(f => f.predicate(file))
        ).ToList();
    }

    public FileFilterEngine RemoveFilter(string name)
    {
        _filters.RemoveAll(f => f.name == name);
        return this;
    }

    public void Clear()
    {
        _filters.Clear();
    }
}

// Advanced filter action
[HttpPost]
public IActionResult ApplyAdvancedFilter([FromBody] AdvancedFilterRequest request)
{
    try
    {
        var files = GetFileDetails(request.RootPath ?? "/");
        var engine = new FileFilterEngine(files);

        if (!string.IsNullOrEmpty(request.FileType))
        {
            engine.AddFilter("type", file => 
                Path.GetExtension(file.Name).TrimStart('.').ToLower() == request.FileType.ToLower()
            );
        }

        if (request.MinSize.HasValue)
        {
            engine.AddFilter("minSize", file => file.Size >= request.MinSize.Value);
        }

        if (request.MaxSize.HasValue)
        {
            engine.AddFilter("maxSize", file => file.Size <= request.MaxSize.Value);
        }

        var results = engine.Apply();
        return Json(new { success = true, count = results.Count, files = results });
    }
    catch (Exception ex)
    {
        return Json(new { success = false, message = ex.Message });
    }
}

public class AdvancedFilterRequest
{
    public string RootPath { get; set; }
    public string FileType { get; set; }
    public long? MinSize { get; set; }
    public long? MaxSize { get; set; }
}
```

**View Code (Client-side Filter Engine)**:
```html
<script>
    // JavaScript Filter Engine for client-side operations
    var FileFilterEngine = function(fileDetails) {
        this.fileDetails = fileDetails || [];
        this.filters = [];
    };

    FileFilterEngine.prototype.addFilter = function(name, predicate) {
        this.filters.push({ name: name, predicate: predicate });
        return this;
    };

    FileFilterEngine.prototype.apply = function() {
        var self = this;
        return this.fileDetails.filter(function(file) {
            return self.filters.every(function(f) {
                return f.predicate(file);
            });
        });
    };

    FileFilterEngine.prototype.removeFilter = function(name) {
        this.filters = this.filters.filter(function(f) {
            return f.name !== name;
        });
        return this;
    };

    FileFilterEngine.prototype.clear = function() {
        this.filters = [];
        return this;
    };

    // Usage example
    function demonstrateFilterEngine() {
        var fileManager = document.getElementById('filemanager').ej2_instances[0];
        var engine = new FileFilterEngine(fileManager.fileDetails);

        engine
            .addFilter('isFile', function(f) { return f.isFile; })
            .addFilter('size', function(f) { return (f.size || 0) > 1048576; })
            .addFilter('type', function(f) {
                var ext = f.name.split('.').pop().toLowerCase();
                return ['pdf', 'doc', 'docx'].indexOf(ext) !== -1;
            });

        var filtered = engine.apply();
        displayFilterResults(filtered, 'Smart Engine: Files + Large + Docs');
    }
</script>
```

## Case Sensitivity

### Case-Sensitive Search

**View Code (Index.cshtml)**:
```html
<ejs-filemanager id="caseFilemanager"
    allowSearching="true"
    searchComplete="onSearchComplete">
    <e-filemanager-searchsettings
        allowSearchInside="true"
        ignoreCase="false">
    </e-filemanager-searchsettings>
    <e-filemanager-ajaxsettings url="/FileManager/FileManager">
    </e-filemanager-ajaxsettings>
</ejs-filemanager>

<div id="caseSearchResults" style="padding: 10px; background-color: #fff3cd; margin-top: 10px;">
    <strong>Note:</strong> Search is case-sensitive<br/>
    "Document" matches only files starting with capital D<br/>
    "document" matches only files starting with lowercase d
</div>

<script>
    function onSearchComplete(args) {
        console.log('Case-sensitive search found: ' + args.files.length + ' files');
    }
</script>
```

### Case-Insensitive Search

**View Code (Index.cshtml)**:
```html
<ejs-filemanager id="filemanager"
    allowSearching="true"
    searchComplete="onSearchComplete">
    <e-filemanager-searchsettings
        allowSearchInside="true"
        ignoreCase="true">
    </e-filemanager-searchsettings>
    <e-filemanager-ajaxsettings url="/FileManager/FileManager">
    </e-filemanager-ajaxsettings>
</ejs-filemanager>

<div id="searchResults" style="padding: 10px; background-color: #d1ecf1; margin-top: 10px;">
    <strong>Note:</strong> Search is case-insensitive (RECOMMENDED)<br/>
    Both "Document" and "document" match the same files<br/>
    This provides better user experience
</div>

<script>
    function onSearchComplete(args) {
        console.log('Case-insensitive search found: ' + args.files.length + ' files');
        
        var html = '<strong>Found ' + args.files.length + ' files:</strong><br/>';
        args.files.forEach(function(file) {
            html += '• ' + file.name + '<br/>';
        });
        document.getElementById('searchResults').innerHTML = html;
    }
</script>
```

### Search Case Configuration Control

**View Code (Index.cshtml)**:
```html
<div style="margin: 10px 0;">
    <label><input type="radio" name="caseOption" value="sensitive" onchange="switchCaseSensitivity()" />
        Case-Sensitive</label>
    <label style="margin-left: 20px;"><input type="radio" name="caseOption" value="insensitive" checked onchange="switchCaseSensitivity()" />
        Case-Insensitive (Recommended)</label>
</div>

<script>
    function switchCaseSensitivity() {
        var option = document.querySelector('input[name="caseOption"]:checked').value;
        
        // Destroy and recreate FileManager with new settings
        var fileManager = document.getElementById('filemanager');
        if (fileManager.ej2_instances) {
            fileManager.ej2_instances[0].destroy();
        }
        
        // Re-create with new case sensitivity setting
        var ignoreCase = option === 'insensitive';
        var FileManagerObj = new ej.filemanager.FileManager({
            ajaxSettings: {
                url: '/FileManager/FileManager',
                uploadUrl: '/FileManager/Upload',
                downloadUrl: '/FileManager/Download',
                getImageUrl: '/FileManager/GetImage'
            },
            searchSettings: {
                ignoreCase: ignoreCase,
                allowSearchInside: true,
                filterType: 'startsWith'
            },
            allowSearching: true,
            height: '400px'
        });
        
        FileManagerObj.appendTo('#filemanager');
        console.log('Search case sensitivity set to: ' + (ignoreCase ? 'insensitive' : 'sensitive'));
    }
</script>
```

## Practical Examples

### Example 1: Advanced Search UI with Form

**View Code (Index.cshtml)**:
```html
<div style="border: 1px solid #ddd; padding: 20px; margin: 15px 0; background-color: #fafafa;">
    <h4>Advanced Search Form</h4>
    
    <label>Search Term:</label>
    <input type="text" id="searchTerm" placeholder="Enter search term" 
        style="padding: 8px; width: 250px; margin-right: 10px;" />
    
    <br style="margin: 10px 0;" />
    
    <label>File Type:</label>
    <select id="fileType" style="padding: 8px; width: 150px; margin-right: 10px;">
        <option value="">All Types</option>
        <option value="pdf">PDF</option>
        <option value="doc">Documents (DOC/DOCX)</option>
        <option value="jpg">Images (JPG/PNG/GIF)</option>
        <option value="xlsx">Spreadsheets (XLSX)</option>
    </select>
    
    <br style="margin: 10px 0;" />
    
    <label>Min Size (bytes):</label>
    <input type="number" id="minSize" value="0" style="padding: 8px; width: 150px; margin-right: 10px;" />
    
    <label>Max Size (bytes):</label>
    <input type="number" id="maxSize" value="1000000000" style="padding: 8px; width: 150px; margin-right: 10px;" />
    
    <br style="margin: 10px 0;" />
    
    <button type="button" onclick="applyAdvancedSearch()" style="padding: 10px 20px; margin: 10px 0;">
        Apply Search
    </button>
    <button type="button" onclick="resetSearchForm()" style="padding: 10px 20px; margin: 10px 5px; background-color: #f0f0f0;">
        Reset
    </button>
</div>

<div id="advancedSearchResults" style="padding: 15px; background-color: #f5f5f5; margin: 10px 0; display: none;"></div>

<script>
    function applyAdvancedSearch() {
        var fileManager = document.getElementById('filemanager').ej2_instances[0];
        if (!fileManager) return;
        
        var searchTerm = document.getElementById('searchTerm').value;
        var fileType = document.getElementById('fileType').value;
        var minSize = parseInt(document.getElementById('minSize').value) || 0;
        var maxSize = parseInt(document.getElementById('maxSize').value) || Infinity;
        
        var fileDetails = fileManager.fileDetails;
        var results = fileDetails.filter(function(file) {
            // Search term
            if (searchTerm && !file.name.toLowerCase().includes(searchTerm.toLowerCase())) {
                return false;
            }
            
            // File type
            if (fileType) {
                var ext = file.name.split('.').pop().toLowerCase();
                if (ext !== fileType) return false;
            }
            
            // Size range
            var size = file.size || 0;
            if (size < minSize || size > maxSize) return false;
            
            return true;
        });
        
        var html = '<strong>Search Results: ' + results.length + ' files found</strong><br/>';
        results.slice(0, 10).forEach(function(file) {
            html += '<div>• ' + file.name + ' (' + formatBytes(file.size) + ', ' + 
                new Date(file.dateModified).toLocaleDateString() + ')</div>';
        });
        if (results.length > 10) {
            html += '<div style="margin-top: 5px;">... and ' + (results.length - 10) + ' more files</div>';
        }
        
        document.getElementById('advancedSearchResults').innerHTML = html;
        document.getElementById('advancedSearchResults').style.display = 'block';
    }
    
    function resetSearchForm() {
        document.getElementById('searchTerm').value = '';
        document.getElementById('fileType').value = '';
        document.getElementById('minSize').value = '0';
        document.getElementById('maxSize').value = '1000000000';
        document.getElementById('advancedSearchResults').style.display = 'none';
    }
</script>
```

### Example 2: Recent Files Finder

**View Code (Index.cshtml)**:
```html
<div style="border: 1px solid #ddd; padding: 15px; margin: 15px 0; background-color: #fafafa;">
    <label>Show recent files from last:</label>
    <select id="daysBack" onchange="findRecentFiles()" style="padding: 8px; width: 150px; margin-right: 10px;">
        <option value="1">1 Day</option>
        <option value="7" selected>7 Days</option>
        <option value="30">30 Days</option>
        <option value="90">90 Days</option>
    </select>
    
    <label style="margin-left: 20px;">Limit to first:</label>
    <select id="limitCount" onchange="findRecentFiles()" style="padding: 8px; width: 100px;">
        <option value="5">5</option>
        <option value="10" selected>10</option>
        <option value="20">20</option>
        <option value="50">50</option>
    </select>
</div>

<div id="recentFilesResults" style="padding: 15px; background-color: #e8f4f8; margin: 10px 0;"></div>

<script>
    function findRecentFiles() {
        var fileManager = document.getElementById('filemanager').ej2_instances[0];
        if (!fileManager) return;
        
        var daysBack = parseInt(document.getElementById('daysBack').value) || 7;
        var limit = parseInt(document.getElementById('limitCount').value) || 10;
        
        var cutoffDate = new Date();
        cutoffDate.setDate(cutoffDate.getDate() - daysBack);
        
        var fileDetails = fileManager.fileDetails;
        var recentFiles = fileDetails
            .filter(function(file) {
                return new Date(file.dateModified) >= cutoffDate;
            })
            .sort(function(a, b) {
                return new Date(b.dateModified) - new Date(a.dateModified);
            })
            .slice(0, limit);
        
        var html = '<strong>Recent Files (Last ' + daysBack + ' days): ' + recentFiles.length + ' files</strong><br/>';
        recentFiles.forEach(function(file) {
            var date = new Date(file.dateModified).toLocaleDateString() + ' ' + 
                       new Date(file.dateModified).toLocaleTimeString();
            html += '<div>• ' + file.name + ' (' + formatBytes(file.size) + ') - ' + date + '</div>';
        });
        
        document.getElementById('recentFilesResults').innerHTML = html;
    }
    
    // Initialize on load
    setTimeout(findRecentFiles, 500);
</script>
```

### Example 3: Duplicate Files Finder

**View Code (Index.cshtml)**:
```html
<button type="button" onclick="findDuplicateFiles()" style="padding: 10px 20px; margin: 10px 0;">
    Find Duplicate Files
</button>

<div id="duplicateResults" style="padding: 15px; background-color: #ffe8e8; margin: 10px 0;"></div>

<script>
    function findDuplicateFiles() {
        var fileManager = document.getElementById('filemanager').ej2_instances[0];
        if (!fileManager) return;
        
        var fileDetails = fileManager.fileDetails;
        var nameMap = {};
        var duplicates = [];
        
        fileDetails.forEach(function(file) {
            if (nameMap[file.name]) {
                // Found a duplicate
                if (!duplicates.some(function(dup) { 
                    return dup.name === file.name; 
                })) {
                    duplicates.push(nameMap[file.name]);
                }
                duplicates.push(file);
            } else {
                nameMap[file.name] = file;
            }
        });
        
        if (duplicates.length === 0) {
            document.getElementById('duplicateResults').innerHTML = 
                '<div style="color: green;"><strong>No duplicate files found!</strong></div>';
            return;
        }
        
        // Group duplicates by name
        var groupedDuplicates = {};
        duplicates.forEach(function(file) {
            if (!groupedDuplicates[file.name]) {
                groupedDuplicates[file.name] = [];
            }
            groupedDuplicates[file.name].push(file);
        });
        
        var html = '<strong>Found ' + Object.keys(groupedDuplicates).length + ' duplicate file names:</strong><br/>';
        Object.keys(groupedDuplicates).forEach(function(name) {
            var group = groupedDuplicates[name];
            html += '<div style="margin-top: 10px; padding: 10px; background-color: #fff0f0; border-left: 3px solid #ff6b6b;">';
            html += '<strong>' + name + '</strong> (' + group.length + ' copies)<br/>';
            group.forEach(function(file) {
                html += '&nbsp;&nbsp;• Size: ' + formatBytes(file.size) + ', Modified: ' + 
                    new Date(file.dateModified).toLocaleDateString() + '<br/>';
            });
            html += '</div>';
        });
        
        document.getElementById('duplicateResults').innerHTML = html;
    }
</script>
```

---

**Related Topics:**
- Selection modes: See `selection-modes.md`
- File operations: See `file-operations.md`
