# Skill: Daily Assistant

## Purpose
Start-of-day planning and end-of-day review companion. Reads every
engagement's tasks, surfaces priorities and overdue items, checks whether
housekeeping skills (email-scanner, context updates) are current, and walks
you through a short interactive triage. Say "daily review", "start my day",
or "morning check" to trigger.

## Required inputs
- All `engagements/<customer>/tasks.md` files (reads automatically)
- All `engagements/<customer>/context.md` files (reads automatically)
- Today's date (auto-detected)
- _(Optional)_ Your calendar — if Google Workspace MCP is authenticated,
  the skill will also pull today's meetings for scheduling context

## Process

### 1. Gather state
For each customer directory under `engagements/` (skip `_template`):
- Read `tasks.md` — collect all open tasks (My Actions + Waiting on Others).
- Read `context.md` — note the most recent History log entry date.
- Check for an `⚠️ Urgent` section and pull any items.

### 2. Build the daily dashboard
Present a single consolidated view:

#### a. Urgent items (across all accounts)
Pull every item from every `## ⚠️ Urgent` section. List them first, sorted
by date (newest on top). Format:

| # | Customer | Item | Source | Date |
|---|----------|------|--------|------|

If none: "✅ No urgent items across any account."

#### b. Today's priorities
From all open My Actions tasks, surface the top priorities using this logic:
1. **Overdue** — due date is before today → 🔴
2. **Due today** — due date is today → 🟠
3. **Due this week** — due date is within 7 days → 🟡
4. **Stale waiting-on-others** — items with no status change in >7 days → 🔵
   (these need a follow-up nudge from you)

Format:

| Priority | Customer | Task | Due | Status |
|----------|----------|------|-----|--------|

#### c. Upcoming this week
List tasks due in the next 7 days, grouped by customer, so you can
plan ahead.

#### d. Waiting-on-others summary
One-line per item that is blocked on someone else. Flag any item
older than 7 days without a status update — suggest a follow-up action.

### 3. Housekeeping checks
Run these checks silently and report findings:

#### a. Email scanner freshness
For each customer, find the most recent `[yyyy-mm-dd] — Email scan` entry
in the History log of `context.md`. If the last scan is **more than 24 hours
ago**, flag it:

> ⚠️ **Email scan overdue** — last scan for **<customer>** was on
> `<date>`. Run `@skills/email-scanner.md` to catch up.

If all scans are current: "✅ Email scans up to date."

#### b. Context freshness
For each customer, check the most recent History log entry date in
`context.md`. If the last update is **more than 14 days ago**, flag it:

> ⚠️ **Context may be stale** — `<customer>/context.md` last updated
> `<date>` (<N> days ago). Consider revisiting with `@skills/context-builder.md`.

If all contexts are current: "✅ All engagement contexts are current."

#### c. Empty task files
If any customer has a `tasks.md` with zero open tasks in both My Actions
and Waiting on Others, flag it — either the engagement is dormant or
tasks are not being captured:

> ℹ️ **No open tasks** for **<customer>**. Is this engagement dormant,
> or are tasks being tracked elsewhere?

### 4. Interactive triage
After presenting the dashboard, ask three questions:

**Q1 — Completions:**
> "Looking at your open tasks, have you completed any of these since the
> last update? Tell me which ones and I'll mark them done."

Wait for response. For each confirmed completion:
- Move the task to the Completed section in the relevant `tasks.md`
  with today's date.
- Update the "Last updated" header in `tasks.md`.

**Q2 — Missing items:**
> "Is there anything not on this list that you need to track today?
> New tasks, follow-ups from yesterday, items from meetings?"

Wait for response. For each new task:
- Append to the correct customer's `tasks.md` under My Actions or
  Waiting on Others as appropriate.

**Q3 — Focus for today:**
> "Based on what's here, I'd suggest focusing on these today:
> 1. [top priority 1]
> 2. [top priority 2]
> 3. [top priority 3]
>
> Does that match your plan, or do you want to reprioritise?"

Wait for confirmation or adjustment.

### 5. Final summary
After the interactive triage, present a clean summary:

```
📋 Daily Plan — <yyyy-mm-dd>
─────────────────────────────
Focus today:
  1. <task>
  2. <task>
  3. <task>

Follow-ups to send:
  - <nudge for stale waiting-on-others item>

Housekeeping:
  - <email scan / context reminders, or "all clear">
```

## Output format
- Dashboard and triage are presented interactively in chat (not saved).
- Task file updates are written directly to `engagements/<customer>/tasks.md`.
- If I say "save my daily plan", save to
  `engagements/_daily-plan-<yyyy-mm-dd>.md`.

## Quality criteria
- Never fabricate tasks or mark items complete without explicit confirmation.
- Preserve existing task numbering and formatting conventions in each
  customer's `tasks.md`.
- Priority recommendations are grounded in due dates and staleness, not
  guesswork. If a task has no due date, rank it below dated items and note
  `[no due date]`.
- Housekeeping checks use actual dates from files, not assumptions.
- The interactive triage waits for responses — do not auto-answer.
- Dates in ISO format (yyyy-mm-dd).
- Keep the dashboard concise: if >20 tasks surface, group and summarise
  rather than listing every line.
