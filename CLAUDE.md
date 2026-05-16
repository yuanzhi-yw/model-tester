# Model Tester — OpenAI-Compatible API Model Tester

A single-page static web app for testing OpenAI-compatible API endpoints. No backend, no build step — just open `index.html` in a browser.

## Project Structure

```
model-tester/
├── index.html          # The entire app (HTML + CSS + JS, self-contained)
├── README.md           # Project docs
├── LICENSE             # MIT
└── CLAUDE.md           # This file
```

## What It Does

1. **Channel Management** — Add one or more API endpoints (name + base URL + API key). Persisted in localStorage. Import/Export as JSON.
2. **Model Discovery** — Fetch `/models` from each channel, list all available models.
3. **Chat Test** — Send a streaming `/chat/completions` request, measure TTFT (time to first token), total latency, token usage. Validate response content.
4. **Tool Calling Test** — Test function calling capability with `tool_choice` (named → auto fallback). Report pass/fail/partial.
5. **Concurrent Testing** — Configurable concurrency pool (1/3/5/10).
6. **Test History** — Persisted in localStorage with timestamps. Shown as colored dots in model list.
7. **Export Results** — JSON and CSV download.

## UI Design Requirements

**CRITICAL: The UI must be redesigned from scratch. The current code has a functional prototype but the design needs a fresh, modern approach.**

### Design Direction

- **Dark theme**, clean, minimal — inspired by Linear/Vercel/Raycast aesthetic
- Background: near-black (#09090b or similar), panels slightly lighter
- One accent color (blue-violet ~#7170ff), used sparingly for primary actions
- Inter font (load from Google Fonts CDN)
- Strong typographic hierarchy — large bold titles, small uppercase labels
- No emoji, no decorative illustrations, no gradient backgrounds
- Monospace font for model IDs, API keys, technical data
- Pill-shaped status badges with colored dots (green pass, red fail, amber partial)
- Smooth micro-animations (hover states, progress bar, spinners)
- Subtle borders (rgba white at 0.05–0.08 opacity), not heavy dividers
- Cards with rounded corners (8px), no visible box shadows except on toasts
- Responsive — works on mobile too

### Layout

- Single column, max-width ~840px, centered
- Sections stacked vertically with generous spacing (80px between sections)
- Channel cards: expandable cards with name/URL/key fields
- Model list: scrollable list with checkboxes, model ID, source tag, history badge
- Results: clean table with model, channel, chat status, tool status columns
- Action buttons: primary (accent bg), ghost (transparent), danger (red text)

### Key UI Components

1. **Header** — "Model Tester" title + description paragraph
2. **Channels Section** — Cards with 3 fields (Name, Base URL, API Key), add/remove buttons, import/export
3. **Fetch Section** — Primary "Fetch models" button + "Clear all" ghost button + status text
4. **Models Section** — Filter chips for channels, select all/deselect/text only links, scrollable model list, test button with concurrency selector
5. **Results Section** — Table with export buttons (JSON/CSV)
6. **Footer Note** — CORS warning + localStorage note

## Technical Requirements

### Streaming Chat Test
- Use `stream: true` with `stream_options: { include_usage: true }`
- If server rejects `stream_options`, **fallback** to streaming without it
- Parse SSE events, capture first content chunk for TTFT, accumulate content, capture usage
- Validate: response should contain "hello" or "hi" — if empty or no expected content, mark as fail

### Tool Calling Test
- First attempt: `tool_choice: { type: 'function', function: { name: 'get_weather' } }` (forced)
- If server errors or model replies with text: fallback to `tool_choice: 'auto'`
- If model still replies with text: mark as `warn` ("Replied with text, no tool call")
- If no tool_calls and no content: mark as `fail`

### Concurrency
- Use an async pool (`runPool` function) — configurable via dropdown (1/3/5/10)
- Default: 3

### Channel Names
- Each channel has a `name` field — editable in the UI
- Display channel name (not "Channel 1") in headers, filter chips, results table
- If name is empty, fall back to "CH1", "CH2", etc.

### Export
- JSON: array of result objects with all fields
- CSV: headers + escaped rows, downloadable file

### Data Persistence
- Channels: `localStorage` key `mt_channels`
- Test history: `localStorage` key `mt_testHistory`

## Color System

```
--bg:         #09090b
--panel:      #0f1011  
--surface:    #191a1b
--text:       #f7f8f8
--text2:      #d0d6e0
--text3:      #8a8f98
--text4:      #62666d
--accent:     #5e6ad2
--accent2:    #7170ff
--accent3:    #828fff
--green:      #10b981
--red:        #ef4444
--amber:      #f59e0b
--border:     rgba(255,255,255,0.05)
--border2:    rgba(255,255,255,0.08)
```

## Source Reference

The original functional prototype is at: `/home/dc/.hermes/cache/documents/doc_d90f4596b57d_model-tester.html`
Read it for the complete JavaScript logic — reuse ALL the JS functionality, but redesign the HTML structure and CSS completely.

## Deliverable

A single `index.html` file that is:
- Self-contained (no external JS/CSS files, only Google Fonts CDN)
- Fully functional with all features listed above
- Beautifully designed per the UI requirements
- Openable directly in a browser

Also create:
- `README.md` — project description, features, screenshot placeholder, usage instructions, license
- `LICENSE` — MIT license
