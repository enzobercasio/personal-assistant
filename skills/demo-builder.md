# Skill: Demo Builder

## Purpose
Build a customer-facing product demo in two phases: first capture a demo
specification through a short interview, then produce the demo package —
environment prep, timed script with click-path, talking points tied to the
customer's outcomes, and fallbacks. Never build before the spec is agreed.

## Required inputs
- `engagements/<customer>/context.md`
- Answers to the specification interview (Phase 1 below)
- Stakeholder map and engagement plan from `outputs/` if they exist
- Any product/demo assets in `reference/docs/` (architecture diagrams,
  environment details, prior demo scripts)

## Official documentation & version references

All demo content — CLI commands, console workflows, architecture claims,
feature descriptions, and environment prerequisites — **must** be sourced from
or verified against the official Red Hat documentation at
[docs.redhat.com](https://docs.redhat.com/en). Do not rely on memory, blog
posts, or community wikis for product behaviour. When the demo script includes
a command or procedure, cite or link the corresponding official doc page so the
presenter (and any reviewer) can verify it.

### Canonical doc entry points by product

| Product | Docs URL | Notes |
|---|---|---|
| OpenShift Container Platform (OCP) | `docs.redhat.com/en/documentation/openshift_container_platform/<version>/` | Replace `<version>` with the target cluster version (e.g. `4.22`) |
| Red Hat OpenShift Service on AWS (ROSA) | `docs.redhat.com/en/documentation/red_hat_openshift_service_on_aws/4/` | Covers both HCP and Classic architectures |
| Azure Red Hat OpenShift (ARO) | `learn.microsoft.com/en-us/azure/openshift/` + `docs.redhat.com/en/documentation/azure_red_hat_openshift/` | ARO docs split between Microsoft and Red Hat |
| Ansible Automation Platform (AAP) | `docs.redhat.com/en/documentation/red_hat_ansible_automation_platform/<version>/` | Replace `<version>` (e.g. `2.7`) |
| Red Hat Enterprise Linux (RHEL) | `docs.redhat.com/en/documentation/red_hat_enterprise_linux/<version>/` | Replace `<version>` (e.g. `10` or `9`) |
| Red Hat OpenStack Services on OpenShift (RHOSO) | `docs.redhat.com/en/documentation/red_hat_openstack_services_on_openshift/` | For OpenStack-on-OCP deployments |
| Red Hat Developer Hub (RHDH) | `docs.redhat.com/en/documentation/red_hat_developer_hub/<version>/` | |

### Version look-up rule

Before building a demo package, confirm the **latest Generally Available (GA)
version** of the target product. Use the product's release notes page on
docs.redhat.com (or the customer's actual installed version if known from
context.md or reference data). Never demo against an EOL or pre-GA version
unless the customer explicitly requests it.

**Current GA versions (as of 2026-09-04 — re-verify before each demo build):**

| Product | Latest GA | Release date | EOL / Maintenance end |
|---|---|---|---|
| OpenShift Container Platform | 4.22 (patch 4.22.10) | 2026-06-09 | 2027-12-31 |
| ROSA | 4.22 | 2026-06 | Follows OCP lifecycle |
| AAP | 2.7 | 2026-06-03 | [check docs] |
| RHEL | 10.2 / 9.8 | 2026-05-19 | 10: 2035 / 9: 2032 |

If the customer's installed version differs from latest GA, build the demo
against their version and note the delta. Flag upgrade-relevant changes only
when the spec calls for it.

## Process

### Phase 1 — Specification interview (always first)
Ask these before building anything, max 3 questions per turn, and write the
answers into a spec section at the top of the output file. Skip anything
already answered by context.md or `reference/` — confirm instead of re-asking.

1. **Product**: which product is this demo for — RHOCP, ROSA, ARO, AAP,
   or another? If a managed vs. self-managed variant matters to this customer,
   confirm which one (e.g., ROSA vs. self-managed OCP on AWS).
2. **Scenario**: what should the demo prove? Push past features to the
   customer's "aha" — e.g., not "show pipelines" but "show a developer going
   from commit to running pod without filing a ticket".
3. **Audience**: who is in the room (exec / platform team / developers /
   ops), their familiarity with the product, and any known skeptics from the
   stakeholder map to aim key moments at.
4. **Environment**: where will the demo run — customer sandbox, our demo
   environment, a demo platform, or local? Any constraints (network, SSO,
   data sensitivity, region)?
5. **Format & duration**: live click-through, guided hands-on, or
   presentation with embedded demo? Time slot, and whether Q&A is inside
   or after the slot.
6. **Success criteria**: what should the audience say, decide, or do
   afterwards?

Present the completed spec back for my approval before Phase 2. If I answer
"you decide" on a point, choose and state the rationale in the spec.

### Phase 2 — Build the demo package
1. **Storyline**: structure the demo as pain → moment of value → how it
   works → what this means for you. Lead with the outcome, not the console:
   the audience should see their problem solved before they see architecture.
2. **Timed script**: for each segment — start/end time, what is on screen,
   the exact click-path or commands, the one line of narration that lands the
   point, and which audience member or objection it speaks to. Mark the 2-3
   "money moments" the demo must reach even if everything else is cut.
   - **Every CLI command or console path must be verified against the
     official docs for the target product version.** Include a doc-reference
     comment (e.g. `# ref: docs.redhat.com/…/cli-reference/…`) next to
     non-trivial commands so the presenter can confirm syntax before the demo.
   - When describing a product capability, use the terminology and scope
     from the official documentation — do not overstate what a feature does.
3. **Environment prep checklist**: prerequisites with owner and ready-by
   time — cluster/instances up, sample apps or playbooks loaded, credentials
   tested, quotas checked, browser tabs pre-opened in order, terminal font
   enlarged. Include a T-60-minute smoke-test run-through.
   - Record the exact product version running in the demo environment and
     confirm it against the version table above. If the environment trails
     the latest GA, note any UI or CLI differences.
4. **Fallbacks**: for each risky step, a plan B — screenshots or a recording
   in `reference/docs/`, a pre-provisioned backup environment, or a pivot to
   whiteboard. State the trigger for switching ("if login takes >30s...").
5. **Q&A prep**: the 5 questions this audience is most likely to ask (use
   the stakeholder map's skeptics), with short answers, and the honest
   "we'll take that away" list for anything unknown or roadmap-adjacent.
   - Source answers from official docs or knowledge-base articles
     (access.redhat.com). Link the source so the presenter can deepen
     their answer if pressed.
6. **Follow-up hooks**: what artifact the audience gets afterwards, and the
   next step the demo sets up (workshop, pilot, hands-on session) — feed
   this to workshop-planning if a workshop is the follow-up.

## Output format
Single markdown doc: Spec (approved) | Storyline | Timed script |
Environment prep | Fallbacks | Q&A prep | Follow-up.
Save the demo spec to `engagements/<customer>/outputs/demo-<product>-<topic>-<yyyy-mm-dd>.md`.
Save demo assets (runbooks, scripts, lab materials) to `demo-assets/<product_demo_name>/`.
The `demo-assets/` folder is gitignored — assets stay local.

## Quality criteria
- No demo content produced before the spec is approved.
- The script proves the scenario's "aha", not a feature tour — every segment
  names the audience pain it addresses.
- Money moments are marked, and the cut-if-behind plan protects them.
- Every risky step has a fallback with an explicit trigger.
- Environment checklist is executable by someone else — owners, times, and a
  smoke test, not vibes.
- Product names and claims match the spec's product exactly; anything
  uncertain about capabilities is marked `[VERIFY]`, never asserted.
- **All CLI commands, console paths, and API calls are verified against
  official Red Hat documentation for the target product version.** No
  commands sourced from blog posts, Stack Overflow, or memory alone.
- **The demo package header states the product version targeted and links to
  its release notes page on docs.redhat.com.**
- Q&A answers cite official docs or KB articles; roadmap-adjacent items are
  clearly flagged as such.
