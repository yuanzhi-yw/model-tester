# Task: Add i18n (中/EN) + Light/Dark Theme Toggle to Model Tester

## Requirements

### 1. Language Toggle (中文 / English)

Add a language switcher in the header area (top-right). Clicking it toggles between English and Chinese.

**Implementation approach:**
- Create a `LANG` object with `en` and `zh` keys containing all UI strings
- Store current language in `localStorage` key `mt_lang` (default: `en`)
- All UI text must use `t('key')` function instead of hardcoded strings
- When language changes, re-render all dynamic content
- The switcher itself shows: "中文" when in English mode, "EN" when in Chinese mode

**Chinese translations needed (non-exhaustive):**
```
en → zh mapping:
"Model Tester" → "模型测试器"
"Add one or more endpoints, discover models, and verify chat + tool-calling across all channels." → "添加一个或多个 API 端点，发现模型，验证聊天和工具调用能力。"
"Channels" → "渠道"
"Export" → "导出"
"Import" → "导入"
"Add channel" → "添加渠道"
"Name" → "名称"
"Base URL" → "接口地址"
"API Key" → "密钥"
"Remove" → "移除"
"Fetch models" → "获取模型"
"Fetching…" → "获取中…"
"Clear all" → "全部清除"
"Models" → "模型"
"Select all" → "全选"
"Deselect all" → "全不选"
"Text only" → "仅文本"
"Clear history" → "清除历史"
"Test selected" → "测试选中"
"Testing…" → "测试中…"
"Concurrency" → "并发数"
"selected" → "已选"
"Results" → "结果"
"Export JSON" → "导出 JSON"
"Export CSV" → "导出 CSV"
"Model" → "模型"
"Channel" → "渠道"
"Chat" → "聊天"
"Tools" → "工具"
"Pass" → "通过"
"Fail" → "失败"
"Partial" → "部分"
"Pending" → "等待中"
"Channel added" → "已添加渠道"
"Channel removed" → "已移除渠道"
"Keep at least one channel" → "至少保留一个渠道"
"Channels exported" → "渠道已导出"
"Channels imported" → "渠道已导入"
"Found X models" → "找到 X 个模型"
"No models found" → "未找到模型"
"Test history cleared" → "测试历史已清除"
"Results exported as JSON" → "结果已导出为 JSON"
"Results exported as CSV" → "结果已导出为 CSV"
"No results to export" → "没有可导出的结果"
"X models tested" → "已测试 X 个模型"
"CORS" → "跨域"
"If the browser blocks requests, use an extension or local proxy." → "如果浏览器阻止请求，请使用浏览器扩展或本地代理。"
"Channels persist in localStorage. Export saves API keys in plain text." → "渠道信息保存在 localStorage 中。导出会包含明文 API 密钥。"
```

### 2. Light/Dark Theme Toggle

Add a theme toggle button in the header area (next to language switcher).

**Implementation approach:**
- Add `[data-theme="light"]` CSS variables that override the dark defaults
- Store current theme in `localStorage` key `mt_theme` (default: `dark`)
- Apply `data-theme` attribute on `<html>` element
- Toggle button: show 🌙 icon in light mode (switch to dark), ☀️ icon in dark mode (switch to light)
- No emoji in buttons per design rules — use simple SVG or Unicode symbols: ◐ (for toggle), or use text "Light"/"Dark", or ◑/◐

**Light theme variables:**
```css
[data-theme="light"] {
  --bg: #ffffff;
  --panel: #f9f9fb;
  --surface: #f0f0f3;
  --hover: rgba(0,0,0,0.03);
  --text: #1a1a1a;
  --text2: #3d3d3d;
  --text3: #6b6b6b;
  --text4: #9a9a9a;
  --accent: #5e6ad2;
  --accent2: #7170ff;
  --accent3: #828fff;
  --green: #10b981;
  --red: #ef4444;
  --amber: #f59e0b;
  --border: rgba(0,0,0,0.06);
  --border2: rgba(0,0,0,0.10);
  --border3: rgba(0,0,0,0.08);
}
```

Also adjust these CSS rules for light mode:
- `.channel:hover` background → `rgba(0,0,0,0.02)`
- `.btn--ghost` background → `rgba(0,0,0,0.03)`
- `.field input` background → `rgba(0,0,0,0.02)`
- `.field input:focus` box-shadow → adjust for light
- `tbody tr:hover` → `rgba(0,0,0,0.015)`
- Scrollbar colors → use rgba(0,0,0,...) instead of rgba(255,255,255,...)
- Toast shadow → lighter shadow for light theme

### 3. Header Layout

The header should have a top row with the title on the left and the toggle controls on the right:
```
[Model Tester]                    [◐ Light] [中文]
[description paragraph]
```

Use flexbox with `justify-content: space-between` for the top row.

### Important Constraints
- Keep it as a single self-contained HTML file
- No external dependencies except Google Fonts CDN
- Preserve ALL existing JS functionality
- Language and theme preferences persist in localStorage
- When switching language, ALL visible text updates immediately (including dynamically rendered content like channels, models, results)
