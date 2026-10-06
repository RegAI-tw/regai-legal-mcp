# Connecting your assistant

**English** · 繁體中文 below

**Sign in (recommended):** apps that support OAuth only need the address
`https://mcp.regai.tw/mcp`. They open RegAI's sign-in page; sign in with your
RegAI account (free) and click **Allow**. Claude, Claude Code, ChatGPT and
Goose work this way.

**API key (for apps without sign-in):** copy your key from
https://regai.tw/account and replace `YOUR_KEY` below.

- **Apps that let you set headers** (preferred): URL `https://mcp.regai.tw/mcp`
  plus header `Authorization: Bearer YOUR_KEY`.
- **Apps that only take a URL:** `https://mcp.regai.tw/mcp?key=YOUR_KEY`.
  URLs can linger in app settings and logs; if a key is exposed, regenerate it
  on your account page.

Menus move between app versions; if yours looks different, look for
"Connectors" or "MCP" in the app's settings. The latest walkthrough with
screenshots is at https://regai.tw/mcp.

## Claude (web and Desktop) — sign in

1. Open [claude.ai/directory/regai](https://claude.ai/directory/regai), or search
   **RegAI** under **Customize → Connectors**.
2. Click **Connect**, sign in on the RegAI page that opens, and click **Allow**.

Or add it by URL: **Customize → Connectors → Add custom connector**, paste
`https://mcp.regai.tw/mcp`, then **Connect**.

## Claude Code — sign in

```bash
claude mcp add --transport http regai https://mcp.regai.tw/mcp
```

Then in Claude Code run `/mcp`, choose `regai` → **Authenticate**, sign in on
the RegAI page and click **Allow**. (The consent screen warns that it returns
to your own computer; that's expected for Claude Code.)

## ChatGPT Desktop — sign in

1. Requires a paid plan (Plus or above).
2. Open **Settings → Connectors**. If you don't see it, turn on
   **Developer mode** under Advanced settings first.
3. Click **Add custom connector**, paste `https://mcp.regai.tw/mcp`, then
   sign in to RegAI and click **Allow** when asked.
4. Turn it on in your chat when you want to use it.

## Goose — sign in

1. **Settings → Extensions → Add custom extension → Remote Extension.**
2. Name it (e.g. `RegAI`), paste `https://mcp.regai.tw/mcp`, save; sign in on
   the RegAI page that opens and click **Allow**.

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

Any client that supports remote MCP over Streamable HTTP works. Clients with
OAuth support only need the URL. Otherwise, a typical JSON configuration with
an API key:

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

**登入連接（建議）**：支援 OAuth 的 App 只需要位址 `https://mcp.regai.tw/mcp`，
會自動開啟 RegAI 的登入頁；以 RegAI 帳號登入（免費）並按「允許」即可。Claude、
Claude Code、ChatGPT、Goose 都是這樣連接。

**API 金鑰（適用不支援登入的 App）**：到 https://regai.tw/account 複製金鑰，
把下面的 `你的金鑰` 換成它。

- **可以設定標頭的 App**（建議）：位址 `https://mcp.regai.tw/mcp`，
  標頭 `Authorization: Bearer 你的金鑰`。
- **只能輸入網址的 App**：`https://mcp.regai.tw/mcp?key=你的金鑰`。
  網址可能留在 App 設定或紀錄中，若金鑰外流，請到帳號頁重新產生。

各 App 的設定步驟（含截圖）請見 https://regai.tw/mcp 。設定畫面可能因 App
更新而略有不同，可以在該 App 的「Connectors」或「MCP」設定中找找看。

- **Claude（網頁版、桌面版）**：開啟 [claude.ai/directory/regai](https://claude.ai/directory/regai)
  （或在「Customize」→「Connectors」中搜尋「RegAI」），點選「Connect」後登入 RegAI 並按「允許」。
  也可以用「Add custom connector」貼上 `https://mcp.regai.tw/mcp`。
- **Claude Code**：執行 `claude mcp add --transport http regai https://mcp.regai.tw/mcp`，
  再於 Claude Code 中輸入 `/mcp`，選擇 `regai` →「Authenticate」，登入並按「允許」。
- **ChatGPT 桌面版**（需 Plus 以上方案）：「設定」→「Connectors」（沒看到請先在
  進階設定開啟「Developer mode」）→「Add custom connector」，貼上
  `https://mcp.regai.tw/mcp`，登入 RegAI 並按「允許」，再在對話中啟用。
- **LM Studio**：在 mcp.json 的 `mcpServers` 新增一筆，`url` 設為
  `https://mcp.regai.tw/mcp`，`headers` 加上
  `"Authorization": "Bearer 你的金鑰"`。
- **Goose**：在 Extensions 新增 Remote Extension，貼上 `https://mcp.regai.tw/mcp`，
  登入 RegAI 並按「允許」。
- **AnythingLLM、Jan**：在 MCP 設定新增遠端伺服器，貼上
  `https://mcp.regai.tw/mcp?key=你的金鑰`。
