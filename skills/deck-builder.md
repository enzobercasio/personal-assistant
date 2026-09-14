# Skill: Deck Builder

## Purpose
Produce a presentation outline — slide-by-slide with speaker notes — that
follows my narrative arc and tone, ready to build in the deck tool. Optionally
generate the deck directly as a Google Slides presentation.

## Required inputs
- `engagements/<customer>/context.md`
- Deck purpose: kickoff, workshop, exec readout, QBR, proposal
- Audience and time slot (drives slide count: ~1.5-2 min per content slide)
- Any upstream artifact the deck presents (engagement plan, health check, etc.)

## Process

### Phase 1 — Build the outline (always first)
1. Choose the narrative arc by purpose. Default arc:
   Current state → Vision (their outcomes) → Path (phases/plan) → Proof
   (examples, quick wins) → Ask (decisions and next steps).
   Exec readouts: Answer first — lead with the one-slide summary.
2. Write the deck's single controlling message in one sentence. Every slide
   must serve it; cut any slide that doesn't.
3. For each slide: an assertion title (a full sentence stating the takeaway,
   e.g., "Weekly active usage doubled after the admin workshop" — never a
   label like "Usage Update"), body content (max 4 bullets or one visual
   description), and 2-4 sentences of speaker notes.
4. Mark data or visuals needed as `[NEEDED: ...]` rather than inventing them.
5. Fit to the time slot; move overflow to an Appendix section.
6. Close with an Ask slide: explicit decisions, owners, dates.

### Phase 2 — Offer Google Slides generation
After the outline is approved, ask:

1. **Generate slides?**: "Do you want me to generate this as a Google Slides
   presentation, or is the outline enough?"
   - If no → done, save the outline only.
   - If yes → proceed to questions 2 and 3.
2. **Template**: "Do you have an existing Google Slides template to use?
   Share the link or file name, or say 'blank' for a new blank deck."
   - If a template link is provided, use `gdrive_duplicate_doc` (via the
     gdrive MCP) to create a copy as the starting point.
   - If blank, create a new presentation in the target folder.
3. **Folder**: "Which Google Drive folder should I save the presentation in?
   Share the folder link or folder name."
   - Use `gdrive_search` or `gdrive_list_files` to locate the folder.
   - If user says "you decide", save to the Drive root and tell them the path.

### Phase 3 — Generate Google Slides (if requested)
Use the Google Drive / Slides MCP tools to build the presentation:

1. Duplicate the template (if provided) or create a new presentation in the
   target folder.
2. For each slide in the outline:
   - Set the assertion title as the slide title.
   - Add body bullets or visual description placeholders.
   - Add speaker notes from the outline.
3. Mark any `[NEEDED: ...]` items as placeholder text on the slides so the
   user can spot and fill them.
4. Share the Google Slides link in the response.

**MCP tool mapping** (use whichever MCP tools are available):
- Google Drive MCP (`gdrive`): `gdrive_duplicate_doc`, `gdrive_search`,
  `gdrive_list_files`, `gdrive_read_file`
- Google Workspace MCP: if Slides-specific tools are available, prefer those
  for creating and editing slides directly.
- If no Slides creation tools are available, create a Google Doc with the
  full outline formatted for easy copy-paste into Slides, and tell the user.

## Output format
Use `templates/deck-outline.md`. Controlling message | Slide-by-slide outline |
Appendix | Needed-inputs list.
Save outline to `engagements/<customer>/outputs/deck-<purpose>-<yyyy-mm-dd>.md`.
If Google Slides generated, include the Slides link in the output file header.

## Quality criteria
- Read the titles alone top to bottom: they must tell the complete story.
- Exec decks: the answer/summary is slide 2 at the latest.
- No feature tour — every product mention is tied to a customer outcome.
- Slide count fits the time slot at 1.5-2 min per content slide.
- If Google Slides generated: every `[NEEDED]` item is visible as a
  placeholder on the slide, not hidden only in speaker notes.
