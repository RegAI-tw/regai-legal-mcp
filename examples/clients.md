# Connecting your assistant

**English** · 繁體中文 below

You need a RegAI API key: sign up free at https://regai.tw and copy the key
from https://regai.tw/account. Below, replace `YOUR_KEY` with it.

- **Apps that let you set headers** (preferred): URL `https://mcp.regai.tw/mcp`
  plus header `Authorization: Bearer YOUR_KEY`.
- **Apps that only take a URL:** `https://mcp.regai.tw/mcp?key=YOUR_KEY`.
  URLs can linger in app settings and logs; if a key is exposed, regenerate it
  on your account page.

Menus move between app versions; if yours looks different, look for
"Connectors" or "MCP" in the app's settings. The latest walkthrough with
screenshots is at https://regai.tw/mcp.

## Claude Desktop

1. Open **Settings → Connectors**.
2. Click **Add custom connector**.
3. Paste `https://mcp.regai.tw/mcp?key=YOUR_KEY` and save.

## ChatGPT Desktop

1. Requires a paid plan (Plus or above).
2. Open **Settings → Connectors**. If you don't see it, turn on
   **Developer mode** under Advanced settings first.
3. Click **Add custom connector** and paste
   `https://mcp.regai.tw/mcp?key=YOUR_KEY`.
4. Turn it on in your chat when you want to use it.

## Goose

1. **Settings → Extensions → Add custom extension → Remote Extension.**
2. Name it (e.g. `RegAI`), paste `https://mcp.regai.tw/mcp?key=YOUR_KEY`, save.

## AnythingLLM

1. **Settings → Agent Skills**, click **+** next to MCP Servers.
2. Paste `https://mcp.regai.tw/mcp?key=YOUR_KEY` and save.

## LM Studio

In the **Program** tab of the right sidebar, choose **Edit mcp.json** and add:

```json
{
  "mcpServers": {
    "regai": {
      "url": "https://mcp.regai.tw/mcp",
      "headers": { "Authorization": "Bearer YOUR_KEY" }
    }
  }
}
```

## Jan

1. **Settings → MCP Servers**, click **+**.
2. Name it (e.g. `RegAI`), paste `https://mcp.regai.tw/mcp?key=YOUR_KEY`, save.

## Other MCP clients

Any client that supports remote MCP over Streamable HTTP works. A typical
JSON configuration:

```json
{
  "mcpServers": {
    "regai": {
      "type": "http",
      "url": "https://mcp.regai.tw/mcp",
      "headers": { "Authorization": "Bearer YOUR_KEY" }
    }
  }
}
```

---

# 連接你的 AI 助理

先到 https://regai.tw 免費註冊，在 https://regai.tw/account 複製 API 金鑰，
把下面的 `你的金鑰` 換成它。

- **可以設定標頭的 App**（建議）：位址 `https://mcp.regai.tw/mcp`，
  標頭 `Authorization: Bearer 你的金鑰`。
- **只能輸入網址的 App**：`https://mcp.regai.tw/mcp?key=你的金鑰`。
  網址可能留在 App 設定或紀錄中，若金鑰外流，請到帳號頁重新產生。

各 App 的設定步驟（含截圖）請見 https://regai.tw/mcp 。設定畫面可能因 App
更新而略有不同，可以在該 App 的「Connectors」或「MCP」設定中找找看。

- **Claude 桌面版**：「設定」→「Connectors」→「Add custom connector」，
  貼上 `https://mcp.regai.tw/mcp?key=你的金鑰`，儲存。
- **ChatGPT 桌面版**（需 Plus 以上方案）：「設定」→「Connectors」（沒看到請先在
  進階設定開啟「Developer mode」）→「Add custom connector」，貼上同一位址，
  並在對話中啟用。
- **LM Studio**：在 mcp.json 的 `mcpServers` 新增一筆，`url` 設為
  `https://mcp.regai.tw/mcp`，`headers` 加上
  `"Authorization": "Bearer 你的金鑰"`。
- **Goose、AnythingLLM、Jan**：在 MCP／Extensions 設定新增遠端伺服器，
  貼上 `https://mcp.regai.tw/mcp?key=你的金鑰`。
