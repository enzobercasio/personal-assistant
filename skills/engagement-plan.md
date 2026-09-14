# Skill: Engagement Plan

## Purpose
Produce a phased adoption plan that takes a customer from onboarding to
sustained value, with milestones, success metrics, owners, and a RACI.
This is the master document other skills reference.

## Required inputs
- `engagements/<customer>/context.md` (industry, size, product tier, stakeholders,
  goals, constraints, contract dates)
- Engagement duration and any fixed dates (go-live, renewal, exec checkpoints)
- Output of `stakeholder-map` if it exists in `outputs/`

## Process
1. Restate the customer's business goals in one paragraph, in their language.
2. Define 3-5 adoption outcomes, each measurable (metric, baseline if known,
   target, date). Distinguish leading indicators (logins, feature activation)
   from lagging outcomes (business KPI movement).
3. Structure the engagement into phases (typically: Align → Enable → Expand →
   Sustain). For each phase: objectives, key activities, deliverables, exit
   criteria, duration.
4. Map milestones onto a timeline table with owners (customer side and our side).
5. Build a RACI for the top 8-12 activities.
6. List the top 5 risks with likelihood, impact, and mitigation.
7. Define the operating rhythm: weekly syncs, monthly steering, QBRs — who
   attends each, and what each meeting decides.

## Output format
Single markdown doc, max 5 pages, using `templates/engagement-plan-template.md`:
Goals | Outcomes & metrics | Phases | Timeline | RACI | Risks | Operating rhythm.
Save to `engagements/<customer>/outputs/engagement-plan-<yyyy-mm-dd>.md`.

## Quality criteria
- Every outcome has a number and a date, or is marked `[NEEDED: baseline]`.
- Every phase has explicit exit criteria — you can tell when it is done.
- The customer owns at least 40% of RACI "R" items; adoption is not done *to* them.
- Risks include at least one people-risk (champion attrition, sponsor change),
  not only technical risks.
