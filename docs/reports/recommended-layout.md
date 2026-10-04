# Recommended Spec-Driven Document Layout

**Document type:** Recommendation report
**Status:** Recommended template design
**Updated:** 2026-10-04

**Evidence basis:** [Case study synthesis](case-study-synthesis.md),
[common-pattern report](common-patterns.md),
[Full Stack FastAPI layout study](../case-studies/full-stack-fastapi-template.md),
and [GPT-5.6 Sol supporting report](gpt-5.6-sol-agent-guidance.md)

## Purpose

This layout gives humans and coding agents one durable, navigable account of
what the product should do, what change has been approved, how it will be
implemented, why significant choices were made, and what evidence establishes
completion. It is deliberately smaller than a full governance framework. Add
folders only when the project has information that genuinely needs them.

The design has four boundaries:

1. **Current truth** describes the product and system as they are now.
2. **An approved delta** describes one behavior-changing unit of work.
3. **History and rationale** preserve important decisions without competing
   with current truth.
4. **Execution and evidence** make work resumable and completion auditable.

`AGENTS.md` is a small operating map across these artifacts, not a duplicate of
them. Start from [`agents-template.md`](../../agents-template.md).

Every behavior-changing feature packet requires `intent.md` and `spec.md` as
its requirements: intent explains the problem and desired outcome; spec defines
the behavior and acceptance criteria. Review them together. ADRs are optional,
reserved for critical features or decisions involving substantial research or
discussion that benefit from a standalone rationale. Keep supporting delivery
records proportional to the change.

## Core Layout

```text
.
├── README.md                         # Human-facing purpose and entry points
├── AGENTS.md                         # Lean universal agent contract and map
├── ARCHITECTURE.md                   # Short current system and dependency map
├── docs/
│   ├── README.md                     # Document index, ownership, and authority
│   ├── governance/
│   │   ├── change-policy.md          # Change classes, gates, and approvers
│   │   └── documentation.md          # Placement, writing, and lifecycle rules
│   ├── product/
│   │   ├── vision.md                 # Users, problem, outcomes, and non-goals
│   │   ├── glossary.md               # Canonical domain language
│   │   ├── requirements.md           # Stable stakeholder outcomes/constraints
│   │   └── specification.md          # Current normative product behavior
│   ├── specs/
│   │   ├── README.md                 # Packet index by packet_status and owner
│   │   └── SPEC-0001-short-name/
│   │       ├── intent.md             # Required WHY, outcome, stakeholder constraints
│   │       ├── spec.md               # Required WHAT, scope, scenarios, acceptance
│   │       ├── research.md           # Optional evidence and open alternatives
│   │       ├── plan.md               # HOW, expected scope, risks, verification
│   │       ├── tasks.md              # Ordered, resumable implementation state
│   │       ├── reviews.md            # Durable human input and approvals
│   │       └── validation.md          # Acceptance evidence and residual risk
│   ├── decisions/                    # Optional; create when an ADR is useful
│   │   ├── README.md                 # ADR index and lifecycle
│   │   └── ADR-0001-short-name.md    # Significant accepted choice and rationale
│   ├── architecture/
│   │   └── <subsystem>.md            # Current detailed structure and contracts
│   ├── operations/
│   │   ├── development.md            # Deterministic setup and canonical commands
│   │   └── testing.md                # Test layers, selectors, and evidence rules
│   └── generated/                    # Optional generated catalogs; never hand-edit
├── references/                       # External sources; non-binding by default
├── templates/                        # Reusable packet and ADR templates
├── scripts/
│   ├── new-spec                      # Optional packet generator
│   └── check                         # One canonical local/CI quality entry point
└── <component>/
    └── AGENTS.md                     # Only genuinely local rules and commands
```

Small projects may keep requirements, current specification, and architecture
in one file each. Split them by domain only when navigation, ownership, or merge
contention justifies it. Keep paths stable; lifecycle belongs in metadata rather
than `active/` and `completed/` directories.

## Python Backend Full-Stack Implementation Layout

