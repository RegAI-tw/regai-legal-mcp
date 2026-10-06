# RegAI 法律 MCP

[English](README.md) · **繁體中文**

讓你的 AI 查台灣法規與法院裁判：法條全文、條號查詢、裁判搜尋，唯讀。

RegAI 法律 MCP 是一個雲端託管的 [Model Context Protocol](https://modelcontextprotocol.io)（MCP）服務，讓 Claude、ChatGPT 等 AI 助理直接查詢台灣法規與法院裁判：以概念或關鍵字搜尋法條、依條號取得條文（可附官方英譯）、檢視法規架構、搜尋最高法院等裁判與大法庭裁定，並可列出含特定詞句的所有裁判。

- **服務位址：** `https://mcp.regai.tw/mcp`（Streamable HTTP）
- **網站與設定說明：** https://regai.tw/mcp
- **安全說明：** https://regai.tw/mcp/security
- **營運：** [RegAI.tw](https://regai.tw)

> 本儲存庫收錄公開文件、MCP 登錄資料（`server.json`）與範例。MCP 伺服器本身為
> 雲端服務，原始碼不在此公開。

## 提供的工具

| 工具 | 功能 |
|---|---|
| `search_law` | 以法律概念、關鍵字或條號搜尋法條 |
| `search_law_titles` | 把完整或部分法規名稱對應到正式名稱 |
| `get_article_by_number` | 依法規名稱與條號取得條文全文（可選官方英譯） |
| `get_law_hierarchy` | 法規的章、節、條架構 |
| `get_law_details` | 整部法規全文（Markdown） |
| `search_decisions` | 以法律概念、事實或關鍵字搜尋裁判，依相關性排序 |
| `search_decisions_exact` | 列出含特定詞句的**所有**裁判，並提供確切筆數 |
| `get_decision_details` | 依裁判 id（`jid`）取得裁判全文 |
| `search_grand_chamber_decisions` | 搜尋統一法律見解的大法庭裁定 |

完整參數與範例：[docs/tools.md](docs/tools.md)（英文）。

所有工具皆為**唯讀**（標示 `readOnlyHint: true`），只查詢公開法律資料。查詢與法規名稱使用**繁體中文**；用英文提問時，AI 助理會自行轉換。

## 資料來源

- **法規：** 法務部「全國法規資料庫」，含各法規的生效狀態與尚未生效的修正。
- **法院裁判：** 司法院裁判書中的終審層級部分：最高法院、最高行政法院、憲法法庭、懲戒法院、智慧財產及商業法院，以及大法庭裁定。不含地方法院與高等法院裁判。

資料定期由官方來源更新。RegAI 與法務部、司法院無隸屬關係。查詢結果僅供研究參考，並非法律意見。

## 開始使用

**RegAI MCP 支援 OAuth**（OAuth 2.1，以 OAuth 2.0 為基礎的 MCP 授權標準），建議以 RegAI 帳號登入的方式連接，不需要 API 金鑰。

**在 Claude 中**：RegAI 法律 MCP 已在 Claude 的連接器目錄上架：開啟 [claude.ai/directory/regai](https://claude.ai/directory/regai)（或在「Customize」→「Connectors」中搜尋「RegAI」），按「Connect」並登入即可，不必貼上網址。

**OAuth 登入連接（建議）**：在其他 AI 助理新增自訂連接器，只要貼上 `https://mcp.regai.tw/mcp`。AI 助理會開啟 RegAI 的登入頁（OAuth），以 RegAI 帳號登入（免費；Email 驗證碼或 LINE）並按「允許」即可。已確認支援：Claude（網頁版、桌面版、Claude Code）、ChatGPT、Goose。

Claude Code 只要一行指令（第一次使用時登入）：

```bash
claude mcp add --transport http regai https://mcp.regai.tw/mcp
```

**或使用 API 金鑰**（適用不支援登入的 App）：在[帳號頁](https://regai.tw/account)複製金鑰，二擇一：
- 標頭：`Authorization: Bearer 你的金鑰`
- 只能輸入網址的 App：`https://mcp.regai.tw/mcp?key=你的金鑰`

各 App 的設定步驟：[examples/clients.md](examples/clients.md) 與 https://regai.tw/mcp 。

設定好後直接提問，例如「民法第 184 條的內容是什麼？」。更多範例：[examples/prompts.md](examples/prompts.md)。

## 方案與用量

| | 免費 | MCP Pro |
|---|---|---|
| 每月呼叫次數 | 500 | 3,000 |
| 短時間上限 | 20 次，每小時回補 5 次 | 60 次，每分鐘回補 1 次 |
| 需要信用卡 | 否 | 是 |

連線與列出工具不計次；每次呼叫工具計 1 次。最新價格：https://regai.tw/pricing

## 安全與隱私

工具皆為唯讀，只查詢公開資料。登入連接依 MCP 授權規範（OAuth 2.1，強制 PKCE）實作：存取權杖有效 1 小時，更新權杖每次使用都會更換；可隨時在帳號頁的「已連接的應用程式」中斷連線。API 金鑰以雜湊值比對（並加密保存，方便你登入後再次查看），可隨時重新產生或停用。我們不記錄查詢內容，只保留無法還原成文字的
指紋以利除錯。詳見 [SECURITY.md](SECURITY.md) 與 https://regai.tw/mcp/security 。
隱私權聲明：https://regai.tw/privacy

## 客服

- 聯絡我們：https://regai.tw/contact · service@regai.tw
- 文件問題：請在本儲存庫開 issue。

## 授權

本儲存庫的文件與範例以 [CC BY 4.0](LICENSE) 授權。RegAI 服務依[RegAI 服務約定條款](https://regai.tw/terms)提供。

Claude 為 Anthropic 的商標，ChatGPT 為 OpenAI 的商標；其他產品名稱屬其各自所有人，僅用於說明相容性。
