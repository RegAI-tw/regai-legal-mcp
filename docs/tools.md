# Tool reference

The RegAI Legal MCP exposes 9 tools. All are read-only and idempotent
(`readOnlyHint: true`, `destructiveHint: false`, `idempotentHint: true`).
Text inputs (queries, law names, phrases) are **Traditional Chinese**.

Most assistants pick the right tool on their own; this page is for checking
what is possible and for writing better prompts.

- [Laws](#laws): `search_law`, `search_law_titles`, `get_article_by_number`,
  `get_law_hierarchy`, `get_law_details`
- [Court decisions](#court-decisions): `search_decisions`,
  `search_decisions_exact`, `get_decision_details`,
  `search_grand_chamber_decisions`

Case-type codes used by the decision tools: `C` 憲法 (constitutional),
`V` 民事 (civil), `M` 刑事 (criminal), `A` 行政 (administrative),
`P` 懲戒 (disciplinary).

---

## Laws

### `search_law`: Search Taiwan law articles

Search law articles by legal concept, keyword, or a specific article
reference. Hybrid (semantic + keyword) search over laws only, not court
decisions.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | yes | Concept or keywords, e.g. `民法 侵權行為`, `勞動基準法 資遣費`; an article reference like `民法第184條` also works |
| `limit` | integer | no | Maximum results (default 5) |

Example: `{"query": "勞動基準法 資遣費", "limit": 5}`

### `search_law_titles`: Resolve a law name

Resolve a full or partial law name to the official name(s). Use it first when
unsure of the exact name before `get_article_by_number`,
`get_law_hierarchy` or `get_law_details`.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | yes | Full or partial law name, e.g. `個人資料`, `營業秘密` |

### `get_article_by_number`: Get a law article by number

The exact text of one article. Returns the current version and, for a pending
amendment, its historical counterpart too.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `law_name` | string | yes | Official law name, e.g. `民法`, `中華民國刑法`, `勞動基準法` |
| `article_number` | string | yes | As in the law, without 條: `184`, `12-1`, `1211-1` |
| `lang` | string | no | `en` for the official government English translation when one exists (about 21% of laws); omit or `zh-TW` for Chinese |

Example: `{"law_name": "民法", "article_number": "184"}`

### `get_law_hierarchy`: Get a law's outline

The chapter / section / article outline of one law, to orient in a large
statute before fetching articles.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `law_name` | string | yes | Official law name, e.g. `公司法` |

### `get_law_details`: Get a law's full text

The full text of an entire law (all articles, with chapter and section
headings) as Markdown. For one article prefer `get_article_by_number`.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `law_name` | string | yes | Official law name, e.g. `公司法` |

---

## Court decisions

Coverage: an apex-tier subset of Judicial Yuan decisions (Supreme Court,
Supreme Administrative Court, Constitutional Court, Disciplinary Court,
Intellectual Property and Commercial Court, Grand Chamber rulings). District
and high courts are not included.

### `search_decisions`: Search Taiwan court decisions

Search decisions by legal concept, fact pattern or keyword; hybrid search,
reranked by relevance. Returns the most relevant decisions, not every match.

When to use another tool instead:
- every decision containing a literal phrase, or an exact count →
  `search_decisions_exact`
- which interpretation prevails where panels diverged (大法庭,
  統一法律見解) → `search_grand_chamber_decisions`
- the text of statutes → `search_law`

Read a result in full with `get_decision_details` (by its `jid`).

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | yes | e.g. `車禍 過失責任`, `勞資 資遣費 認定` |
| `limit` | integer | no | Maximum results (default 15) |
| `case_types` | string[] | no | Any of `C`, `V`, `M`, `A`, `P`; omit for all. Set only when the user asks to limit the case type, not from words in the query |

Example: `{"query": "借名登記 返還請求", "case_types": ["V"]}`

### `search_decisions_exact`: List decisions containing exact phrases

Lists **every** decision whose text contains all the given literal phrases,
newest first, with the exact total on the first page. Matching is literal
(whitespace ignored), never by meaning: use `search_decisions` for concepts.
Each row gives the citation, date, 案由, match count, `jid` and the first
matching passage.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `terms` | string[] | yes | Phrases that must **all** appear, 2–100 characters each, e.g. `["契約承擔"]` |
| `exclude` | string[] | no | Phrases that must **not** appear; `terms` + `exclude` ≤ 6 |
| `court` | string | no | One of `最高法院`, `最高行政法院`, `憲法法庭`, `懲戒法院`, `懲戒法院懲戒法庭`, `懲戒法院職務法庭`, `智慧財產及商業法院`, `大法庭` |
| `case_types` | string[] | no | Any of `C`, `V`, `M`, `A`, `P` |
| `year_from` | integer | no | Earliest decision-date year, inclusive: ROC year (`109`) or Gregorian (`2020`); values up to 200 are read as ROC |
| `year_to` | integer | no | Latest decision-date year, inclusive |
| `page` | integer | no | 1-based page (default 1) |
| `page_size` | integer | no | Rows per page (default 20, max 50) |

Example: `{"terms": ["借名登記"], "exclude": ["信託"], "court": "最高法院", "year_from": 110}`

### `get_decision_details`: Get a court decision's full text

The full text of one decision, every fragment in order.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `jid` | string | yes | The decision id exactly as returned by the search tools, e.g. `TPSM,110,台上大,3997,20220428,2` |

### `search_grand_chamber_decisions`: Search Grand Chamber rulings

Search Grand Chamber (大法庭) rulings: how the Supreme Court (civil and
criminal) and the Supreme Administrative Court resolve divergent legal
interpretations (統一法律見解) and set binding precedent on a point of law.
Covers the full thread for each question: the referral order, the Grand
Chamber's answer, and the final judgment applying it (about 225 decisions).

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | yes | The legal question, e.g. `未遂犯與既遂犯的區分標準`, `借名登記契約的效力` |
| `limit` | integer | no | Maximum results (default 15) |
| `case_types` | string[] | no | Any of `V`, `M`, `A` |

---

## Errors and limits

- A tool that finds nothing returns a short message saying so (in Chinese).
- Internal errors return a generic message; details are never exposed.
- Over-quota or rate-limited calls return a JSON-RPC error that names the
  limit (on the Free plan, with an upgrade link), so your assistant can tell
  you. Plans: see the [README](../README.md#plans-and-limits).
