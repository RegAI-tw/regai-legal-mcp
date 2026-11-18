# Tool reference

The RegAI Legal MCP exposes 11 tools (the 2 agency-regulation tools, 金融法規 and 經濟法規, return results on MCP Pro). All are read-only and idempotent
(`readOnlyHint: true`, `destructiveHint: false`, `idempotentHint: true`).
Text inputs (queries, law names, phrases) are **Traditional Chinese**.

Most assistants pick the right tool on their own; this page is for checking
what is possible and for writing better prompts.

- [Laws](#laws): `search_law`, `search_law_titles`, `get_article_by_number`,
  `get_law_hierarchy`, `get_law_details`
- [Court decisions](#court-decisions): `search_decisions`,
  `search_decisions_exact`, `get_decision_details`,
  `search_grand_chamber_decisions`
- [Financial regulations](#financial-regulations-金融法規) and [economic regulations](#economic-regulations-經濟法規):
  `search_domain_corpus`, `get_domain_document` (MCP Pro)

Case-type codes used by the decision tools: `C` 憲法 (constitutional),
`V` 民事 (civil), `M` 刑事 (criminal), `A` 行政 (administrative),
`P` 懲戒 (disciplinary).

---

## Laws

### `search_law`: Search Taiwan law articles

Search law articles by legal concept, keyword, or a specific article
reference. Hybrid (semantic + keyword) search over laws only, not court
decisions. Use it when you don't yet know which article applies. Returns
matching articles with law name, article number, relevance and text;
historical versions are marked.

When to use another tool instead:
- you already know the law and article number → `get_article_by_number`
- you only need a law's official name → `search_law_titles`
- court rulings → `search_decisions`

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | yes | Concept or keywords, e.g. `民法 侵權行為`, `勞動基準法 資遣費`; an article reference like `民法第184條` also works |
| `limit` | integer | no | Maximum results (default 5) |

Example: `{"query": "勞動基準法 資遣費", "limit": 5}`

### `search_law_titles`: Resolve a law name

Resolve a full or partial law name to the official name(s). Use it first when
unsure of the exact name before `get_article_by_number`,
`get_law_hierarchy` or `get_law_details`. To find articles by topic, use
`search_law` instead.

Returns a numbered list: an exact match first, then names starting with the
query, then other names containing it; at most 50, with the total when more
match.

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

Also included: all 813 Council of Grand Justices Interpretations
(司法院大法官會議解釋, 釋字第1–813號) from the Constitutional Court (憲法法庭).
They are final, apex-level constitutional interpretations, equivalent in rank
to today's Constitutional Court judgments. `search_decisions` and
`search_decisions_exact` return them next to court decisions (shown as
釋字第N號; case type `C` includes them, `V`/`M`/`A`/`P` filters do not), and
`get_decision_details` reads one in full. Attached files (opinions, petitions)
are listed as links, not extracted. Cite as 釋字第N號; source: 憲法法庭.

The Constitutional Court's **judgments** (憲法法庭判決, 憲判字; from 2022, judgments
only, not rulings/裁定) are included the same way, with the same rank: shown as
憲法法庭111年憲判字第N號判決, returned by `search_decisions` and
`search_decisions_exact` (case type `C`), and read in full with `get_decision_details`
by jid (`JCCC,111,憲判,8,20220527`) or as `111年憲判字第8號` (holding, reasoning, the
participating justices, related laws). Opinions, the full text and the case files are
listed as links, not extracted. Source: 憲法法庭.

### `search_decisions`: Search Taiwan court decisions

Search decisions by legal concept, fact pattern or keyword; hybrid search,
reranked by relevance. Returns the most relevant decisions, not every match.

When to use another tool instead:
- every decision containing a literal phrase, or an exact count →
  `search_decisions_exact`
- which interpretation prevails where panels diverged (大法庭,
  統一法律見解) → `search_grand_chamber_decisions`
- the text of statutes → `search_law`

A citation works as the query too, e.g. `最高法院 112年度台上字第1234號`:
the matching decision ranks first. The same number can exist in civil and
criminal matters, so check the court/case-type code, date and 案由.

Each result row shows the citation, decision date, 案由, relevance, section
and `jid`; read a result in full with `get_decision_details` (by its `jid`).

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | yes | e.g. `車禍 過失責任`, `勞資 資遣費 認定` |
| `limit` | integer | no | Maximum results (default 15) |
| `case_types` | string[] | no | Any of `C`, `V`, `M`, `A`, `P`; omit for all. Set only when the user asks to limit the case type, not from words in the query |
| `sources` | string[] | no | Limit the kinds of decisions: `decisions` (court rulings), `judgments` (憲法法庭判決, 憲判字), `interpretations` (大法官解釋, 釋字) or `constitutional` (both); omit to search everything. Set only when the user asks to limit the source |

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
| `sources` | string[] | no | Same values as in `search_decisions`; `constitutional` lists 憲判字 and 釋字 as one 憲法審查 block with its own exact total |

Example: `{"terms": ["借名登記"], "exclude": ["信託"], "court": "最高法院", "year_from": 110}`

### `get_decision_details`: Get a court decision's full text

The full text of one decision, every fragment in order.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `jid` | string | yes | The decision id exactly as returned by the search tools, e.g. `TPSM,110,台上大,3997,20220428,2`. A Council of Grand Justices Interpretation is read the same way, by its jid (`JCCC,110,釋,812,20211210`) or as `釋字第812號`; a Constitutional Court judgment by its jid (`JCCC,111,憲判,8,20220527`) or as `111年憲判字第8號` |
| `lang` | string | no | `en` returns the official English text of a 釋字 interpretation where one exists; ignored for other decisions |

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

## Financial regulations (金融法規)

The Financial Supervisory Commission's regulations (金融監督管理委員會 and its banking, securities
and futures, insurance and examination bureaus), from the commission's law database
(law.fsc.gov.tw): 法律 (statutes), 法規命令 (regulations), 實質法規 (substantive rules such as
the mandatory and prohibited terms of standard-form contracts), 行政規則 (administrative rules),
函釋 (rulings: the regulator's official interpretations) and 公告 (notices).

Every record carries its **category** (法規類別) - a ruling or administrative rule is the
regulator's interpretation, not statute text - and its **status** as of the last update:
已停止適用 (no longer in force), 部分停止適用 (partly repealed), 尚未生效 (not yet in force), or a
repeal or amendment that takes effect on a later date (e.g. 將自民國117年1月1日起停止適用).
Records no longer in force are left out unless `include_repealed`; a record named by its
字號 is always returned, with its status.

**MCP Pro / RegAI Pro.** On the Free plan both tools return an upgrade message instead of
results, and the call is not counted toward the monthly limit.

### `search_domain_corpus`: Search financial regulations

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | yes | A question or keywords, e.g. `境外結構型商品可否以人民幣計價`; or a document number (字號) such as `金管銀法字第11101445721號令` |
| `law_class` | string[] | no | Only these categories: `statute`, `regulation`, `substantive_rule`, `admin_rule`, `interpretation` (函釋), `notice`, `judicial` (司法判解, 經濟法規), `other` |
| `unit` | string | no | Only one issuing unit: `銀行局`, `證券期貨局`, `保險局`, `檢查局` or `本會` |
| `include_repealed` | boolean | no | Include records no longer in force (default false) |
| `limit` | integer | no | Maximum records (default 10, max 20) |
| `domain` | string[] | no | Only these databases: `finance` (金融法規), `economy` (經濟法規); omit for both |

Each result shows the title (with the article or point that matched), category, issuing unit,
字號, date, id (`fsc:GL004294`), status when not plainly in force, and the matching text.

### `get_domain_document`: Get a financial-regulation record

| Parameter | Type | Required | Description |
|---|---|---|---|
| `id_or_number` | string | yes | The id from `search_domain_corpus` (`fsc:GL004294` or `GL004294`) or a 字號; several records can share a 字號 (a 令 and its companion 函), and all are returned |

Returns the record as Markdown: category, issuing unit, 字號, dates, status, content, 編註
(editor's notes), legislative history and attachment links. Source: 金融監督管理委員會主管法規查詢系統.

## Economic regulations (經濟法規)

The Ministry of Economic Affairs' regulations (經濟部 and its agencies: 商業發展署, 水利署, 標準檢驗局,
能源署, 產業發展署, 國際貿易署 and others), searched with the same two tools; pass
`domain: ["economy"]` to search them alone. Sources:

- 經濟部主管法規查詢系統 (law.moea.gov.tw): 法規命令, 實質法規, 行政規則, 函釋 and 公告.
- 商工行政法規檢索系統 (gcis.nat.gov.tw/elaw): the 函釋 and the court rulings (司法判解,
  `law_class: ["judicial"]`) filed under each article of 公司法, 商業登記法, 商業會計法 and related laws.
- 產業發展署 (www.ida.gov.tw): notices.

**Laws already in the laws database are not copied.** A statute or 法規命令 the MOEA administers
that is also in the laws database appears as an article hit marked **綜合法規**, ranked with the
rulings; read it with `get_article_by_number` or `get_law_details`.

**Precedents.** A ruling labelled 判例 (and every 大理院 entry) carries a note on 法院組織法第57條之1:
判例 with no full text on record have ceased to apply, and the others have the same effect as an
ordinary Supreme Court decision.

The `unit` filter takes the MOEA unit's full name, e.g. `經濟部水利署`. Ids look like
`moea:GL001967`, `gcis:cons:9980` (a ruling) and `gcis:case:26` (a court ruling). Same plans as
financial regulations: MCP Pro / RegAI Pro, an upgrade message on Free.

---

## Errors and limits

- A tool that finds nothing returns a short message saying so (in Chinese).
- Internal errors return a generic message; details are never exposed.
- Over-quota or rate-limited calls return a JSON-RPC error that names the
  limit (on the Free plan, with an upgrade link), so your assistant can tell
  you. Plans: see the [README](../README.md#plans-and-limits).
