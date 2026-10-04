---
id: SPEC-NNNN
title: <outcome-focused title>
intent: intent.md
spec_revision: 1
# Allowed: draft, in_review, ready, implementing, validating, complete, paused, abandoned, superseded
packet_status: draft
# Allowed: low, medium, high
risk: medium
owner: <person or team>
requirements:
  - REQ-AREA-NNN
current_spec:
  - <link to baseline behavior>
reviewers: []
created: YYYY-MM-DD
last_reviewed: YYYY-MM-DD
---

# SPEC-NNNN: <Title>

## Intent

Read the companion [intent](intent.md) for the problem, desired outcome, and
stakeholder constraints. This required spec defines the behavior and observable
criteria that satisfy that intent; link to its rationale instead of repeating it.

## Context

<Baseline behavior and relevant evidence. Link rather than duplicate current
product and architecture documents.>

## Scope

### In scope

- <Observable behavior included in this change.>

### Out of scope

- <Explicit non-goal or deferred behavior.>

## Behavioral Scenarios

### <Scenario name>

- Given: <starting state>
- When: <action or event>
- Then: <observable result>

Include important failure, edge, permission, and recovery scenarios.

## Acceptance Criteria

| ID | Criterion | Evidence expected |
| --- | --- | --- |
| AC-NNNN-01 | <Observable, measurable result> | <Automated test or manual observation> |

## Quality and Trust Requirements

Address security, privacy, accessibility, performance, reliability,
observability, compatibility, migration, and retention where relevant. Mark an
item `Not applicable` with a short reason instead of silently omitting it.

## Interfaces and Data

<Public contracts, schemas, compatibility behavior, migrations, and rollback
constraints. Use `None` when there is no effect.>

## Assumptions and Dependencies

- <Assumption or dependency and how it will be verified.>

## Open Questions

- [ ] <Blocking question, owner, and decision deadline.>

## Approval

- Approval event: <link to the canonical HIN-NNNN-NN entry approving intent.md and this spec at spec_revision>

Increment `spec_revision` and reset `packet_status` to `in_review` when a
normative edit to intent or spec changes the agreed outcome, behavior, scope,
criteria, constraints, or non-goals. An ADR is optional and is not a prerequisite
for spec approval.
