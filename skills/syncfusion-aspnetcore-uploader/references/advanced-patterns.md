# Advanced Patterns in ASP.NET Core Uploader

## Table of Contents
- [Chunk Upload](#chunk-upload)
- [Form Submission](#form-submission)
- [Directory Upload](#directory-upload)
- [File Removal](#file-removal)
- [Error Handling](#error-handling)
- [Security Considerations](#security-considerations)
- [Performance Optimization](#performance-optimization)

## Chunk Upload

Upload large files in smaller chunks for reliability:

### Enable Chunk Upload

```html
<ejs-uploader id="uploader"
    max-file-size="1073741824">
    <e-async-settings save-url="Home/Save" 
                      remove-url="Home/Remove" 
                      chunk-size="512000"></e-async-settings>
</ejs-uploader>
```

### Server-Side Chunk Handling

> **Security note:** Never use the client-supplied filename directly as a filesystem path. Instead, generate a server-side safe name and scope all temporary files to a session-specific directory validated with a server-issued session token. Reject any path separators or `..` sequences in the original filename.

```csharp
[HttpPost]
public IActionResult Save(IFormFile[] uploader)
{
    string uploadPath = Path.Combine(_webHostEnvironment.WebRootPath, "Uploads");
    if (!Directory.Exists(uploadPath))
        Directory.CreateDirectory(uploadPath);

    if (uploader != null)
    {
        foreach (IFormFile file in uploader)
        {
            if (file.Length > 0)
            {
                // Reject filenames containing path separators or traversal sequences
                string originalName = Path.GetFileName(file.FileName);
                if (string.IsNullOrWhiteSpace(originalName) ||
                    originalName.IndexOfAny(Path.GetInvalidFileNameChars()) >= 0)
                    return BadRequest("Invalid file name.");

                // Use a server-generated safe name to avoid overwrite/traversal
                string safeFileName = Guid.NewGuid().ToString("N") +
                                      Path.GetExtension(originalName).ToLowerInvariant();

                // Check for chunk metadata
                string chunkIndexStr  = Request.Form["chunkIndex"];
                string totalChunksStr = Request.Form["totalChunks"];
                // Session token issued by the server on the first chunk request
                string sessionId      = Request.Form["uploadSessionId"];

                if (!string.IsNullOrEmpty(chunkIndexStr))
                {
                    // Validate session token: must be a non-empty GUID issued earlier
                    if (string.IsNullOrWhiteSpace(sessionId) ||
                        !Guid.TryParse(sessionId, out Guid parsedSession))
                        return BadRequest("Invalid upload session.");

                    if (!int.TryParse(chunkIndexStr,  out int chunkIndex)  ||
                        !int.TryParse(totalChunksStr, out int totalChunks) ||
                        chunkIndex < 0 || totalChunks <= 0 || chunkIndex >= totalChunks)
                        return BadRequest("Invalid chunk parameters.");

                    // Scope chunk files to a session-specific temp directory
                    string sessionTempDir = Path.Combine(uploadPath, "tmp_" + parsedSession.ToString("N"));
                    Directory.CreateDirectory(sessionTempDir);

                    string chunkPath = Path.Combine(sessionTempDir, $"chunk_{chunkIndex}");
                    // Confirm the resolved path is inside the expected directory
                    if (!Path.GetFullPath(chunkPath)
                             .StartsWith(Path.GetFullPath(sessionTempDir) + Path.DirectorySeparatorChar,
                                         StringComparison.OrdinalIgnoreCase))
                        return BadRequest("Invalid chunk path.");

                    using (FileStream fs = System.IO.File.Create(chunkPath))
                    {
                        file.CopyTo(fs);
                        fs.Flush();
                    }

                    // Combine once all chunks have arrived
                    if (chunkIndex == totalChunks - 1)
                    {
                        string finalPath = GetSafePath(uploadPath, safeFileName);
                        CombineChunks(finalPath, sessionTempDir, totalChunks);
                        Directory.Delete(sessionTempDir, recursive: true);
                    }
                }
                else
                {
                    // Regular single-file upload — write to safe server-generated path
                    string filePath = GetSafePath(uploadPath, safeFileName);
                    using (FileStream fs = System.IO.File.Create(filePath))
                    {
                        file.CopyTo(fs);
                        fs.Flush();
                    }
                }
            }
        }
    }
    return Ok();
}

private void CombineChunks(string finalPath, string sessionTempDir, int totalChunks)
{
    using (FileStream fs = System.IO.File.Create(finalPath))
    {
        for (int i = 0; i < totalChunks; i++)
        {
            string chunkPath = Path.Combine(sessionTempDir, $"chunk_{i}");
            if (!System.IO.File.Exists(chunkPath))
                throw new InvalidOperationException($"Missing chunk {i}.");

            using (FileStream chunkFs = System.IO.File.OpenRead(chunkPath))
            {
                chunkFs.CopyTo(fs);
            }
        }
    }
}
```

## Form Submission

Upload files as part of form submission:

### Form Integration

```html
<form method="post" asp-action="UploadWithForm" enctype="multipart/form-data">
    <div class="form-group">
        <label asp-for="Name">Name:</label>
        <input asp-for="Name" required />
    </div>
    
    <div class="form-group">
        <label asp-for="Description">Description:</label>
        <textarea asp-for="Description"></textarea>
    </div>
    
    <div class="form-group">
        <label for="documents">Upload Documents:</label>
        <ejs-uploader id="documents"
            allowed-extensions=".pdf,.doc,.docx"
            max-file-size="5242880"
            auto-upload="false">
            <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
        </ejs-uploader>
    </div>
    
    <button type="submit">Submit Form</button>
</form>
```

### Form Submission Handler

```csharp
[HttpPost]
public async Task<IActionResult> UploadWithForm(string name, string description, IFormFile[] documents)
{
    try
    {
        // Validate form data
        if (string.IsNullOrEmpty(name))
            return BadRequest("Name is required");

        // Use a fixed upload directory — never embed user-supplied values in the path
        string uploadPath = Path.Combine(_webHostEnvironment.WebRootPath, "Uploads");
        if (!Directory.Exists(uploadPath))
            Directory.CreateDirectory(uploadPath);

        if (documents != null && documents.Length > 0)
        {
            foreach (IFormFile doc in documents)
            {
                if (doc.Length > 0)
                {
                    // Generate a server-side safe filename; reject any path traversal
                    string originalName = Path.GetFileName(doc.FileName);
                    if (string.IsNullOrWhiteSpace(originalName) ||
                        originalName.IndexOfAny(Path.GetInvalidFileNameChars()) >= 0)
                        return BadRequest("Invalid file name.");

                    string safeFileName = Guid.NewGuid().ToString("N") +
                                          Path.GetExtension(originalName).ToLowerInvariant();
                    string filePath = GetSafePath(uploadPath, safeFileName);

                    using (FileStream fs = System.IO.File.Create(filePath))
                    {
                        await doc.CopyToAsync(fs);
                    }
                }
            }
        }

        // Save form data to database
        // var record = new UploadRecord { Name = name, Description = description, ... };
        // await _context.SaveChangesAsync();

        return RedirectToAction("Success");
    }
    catch (Exception)
    {
        // Do not expose exception details to the client
        return StatusCode(500, "An error occurred while processing your request.");
    }
}
```

## Directory Upload

Upload complete folder structure:

### Server-Side Directory Handling

> **Security note:** Client-supplied relative paths (`file.Name`) can contain `..` sequences that escape the upload directory. Each path segment must be validated and the fully resolved path must be confirmed to remain under the base upload directory.

```csharp
[HttpPost]
public IActionResult Save(IFormFile[] uploader)
{
    string baseUploadPath = Path.Combine(_webHostEnvironment.WebRootPath, "Uploads");
    if (!Directory.Exists(baseUploadPath))
        Directory.CreateDirectory(baseUploadPath);

    if (uploader != null)
    {
        foreach (IFormFile file in uploader)
        {
            if (file.Length > 0)
            {
                // Sanitize every path segment — reject any traversal sequences
                string relativePath = file.Name ?? string.Empty;
                string[] segments = relativePath.Split('/', '\\');
                var safeSegments = new List<string>();
                foreach (string segment in segments)
                {
                    string safe = Path.GetFileName(segment); // strips directory parts
                    if (string.IsNullOrWhiteSpace(safe) ||
                        safe.IndexOfAny(Path.GetInvalidFileNameChars()) >= 0)
                        return BadRequest("Invalid file path.");
                    safeSegments.Add(safe);
                }

                string safePath = Path.Combine(safeSegments.ToArray());
                string filePath = Path.GetFullPath(Path.Combine(baseUploadPath, safePath));

                // Final guard: resolved path must stay inside the base directory
                if (!filePath.StartsWith(Path.GetFullPath(baseUploadPath) + Path.DirectorySeparatorChar,
                                         StringComparison.OrdinalIgnoreCase))
                    return BadRequest("Invalid file path.");

                string directoryPath = Path.GetDirectoryName(filePath)!;
                if (!Directory.Exists(directoryPath))
                    Directory.CreateDirectory(directoryPath);

                using (FileStream fs = System.IO.File.Create(filePath))
                {
                    file.CopyTo(fs);
                    fs.Flush();
                }
            }
        }
    }
    return Ok();
}
```

## File Removal

Handle file deletion after upload:

### Removal Handler

```csharp
[HttpPost]
public IActionResult Remove(string[] files)
{
    string uploadPath = Path.Combine(_webHostEnvironment.WebRootPath, "Uploads");
    
    if (files != null)
    {
        foreach (string file in files)
        {
            try
            {
                string filePath = Path.Combine(uploadPath, file);
                
                // Security check: Prevent directory traversal
                string fullPath = Path.GetFullPath(filePath);
                string fullUploadPath = Path.GetFullPath(uploadPath);
                
                if (!fullPath.StartsWith(fullUploadPath))
                    continue; // Skip suspicious paths
                
                if (System.IO.File.Exists(filePath))
                {
                    System.IO.File.Delete(filePath);
                }
            }
            catch (Exception ex)
            {
                // Log error but continue with other files
                Console.WriteLine($"Error deleting file: {ex.Message}");
            }
        }
    }
    return Ok();
}
```

### Track Removed Files

```html
<ejs-uploader id="uploader"
    success="onSuccess">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>

<script>
function onSuccess(args) {
    if (args.operation === 'remove') {
        console.log('Removed: ' + args.file.name);
        
        // Update UI or database to track removed files
        fetch('Home/LogRemoved', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ fileName: args.file.name })
        });
    }
}
</script>
```

## Error Handling

Handle upload failures gracefully:

### Comprehensive Error Handler

```html
<ejs-uploader id="uploader"
    failure="onFailure"
    success="onSuccess">
    <e-async-settings save-url="Home/Save" remove-url="Home/Remove"></e-async-settings>
</ejs-uploader>

<div id="errorContainer"></div>

<script>
function onFailure(args) {
    // Map status codes to safe, static messages — never render raw server responses
    let errorMsg;
    if (args.statusCode === 400) {
        errorMsg = 'Bad request. Please check the file and try again.';
    } else if (args.statusCode === 413) {
        errorMsg = 'File size exceeds the allowed limit.';
    } else if (args.statusCode === 500) {
        errorMsg = 'Server error. Please try again later.';
    } else if (args.statusCode === 0) {
        errorMsg = 'Network error or server unreachable.';
    } else {
        errorMsg = 'Upload failed. Please try again.';
    }

    // Use textContent — never innerHTML with server-supplied content (XSS risk)
    const container = document.getElementById('errorContainer');
    container.textContent = '';                       // clear previous content
    const div = document.createElement('div');
    div.style.cssText = 'color:red;padding:10px;border:1px solid #ffcccc;border-radius:4px;';
    div.textContent = 'Error: ' + errorMsg;           // safe: textContent only
    container.appendChild(div);
}

function onSuccess(args) {
    if (args.operation === 'upload') {
        console.log('Uploaded: ' + args.file.name);
        document.getElementById('errorContainer').textContent = '';
    }
}
</script>
```

### Server-Side Validation with Errors

```csharp
[HttpPost]
public IActionResult Save(IFormFile[] uploader)
{
    try
    {
        string uploadPath = Path.Combine(_webHostEnvironment.WebRootPath, "Uploads");

        if (uploader == null || uploader.Length == 0)
            return BadRequest("No files provided.");

        foreach (IFormFile file in uploader)
        {
            // Reject filenames with path separators or invalid characters
            string originalName = Path.GetFileName(file.FileName);
            if (string.IsNullOrWhiteSpace(originalName) ||
                originalName.IndexOfAny(Path.GetInvalidFileNameChars()) >= 0)
                return BadRequest("Invalid file name.");

            if (file.Length == 0)
                return BadRequest("One or more files are empty.");

            if (file.Length > 5242880)  // 5 MB
                return StatusCode(413, "One or more files exceed the size limit.");

            // Validate extension against an allowlist
            string ext = Path.GetExtension(originalName).ToLowerInvariant();
            var allowedExts = new[] { ".jpg", ".png", ".pdf" };
            if (!allowedExts.Contains(ext))
                return BadRequest("File type not allowed.");

            // Save with a server-generated safe filename
            if (!Directory.Exists(uploadPath))
                Directory.CreateDirectory(uploadPath);

            string safeFileName = Guid.NewGuid().ToString("N") + ext;
            string filePath = GetSafePath(uploadPath, safeFileName);
            using (FileStream fs = System.IO.File.Create(filePath))
            {
                file.CopyTo(fs);
                fs.Flush();
            }
        }

        return Ok(new { message = "Upload successful", count = uploader.Length });
    }
    catch (IOException)
    {
        // Do not expose internal exception details to the client
        return StatusCode(500, "A file I/O error occurred. Please try again.");
    }
    catch (Exception)
    {
        return StatusCode(500, "An unexpected error occurred. Please try again.");
    }
}
```

## Security Considerations

Implement security best practices:

### File Type Validation

```csharp
// Whitelist allowed MIME types
private readonly Dictionary<string, string> _allowedMimeTypes = new()
{
    { "image/jpeg", ".jpg" },
    { "image/png", ".png" },
    { "application/pdf", ".pdf" },
    { "application/msword", ".doc" }
};

[HttpPost]
public IActionResult Save(IFormFile[] uploader)
{
    foreach (IFormFile file in uploader)
    {
        // Check MIME type
        if (!_allowedMimeTypes.ContainsKey(file.ContentType))
            return BadRequest("Invalid file type");
        
        // Check file signature (magic bytes)
        byte[] buffer = new byte[512];
        file.OpenReadStream().Read(buffer, 0, 512);
        
        if (!IsValidFileSignature(buffer, file.ContentType))
            return BadRequest("File content does not match extension");
    }
    
    return Ok();
}

private bool IsValidFileSignature(byte[] buffer, string mimeType)
{
    // PNG: 89 50 4E 47
    if (mimeType == "image/png")
        return buffer[0] == 0x89 && buffer[1] == 0x50 && buffer[2] == 0x4E && buffer[3] == 0x47;
    
    // JPEG: FF D8 FF
    if (mimeType == "image/jpeg")
        return buffer[0] == 0xFF && buffer[1] == 0xD8 && buffer[2] == 0xFF;
    
    return true;
}
```

### Prevent Directory Traversal

```csharp
private string GetSafePath(string baseDir, string fileName)
{
    string fullPath    = Path.GetFullPath(Path.Combine(baseDir, fileName));
    string fullBaseDir = Path.GetFullPath(baseDir);

    // Append a separator so "Uploads_evil" cannot prefix-match "Uploads"
    if (!fullPath.StartsWith(fullBaseDir + Path.DirectorySeparatorChar,
                             StringComparison.OrdinalIgnoreCase))
        throw new InvalidOperationException("Invalid file path.");

    // Return the fully-resolved path, not the raw Path.Combine result
    return fullPath;
}
```

## Performance Optimization

Optimize upload performance:

### Async File Handling

```csharp
[HttpPost]
public async Task<IActionResult> Save(IFormFile[] uploader)
{
    string uploadPath = Path.Combine(_webHostEnvironment.WebRootPath, "Uploads");
    Directory.CreateDirectory(uploadPath);

    if (uploader != null)
    {
        var tasks = uploader.Select(async file =>
        {
            if (file.Length > 0)
            {
                // Always generate a server-side safe filename
                string originalName = Path.GetFileName(file.FileName);
                if (string.IsNullOrWhiteSpace(originalName) ||
                    originalName.IndexOfAny(Path.GetInvalidFileNameChars()) >= 0)
                    throw new InvalidOperationException("Invalid file name.");

                string safeFileName = Guid.NewGuid().ToString("N") +
                                      Path.GetExtension(originalName).ToLowerInvariant();
                string filePath = GetSafePath(uploadPath, safeFileName);

                using (FileStream fs = System.IO.File.Create(filePath))
                {
                    await file.CopyToAsync(fs);
                }
            }
        });

        await Task.WhenAll(tasks);
    }

    return Ok();
}
```

### Concurrent Upload Limits

```csharp
// Configure in Program.cs
private static readonly SemaphoreSlim _uploadSemaphore = new SemaphoreSlim(5); // Max 5 concurrent

[HttpPost]
public async Task<IActionResult> Save(IFormFile[] uploader)
{
    await _uploadSemaphore.WaitAsync();
    
    try
    {
        // Save files...
        return Ok();
    }
    finally
    {
        _uploadSemaphore.Release();
    }
}
```

## Summary

| Feature | Implementation | Benefit |
|---------|----------------|---------|
| Chunk Upload | chunk-size property + server handling | Upload large files reliably |
| Form Integration | no auto-upload + form submission | Combine file upload with data |
| Directory Upload | directory-upload property | Upload folder structures |
| Error Handling | Status codes + error messages | Better UX |
| Security | MIME validation + path sanitization | Prevent attacks |
| Performance | Async + concurrency limits | Handle multiple uploads |
