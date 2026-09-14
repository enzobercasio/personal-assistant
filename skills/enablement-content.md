# Skill: Enablement Content

## Purpose
Produce content the customer distributes internally: quick-start guides, FAQs,
and rollout comms — written in the customer's voice, for their users.

## Required inputs
- `engagements/<customer>/context.md`
- Content type: quick-start guide | FAQ | announcement email | champion kit
- Target user persona and the top 3 tasks they must be able to do
- Workshop outputs or common questions gathered so far, if available

## Process
1. Write for the end user inside the customer org, not for our customer contact.
   Assume zero prior product knowledge unless context says otherwise.
2. Quick-start guide: goal statement ("After 15 minutes you can...") → the top
   3 tasks as numbered steps with `[SCREENSHOT: ...]` placeholders → where to
   get help. One page per persona.
3. FAQ: harvest real questions from workshop notes and discovery; group by
   theme; answer in 2-4 sentences each; link deeper docs rather than duplicating.
4. Announcement email: from the sponsor's voice — why this change, what's in it
   for the reader, what to do first, where to get help, and the date anything
   old goes away. Under 250 words.
5. Champion kit: talking points, objection responses, and a checklist of what
   champions do in weeks 1-4.
6. All content: name the internal support channel and owner — enablement
   content without a help path creates frustrated silence.

## Output format
One markdown file per artifact.
Save to `engagements/<customer>/outputs/enablement-<type>-<persona>-<yyyy-mm-dd>.md`.

## Quality criteria
- A new user could follow the quick-start without asking anyone anything.
- Announcement email leads with reader benefit, not product launch pride.
- Every artifact names a help channel and an owner.
- Voice matches the customer's culture as described in context.md.