When the product is a full-stack application with a Python backend, use the
FastAPI organization's
[Full Stack FastAPI Template](https://github.com/fastapi/full-stack-fastapi-template)
as the default implementation layout. The recommendation is based on upstream
revision [`cb740b6`](https://github.com/fastapi/full-stack-fastapi-template/tree/cb740b656d7a0a6c5e12c7bf8e50343ec94ee9c7),
reviewed on 2026-09-02. See the
[attributed layout case study](../case-studies/full-stack-fastapi-template.md) for the
complete observations and source links.

```text
.
├── backend/
│   ├── app/
│   │   ├── api/routes/              # Endpoint modules and router composition
│   │   ├── core/                    # Config, database, security, infrastructure
│   │   ├── alembic/                 # Database migrations when applicable
│   │   └── main.py                  # Backend application entry point
│   ├── scripts/                     # Backend lifecycle commands
│   ├── tests/                       # Backend-owned automated tests
│   ├── pyproject.toml               # Python package and tool configuration
│   └── Dockerfile
├── frontend/
│   ├── src/
│   │   ├── client/                  # Generated client from the backend contract
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── lib/
│   │   └── routes/
│   ├── public/
│   └── tests/                       # Browser/end-to-end tests
├── packages/                        # Optional shared/build-time packages
├── scripts/                         # Cross-stack generation and checks
├── .github/workflows/               # Component and integration CI/CD
├── compose.yml                      # Shared service topology
├── compose.override.yml             # Development specialization
├── compose.deploy.yml               # Deployment specialization
├── pyproject.toml                   # Optional Python workspace façade
└── package.json                     # Optional frontend workspace façade
```

This implementation tree composes with the spec-driven document tree above;
it does not replace it. Preserve the `backend/`, `frontend/`, root orchestration,
component-owned tests, and explicit generated-contract boundaries. Adapt
framework-specific internals and omit unused optional packages. Document and
obtain approval for a different top-level layout in the plan or an optional ADR,
within approved project constraints.

## Extensions, Not Defaults

Add these only when the project needs them:

| Need | Suggested location |
| --- | --- |
| Durable engineering principles | `docs/governance/principles.md` |
| Stable maintainer-only invariants that are easy to violate locally and expensive to rediscover | `.agents/references/<topic>.md` |
| Threat models and security procedures | `docs/security/` |
| Reliability objectives, observability, and runbooks | `docs/reliability/` or expanded `docs/operations/` |
| Release and migration procedures | `docs/operations/release.md` and `migration.md` |
| Incident learning and corrective actions | `docs/postmortems/` |
| A cross-cutting proposal spanning several packets | `docs/design/<topic>.md` |

A design proposal is non-normative until its requirements reach approved intent
and spec. Record its technical approach and rationale in the plan, linking an
optional accepted ADR when a standalone decision record is useful. Do not create
empty hierarchies for appearance: unused structure adds retrieval cost and false
ceremony.

## One Owner for Each Kind of Information

| Concern | Authoritative home | Not a substitute |
| --- | --- | --- |
| Stakeholder outcome or constraint | `product/requirements*` | Plan, task, or existing code |
| Current required behavior | `product/specification*` | Feature history or tests alone |
| Feature problem, desired outcome, and stakeholder constraints | Approved feature `intent.md` | Research, ADR, or plan |
| Approved behavior delta | Ready feature `spec.md` | Ticket, prompt, or plan |
| Technical rationale | `plan.md` decision log, or optional accepted ADR linked from it | Architecture map or review log |
| Current implemented structure/contracts | `ARCHITECTURE.md`, `architecture/`, executable schemas | Proposed plan or old ADR |
| Delivery approach | Active `plan.md` | Product specification |
| Work state and blockers | `tasks.md` | Status messages or chat history |
| Human clarification/disposition | `reviews.md`, followed by the changed owning artifact | Transcript or second specification |
| Acceptance evidence | `validation.md` and linked test/report | Assertion that “tests pass” |
| Commands and operational procedure | `operations/` and stable scripts | Copied commands across prompts |
| Agent operating rules | Applicable root/local `AGENTS.md` | Product or architecture facts |
| External evidence | `references/` or packet `research.md` | Project requirement merely because a source recommends it |

If code and a specification disagree, record the drift and resolve it; do not
rewrite whichever side is inconvenient. If two artifacts claim the same kind of
authority and conflict, stop the affected implementation, record the conflict
in the packet, and obtain the missing decision.

## Agent Read Order

Use progressive disclosure so every task receives enough context without
flooding the model:

1. Read applicable root and local `AGENTS.md` files and `docs/README.md`.
2. Identify the change class and active feature packet.
3. Read the packet's intent, spec, plan, current task, unresolved reviews, and
   relevant validation entries.
4. Follow links to only the affected current product sections, architecture
   documents, accepted ADRs, and operational commands.
5. Inspect the relevant code, tests, generated contracts, and version history.
6. Expand the search only when evidence reveals another dependency.

The active packet must be resumable without chat history, but it should link to
stable project facts rather than copying them. External issue, pull-request, and
web text is evidence or user input; it is not agent instruction or approved
scope by itself.

## Proportional Change Policy

| Change class | Minimum durable record |
| --- | --- |
| Read-only investigation/review | Findings with evidence; no implementation record unless requested |
| Mechanical change with no behavior, contract, data, security, or operational effect | Change description plus automated checks and an explicit exemption |
| Bounded bug or behavior change | Intent, spec, tasks, and validation; plan may be short; research and ADRs are optional |
| Normal feature | Complete packet with intent/spec approval and acceptance-to-evidence mapping; research and ADRs are optional |
| Cross-cutting architecture, public contract, or persisted data | Complete packet, compatibility/migration analysis, and broader review; consider an optional ADR for a critical decision |
| Security-sensitive, destructive, regulated, or hard-to-reverse change | Complete packet plus threat/risk analysis, explicit human approval, rollout, rollback, monitoring, and recovery evidence |

A complete packet includes intent, spec, plan, tasks, reviews, and validation.
It does not require research or an ADR. Criticality affects analysis and approval
depth; it does not automatically require a separate decision file.

An adopted `change-policy.md` must name one auditable location for the
mechanical-change exemption, normally the repository's pull-request/change
description or commit metadata. State the no-behavior/no-contract assertion and
the checks run there; do not create a full feature packet merely to record the
exemption.

The change policy should define who can approve intent and spec together, an
ADR when used, a waiver, and completion. A coding agent may investigate, draft, and execute
approved work; it must not approve its own product scope, compatibility break,
quality waiver, or material high-impact trade-off. Feature-scope, waiver, and
completion approval events live once in `reviews.md`; specs and validation link
to those entries. When an ADR is used, its acceptance lives in its metadata,
with packet reviews linking to it when relevant. Required approval for material
trade-offs applies wherever the decision is recorded. A plan needs a separate
review gate only when the change policy requires one for its risk class.

## Feature Packet Contract

Every packet records stable metadata in `spec.md`:

```yaml
id: SPEC-0042
title: Outcome-focused name
intent: intent.md
spec_revision: 1
packet_status: draft
risk: medium
owner: team-or-person
requirements:
  - REQ-CAT-001
reviewers: []
created: 2026-08-31
last_reviewed: 2026-08-31
```

### `intent.md` (required)

Owns the problem, intended users or stakeholders, desired outcome, motivation,
and stakeholder constraints. It states **why** the change is needed and links to
existing requirements and evidence. Keep it concise; detailed behavior and
acceptance criteria belong in the spec.

Intent and spec are reviewed together using the `spec_revision`, `packet_status`,
and approval event in `spec.md`; intent has no separate approval gate.

### `spec.md` (required)

Links to intent and owns current baseline links, in-scope behavior,
non-goals, observable scenarios, measurable acceptance criteria, quality/trust
requirements, interfaces/data effects, assumptions, dependencies, open
questions, and a link to its approval event. It states **what** must be true.
Technology belongs here only when it is a stakeholder constraint.

The `packet_status` in this file's metadata is the single owner of feature
packet lifecycle. The plan's `plan_status`, task execution status, and
`validation_status` describe only their own subordinate artifact and must not be
interpreted as packet approval.

The approval event covers intent and spec at the exact `spec_revision` it
approves. Increment that revision and reset `packet_status` to `in_review`
whenever a normative edit to either file changes the agreed outcome, behavior,
scope, acceptance criteria, constraints, or non-goals.
Administrative corrections that cannot change interpretation may retain the
revision and should be visible in version history.

### `research.md` (optional)

Owns reproducible observations, sources, prototypes, alternatives, uncertainty,
and clearly labelled inference. Research informs decisions but is not binding
and does not automatically require an ADR.

### `plan.md`

Owns the relevant implementation baseline, smallest proposed approach, expected
files and component ownership, architecture fit, interfaces and migrations,
decision links, sequence, risks, verification for every criterion, and
rollout/rollback when relevant. It states **how** within approved scope.

Keep a short living decision log for technical choices within approved scope.
Use an optional ADR only when a critical feature or substantial research or
discussion needs a standalone rationale, and link to it instead of duplicating
the decision. A plan can refine delivery; it cannot create product requirements.

### `tasks.md`

Owns dependency-aware execution state. Each task has a stable ID, bounded
outcome, expected file area, linked acceptance criteria and any applicable ADRs,
parallel-safety information, focused check, and resumption notes. Record blockers as they occur.
Do not mark a task complete before its declared evidence passes.

### `reviews.md`

Owns durable human clarifications, corrections, rejections, waivers, and
approvals. Each entry records date, contributor or role, concise input,
disposition, rationale where useful, and links to the artifacts changed as a
result. It is evidence that a decision occurred, not another normative source.

### `validation.md`

Maps every acceptance criterion to an exact automated command or manual
procedure, result, revision/date, and evidence location. It also records quality
gates, authorized waivers, residual risk, rollout evidence, and follow-ups. Do
not claim a check passed unless it ran successfully in the stated environment.

Use the concrete files in [`templates/`](../../templates/).

## Lifecycle and Approval

```text
draft → in_review → ready → implementing → validating → complete
```

| State | Meaning | Exit authority |
| --- | --- | --- |
| `draft` | Intent and spec are being captured; production implementation is not authorized | Author requests review |
| `in_review` | Scope, criteria, risk, and open questions are under review | Authorized reviewer |
| `ready` | Intent and spec are approved and no blocking product decision remains | Authorized human or explicit repository policy |
| `implementing` | Agent/human executes the current plan within the approved spec | Implementer after task completion |
| `validating` | Implementation is ready for acceptance and quality evidence | Required automation and reviewer |
| `complete` | Evidence passes and current-truth docs agree with implementation | Authorized reviewer or explicit policy |

Terminal alternatives are `abandoned` and `superseded`; a packet may be
`paused`. Keep blockers as explicit fields or lists rather than inventing
ambiguous lifecycle states. When implementation exposes a missing requirement,
contradiction, unsafe assumption, or material trade-off, stop dependent work and
return the affected artifact to review.

## Stable IDs and Traceability

| Item | Format | Example |
| --- | --- | --- |
| Requirement | `REQ-<AREA>-NNN` | `REQ-CAT-001` |
| Feature | `SPEC-NNNN` | `SPEC-0042` |
| Acceptance criterion | `AC-<SPEC>-NN` | `AC-0042-03` |
| Architecture decision | `ADR-NNNN` | `ADR-0017` |
| Human input | `HIN-<SPEC>-NN` | `HIN-0042-02` |
| Task | `TASK-<SPEC>-NN` | `TASK-0042-05` |
| Validation evidence | `VAL-<SPEC>-NN` | `VAL-0042-03` |

Use this minimum chain:

```text
REQ-CAT-001
  └── SPEC-0042 / intent.md
        └── spec.md / AC-0042-03
              ├── ADR-0017 (optional standalone rationale)
              └── TASK-0042-05
                    └── VAL-0042-03 → test or manual evidence
```

Do not annotate every code line with requirement IDs. Link requirements to
criteria, tasks to the criteria they implement, and validation to observable
evidence.

## When and Where to Record a Decision

| Discovery | Record in | Timing |
| --- | --- | --- |
| New/changed problem, desired outcome, or stakeholder constraint | `intent.md` plus disposition in `reviews.md`; increment `spec_revision` | Stop dependent implementation until intent/spec are re-approved |
| New/changed behavior, scope, criterion, or quality constraint | `spec.md` plus disposition in `reviews.md`; increment `spec_revision` | Stop dependent implementation until intent/spec are re-approved |
| Human clarification, rejection, waiver, or approval | `reviews.md`, then update the owning artifact; normative intent/spec edits increment `spec_revision` | Immediately |
| Plan refinement inside approved scope | `plan.md` decision log | Before executing the deviation |
| Critical feature decision or substantial research/discussion needing a standalone rationale | Optional ADR linked from the plan; otherwise the plan's decision log | Record before dependent implementation; obtain required decision approval wherever recorded |
| Local reversible implementation detail | Code/test, or plan only if needed to resume | No separate ADR |
| Progress, failure, or blocker | `tasks.md` | As it occurs |
| Adjacent finding outside approved scope | `plan.md` deferred work; validation links if relevant | Record once without automatically investigating or fixing it |
| Current implemented behavior/structure/procedure | Product spec, architecture, or operations owner | Before completion |
| Test/manual result | `validation.md` | Only after evidence exists |
| Repeated agent failure caused by missing context or tooling | Owning doc, script, check, fixture, or local `AGENTS.md` | Before completion when the improvement is reusable |

ADRs are optional. Consider one for a critical feature decision, such as a
security boundary or a difficult-to-reverse data model, or when substantial
research or discussion produces rationale that merits its own durable record.
Criticality, research, or discussion alone does not mandate an ADR. Keep
ordinary technical choices in the plan and do not create empty decision files.
Intent and spec remain the requirements; an ADR cannot add product scope or
replace their approval. An absent optional ADR is not an implementation blocker.

Accepted ADRs are historical records. Make only administrative corrections and
supersession links; replace the decision with a new linked ADR rather than
rewriting its history.

Do not record raw transcripts, private reasoning, routine tool output, secrets,
or several copies of the same rule. Preserve the durable outcome, evidence,
rationale, disposition, owner, and affected links.

## End-to-End Loop

1. **Classify:** choose the risk-proportional path.
2. **Capture intent and specify:** record the problem and desired outcome in
   intent; define behavior, non-goals, scenarios, constraints, and measurable
   criteria in spec; record durable human input.
3. **Approve:** an authorized human reviews intent and spec together and makes
   the behavior delta `ready`.
4. **Plan:** inspect current code/history, record technical choices in the plan
   or an optional ADR, and define expected scope, tasks, and verification.
5. **Implement:** complete one bounded task at a time with the smallest coherent
   diff and focused feedback.
6. **Validate:** run focused then complete risk-appropriate checks and map every
   criterion to evidence.
7. **Integrate:** update current product, architecture, operations, indexes, and
   generated contracts; complete the packet only when they agree.
8. **Improve the harness:** turn a repeated failure caused by missing context,
   tools, fixtures, observability, or enforcement into the smallest reusable
   correction.

## Agent and Harness Guardrails

The root instruction file should state each universal rule once, define
autonomy and approval boundaries in one place, and link to deeper owners.
Component files contain only local commands, invariants, generated paths, and
review concerns. Formatting and mechanically checkable policy belongs in tools
and CI.

For an autonomous coding model:

- define success and stop conditions so passing acceptance does not expand into
  unrequested cleanup or governance work;
- require the planned change surface and non-goals before implementation, then
  review the diff against them;
- use scoped search, indexes, and task-local context instead of exhaustive
  repository scans;
- retry a failure only after new evidence or a changed hypothesis, and record a
  persistent blocker rather than brute-forcing commands;
- delegate only cleanly separable work with clear ownership and no overlapping
  edits;
- preserve a living task checkpoint so work can resume after context
  compaction, then reconcile it with the actual working tree and test state;
- require exact receipts for completion claims; and
- require a concise final handoff containing outcome, changed artifacts,
  checks/results, residual risk, and open questions.

These constraints are intentionally model-agnostic. The supporting rationale
for GPT-5.6 Sol is in
[`gpt-5.6-sol-agent-guidance.md`](gpt-5.6-sol-agent-guidance.md).

## Mechanical Checks

Provide one local command that CI also runs. It should produce actionable error
messages and, as the repository matures, check:

- Markdown links, metadata, unique IDs, allowed lifecycle states, and indexes;
- every ready spec's intent, requirements, criteria, tasks, and validation links;
- accepted-ADR supersession rules and generated-document freshness;
- formatting, lint, types, unit/integration/contract/end-to-end tests as relevant;
- architecture boundaries, schema compatibility, migrations, security, and
  performance when the spec makes them relevant; and
- instruction discovery from the root and representative component paths.

During development run the smallest relevant check first; before completion run
the full risk-appropriate gate. Documentation is not a substitute for an
agent-legible environment: provide deterministic setup, stable test selectors,
representative fixtures, observable logs/metrics/UI behavior, safe recovery,
and exact operational commands.

## Adoption Sequence

Do not backfill elaborate packets for completed history. Adopt incrementally:

1. establish the documentation map, current product/architecture truth, root
   `AGENTS.md`, canonical setup/check commands, and change policy;
2. use packets with required intent and spec, plus stable IDs, for new
   behavior-changing work;
3. add optional ADRs only when critical features or substantial research or
   discussion need a standalone decision record;
4. automate link, metadata, traceability, and generated-artifact checks; and
5. add specialized folders or local instructions only after real navigation or
   enforcement needs appear.

The goal is the smallest durable set of artifacts that lets a new engineer or
coding agent understand what was agreed, why, what remains, how to act, and how
to prove completion.
