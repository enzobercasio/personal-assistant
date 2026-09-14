# Skill: QBR Prep

## Purpose
Assemble the quarterly business review narrative: what was promised, what
happened, what it means, what's next — plus the deck outline via deck-builder.

## Required inputs
- `engagements/<customer>/context.md`
- The engagement plan (for what was promised)
- Latest health-check output (for what happened)
- Notes on wins, misses, and changes during the quarter

## Process
1. Rebuild the promise baseline: the outcomes and milestones committed for
   this quarter, verbatim from the engagement plan.
2. Score each: achieved / partial / missed / deferred, with evidence. Never
   silently drop a missed commitment — execs remember what was promised.
3. Extract 2-3 win stories in outcome language (who, what changed, business
   effect), and state each miss with its cause and correction.
4. Connect adoption metrics to the customer's business goals — the "so what"
   layer: what did this usage actually buy them?
5. Draft next quarter's commitments: outcomes, milestones, and what we need
   from the customer (the mutual accountability slide).
6. Identify the one strategic conversation to have live (expansion, risk,
   sponsorship change) and prep talking points for it.
7. Hand the narrative to `deck-builder` (purpose: QBR, answer-first arc) for
   the slide outline.

## Output format
Markdown doc: One-page summary | Promise scorecard | Wins & misses |
Business impact | Next-quarter commitments | Strategic topic prep.
Save to `engagements/<customer>/outputs/qbr-narrative-<yyyy-qN>.md`,
plus the deck outline from deck-builder.

## Quality criteria
- Every commitment from last quarter appears in the scorecard — none vanish.
- Wins are stated as customer outcomes, not our activity ("we ran 3 workshops"
  is activity; "support ticket volume down 30%" is an outcome).
- Misses come with causes and corrections, not apologies.
- Next quarter includes asks OF the customer, not only promises to them.
