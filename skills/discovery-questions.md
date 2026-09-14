# Skill: Discovery Questions

## Purpose
Generate a tailored discovery questionnaire for kickoff and early-stage calls,
so every conversation surfaces the goals, constraints, and politics that shape
the engagement plan.

## Required inputs
- `engagements/<customer>/context.md` (whatever is known so far — may be sparse)
- Meeting type: exec kickoff, admin/technical discovery, end-user focus group
- Time available for the call

## Process
1. Identify what we already know from context.md — never ask questions the
   customer has already answered; instead draft confirmation statements
   ("We understand X — is that still accurate?").
2. Generate questions in five tracks, weighted by meeting type:
   - Business outcomes: what does success look like in 12 months, and who judges it?
   - Current state: existing tools, workflows being replaced, past rollout attempts
     and why they succeeded or failed.
   - People & politics: champions, skeptics, teams most affected, decision process.
   - Technical & operational: environments, integrations, security constraints,
     admin capacity.
   - Adoption mechanics: training culture, comms channels, how change usually
     lands in this org.
3. Order questions from open/strategic to specific/tactical.
4. Cut to fit the time budget: ~4 minutes per substantive question. Mark
   overflow questions as "if time / async follow-up".
5. Add a closing section: agreed next steps and information the customer owes us.

## Output format
Markdown doc: Confirmations | Questions by track (numbered) | If-time questions |
Closing asks. Include a one-line "why this matters" under each strategic question.
Save to `engagements/<customer>/outputs/discovery-<meeting-type>-<yyyy-mm-dd>.md`.

## Quality criteria
- Fits the stated time budget.
- No question answerable from context.md.
- At least two questions probe past failed rollouts — the richest predictor of
  adoption risk.
- Every question is open ("how", "what", "walk me through"), not leading.
