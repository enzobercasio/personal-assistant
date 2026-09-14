# Skill: Technical Architecture

## Purpose
Create, review, or recommend changes to a customer's technical architecture
for the products we drive adoption of (RHOCP, ROSA, ARO, AAP, and adjacent
platform components). Three modes, one method: every architecture decision is
traced to a requirement, assessed against the same pillars, and recorded so
it can be defended in a design authority or steering meeting.

## Required inputs
- `engagements/<customer>/context.md`
- **Mode**: `create` | `review` | `recommend` (ask if not stated)
- Upstream outputs if they exist: `engagement-plan` (for adoption goals and
  phases), `stakeholder-map` (for decision-makers and skeptics who will
  challenge the architecture)
- Mode-specific inputs — ask for what is missing, max 3 questions per turn:
  - `create`: product(s) and target platform (cloud/on-prem/hybrid),
    workloads in scope, non-functional requirements (availability, RPO/RTO,
    scale, latency), security & compliance constraints, integration points
    (identity, network, registry, CI/CD, observability), team skills and
    operating model, budget or sizing envelope
  - `review`: the architecture to review — diagrams, design docs, cluster
    configs, playbooks — placed in `reference/docs/`; the requirements it
    claims to meet; what the customer wants from the review (validation,
    go/no-go, risk list)
  - `recommend`: the problem or decision at hand (e.g., "ROSA vs. self-managed
    OCP on AWS", "one cluster or many", "AAP controller topology for three
    regions"), options already under consideration, and the constraints that
    rule options in or out
- Harvest first: scan `reference/docs/` and `reference/calls/` for existing
  designs, constraints, and past decisions before asking anything.

## Official documentation & verification

All architecture claims — component capabilities, supported topologies,
sizing limits, upgrade paths, managed-service responsibilities — **must** be
verified against official Red Hat documentation at
[docs.redhat.com](https://docs.redhat.com/en). For managed services (ROSA,
ARO), also check the cloud provider's responsibility matrix. Key entry points:

| Product | Architecture & planning docs |
|---|---|
| OCP | `docs.redhat.com/en/documentation/openshift_container_platform/<version>/html/architecture/` |
| ROSA | `docs.redhat.com/en/documentation/red_hat_openshift_service_on_aws/4/html/introduction_to_rosa/` |
| ARO | `learn.microsoft.com/en-us/azure/openshift/` |
| AAP | `docs.redhat.com/en/documentation/red_hat_ansible_automation_platform/<version>/` |
| RHOSO | `docs.redhat.com/en/documentation/red_hat_openstack_services_on_openshift/` |

When the output states a platform limit, supported topology, or upgrade
constraint, cite the doc page. Mark anything not confirmed as `[VERIFY]`.

## Process

### Common to all modes
1. **Restate requirements** as a numbered list — functional, non-functional,
   security/compliance, operational. Mark assumptions and `[NEEDED]` gaps.
   Architecture without explicit requirements is decoration.
2. **Assess against the pillars**, each with evidence and a Green/Amber/Red:
   - Security & compliance (identity, network segmentation, secrets, supply
     chain, policy enforcement, audit)
   - Reliability & resilience (HA topology, failure domains, DR, RPO/RTO,
     upgrade strategy)
   - Operability (Day-2: observability, patching, GitOps/automation coverage,
     runbooks, on-call reality)
   - Scalability & performance (capacity model, growth path, limits)
   - Cost (right-sizing, managed vs. self-managed trade-off, licensing/
     subscription fit)
   - Adoption fit (does the team have the skills and operating model to run
     this? — the pillar architects skip and adoption architects cannot)
3. **Record decisions as ADRs**: for each significant decision — context,
   options considered, decision, consequences, and the requirement(s) it
   serves. Keep each ADR under half a page.
4. **Diagram**: produce a text diagram (Mermaid) of the target or reviewed
   architecture — components, trust boundaries, data/traffic flows. Mark
   anything not yet confirmed with dashed lines or `[VERIFY]`.
5. **Separate facts from opinions**: product capabilities and limits are
   stated only if confirmed by documentation or the reference files; anything
   uncertain is `[VERIFY]`, never asserted.

### Mode: create
6. Propose a target architecture in layers: platform (clusters/instances,
   regions, networking), identity & security, delivery (CI/CD, GitOps,
   registries), operations (observability, backup, automation), and
   workload placement.
7. Give a phased build sequence — what is minimum-viable to onboard the first
   workload, and what is deferred — with the adoption implications of each
   phase (which team learns what, when). Feed phases to engagement-plan.
8. List open decisions the customer must make, with a recommended default.

### Mode: review
6. Walk the submitted design against the requirements: for each requirement,
   met / partially met / not met / not addressed, with the evidence.
7. Findings table: severity (Critical / High / Medium / Low), finding,
   consequence, recommendation, effort. Lead with what would cause an outage,
   a breach, or a failed audit.
8. Name what is good — reviews that only list faults are ignored.
9. Give a verdict: proceed / proceed with conditions / rework, and the
   conditions.

### Mode: recommend
6. Frame the decision and the criteria that matter to this customer, weighted
   (draw weights from context.md goals and constraints, not from generic
   best practice).
7. Options comparison: 2-4 options scored against the weighted criteria, with
   the strongest argument for AND against each.
8. Recommendation with rationale, the conditions under which it would flip,
   and the reversibility of the choice (one-way door or two-way door).
9. Next steps to validate: spike, PoC scope, or questions to take to the
   product team.

## Output format
Single markdown doc, structured by mode:
- `create`: Requirements | Target architecture (by layer, with Mermaid
  diagram) | Pillar assessment | ADRs | Build sequence | Open decisions
- `review`: Requirements traceability | Findings | Strengths | Pillar
  assessment | ADRs (for decisions the design implies) | Verdict & conditions
- `recommend`: Decision framing & weighted criteria | Options comparison |
  Recommendation & reversibility | ADR | Validation steps
Save to `engagements/<customer>/outputs/architecture-<mode>-<topic>-<yyyy-mm-dd>.md`.

## Downstream chaining
- `create` → feed phases and open decisions to `engagement-plan`; feed
  Day-2 operations layer to `troubleshooting-playbook` and `workshop-planning`
  (ops enablement).
- `review` → Critical/High findings feed `troubleshooting-playbook`;
  conditions feed `engagement-plan` as risks; strengths feed `qbr-prep`.
- `recommend` → validation steps (PoC/spike) feed `workshop-planning` or
  `demo-builder` for hands-on proof.

## Quality criteria
- Every recommendation traces to a numbered requirement or an explicit
  customer constraint — no "best practice says" without a "because you need".
- All six pillars assessed, including adoption fit; an architecture the team
  cannot operate is not recommended, however elegant.
- Findings are severity-ranked and lead with outage / breach / audit risk.
- Product claims are confirmed or marked `[VERIFY]`; ROSA, ARO, self-managed
  OCP, and AAP differences are stated precisely, never blurred.
- ADRs exist for every significant decision, each stating consequences and
  reversibility.
- Written so it survives a design authority: a sceptical architect can follow
  the reasoning from requirement to decision without asking me.