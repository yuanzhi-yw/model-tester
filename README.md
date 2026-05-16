# Model Tester

A single-page static web app for testing OpenAI-compatible API endpoints. No backend, no build step — just open `index.html` in a browser.

![Screenshot placeholder](screenshot.png)

## Features

- **Channel Management** — Add multiple API endpoints (name, base URL, API key). Persisted in localStorage. Import/export as JSON.
- **Model Discovery** — Fetch `/models` from each channel and list all available models.
- **Streaming Chat Test** — Send a streaming `/chat/completions` request, measure TTFT (time to first token), total latency, and token usage. Validates response content.
- **Tool Calling Test** — Test function calling with `tool_choice` (forced named → auto fallback). Reports pass/fail/partial.
- **Concurrent Testing** — Configurable concurrency pool (1/3/5/10 simultaneous requests).
- **Test History** — Results persisted in localStorage with timestamps, shown as colored dots in the model list.
- **Export Results** — Download results as JSON or CSV.

## Usage

1. Open `index.html` in any modern browser.
2. Add one or more channels (API endpoint name, base URL, and API key).
3. Click **Fetch models** to discover available models from all channels.
4. Select models to test, choose concurrency level, and click **Test selected**.
5. View results in the table below. Export as JSON or CSV.

## Technical Details

- Self-contained single HTML file (no external JS/CSS dependencies, only Google Fonts CDN)
- Dark theme UI inspired by Linear/Vercel/Raycast aesthetic
- Streaming SSE parsing with TTFT measurement
- Fallback logic for `stream_options` and `tool_choice` compatibility
- Responsive design — works on desktop and mobile

## CORS Note

If the browser blocks requests to your API endpoints, use a CORS browser extension or a local proxy.

## Data Storage

All data is stored in `localStorage`:
- Channels: `mt_channels`
- Test history: `mt_testHistory`

Exporting channels saves API keys in plain text — handle exported files accordingly.

## License

MIT
