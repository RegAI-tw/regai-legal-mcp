# RegAI Legal MCP

**English** · [繁體中文](README.zh-TW.md)

Taiwan law, court decisions and agency regulations for your AI assistant: statutes, articles, rulings and interpretive letters, read-only.

RegAI Legal MCP is a hosted [Model Context Protocol](https://modelcontextprotocol.io) server that gives AI assistants such as Claude and ChatGPT grounded access to Taiwan's laws, court decisions and agency regulations. Your assistant can search statutes by concept or keyword, fetch an article by number (with the official English translation where one exists), browse a law's structure, search apex-court decisions and Grand Chamber (大法庭) rulings, look up Constitutional Court (憲法法庭) judgments and Constitutional Interpretations (釋字), list every decision that contains an exact phrase, and, on MCP Pro, search the financial (金融法規) and economic (經濟法規) regulations of Taiwan's regulators and the Intellectual Property Office's copyright, patent and trademark rulings (智慧財產), including their interpretive letters (函釋).

- **Endpoint:** `https://mcp.regai.tw/mcp` (Streamable HTTP)
- **Website and setup guide:** https://regai.tw/mcp
- **Security:** https://regai.tw/mcp/security
- **Operated by:** [RegAI.tw](https://regai.tw)

> This repository holds the public documentation, the registry metadata
> (`server.json`) and examples. The server itself is a hosted service; its
> source code is not published here.

## Tools

| Tool | What it does |
|---|---|
| `search_law` | Search law articles by legal concept, keyword or article reference |
| `search_law_titles` | Resolve a full or partial law name to the official name(s) |
| `get_article_by_number` | Exact text of one article by law name + number (optionally the official English) |
| `get_law_hierarchy` | Chapter / section / article outline of one law |
| `get_law_details` | Full text of an entire law, as Markdown |
| `search_decisions` | Search court decisions by concept, fact pattern or keyword, ranked by relevance |
| `search_decisions_exact` | List **every** decision containing literal phrase(s), with the exact total |
| `get_decision_details` | Full text of one decision by its id (`jid`) |
| `search_grand_chamber_decisions` | Search Grand Chamber (大法庭) rulings that resolve divergent interpretations |
| `search_domain_corpus` | Search Taiwan agency regulations: financial (金融法規), economic (經濟法規), intellectual property (智慧財產) and tax (租稅法規) statutes, regulations, rulings (函釋), court rulings and more, each with its category and status. **MCP Pro** |
| `get_domain_document` | Full text of one agency-regulation record by id or document number (字號). **MCP Pro** |

Full parameters and examples: [docs/tools.md](docs/tools.md).

All tools are **read-only** (annotated `readOnlyHint: true`) and query only public legal data. Queries and law names are in **Traditional Chinese**; your assistant translates for you if you ask in English.

## Data

- **Laws and regulations:** Taiwan's Ministry of Justice national law database (全國法規資料庫), including each law's effective status and pending amendments.
- **Court decisions:** an apex-tier subset of Judicial Yuan decisions: the Supreme Court, Supreme Administrative Court, Constitutional Court, Disciplinary Court and Intellectual Property and Commercial Court, plus Grand Chamber rulings. District and high court decisions are not included.
- **Constitutional interpretations:** all 813 Council of Grand Justices Interpretations (司法院大法官會議解釋, 釋字第1–813號), final constitutional interpretations equivalent in rank to today's Constitutional Court judgments, from the [Constitutional Court](https://cons.judicial.gov.tw/) (憲法法庭). They are returned by the decision tools next to court decisions; read one in full with `get_decision_details` using its `jid` or `釋字第812號`.
- **Financial regulations (MCP Pro):** the Financial Supervisory Commission's regulations (金融監督管理委員會主管法規查詢系統, law.fsc.gov.tw): statutes, regulations, substantive rules (實質法規), administrative rules, rulings (函釋) and notices. Each record carries its category (法規類別) and its status as of the last update: no longer in force, partly repealed, not yet in force, or a repeal or amendment taking effect later. On the Free plan the two tools return an upgrade message, and the call is not counted.
- **Economic regulations (MCP Pro):** the Ministry of Economic Affairs' regulations (經濟部主管法規查詢系統, law.moea.gov.tw), the rulings and court rulings filed under each article in its GCIS commercial law database (商工行政法規檢索系統, gcis.nat.gov.tw/elaw), and the Industrial Development Administration's notices. Laws already in the laws database appear as article hits marked 綜合法規. Search them alone with `domain: ["economy"]`.
- **Intellectual property (MCP Pro):** the Intellectual Property Office's (經濟部智慧財產局) copyright interpretive letters (著作權解釋令函, about 7,000, from data.gov.tw), patent and trademark rulings (專利法令函釋, 商標法令解釋, tipo.gov.tw), and the rules on trade secrets, integrated circuit layouts and optical discs. TIPO marks no ruling as repealed: each carries a reminder to check the current article, and older ones are marked 舊法時期. 智慧局 rules held in 經濟法規 appear tagged 經濟法規. Search them alone with `domain: ["ip"]`, by right with `topic`.
- **Tax regulations (MCP Pro):** the Ministry of Finance's (財政部) tax rulings from its tax rulings database (財政部各稅法令函釋檢索系統, ttc.mof.gov.tw): the rulings in the latest edition of each tax's compilation (法令彙編) for 18 taxes, from income tax, business tax and estate and gift tax to land tax, deed tax and customs (about 7,600), and the inland-tax rulings issued since (新頒令釋); plus, from the MOF's regulations system (財政部主管法規共用系統, law-out.mof.gov.tw), the 法規命令, 行政規則 and rulings the compilations do not hold (about 1,900). Each ruling is filed under the articles it interprets and names its tax and edition; rulings a newer edition no longer cites are marked 不再援引 and left out by default. Tax statutes appear as 綜合法規 article hits. Search them alone with `domain: ["tax"]`, by tax with `topic` (its law name, e.g. `所得稅法`).
- **Constitutional Court judgments:** the Constitutional Court's judgments (憲法法庭判決, 憲判字; from 2022, judgments only), equal in rank to the interpretations and returned by the same tools; read one in full with `get_decision_details` using its `jid` or `111年憲判字第8號`. Opinions and case files are links, not extracted.

The data is updated periodically from the official sources. RegAI is not affiliated with the Ministry of Justice, the Judicial Yuan, the Financial Supervisory Commission, the Ministry of Economic Affairs, the Intellectual Property Office or the Ministry of Finance. Results are for research and are not legal advice.

## Get started

**RegAI MCP supports OAuth** (OAuth 2.1, the MCP authorization standard built on OAuth 2.0), so the recommended way to connect is to sign in with your RegAI account. No API key needed.

**In Claude:** RegAI Legal MCP is listed in Claude's connector directory: open [claude.ai/directory/regai](https://claude.ai/directory/regai) (or search "RegAI" under Customize → Connectors), click **Connect** and sign in. No URL to paste.

**In ChatGPT:** [RegAI Legal (Taiwan)](https://chatgpt.com/plugins/plugin_asdk_app_6ac313132d7c8191b12c67fe863969a7?q=regai) is listed in ChatGPT's plugin directory: open Plugins in ChatGPT, search "RegAI", click **Install** and sign in with your RegAI account (free). No URL to paste.

**Sign in with OAuth (recommended).** In other assistants, add a custom connector in your AI assistant with just `https://mcp.regai.tw/mcp`. The assistant opens RegAI's sign-in page (OAuth); sign in with your RegAI account (free; email code, Google or LINE) and click **Allow**. Confirmed with Claude (web, Desktop, Claude Code), ChatGPT and Goose.

In Claude Code, one command (you sign in on first use):

```bash
claude mcp add --transport http regai https://mcp.regai.tw/mcp
```

**Or use an API key**, for apps without sign-in support: copy your key from your [account page](https://regai.tw/account) and send it either
- as a header: `Authorization: Bearer YOUR_KEY`, or
- in the URL, for apps that only take a URL: `https://mcp.regai.tw/mcp?key=YOUR_KEY`

Step-by-step instructions for each app: [examples/clients.md](examples/clients.md) and https://regai.tw/mcp.

Then just ask, for example:
「民法第 184 條的內容是什麼？」 or 
"What does Taiwan's Civil Code say about tort liability? Cite the article." 
More in [examples/prompts.md](examples/prompts.md).

## Plans and limits

| | Free | MCP Pro |
|---|---|---|
| Calls per month | 500 | 3,000 |
| Burst | 20, refills 5 per hour | 60, refills 1 per minute |
| Card required | No | Yes |

Connecting and listing tools are free; each tool call counts as one call.
Current prices: https://regai.tw/pricing

## Security and privacy

Read-only tools over public data only. Sign-in follows the MCP authorization spec (OAuth 2.1 with PKCE); access tokens last 1 hour and refresh tokens rotate on every use, and you can disconnect any app under "Connected apps" on your account page. API keys are matched by their hash (and stored encrypted so you can view yours again); you can regenerate or revoke your key at any time. We don't log the content of your queries, only a keyed fingerprint for troubleshooting. Details: [SECURITY.md](SECURITY.md) and https://regai.tw/mcp/security. Privacy policy: https://regai.tw/privacy.

## Support

- Contact: https://regai.tw/contact · service@regai.tw
- Problems with these docs: open an issue in this repository.

## License

Documentation and examples in this repository are licensed under [CC BY 4.0](LICENSE). The RegAI service is provided under the [RegAI Terms](https://regai.tw/terms).

Claude is a trademark of Anthropic, ChatGPT of OpenAI; other product names belong to their owners. They are mentioned only to describe compatibility.
