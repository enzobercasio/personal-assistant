# Skill: Stakeholder Map

## Purpose
Profile the people who determine adoption success — champions, blockers,
sponsors, and the silent majority — and define how to engage each one.

## Required inputs
- `engagements/<customer>/context.md`
- Discovery notes or call transcripts if available in `outputs/`

## Process
1. List every named stakeholder with role, team, and relationship to the rollout.
2. Place each on a 2x2: Influence (high/low) x Disposition
   (champion / neutral / skeptic). Mark unknowns explicitly as unknown, not neutral.
3. For each high-influence stakeholder write a profile: what they care about,
   what winning looks like for them personally, likely objections, best channel
   and cadence to reach them.
4. Identify gaps: missing sponsor? no champion in a critical team? single point
   of failure if one person leaves?
5. Draft one tailored key message per persona group (exec, team lead, admin,
   end user) — two sentences each, in language that persona uses.
6. Recommend 3-5 concrete engagement moves for the next 30 days
   (e.g., "1:1 with Head of Ops to convert skeptic before the workshop").

## Output format
Markdown doc: Stakeholder table | 2x2 summary | High-influence profiles |
Gaps & risks | Messages per persona | Next 30 days.
Save to `engagements/<customer>/outputs/stakeholder-map-<yyyy-mm-dd>.md`.

## Quality criteria
- Every high-influence skeptic has a named conversion strategy.
- Messages per persona reference *their* outcomes, not product features.
- At least one identified gap or single point of failure — a map with no gaps
  usually means we haven't looked hard enough.
- No invented people: only stakeholders present in context or discovery notes.
