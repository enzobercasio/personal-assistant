# Skill: Health Check

## Purpose
Assess adoption health from usage data and qualitative signals, and recommend
prioritized interventions. Feeds qbr-prep and troubleshooting-playbook.

## Required inputs
- `engagements/<customer>/context.md`
- Available signals: usage/metrics export, survey results, call notes,
  support ticket themes — whatever exists (state clearly what is missing)
- The outcomes/metrics defined in the engagement plan

## Process
1. Score against the engagement plan's outcomes: for each, current value vs.
   target, trend (improving/flat/declining), and confidence in the data.
2. Assess four dimensions, each rated Green/Amber/Red with one-line evidence:
   - Breadth: % of intended users active
   - Depth: are they using the valuable workflows or only logging in?
   - Sentiment: what champions, skeptics, and tickets are saying
   - Sponsorship: is the exec sponsor still visibly engaged?
3. For every Amber/Red, state the most likely root cause as a hypothesis with
   evidence — and what evidence would confirm or refute it.
4. Recommend max 3 interventions, prioritized by impact vs. effort, each with
   owner, first step, and a date to re-measure.
5. Write a 3-sentence executive summary: overall status, biggest risk,
   single most important action.

## Output format
Markdown doc: Exec summary | Outcome scorecard | Four-dimension assessment |
Root-cause hypotheses | Interventions | Data gaps.
Save to `engagements/<customer>/outputs/health-check-<yyyy-mm-dd>.md`.

## Quality criteria
- Never present a hypothesis as a confirmed cause.
- Max 3 interventions — a list of ten is a wish list, not a plan.
- Data gaps are listed explicitly; missing data is a finding, not a footnote.
- A Red in Sponsorship always triggers an intervention — it predicts everything else.
