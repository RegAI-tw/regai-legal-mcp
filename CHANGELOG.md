# Changelog

Changes to the RegAI Legal MCP service as seen by clients (tools, parameters,
auth, limits). Dates are when the change went live on `https://mcp.regai.tw/mcp`.

## 2026-10-04 — sign in with OAuth

- Clients can now connect by signing in with a RegAI account (OAuth 2.1 per
  the MCP authorization spec: Protected Resource Metadata, PKCE, Client ID
  Metadata Documents and dynamic client registration). No API key needed.
  Tested with Claude (web, Desktop, Claude Code), ChatGPT and Goose.
- API keys keep working unchanged (`Authorization: Bearer` header or `?key=`).
- `server.json` 1.1.0: the API-key header is now optional.

## 2026-10-03 — documentation repository

- Public documentation, `server.json` and examples published.

## 2026-10-01

- New tool `search_decisions_exact`: list every decision containing literal
  phrase(s), with the exact total; optional court, case-type, decision-year
  and NOT-phrase filters.

## 2026-09-29

- All tools annotated read-only (`readOnlyHint: true`,
  `destructiveHint: false`) with human-readable titles.

## 2026-09-11

- New tool `search_grand_chamber_decisions` (大法庭 rulings).
