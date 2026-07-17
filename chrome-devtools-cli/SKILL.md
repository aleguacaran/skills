---
name: chrome-devtools-cli
description: Uses Chrome DevTools MCP CLI via bash for browser automation, debugging, and inspection. Activates when you need to navigate pages, click elements, fill forms, take screenshots, inspect network/console, or run Lighthouse audits.
triggers:
  - navigate to a url in the browser
  - open a web page
  - take a screenshot of a page
  - click on an element
  - fill out a form
  - inspect browser console errors
  - check network requests
  - run a lighthouse audit
  - debug a web page
  - automate browser interactions
  - extract data from a web page
  - test a web form
  - record a performance trace
  - analyze web performance
  - inspect page accessibility
  - upload a file to a web page
  - press keyboard keys in a browser
---

## Overview

This skill wraps `chrome-devtools-mcp` — Google's official tool for browser automation, debugging, and performance analysis — into CLI commands you run via bash.

**Daemon model**: The CLI runs a background MCP server + browser daemon (Unix socket on Linux). Start once, all subsequent commands reuse the same instance. Browser state (cookies, tabs, login) persists — no new browser per interaction.

## Setup

### Installation

```bash
# Install globally (optional, avoids npx fetch latency)
bunx --global chrome-devtools-mcp@latest

# Or use via npx/bunx directly (no install needed)
bunx chrome-devtools-mcp@latest <command>
```

### Start the daemon

```bash
bunx chrome-devtools-mcp@latest start \
  --executable-path=/usr/bin/brave \
  --no-usage-statistics \
  --performance-crux=false
```

Start once at the session beginning. The daemon auto-starts on first command if not running.

### Stop the daemon

```bash
bunx chrome-devtools-mcp@latest stop
```

### Check status

```bash
bunx chrome-devtools-mcp@latest status
```

### Configuration flags

Pass during `start` or as persistent flags:

| Flag | Purpose |
|---|---|
| `--headless` | Run without UI |
| `--executable-path=<path>` | Custom browser (e.g., Brave, Chromium) |
| `--browser-url=<url>` | Connect to running browser (e.g., `http://127.0.0.1:9222`) |
| `--auto-connect` | Auto-connect to local Chrome 144+ |
| `--isolated` | Temp profile (cleaned up on close) |
| `--user-data-dir=<path>` | Persistent profile path |
| `--channel=canary|dev|beta|stable` | Chrome channel |
| `--viewport=<WxH>` | Viewport size (e.g., `1280x720`) |
| `--no-usage-statistics` | Disable telemetry |
| `--no-performance-crux` | Disable CrUX field data |
| `--slim` | Enable only navigation + screenshot + evaluate (3 tools) |
| `--screenshot-format=jpeg|png|webp` | Default screenshot format |
| `--screenshot-quality=<0-100>` | Screenshot compression quality |
| `--categoryExtensions` | Enable extension tools |
| `--experimentalVision` | Enable coordinate-based click_at |
| `--experimentalScreencast` | Enable screencast recording (needs ffmpeg) |
| `--memoryDebugging` | Enable heap snapshot tools |
| `--categoryExperimentalThirdParty` | Enable 3P devtools from page |
| `--categoryExperimentalWebmcp` | Enable WebMCP tools (Chrome 149+) |

## Tool Reference

All tools run as: `bunx chrome-devtools-mcp@latest <tool> [args] [--flags]`

Required args are positional, optional args use `--flag value`.

Append `--output-format=json` for machine-parseable output.

### Navigation (6 tools)

