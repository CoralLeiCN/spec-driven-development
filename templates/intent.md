---
spec: SPEC-NNNN
---

# Intent for SPEC-NNNN

This required file owns why the change matters and the outcome it must serve.
The companion `spec.md` defines detailed behavior and acceptance criteria. Keep
this file concise and link to existing requirements instead of copying them.

## Problem

<What problem or unmet need motivates this change? Describe the current impact.>

## Intended Users or Stakeholders

<Who is affected, and whose outcome should improve?>

## Desired Outcome

<What should become possible or improve, and why does that matter? Leave
detailed acceptance criteria to spec.md.>

## Motivation and Evidence

<Why address this now? Link to relevant user input, observations, or research
when available. A separate research file or ADR is optional.>

## Stakeholder Constraints

- <Required boundary or constraint, or link to an existing REQ-AREA-NNN. Use
  `None` if there are no additional constraints.>

## Review

Intent and spec share the `spec_revision`, `packet_status`, and approval event
recorded in `spec.md`. A change to this file's problem, outcome, or constraints
that changes agreed requirements must increment `spec_revision` and return the
packet to `in_review`. No separate intent approval is needed.
