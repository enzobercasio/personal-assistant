# Skill: Troubleshooting Playbook

## Purpose
Diagnose why adoption is stalling and produce an intervention plan. Works like
a decision tree: symptom → diagnostic questions → likely root causes →
intervention options.

## Required inputs
- `engagements/<customer>/context.md`
- The symptom, as specifically as possible (e.g., "logins fine, feature X unused",
  "Team A adopted, Team B refuses", "usage dropped after month 2")
- Latest health-check output if available

## Process
1. Classify the symptom into a stall pattern:
   - Never started: access/awareness/onboarding failure
   - Shallow usage: logins without valuable workflows — value not understood
     or workflow friction
   - Pocketed adoption: some teams yes, others no — local leadership,
     workflow fit, or politics
   - Decay: initial usage declining — habit never formed, champion left,
     competing priority, or unresolved friction
   - Active resistance: vocal pushback — threat perception, past failures,
     or genuine workflow mismatch
2. Generate 5-8 diagnostic questions that discriminate between the root causes
   within that pattern; note what each answer would imply.
3. Using whatever evidence exists, rank the 2-3 most likely root causes with
   supporting and contradicting evidence for each.
4. For each likely cause, propose interventions at two levels: a quick move
   (this week) and a structural fix (this quarter).
5. Define the re-measure: which metric or signal, checked when, tells us the
   intervention worked.
6. Flag when the honest answer is a hard conversation (sponsor gone quiet,
   product genuinely doesn't fit a team's workflow) rather than another
   training session.

## Output format
Markdown doc: Symptom & pattern | Diagnostic questions | Ranked root causes |
Interventions (quick + structural) | Re-measure plan.
Save to `engagements/<customer>/outputs/troubleshooting-<topic>-<yyyy-mm-dd>.md`.

## Quality criteria
- Diagnostics discriminate between causes — no generic "talk to users".
- Evidence against a hypothesis is stated, not hidden.
- At least one intervention is not "more training" — training is the default
  crutch and rarely the root cause.
- Politically uncomfortable causes are named plainly.