| Tool | Usage |
|---|---|
| `new_page <url>` | Open new tab and load URL. `--background` to open without focus. `--isolatedContext <name>` for separate cookie jar |
| `navigate_page <url>` | Go to URL. `--type back\|forward\|reload` for nav actions. `--ignore-cache` for fresh load |
| `list_pages` | List all open pages/IDs |
| `select_page <page-id>` | Switch active page. `--bring-to-front` to focus |
| `close_page <page-id>` | Close a page (can't close the last one) |
| `wait_for <text>` | NOT AVAILABLE in CLI mode. Use `evaluate_script` with polling instead |

### Input Automation (10 tools)

| Tool | Usage |
|---|---|
| `take_snapshot` | Get page accessibility tree with element `uid`s. Always the first step before interaction |
| `click <uid>` | Click element by uid. `--dbl-click` for double click. `--include-snapshot` for updated state |
| `fill <uid> <value>` | Type into input/textarea. `"true"`/`"false"` for checkboxes/toggles |
| `fill_form` | NOT AVAILABLE in CLI mode. Fill multiple elements individually instead |
| `type_text <text>` | Type into focused element. `--submit-key Enter\|Tab\|Escape` |
| `press_key <key>` | Key combo: `"Enter"`, `"Control+A"`, `"Control+Shift+R"` |
| `hover <uid>` | Hover over element |
| `drag <from-uid> <to-uid>` | Drag element onto another |
| `upload_file <file-path> <uid>` | Upload file via file input element |
| `handle_dialog <accept\|dismiss>` | Handle browser dialog. `--prompt-text` for prompt dialogs |
| `click_at <x> <y>` | Click at coordinates (needs `--experimentalVision`) |

### Debugging (8 tools)

| Tool | Usage |
|---|---|
| `take_screenshot` | Capture page screenshot. `--file-path /tmp/shot.png`. `--full-page` for entire page. `--uid <uid>` for element-only. `--format png\|jpeg\|webp` |
| `take_snapshot` | Get a11y tree snapshot. `--file-path /tmp/snap.txt`. `--verbose` for full tree |
| `evaluate_script <function>` | Run JS in page context. Pass `() => document.title` or `async () => await fetch(url)`. `--file-path /tmp/out.json` to save output. Supports `--args` |
| `list_console_messages` | Get console logs. `--types error\|warn\|info`. `--include-preserved-messages` for history |
| `get_console_message <msgid>` | Get single console message details |
| `lighthouse_audit` | Run Lighthouse. `--mode navigation\|snapshot`. `--device desktop\|mobile`. `--output-dir-path /tmp/lh` |
| `screencast_start` | Start video recording (needs `--experimentalScreencast`). `--file-path /tmp/recording.webm` |
| `screencast_stop` | Stop recording |

### Network (2 tools)

| Tool | Usage |
|---|---|
| `list_network_requests` | List network requests. `--resource-types xhr\|fetch\|document\|...`. `--page-idx`, `--page-size` for pagination. `--include-preserved-requests` for history |
| `get_network_request [reqid]` | Get request/response details. `--request-file-path` / `--response-file-path` to save bodies |

### Emulation (2 tools)

| Tool | Usage |
|---|---|
| `emulate` | Set device/network/color. `--viewport 375x812x3,mobile,touch`. `--network-conditions "Slow 3G"`. `--geolocation "40.7,-74.0"`. `--user-agent "..."`. `--color-scheme dark\|light`. `--cpu-throttling-rate 4` |
| `resize_page <width> <height>` | Set page viewport size |

### Performance (3 tools)

| Tool | Usage |
|---|---|
| `performance_start_trace` | Start tracing. `--reload` to reload on start. `--file-path /tmp/trace.json.gz`. `--auto-stop` |
| `performance_stop_trace` | Stop tracing. `--file-path /tmp/trace.json.gz` |
| `performance_analyze_insight <insight-name> <insight-set-id>` | Get detailed insight from trace results |

### Memory (12 tools — needs `--memoryDebugging`)

| Tool | Usage |
|---|---|
| `take_heapsnapshot <file-path>` | Capture heap snapshot to .heapsnapshot file |
| `get_heapsnapshot_summary <file-path>` | Summary stats with native contexts |
| `get_heapsnapshot_details <file-path>` | Class list with counts/sizes. `--filter-name`, pagination |
| `get_heapsnapshot_class_nodes <file-path> <id>` | Instances of a class. `--filter-name`, pagination |
| `get_heapsnapshot_object_details <file-path> <node-id>` | Size, type, distance, detachedness |
| `get_heapsnapshot_edges <file-path> <node-id>` | Outgoing references from a node |
| `get_heapsnapshot_retainers <file-path> <node-id>` | What retains this node. Pagination |
| `get_heapsnapshot_retaining_paths <file-path> <node-id>` | Why not GC'd. `--max-depth`, `--max-nodes` |
| `get_heapsnapshot_dominators <file-path> <node-id>` | Dominator chain (who keeps what alive) |
| `get_heapsnapshot_duplicate_strings <file-path>` | Duplicate strings. Pagination |
| `compare_heapsnapshots <base-path> <current-path>` | Diff two snapshots. `--class-index` for per-class |
| `close_heapsnapshot <file-path>` | Free snapshot memory |

### Extensions (5 tools — needs `--categoryExtensions`)

| Tool | Usage |
|---|---|
| `install_extension <path>` | Install unpacked extension |
| `list_extensions` | List all installed extensions |
| `reload_extension <id>` | Reload an unpacked extension |
| `trigger_extension_action <id>` | Trigger extension popup/action |
| `uninstall_extension <id>` | Uninstall extension |

### Third-party (2 tools — needs `--categoryExperimentalThirdParty`)

| Tool | Usage |
|---|---|
| `list_3p_developer_tools` | List tools exposed by the page via `window.__dtmcp` |
| `execute_3p_developer_tool <tool-name>` | Execute a page-exposed tool. `--params '{"key":"val"}'` |

### WebMCP (2 tools — needs `--categoryExperimentalWebmcp` + Chrome 149+)

| Tool | Usage |
|---|---|
| `list_webmcp_tools` | List WebMCP tools the page exposes |
| `execute_webmcp_tool <tool-name>` | Execute WebMCP tool. `--input '{"key":"val"}'` |

## Workflow Patterns

### Basic interaction loop

```bash
# 1. Start daemon once
bunx chrome-devtools-mcp@latest start --executable-path=/usr/bin/brave --no-usage-statistics

# 2. Navigate
bunx chrome-devtools-mcp@latest new_page "https://example.com"

# 3. Get page structure
bunx chrome-devtools-mcp@latest take_snapshot

# 4. Interact using element uids
bunx chrome-devtools-mcp@latest click "element-uid-123"
bunx chrome-devtools-mcp@latest fill "input-uid-456" "search query"
bunx chrome-devtools-mcp@latest press_key "Enter"

# 5. Verify
bunx chrome-devtools-mcp@latest take_screenshot --file-path /tmp/result.png

# 6. Stop when done
bunx chrome-devtools-mcp@latest stop
```

### Data extraction

```bash
# Navigate
bunx chrome-devtools-mcp@latest navigate_page "https://example.com/products"

# Extract structured data
bunx chrome-devtools-mcp@latest evaluate_script \
  "() => Array.from(document.querySelectorAll('.product')).map(p => ({title: p.querySelector('h2')?.textContent, price: p.querySelector('.price')?.textContent}))" \
  --output-format=json
```

### Debugging

```bash
# Navigate to page
bunx chrome-devtools-mcp@latest navigate_page "https://app.example.com"

# Check console errors (with source-mapped stacks)
bunx chrome-devtools-mcp@latest list_console_messages --types error --include-preserved-messages

# Inspect network
bunx chrome-devtools-mcp@latest list_network_requests --resource-types xhr,fetch

# Run Lighthouse
bunx chrome-devtools-mcp@latest lighthouse_audit --mode snapshot
```

### Performance audit

```bash
# Start trace with reload
bunx chrome-devtools-mcp@latest performance_start_trace --reload --file-path /tmp/trace.json.gz

# Wait for page to load, then stop
bunx chrome-devtools-mcp@latest performance_stop_trace --file-path /tmp/trace.json.gz
```

### Form testing

```bash
bunx chrome-devtools-mcp@latest navigate_page "https://example.com/login"
bunx chrome-devtools-mcp@latest take_snapshot
# (get uids from snapshot output)
bunx chrome-devtools-mcp@latest fill "uid-email" "user@example.com"
bunx chrome-devtools-mcp@latest fill "uid-password" "mypassword"
bunx chrome-devtools-mcp@latest click "uid-submit"
bunx chrome-devtools-mcp@latest take_snapshot
```

### Mobile emulation

```bash
bunx chrome-devtools-mcp@latest start --executable-path=/usr/bin/brave --no-usage-statistics
bunx chrome-devtools-mcp@latest new_page "https://example.com"
bunx chrome-devtools-mcp@latest emulate \
  --viewport "375x812x3,mobile,touch,landscape" \
  --network-conditions "Slow 3G" \
  --color-scheme dark
bunx chrome-devtools-mcp@latest take_screenshot --file-path /tmp/mobile-view.png
```

## Daemon Lifecycle & Browser Persistence

- **Start**: `chrome-devtools start` launches the MCP server + browser. The daemon listens on a Unix socket.
- **Reuse**: Every subsequent `chrome-devtools <tool>` connects to the same daemon. Cookies, tabs, login state all persist.
- **Idle**: The daemon stays alive until explicitly stopped. No overhead per interaction.
- **Stop**: `chrome-devtools stop` kills the daemon and browser. Cleanup is automatic.
- **Crash recovery**: `chrome-devtools stop` then `start` resets everything.

Multiple terminal/agent sessions share the same daemon.

## Troubleshooting

| Issue | Fix |
|---|---|
| Daemon hangs | `bunx chrome-devtools-mcp@latest stop` then restart |
| Browser not found | Set `--executable-path` to your browser binary |
| Port conflicts | Use `--browser-url` to connect to existing instance |
| Need verbose logs | Run with `DEBUG=* bunx chrome-devtools-mcp@latest <cmd>` |
| CLI tool not found | Install globally: `bunx --global chrome-devtools-mcp@latest` |

## References

- Official docs: https://github.com/ChromeDevTools/chrome-devtools-mcp
- Tool reference: https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/docs/tool-reference.md
- CLI docs: https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/docs/cli.md
- Troubleshooting: https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/docs/troubleshooting.md
