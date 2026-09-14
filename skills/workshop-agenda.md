# Skill: Workshop Agenda

## Purpose
Produce the timed run-of-show for a workshop: segments, exercises, talking
points, and a materials checklist. Consumes the workshop plan.

## Required inputs
- `engagements/<customer>/outputs/workshop-plan-<topic>-*.md` — REQUIRED.
  If no plan exists, say so and offer to run `workshop-planning` first.
- Facilitator(s) and any co-presenters from the customer side

## Process
1. Read the workshop plan; carry over objectives, audience, format, and
   duration exactly. Do not re-decide them.
2. Structure the session with an energy arc: open with a hook tied to the
   audience's pain (10%), context-setting (15%), hands-on core (50%),
   application to their real work (15%), commitments & close (10%).
   Adjust percentages to format; hands-on core never below 40% for enablement
   workshops.
3. For each segment: start/end time, title, format (demo, exercise, discussion),
   facilitator, 2-3 bullet talking points, and — for exercises — step-by-step
   instructions plus what "done" looks like.
4. Insert breaks: at least 10 minutes per 90 of session time.
5. Build a materials & setup checklist: environments, sample data, slides,
   handouts, room/tooling — each with owner and ready-by time.
6. Add a contingency block: what to cut if running 15 minutes behind, and a
   backup activity if a demo fails (pull from the plan's risk register).
7. End with a commitments segment: each attendee names one thing they will do
   in their real work within 7 days — this feeds the follow-up checkpoints.

## Output format
Use `templates/workshop-agenda-template.md`. Timed table + exercise appendix +
materials checklist + contingencies.
Save to `engagements/<customer>/outputs/workshop-agenda-<topic>-<yyyy-mm-dd>.md`.

## Quality criteria
- Times add up to the planned duration exactly, breaks included.
- Every exercise has success criteria an attendee can self-check.
- The cut-if-behind plan removes content, never the commitments segment.
- Nothing contradicts the workshop plan; flag conflicts instead of silently
  resolving them.
