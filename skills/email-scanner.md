# Skill: Email Scanner

## Purpose
Scan Gmail for emails relevant to active customer engagements and update
tasks, context, call notes, and surface urgent items. Designed to run as a
morning and afternoon check — say "scan emails" or "check emails" to trigger.

## Required inputs
- Google Workspace MCP (authenticated)
- All `engagements/<customer>/context.md` files (reads automatically)
- Time window: defaults to last 8 hours; say "scan emails since Monday"
  or "scan last 24 hours" to override

## Process

### 1. Build the search index
For each customer directory under `engagements/` (skip `_template`), read
`context.md` and extract:
- Customer name and abbreviations (e.g. "MAS", "Monetary Authority of Singapore")
- All stakeholder names from the Stakeholders table
- Key product names relevant to the engagement

### 2. Search Gmail
Use `search_emails` from the Google Workspace MCP. Search for each
customer's terms within the time window. Cast a wide net — surface
borderline emails rather than miss action items.

### 3. Process each relevant email
Read the full email with `read_email`. For each, perform ALL applicable:

#### a. Update tasks.md
- Extract action items, commitments, deadlines, and requests.
- Append new tasks to `engagements/<customer>/tasks.md` following the
  existing format: source (email subject + date), owner, due date, status.
- If a task already exists, update its status or add a note — never duplicate.

#### b. Flag context.md updates
- If the email reveals a new stakeholder, a change in goals, a new
  constraint, a contract update, or a shift in disposition:
  - Add a dated entry to the **History log** section of context.md.
  - For new stakeholders, add to the Stakeholders table with `[NEEDED]`
    for unknown fields.
- Do NOT overwrite existing context — append to the history log and update
  specific fields only when the email provides clear, authoritative info.

#### c. Save call/meeting summaries
- If the email is a meeting invite, meeting notes, or call summary, save
  to `engagements/<customer>/reference/calls/<yyyy-mm-dd>-<topic>.md`.
- Derive a short topic slug from the subject line.
- Include: date, attendees, key points, decisions, action items.

#### d. Surface urgent items
- Escalations, production issues, deadline changes, exec requests, or
  renewal-related communications are URGENT.
- Prepend to `engagements/<customer>/tasks.md` under a `## ⚠️ Urgent`
  section at the top (create the section if it does not exist).
- Include: email subject, sender, date, one-line summary of why it is urgent.

### 4. Handle unmatched emails
If an email mentions Red Hat products or seems work-related but cannot be
matched to a specific customer, log it in the scan summary as "Unmatched"
so I can triage it manually.

### 5. Report
After processing, present a summary:

| Customer | Emails | Tasks added/updated | Context flags | Urgent items | Calls saved |
|----------|--------|---------------------|---------------|--------------|-------------|

List each urgent item with a one-line summary. End with "No urgent items"
if none found.

## Output format
- Updates are written directly to the relevant engagement files.
- Summary is presented in chat (not saved to a file).
- If I ask for a log, save to `engagements/_scan-log.md`.

## Quality criteria
- Never fabricate content. Quote or paraphrase the email; do not invent.
- If unsure which customer an email belongs to, check all context files for
  matching stakeholder names. If still ambiguous, list under Unmatched.
- Respect existing file structure and formatting conventions.
- Dates in ISO format (yyyy-mm-dd).
- Existing tasks are updated, not duplicated. Match by description similarity.
- The urgent section is always at the top of tasks.md, never buried.
