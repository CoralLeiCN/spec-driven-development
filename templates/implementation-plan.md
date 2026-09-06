---
spec: SPEC-NNNN
# Allowed: draft, current, superseded
plan_status: draft
owner: <person or team>
updated: YYYY-MM-DD
---

# Implementation Plan for SPEC-NNNN

## Approved Outcome and Non-Goals

- Outcome: <one-sentence link-backed summary>
- Non-goals: <links or concise list from spec.md>

## Current Baseline

<Relevant implementation, architecture, tests, and known baseline failures.>

## Proposed Approach

<Smallest approach that satisfies the spec and fits current architecture.>

## Expected Change Surface

| Area or path | Intended change | Owner | Parallel-safe? |
| --- | --- | --- | --- |
| `<path>` | <bounded outcome> | <agent/person> | `yes` or `no` |

- New dependency: yes | no — <reason or link>
- New public interface: yes | no — <reason or link>
- Data migration: yes | no — <reason or link>
- Trust-boundary change: yes | no — <reason or link>

Unexpected changes to these answers require a plan or spec review before work
continues.

## Implementation Sequence

1. <Dependency-aware stage and completion signal.>

## Decision Log

Use this table only for bounded, reversible choices. Create an ADR for a
cross-cutting, externally visible, security-sensitive, or costly-to-reverse
decision.

| Date | Choice | Rationale | Alternatives | Consequence |
| --- | --- | --- | --- | --- |
| YYYY-MM-DD | <choice> | <why> | <what was rejected> | <trade-off> |

## Discoveries and Deviations

Record only observations that change the plan, risk, or ability to resume. A
discovery cannot expand approved product scope.

| Date | Observation and evidence | Plan impact | Disposition |
| --- | --- | --- | --- |
| YYYY-MM-DD | <fact plus link/error> | <none or exact change> | `incorporated`, `blocker`, or `follow-up` |

## Verification Strategy

| Acceptance criterion | Test or procedure | Expected result |
| --- | --- | --- |
| AC-NNNN-01 | `<command or manual procedure>` | <observable evidence> |

## Rollout, Rollback, and Recovery

<Deployment sequence, monitoring, rollback trigger, and recovery. Use `Not
applicable` with a reason for a local-only change.>

## Risks and Deferred Work

- Risk: <probability, impact, mitigation, and owner.>
- Deferred: <explicitly out-of-scope follow-up.>
- Active blockers: <links to canonical entries in tasks.md, or `none`.>
