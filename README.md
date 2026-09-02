# spec-driven-development

This repository is a reference implementation and reusable template for spec-driven development. It defines a consistent project layout and workflow in which every change begins with a reviewed specification describing the problem, intended outcomes, functional and non-functional requirements, constraints, assumptions, edge cases, and measurable acceptance criteria before implementation choices are made. Each specification is refined into a technical plan, traceable tasks, tests, and validation gates, following a structured **Spec → Plan → Tasks → Implement → Validate → Integrate** lifecycle. Important human input—including clarifications, approvals, trade-offs, rejected alternatives, and changes of direction—remains versioned alongside the work, while significant decisions are recorded with their context, rationale, consequences, owner, and status; accepted decisions are superseded rather than silently rewritten. The goal is to make development understandable, auditable, testable, and resumable, so that humans and coding agents implement agreed intent rather than relying on undocumented assumptions or transient conversations.

## Documentation

- [Documentation map](docs/README.md)
- [Recommended spec-driven document layout](docs/recommended-layout.md)
- [`AGENTS.md` template](agents-template.md)
- [Feature-packet and ADR templates](templates/README.md)
- [Research review and synthesis](docs/research/README.md)
- [Python backend full-stack layout review](docs/research/full-stack-fastapi-template.md)

## References

- [Source reference index](references/README.md)
