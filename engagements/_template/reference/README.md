# Reference files for this customer

Raw source material for this engagement. Skills read this folder to ground
their outputs in evidence instead of memory. Drop files here as you get them —
the context-builder skill will harvest them into context.md.

## Where things go
| Folder | What goes here | Which skills use it |
|---|---|---|
| `calls/` | Call notes, transcripts, meeting minutes. Name as `yyyy-mm-dd-<topic>.md` | context-builder, discovery-questions, stakeholder-map, troubleshooting-playbook, qbr-prep |
| `data/`  | Usage/metrics exports, survey results, support ticket summaries | health-check, qbr-prep, troubleshooting-playbook |
| `docs/`  | Contract/order form, org chart, customer strategy decks/OKRs, security requirements | context-builder, engagement-plan, stakeholder-map, deck-builder |
| `links/`     | Curated link collections: blog posts, KBAs, official docs, architecture references, community resources, and any external URLs relevant to the account. One `.md` file per topic or category (e.g. `ocp-upgrade-refs.md`, `rosa-networking.md`). | demo-builder, technical-architecture, enablement-content, workshop-planning |
| `shadowbot/` | Shadowbot AI outputs: account plans, adoption analyses, risk assessments, renewal forecasts, whitespace maps. Name as `yyyy-mm-dd-<type>.md` | context-builder, engagement-plan, health-check, qbr-prep |

## Rules
- These files are evidence, not deliverables — deliverables go in `../outputs/`.
- Prefix dated material with `yyyy-mm-dd-` so history stays sortable.
- When a fact from here lands in context.md, cite the file, e.g.
  `(source: reference/calls/2026-08-20-kickoff.md)`.
- Customer-confidential: never copy content from this folder into another
  customer's engagement or into `examples/`.
