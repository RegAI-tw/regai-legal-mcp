# RegAI Legal MCP

**English** · [繁體中文](README.zh-TW.md)

Taiwan law and court decisions for your AI assistant: statutes, articles and rulings, read-only.

RegAI Legal MCP is a hosted [Model Context Protocol](https://modelcontextprotocol.io) server that gives AI assistants such as Claude and ChatGPT grounded access to Taiwan's laws and court decisions. Your assistant can search statutes by concept or keyword, fetch an article by number (with the official English translation where one exists), browse a law's structure, search apex-court decisions and Grand Chamber (大法庭) rulings, and list every decision that contains an exact phrase.

- **Endpoint:** `https://mcp.regai.tw/mcp` (Streamable HTTP)
- **Website and setup guide:** https://regai.tw/mcp
- **Security:** https://regai.tw/mcp/security
- **Operated by:** [RegAI.tw](https://regai.tw)

> This repository holds the public documentation, the registry metadata
> (`server.json`) and examples. The server itself is a hosted service; its
> source code is not published here.

## What it can do

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

Full parameters and examples: [docs/tools.md](docs/tools.md).

All tools are **read-only** (annotated `readOnlyHint: true`) and query only public legal data. Queries and law names are in **Traditional Chinese**; your assistant translates for you if you ask in English.

## Data

- **Laws and regulations:** Taiwan's Ministry of Justice national law database (全國法規資料庫), including each law's effective status and pending amendments.
- **Court decisions:** an apex-tier subset of Judicial Yuan decisions: the Supreme Court, Supreme Administrative Court, Constitutional Court, Disciplinary Court and Intellectual Property and Commercial Court, plus Grand Chamber rulings. District and high court decisions are not included.

The data is updated periodically from the official sources. RegAI is not affiliated with the Ministry of Justice or the Judicial Yuan. Results are for research and are not legal advice.

## Get started

**RegAI MCP supports OAuth** (the MCP authorization standard), so the recommended way to connect is to sign in with your RegAI account. No API key needed.

**Sign in with OAuth (recommended).** Add a custom connector in your AI assistant with just `https://mcp.regai.tw/mcp`. The assistant opens RegAI's sign-in page (OAuth); sign in with your RegAI account (free; email code or LINE) and click **Allow**. Confirmed with Claude (web, Desktop, Claude Code), ChatGPT and Goose.

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
