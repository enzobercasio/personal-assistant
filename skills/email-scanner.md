# Skill: Email Scanner

## Purpose
Scan Gmail for emails relevant to active customer engagements **and** Red Hat
product news. Update tasks, context, call notes, surface urgent items, and
log product updates to `products/<product>/`. Designed to run as a morning
and afternoon check — say "scan emails" or "check emails" to trigger.

## Required inputs
- Google Workspace MCP (authenticated)
- All `engagements/<customer>/context.md` files (reads automatically)
- Time window: defaults to last 8 hours; say "scan emails since Monday"
  or "scan last 24 hours" to override

## Process

### 1. Build the search index
For each customer directory under `engagements/` (skip `_template`), read
`context.md` and extract:
- Customer name and abbreviations (e.g. "ACME", "Acme Corp")
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

### 4. Scan for Red Hat product news and updates
Run a separate `search_emails` query for product-related content within the
same time window. Use queries such as:

```
newer_than:1d (from:product-announce OR from:noreply@redhat.com
  OR subject:"release notes" OR subject:"what's new"
  OR subject:"GA" OR subject:"tech preview"
  OR subject:"win wire" OR subject:"adoption impact"
  OR from:product-updates OR from:rh-newsletter
  OR "now available" OR "new feature" OR "deprecation"
  OR "end of life" OR "OpenShift" OR "Ansible" OR "RHEL")
```

Widen or narrow the query based on signal quality in previous scans.

For each product-related email:

#### a. Identify the product(s)
Map the email to one or more product abbreviations:

| Abbreviation | Product |
|--------------|---------|
| `rhocp` | Red Hat OpenShift Container Platform |
| `rhaap` | Red Hat Ansible Automation Platform |
| `rhacm` | Red Hat Advanced Cluster Management |
| `rhel` | Red Hat Enterprise Linux |
| `rhelai` | RHEL AI |
| `rhbk` | Red Hat Build of Keycloak |
| `rhods` | Red Hat OpenShift Data Science |
| `acs` | Red Hat Advanced Cluster Security |
| `odf` | Red Hat OpenShift Data Foundation |
| `ansible-lightspeed` | Ansible Lightspeed |
| `openshift-lightspeed` | OpenShift Lightspeed |
| `openshift-ai` | OpenShift AI (RHOAI) |
| `rhoso` | Red Hat OpenStack Services on OpenShift |

If the product is not in the table, create a new folder under `products/`
using a lowercase abbreviation and note it in the scan summary.

#### b. Save the update
Save to `products/<product>/<yyyy-mm-dd>-<topic-slug>.md` with this format:

```markdown
# <Headline>

| Field | Value |
|-------|-------|
| **Date** | yyyy-mm-dd |
| **Source** | email subject, sender |
| **Type** | Release / Feature / Deprecation / Advisory / Win Wire / Blog |
| **Version** | e.g. 4.18, 2.7 (if applicable) |

## Summary

<2–4 sentence summary of what changed and why it matters>

## Engagement relevance

<Which active engagements could benefit from this? Check context.md files
for product matches. List customer abbreviations or "None currently".>

## Links

- <any URLs from the email body>
```

- Do NOT duplicate: if a file for the same topic+date already exists, skip
  or append new information.
- Consolidate multiple emails about the same announcement into one file.

### 5. Handle unmatched emails
If an email mentions Red Hat products or seems work-related but cannot be
matched to a specific customer, log it in the scan summary as "Unmatched"
so I can triage it manually.

### 6. Report
After processing, present a summary:

| Customer | Emails | Tasks added/updated | Context flags | Urgent items | Calls saved |
|----------|--------|---------------------|---------------|--------------|-------------|

List each urgent item with a one-line summary. End with "No urgent items"
if none found.

Then present a product intel summary:

| Product | Update | Type | Relevant to |
|---------|--------|------|-------------|

List each product update with a one-line description. End with
"No product updates" if none found.

## Output format
- Engagement updates are written directly to the relevant engagement files.
- Product updates are saved to `products/<product>/<yyyy-mm-dd>-<slug>.md`.
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
- Product updates: one file per distinct announcement. Do not duplicate
  across scans. Cross-reference engagement relevance by checking which
  customers use the product (from context.md).
- If a product update is critical (e.g. CVE, EOL, breaking change), also
  flag it under the affected customer's `## ⚠️ Urgent` section in tasks.md.
