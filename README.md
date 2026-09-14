# Adoption Architect — Skills & Engagements

Playbooks, templates, and per-customer engagement artifacts for Adoption Architects. This repo is designed to be used with [Cursor](https://cursor.com) as an AI-powered assistant that follows structured skills to produce consistent, high-quality deliverables.

---

## Getting Started (New Architect Setup)

### 1. Clone the repo

```bash
git clone <repo-url> adoption-architect-skills
cd adoption-architect-skills
```

### 2. Open in Cursor

```bash
cursor .
```

Cursor will automatically load the project rules from `.cursor/rules/core.mdc`, which define how the AI assistant behaves — skills-first workflow, context-first outputs, citation conventions, and quality standards.

### 3. Configure MCP Servers

MCP (Model Context Protocol) servers give the AI assistant access to external tools — Gmail, Google Drive, Slack, and Smartsheet. These power the email-scanner, drive-scanner, and other automated skills.

**Go to:** Cursor → Settings (⌘,) → MCP

Add the following servers:

| MCP Server | Purpose | Required for |
|------------|---------|-------------|
| **Google Workspace** | Gmail search & read, calendar | `email-scanner`, `daily-assistant` |
| **Google Drive** | Search, read, list files across Drive | `drive-scanner` |
| **Slack** | Search messages, post updates | Slack-based collaboration |
| **Smartsheet** | Read adoption pipeline sheets | `adoption-architect-smartsheet` |

Each server requires OAuth authentication. After adding a server, click **Authenticate** and follow the browser flow. Verify connection status:

> In any chat, ask: *"Check MCP status"* — the assistant will test each server.

**Note:** MCP config is stored in `.cursor/mcp.json` which is gitignored (it may contain tokens). Each architect configures their own.

### 4. Create your first engagement

```bash
cp -r engagements/_template engagements/<customer-name>
```

Then populate `engagements/<customer-name>/context.md` with:
- Customer name, abbreviations, and account ID
- Key stakeholders (name, role, email, disposition)
- Engagement goals and success metrics
- Products in scope and contract dates
- Constraints and history

Or run the context-builder skill to populate it interactively:

> `@skills/context-builder.md` for `<customer-name>`

### 5. Verify everything works

Run the daily assistant to confirm skills, tasks, and MCP connections are working:

> `@skills/daily-assistant.md` start my day

---

## How to Use Skills

Skills are playbooks. Each one defines a process, output format, and quality criteria for a specific deliverable. To invoke a skill:

> `@skills/<skill-name>.md` `<your instruction>`

### Available Skills

| Skill | What it produces |
|-------|-----------------|
| `context-builder` | Builds or updates `context.md` from reference materials |
| `daily-assistant` | Morning planning — task review, priorities, housekeeping checks |
| `email-scanner` | Scans Gmail, updates tasks/context/call notes, surfaces urgent items |
| `drive-scanner` | Searches Google Drive for account-related files, assesses relevance |
| `engagement-plan` | Phased adoption plan with milestones, RACI, risks, operating rhythm |
| `stakeholder-map` | Stakeholder analysis with influence/interest grid |
| `discovery-questions` | Tailored discovery questions for workshops or meetings |
| `workshop-planning` | End-to-end workshop plan with objectives, logistics, and outcomes |
| `workshop-agenda` | Detailed session agenda with timings and facilitator notes |
| `deck-builder` | Slide deck outline with talk track |
| `demo-builder` | Demo script with pre-flight checks, verified against official docs |
| `technical-architecture` | Architecture create, review, or recommendation |
| `health-check` | Adoption health assessment with metrics and recommendations |
| `qbr-prep` | Quarterly Business Review preparation |
| `enablement-content` | Training material, guides, and enablement content |
| `troubleshooting-playbook` | Troubleshooting runbook for a specific issue pattern |

### Workflow Skills (run regularly)

| Skill | Frequency | What it does |
|-------|-----------|-------------|
| `daily-assistant` | Daily | Reviews all tasks, surfaces priorities, checks for overdue scans and stale contexts |
| `email-scanner` | Twice daily | Scans Gmail for engagement-relevant emails, updates engagement files |
| `drive-scanner` | As needed | Searches Google Drive for files to import into engagements |

---

## Project Structure

```
.cursor/
  rules/core.mdc         # AI behaviour rules (auto-loaded by Cursor)
  mcp.json               # MCP server config (gitignored — per-architect)
skills/                   # Playbooks — one per deliverable type
templates/                # Skeletons used by skills
examples/                 # Example outputs for reference
demo-assets/              # Runnable demos and teaching kits
  README.md               # Demo inventory with products, use cases, customers
  byok-runbooks/          # BYOK knowledge base for Lightspeed demos
  ocp-workload-resiliency-demo/   # PDB + topology spread drain demo
  ocp-resiliency-showcase/        # Animated resiliency explainers
  zdu-demo/               # Zero-downtime upgrade with live probes
engagements/
  _template/              # Copy this to start a new customer
  <customer>/
    context.md            # Single source of truth for the engagement
    tasks.md              # Action items tracker
    outputs/              # Deliverables produced by skills
    reference/
      calls/              # Call notes and meeting summaries
      data/               # Telemetry exports, usage data, spreadsheets
      docs/               # Contracts, org charts, strategy decks
      links/              # Curated reference links (docs, KBAs, blogs)
      shadowbot/          # Shadowbot AI outputs (account plans, analyses)
scripts/
  generate-sidebar.sh     # Rebuilds _sidebar.md for the local docs site
```

### Engagement files — what goes where

| Folder | Content | Naming convention |
|--------|---------|-------------------|
| `context.md` | Customer profile, stakeholders, goals, history log | Single file |
| `tasks.md` | Open/waiting/completed action items | Single file |
| `outputs/` | Deliverables (plans, decks, demos, architectures) | `<type>-<topic>-<yyyy-mm-dd>.md` |
| `reference/calls/` | Call notes, meeting minutes, transcripts | `<yyyy-mm-dd>-<topic>.md` |
| `reference/data/` | Usage metrics, case summaries, spreadsheet exports | `<yyyy-mm-dd>-<topic>.md` |
| `reference/docs/` | Contracts, org charts, customer strategy decks | `<yyyy-mm-dd>-<topic>.md` |
| `reference/links/` | Curated external links by topic | `<topic>.md` (use `templates/links-template.md`) |
| `reference/shadowbot/` | Shadowbot AI outputs (account plans, risk assessments) | `<yyyy-mm-dd>-<type>.md` |

---

## Local Docs Site

Browse all skills, templates, and engagement outputs in a searchable, collapsible site that runs entirely on your machine — no data leaves localhost.

### Quick start

```bash
npx serve .
```

Open the URL printed in the terminal (usually **http://localhost:3000**).

### After adding new files

Regenerate the sidebar first:

```bash
bash scripts/generate-sidebar.sh && npx serve .
```

### Prerequisites

- [Node.js](https://nodejs.org/) (for `npx serve`)
- No other dependencies — Docsify loads from CDN via `index.html`

---

## Automation Setup (Optional)

For architects who want automated email scanning, see `scripts/automation-email-scanner.md` for setting up a Cursor Automation that runs the email scanner on a cron schedule (morning and afternoon, weekdays).

Alternatively, run the skills manually:

```
# Morning check
@skills/daily-assistant.md start my day

# Email scan
@skills/email-scanner.md check latest emails

# Drive scan for a specific account
@skills/drive-scanner.md scan for <customer>
```

---

## Quality Standards

- Professional, direct, customer-empathetic. No filler, no hype words.
- Every plan states measurable outcomes, owners, and dates.
- Executive content leads with the business outcome, not the feature.
- Practitioner content is concrete: steps, exercises, examples.
- Dates in ISO format (`yyyy-mm-dd`). Spell out acronyms on first use.
- Never fabricate usage data, quotes, or commitments.
- Never promise product roadmap items.
- Never reuse one customer's confidential data in another customer's outputs.

---

## Quick Links

- [Demo Assets Summary](demo-assets/README.md)
- [Technical Architecture](skills/technical-architecture.md)
- [Demo Builder](skills/demo-builder.md)
- [Engagement Plan Template](templates/engagement-plan-template.md)
- [Links Template](templates/links-template.md)
- [Tasks Template](templates/tasks-template.md)
