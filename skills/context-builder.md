# Skill: Context Builder

## Purpose
Build or update `engagements/<customer>/context.md` through a structured
interview. This skill is conversational: it asks me questions section by
section, writes my answers into context.md, and tells me what is still
missing before the context can be considered complete. Run this at the start
of every engagement and after any major change (new sponsor, re-scope, renewal).

## Required inputs
- Customer name (to create or locate `engagements/<customer>/`)
- Anything already available: existing context.md, files in
  `engagements/<customer>/reference/`, notes I paste into chat

## Process
1. **Set up the directory.** If `engagements/<customer>/` does not exist, copy
   the structure from `engagements/_template/` (context.md, `outputs/`, and
   `reference/` with its subfolders).
2. **Harvest before asking.** Read the existing context.md and every file in
   `reference/` (call notes, contracts, org charts, usage exports, emails).
   Pre-fill every context.md field you can, and cite the source file next to
   each pre-filled fact, e.g. `(source: reference/calls/2026-08-20-kickoff.md)`.
   Never ask me for information that already exists in these files — confirm
   it instead ("From the kickoff notes, the sponsor is the COO — still true?").
3. **Interview section by section**, in this order, ONE section at a time —
   ask, wait for my answer, write it into context.md, then move on:
   1. Snapshot (industry, size, product tier, licenses, contract dates)
   2. Business goals — push for the customer's own words, and for the "judged
      by whom, by when" behind each goal
   3. Adoption outcomes & metrics — push for baseline, target, date; accept
      `[NEEDED]` if unknown
   4. Stakeholders — for each: role, influence, disposition; probe for the
      sponsor, the day-to-day contact, at least one champion, and any known
      skeptic
   5. Current state — tools replaced, past rollout attempts and their fate,
      technical constraints, comms channels and training culture
   6. Risks, open issues, and mutual obligations
   Keep questions concrete and max 3 per turn. If I answer vaguely, ask one
   sharpening follow-up ("roughly how many of the 400 licenses are meant for
   the field team?"), then move on — this is an interview, not an interrogation.
4. **Write as you go.** After each section, update context.md immediately and
   show me the diff or the updated section so I can correct it in the moment.
5. **Score completeness.** When the interview ends (or I stop it), append a
   `## Context completeness` section to context.md:
   - A per-section status table (Complete / Partial / Empty)
   - Every `[NEEDED]` item listed in one place
6. **Suggest the evidence to collect.** Recommend concrete artifacts that would
   close the gaps, mapped to where they should be saved in `reference/`:
   - Usage/metrics export → `reference/data/` (needed for health-check, qbr-prep)
   - Contract or order form → `reference/docs/` (dates, license counts, tiers)
   - Org chart → `reference/docs/` (feeds stakeholder-map)
   - Call recordings/notes → `reference/calls/` (feeds everything)
   - Past survey results or old rollout post-mortems → `reference/data/`
   - Customer's own strategy deck or OKRs → `reference/docs/` (feeds
     engagement-plan's goal statements)
   Only suggest items actually missing; state which downstream skill each
   unlocks so I can prioritize what to chase from the customer.
7. **Log the session.** Add a line to the History log:
   `[yyyy-mm-dd] — context updated via context-builder; gaps: <n>`.

## Output format
The updated `engagements/<customer>/context.md` itself — this skill produces
no separate file in `outputs/`. Plus a short closing summary in chat: what was
filled, what remains `[NEEDED]`, and the top 3 artifacts to request from the
customer next.

## Quality criteria
- Nothing invented: every fact came from me, a reference file, or is `[NEEDED]`.
- Pre-filled facts cite their source file.
- No question asked that a reference file already answers.
- The completeness section makes gaps visible at a glance — a future skill run
  (or a colleague) can see instantly what the context can and cannot support.
- Gap suggestions name the downstream skill each artifact unlocks.
