# Agent Rumble Review

**Document type:** Workflow case study
**Status:** Completed source review

**Reviewed:** 2026-08-22
**Source snapshot:** [`CoralLeiCN/Agent-Rumble` at `56024b7`](https://github.com/CoralLeiCN/Agent-Rumble/tree/56024b70244ff4f76ff137a37360a9d422c09471)

**Used by:** [Case study synthesis](../reports/case-study-synthesis.md) and
[common-pattern report](../reports/common-patterns.md)

## Scope

This review examines the Agent Rumble repository as an example of specification-first development. It focuses on repository organization, document authority, preservation of stakeholder input, planning, agent guidance, traceability, and executable validation. It does not evaluate the product's business value or perform a full code-quality review.

## Observed Documentation Model

Agent Rumble separates durable information into distinct areas:

- root `AGENTS.md` for repository-wide agent instructions, safety constraints, setup, implementation guidance, and definition of done;
- `docs/documentation_guidelines.md` for document responsibilities, session capture, workflows, maintenance, and writing rules;
- `docs/requirements.md` for stakeholder-requested outcomes and constraints;
- `docs/specification/` for current normative product behavior;
- `docs/design-docs/` for proposed technical approaches and trade-offs;
- `docs/decisions.md` for accepted architecture decisions;
- `docs/exec-plans/` for proposed and active delivery plans;
- `docs/open-decisions.md` and `docs/backlog.md` for unresolved and explicitly deferred work;
- `docs/architecture.md` and `docs/development.md` for the implemented system and contributor workflow; and
- schema validation, fixtures, automated tests, and a root `make check` command for implementation verification.

The documentation guidelines explicitly prevent lower-authority artifacts—plans, design proposals, backlog entries, and implementation details—from silently overriding requirements or the normative specification.

## Strengths

### Clear artifact authority

The most valuable pattern is the explicit distinction between what a stakeholder requested, what the product must do, how it might be designed, what architecture choice was accepted, and how delivery work is sequenced. This substantially reduces accidental scope creation.

### Durable human-input workflow

The session-capture rules translate useful human input into a requirement, specification change, design proposal, accepted decision, open decision, backlog entry, execution plan, or project story. They avoid treating a raw transcript as documentation and preserve the authority of the resulting artifact.

### Requirement-to-specification traceability

The product-specification index maps requirement topics to specification sections and related decisions. This creates a navigable intent chain before implementation starts.

### Decisions with context and consequences

Accepted decisions record status, date, related requirements, context, decision, consequences, and references. They also state that decisions cannot independently create stakeholder scope.

### Plans grounded in contracts and verification

The active backend and frontend plans identify outcomes, current baselines, boundaries, milestones, validation, and completion conditions. The backend vertical-slice plan is especially strong in preserving schema semantics, safety boundaries, API contracts, and a verification matrix.

### Executable implementation baseline

The project includes versioned schemas, validators, representative fixtures, backend and frontend tests, typed builds, and a single top-level verification command. These make a meaningful portion of the written contract mechanically checkable.

## Gaps and Scaling Risks

### Large topic files

The requirements and decisions records are already several hundred lines each. Continuing with one file per artifact type will increase context cost, merge contention, and the chance that an agent reads unrelated material. Topic files with a stable index would preserve the authority model while enabling progressive disclosure.

### Heading-based identity

Traceability primarily uses Markdown heading anchors. Renaming or reorganizing a heading can break references or change identity. Stable IDs such as `REQ-CAT-001`, `AC-0042-03`, and `ADR-0017` would support automated checks and safer refactoring.

### Narrative rather than executable task state

Execution plans contain useful milestones and status prose, but no uniform task IDs, dependency fields, per-task verification, or completion state. A coding agent can implement from the plans, but resumability and machine validation would improve with a dedicated `tasks.md` contract.

### Product guidance duplicated in root instructions

The root `AGENTS.md` repeats important product and schema principles that also live in requirements and specifications. Keeping critical guardrails visible is useful, but duplicated normative language creates synchronization risk. The root file should retain concise operational rules and link to the owning product artifact.

### Missing explicit documentation gate

The root check command runs implementation tests and builds, but it does not expose a specific documentation gate for links, required metadata, stable identifiers, lifecycle consistency, or requirement-to-acceptance coverage. Those checks are important for a reusable spec-driven harness.

### Only repository-wide agent scope

The repository has one root `AGENTS.md`. Backend, frontend, and skill/plugin areas have different commands and invariants, so concise local instruction files would reduce irrelevant context and make review rules more precise.

## Practices to Adopt

- Retain the separation among requirements, normative specification, design proposals, accepted decisions, plans, open questions, and deferred work.
- Retain the rule that durable human input is captured as an outcome in the artifact matching its authority.
- Retain current-state architecture and development guides rather than making plans describe the implemented system forever.
- Add stable identifiers and automated traceability without replacing readable topic organization.
- Add self-contained feature packets for new behavior-changing work instead of retroactively restructuring completed history.
- Split plans into a technical `plan.md`, resumable `tasks.md`, human `reviews.md`, and acceptance-level `validation.md`.
- Reduce the root instruction file to universal operating rules and navigation; add local instruction files only where workflows differ.
- Extend the canonical check command with documentation, traceability, generated-artifact, and architecture gates.

## Practices Not to Generalize Automatically

- A single requirements or decisions file can remain appropriate for a small project; splitting should respond to size and contention rather than a fixed file count.
- Stakeholder-mandated technology choices belong in requirements in Agent Rumble, but other projects should preserve their own authority rules.
- Agent Rumble's domain-specific evidence, card-schema, and untrusted-repository rules are product constraints, not generic spec-driven development requirements.

## Influence on the Recommended Layout

Agent Rumble contributes the recommended layout's authority table, distinction between current product truth and change-local work, human-input disposition model, requirement traceability, and separation of proposals from accepted decisions. The additional feature-packet and CI mechanisms address the scaling gaps identified above.
