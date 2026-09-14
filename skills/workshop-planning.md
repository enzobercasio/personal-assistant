# Skill: Workshop Planning

## Purpose
Produce a complete workshop plan (not the agenda) that ensures the right people
attend, prepared, with clear success criteria and a follow-up plan. Run this
BEFORE workshop-agenda; the agenda consumes this plan.

## Required inputs
- `engagements/<customer>/context.md`
- Workshop topic and the adoption goal it serves (from the engagement plan
  if one exists)
- Known constraints: dates, budget, remote/in-person, attendee cap
- `stakeholder-map` output if available

## Process
1. Restate the adoption goal and derive 2-3 measurable success criteria —
   behavior change after the workshop, not attendance.
2. Identify target audience segments; mark mandatory vs. optional attendees,
   using the stakeholder map where available.
3. Recommend format (duration, modality, single session vs. series, hands-on
   lab vs. discussion) with a one-paragraph rationale tied to objectives and
   audience size.
4. List pre-work per audience segment, including access/provisioning checks,
   each with an owner and deadline.
5. Draft the comms plan: invitation copy (ready to send), reminder schedule,
   and a sponsor announcement blurb.
6. Build a risk register (minimum 4 risks — always include low attendance and
   technical failure) with mitigations.
7. Define the follow-up plan: how outcomes are captured, the 1-week and 30-day
   checkpoints, and how results feed the health-check skill.

## Output format
Single markdown doc, max 3 pages: Objectives | Audience | Format | Pre-work |
Comms | Risks | Follow-up.
Save to `engagements/<customer>/outputs/workshop-plan-<topic>-<yyyy-mm-dd>.md`.

## Quality criteria
- Success criteria describe behavior change, not attendance.
- Every pre-work item has an owner and a deadline.
- Comms copy is ready to send, not placeholder text.
- Follow-up plan names who does what, when.
