# Complete Guide: D3.js Setup with Local Server

## Initial Problem
- HTML file with D3.js couldn't load CSV data
- CORS errors due to browser security
- D3.js couldn't load from internet

## Required Files
```
d3Example/
├── d3-example1.html     # Main HTML file
├── PovertyRate.csv      # CSV data
└── d3.v5.min.js         # D3.js library (downloaded)
```

## Step 1: Verify Files
```bash
# Navigate to project folder
cd d3Example

# List files to verify they exist
ls -la
```

## Step 2: Download D3.js Locally
```bash
# Download D3.js to avoid connection issues
curl -o d3.v5.min.js https://d3js.org/d3.v5.min.js

# Verify successful download (should show ~248K)
ls -la
```

## Step 3: Modify HTML to Use Local D3.js
```bash
# Change D3.js reference from external URL to local file
sed -i '' 's|https://d3js.org/d3.v5.min.js|d3.v5.min.js|g' d3-example1.html

# Verify the change was made correctly
grep -n "d3.v5.min.js" d3-example1.html
# Should show: 45: <script src="d3.v5.min.js"></script>
```

## Step 4: Fix CSV Path (CORS Issue)
```bash
# Remove absolute URL from CSV to use relative path
sed -i '' 's|http://0.0.0.0:8000/||g' d3-example1.html
```

## Step 5: Start Local Server
```bash
# Start Python server on port 8000
python3 -m http.server

# Server should display:
# Serving HTTP on :: port 8000 (http://[::]:8000/) ...
```

## Step 6: Open in Browser
1. Go to: `http://localhost:8000`
2. Click on: `d3-example1.html`
3. **NEVER** open file directly from Finder

## Step 7: Verify Functionality
**Server logs should show 200 codes (success):**
```
::1 - - "GET /d3-example1.html HTTP/1.1" 200 -
::1 - - "GET /d3.v5.min.js HTTP/1.1" 200 -
::1 - - "GET /PovertyRate.csv HTTP/1.1" 200 -
```

## Troubleshooting

### Error: "ERR_CONNECTION_REFUSED"
```bash
# Restart server if it closed
cd d3Example
python3 -m http.server
```

### Error: "Can't be reloaded"
- Close Chrome completely (Cmd + Q)
- Reopen and navigate to localhost:8000

### Error: CORS Policy
```bash
# Verify paths are relative, not absolute URLs
grep -n "http://" d3-example1.html
# Should show no results
```

### Check Browser Console
- Right-click → "Inspect" → "Console" tab
- Look for red errors
- All files should load without errors

## Useful Verification Commands

```bash
# Check file structure
ls -la

# Check D3.js references
grep -n "d3.v5.min.js" d3-example1.html

# Check for absolute URLs
grep -n "http://" d3-example1.html

# Check CSV content
head PovertyRate.csv
```

## Final Result
- Table with descriptive statistics filled with data
- Chart with blue line (US) and red line (LA County)
- No errors in browser console
- Server showing HTTP 200 codes for all resources

## Important Notes
1. **Always use local server** for D3.js projects
2. **Keep files in same folder** to avoid path issues
3. **Use relative paths** in code, never absolute URLs
4. **Check browser console** for any problems
5. **Server must keep running** while working with visualization
