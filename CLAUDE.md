# WTFDIN Development Guide

## Testing with Playwright

The Playwright MCP plugin only allows `http:`, `https:`, `about:`, and `data:` protocols. Start a local server to test HTML files.

### Start server and verify it's ready:
```bash
nohup python3 -m http.server 8080 > /dev/null 2>&1 &
sleep 1
curl -s -o /dev/null -w "%{http_code}" http://localhost:8080/index.html
```

Then navigate to `http://localhost:8080/index.html` in Playwright.

### Single command pattern:
```bash
nohup python3 -m http.server 8080 > /dev/null 2>&1 & sleep 1 && curl -sf http://localhost:8080/ > /dev/null && echo "Server ready"
```

## Project Structure

- `index.html` - Single-file app (HTML + CSS + JS inline)
- `prd.json` - Requirements with `passes: true/false` status
- `progress.txt` - Development log for context across sessions
