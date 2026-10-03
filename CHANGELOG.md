# Changelog

Changes to the RegAI Legal MCP service as seen by clients (tools, parameters,
auth, limits). Dates are when the change went live on `https://mcp.regai.tw/mcp`.

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
