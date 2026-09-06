# Human Review for SPEC-NNNN

Capture durable outcomes, not conversation transcripts. When accepted input
changes an authoritative artifact, update that artifact in the same change and
link it below. For rejected, deferred, or evidence-seeking input, record the
disposition and explicitly state that no authoritative artifact changed.

## Review Events

### HIN-NNNN-01 — <concise outcome>

- Date: YYYY-MM-DD
- Contributor or role: <name or role>
- Type: clarification | correction | rejection | waiver | approval
- Gate: none | spec ready | material trade-off | completion
- Input: <clarification, correction, constraint, or review concern>
- Disposition: accepted | rejected | deferred | needs evidence
- Rationale: <why this disposition was chosen>
- Changed artifacts: <links to owning artifacts, or `none` with reason>
- Follow-up: <owner and due condition, or `none`>

For an approval event, include the exact artifact revision or scope and any
conditions in `Input`, and link to the approved artifact. This entry is the
canonical approval record; other files link here instead of copying it.
