# Changelog

Changes to the RegAI Legal MCP service as seen by clients (tools, parameters,
auth, limits). Dates are when the change went live on `https://mcp.regai.tw/mcp`.

## 2026-10-05 — decision results show the jid; look up by citation

- `search_decisions` and `search_grand_chamber_decisions`: every result row
  now shows the decision date, 案由 and `jid`, so `get_decision_details` can
  be called directly (one case number can hold several documents, told apart
  by date).
- `search_decisions`: a citation such as `最高法院 112年度台上字第1234號`
  works as the query; the description now says so.
- `get_decision_details`: accepts a `jid` from any of the three search tools.

## 2026-10-05 — clearer law-search descriptions

- `search_law`: the description now says when to use it, and when to use
  `get_article_by_number`, `search_law_titles` or `search_decisions` instead,
  and what each result contains.
- `search_law_titles`: the description now says how matches are ordered
  (exact, then starts-with, then contains) and that at most 50 are listed,
  with the total when more match.
- No parameter or behaviour changes.

## 2026-10-04 — clearer tool descriptions

- `search_decisions`: the description now says when to use it and when to use
  `search_decisions_exact`, `search_grand_chamber_decisions` or `search_law`
  instead; the `case_types` hint no longer refers to internal source code.
- `search_law_titles`: the description referred to a non-existent
  `export_law` tool; it now names `get_law_details`.
- No parameter or behaviour changes.

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
