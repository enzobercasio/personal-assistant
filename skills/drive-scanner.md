# Skill: Drive Scanner

## Purpose
Scan Google Drive for files relevant to active customer engagements. List
each file with metadata and assess whether it should be added to the
engagement's `reference/` folder. Designed to run periodically — say
"scan drive", "check drive", or "scan drive for Acme Corp" to trigger.

## Required inputs
- Google Drive MCP (authenticated)
- All `engagements/<customer>/context.md` files (reads automatically)
- Scope: defaults to all engagements; say "scan drive for Acme Corp" to limit
  to one customer. Say "scan drive last 7 days" to filter by recency.

## Process

### 1. Build the search index
For each customer directory under `engagements/` (skip `_template`), read
`context.md` and extract:
- Customer name and common abbreviations (e.g. "Acme Corp", "ACME",
  "AcmeCo", "AC")
- Key stakeholder names
- Product names and project codenames (e.g. "Lightspeed", "Heidi",
  "NGINE", "TMRW", "Digital Factory")
- Active initiative names from engagement tracks

### 2. Search Google Drive
Use `gdrive_search` from the Google Drive MCP. For each customer, search
using the customer name, abbreviations, and key project/initiative names.
Also search for common Red Hat deliverable terms combined with the customer
name (e.g. "Acme architecture", "Globex workshop", "Initech upgrade").

Search in batches — max 100 results per query. If results are truncated,
paginate.

### 3. Inventory and deduplicate
For each file found, record:
- **File name**
- **Type** (Google Doc, Sheets, Slides, PDF, etc.)
- **Last modified date**
- **Owner / last editor**
- **Customer match** (which engagement it maps to)
- **Drive link** (webViewLink)

Deduplicate across searches (same file may match multiple customer terms).

### 4. Check against existing references
For each customer, read the file listing in:
- `engagements/<customer>/reference/docs/`
- `engagements/<customer>/reference/data/`
- `engagements/<customer>/reference/calls/`
- `engagements/<customer>/outputs/`

Flag files that are **already tracked** (name or content overlap with an
existing reference file) vs. **new finds**.

### 5. Assess relevance
For each new file, classify into one of:

| Category | Criteria | Save to |
|----------|----------|---------|
| **Architecture / design docs** | Architecture diagrams, HLD/LLD, topology docs, cluster configs | `reference/docs/` |
| **Decks / presentations** | Workshop slides, QBR decks, exec presentations, demo recordings | `reference/docs/` |
| **Data exports / reports** | Usage data, telemetry, support case exports, compliance reports | `reference/data/` |
| **Call / meeting notes** | Meeting minutes, call summaries, decision logs | `reference/calls/` |
| **Contracts / commercial** | SOWs, quotes, order forms, renewal trackers | `reference/docs/` |
| **Org charts / stakeholder docs** | Team structures, RACI matrices, contact lists | `reference/docs/` |
| **Irrelevant / stale** | Generic templates, personal notes, unrelated projects | Skip |

Mark files as:
- ✅ **Recommend import** — clearly relevant and not yet tracked
- ⚠️ **Review needed** — possibly relevant but ambiguous (e.g. shared
  across customers, outdated, or partially relevant)
- ⏭️ **Skip** — already tracked or not relevant

### 6. Report
Present a summary table per customer:

```
## [CUSTOMER]
| # | File name | Type | Modified | Owner | Category | Action | Link |
|---|-----------|------|----------|-------|----------|--------|------|
```

Followed by:
- Total files found per customer
- Files recommended for import
- Files to review
- Files skipped (already tracked or irrelevant)

### 7. Import (on request only)
Do NOT auto-import files. After presenting the report, wait for
instructions like:
- "Import all recommended for Acme Corp"
- "Import file #3 and #7 for Globex"
- "Skip all, just log the report"

When importing:
- Use `gdrive_read_file` to read the file content
- Save to the appropriate `reference/` subfolder as markdown:
  `<yyyy-mm-dd>-<slug>.md`
- For Google Sheets, save as CSV or extract the key data
- For Slides/PDFs, save a text summary with a link to the original
- Add a header to each imported file citing the Drive source:
  `> Source: Google Drive — [filename](webViewLink) — imported yyyy-mm-dd`
- Update context.md history log noting the import

## Output format
- Report is presented in chat as a summary table per customer.
- Imports are saved to the appropriate `reference/` subfolder.
- If asked for a log, save to `engagements/_drive-scan-log.md`.

## Quality criteria
- Never auto-import without explicit approval.
- Every file is matched to exactly one customer, or flagged as ambiguous
  (matching multiple) for manual triage.
- Already-tracked files are identified and not recommended for re-import.
- File type classification is based on actual content/name, not guesswork.
- Import preserves source attribution (Drive link + import date).
- Dates in ISO format (yyyy-mm-dd).
