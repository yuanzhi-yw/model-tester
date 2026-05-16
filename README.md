# 模型测试器 / Model Tester

[English](#english) | [中文](#中文)

---

## 中文

一个单页静态 Web 应用，用于测试 OpenAI 兼容 API 端点。无需后端，无需构建，直接浏览器打开即用。

**在线体验：** https://yuanzhi-yw.github.io/model-tester/

### 功能

- **多渠道管理** — 添加多个 API 端点（名称、接口地址、密钥），支持导入/导出 JSON
- **模型发现** — 从每个渠道拉取 `/models` 列表，汇总展示所有可用模型
- **聊天测试** — 流式请求 `/chat/completions`，测量首 Token 时间（TTFT）、总延迟、Token 用量
- **工具调用测试** — 测试 Function Calling 能力，支持 `tool_choice` 强制调用 → 自动回退
- **并发控制** — 可配置并发数（1 / 3 / 5 / 10），批量测试更快
- **历史记录** — 测试结果持久化到 `localStorage`，模型列表显示彩色历史点
- **导出结果** — 支持 JSON 和 CSV 格式下载
- **中英文切换** — 界面支持中文 / English，偏好自动保存
- **明暗主题** — 支持浅色 / 深色主题，偏好自动保存

### 使用方法

#### 第一步：添加渠道

1. 打开页面，在「渠道」区域填写：
   - **名称**：自定义名称，如 `OpenAI`、`我的代理`
   - **接口地址**：API 的 base URL，如 `https://api.openai.com/v1`
   - **密钥**：API Key，如 `sk-...`
2. 点击「添加渠道」可添加多个端点同时测试
3. 点击「导出」可将渠道配置保存为 JSON；点击「导入」可从 JSON 文件恢复

#### 第二步：获取模型

点击「**获取模型**」按钮，工具会请求每个渠道的 `/models` 接口，汇总所有可用模型。

- 多渠道时，顶部会出现渠道筛选标签，可按渠道过滤模型列表
- 点击「仅文本」可自动过滤掉图像、音频、嵌入等非文本模型

#### 第三步：选择并测试

1. 勾选要测试的模型（支持全选 / 全不选 / 仅文本）
2. 右下角选择并发数（默认 3）
3. 点击「**测试选中**」开始测试

每个模型会依次进行：
- **聊天测试**：发送 `"Say exactly: hello"`，验证响应内容，记录 TTFT 和总延迟
- **工具调用测试**：发送天气查询，强制调用 `get_weather` 工具，验证是否返回 tool_calls

#### 第四步：查看结果

结果表格显示每个模型的：
- **聊天**：通过 / 失败 + TTFT + 总延迟 + Token 用量 + 响应预览
- **工具**：通过 / 部分 / 失败 + 函数名 + 参数 + 耗时

点击「导出 JSON」或「导出 CSV」可下载完整结果。

### 注意事项

- **跨域（CORS）**：浏览器直接请求第三方 API 可能被 CORS 策略拦截。解决方案：
  - 使用支持跨域的 API 代理
  - 安装浏览器 CORS 扩展（仅用于开发测试）
  - 在本地起一个反向代理
- **数据安全**：渠道信息（含 API Key）保存在浏览器 `localStorage` 中，导出文件包含明文密钥，请妥善保管

---

## English

A single-page static web app for testing OpenAI-compatible API endpoints. No backend, no build step — just open `index.html` in a browser.

**Live demo:** https://yuanzhi-yw.github.io/model-tester/

### Features

- **Channel Management** — Add multiple API endpoints (name, base URL, API key). Import/export as JSON
- **Model Discovery** — Fetch `/models` from each channel and list all available models
- **Chat Test** — Streaming `/chat/completions` with TTFT, total latency, and token usage measurement
- **Tool Calling Test** — Tests Function Calling with forced `tool_choice` → auto fallback
- **Concurrency Control** — Configurable pool size (1 / 3 / 5 / 10) for batch testing
- **Test History** — Results persisted in `localStorage`, shown as colored dots in the model list
- **Export Results** — Download as JSON or CSV
- **i18n** — Switch between Chinese and English; preference is saved
- **Light/Dark Theme** — Toggle between themes; preference is saved

### How to Use

#### Step 1: Add Channels

1. Fill in the **Channels** section:
   - **Name**: A label for this endpoint, e.g. `OpenAI`, `My Proxy`
   - **Base URL**: The API base URL, e.g. `https://api.openai.com/v1`
   - **API Key**: Your API key, e.g. `sk-...`
2. Click **Add channel** to add more endpoints for parallel testing
3. Use **Export** to save your channel config as JSON; **Import** to restore it

#### Step 2: Fetch Models

Click **Fetch models** — the tool calls `/models` on each channel and aggregates the results.

- With multiple channels, filter chips appear so you can narrow the model list by channel
- Click **Text only** to auto-deselect image, audio, embedding, and other non-text models

#### Step 3: Select and Test

1. Check the models you want to test (Select all / Deselect all / Text only)
2. Choose concurrency in the bottom-right (default: 3)
3. Click **Test selected**

Each model runs two tests:
- **Chat**: Sends `"Say exactly: hello"`, validates the response, records TTFT and total latency
- **Tools**: Sends a weather query with a forced `get_weather` tool call, checks for `tool_calls` in the response

#### Step 4: View Results

The results table shows for each model:
- **Chat**: Pass/Fail + TTFT + total latency + token usage + response preview
- **Tools**: Pass/Partial/Fail + function name + arguments + latency

Click **Export JSON** or **Export CSV** to download the full results.

### Notes

- **CORS**: Browser requests to third-party APIs may be blocked by CORS policy. Workarounds:
  - Use an API proxy that allows cross-origin requests
  - Install a browser CORS extension (for development only)
  - Run a local reverse proxy
- **Security**: Channel data (including API keys) is stored in `localStorage`. Exported files contain keys in plain text — handle with care.

### License

MIT
