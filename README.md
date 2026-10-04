# spec-driven-development

This repository is a reference implementation and reusable template for spec-driven development. Every behavior-changing feature packet requires **intent** and **spec**: `intent.md` captures the problem, intended users, desired outcome, and stakeholder constraints; `spec.md` defines the required behavior, scope, edge cases, and measurable acceptance criteria. They are reviewed together before implementation.

The workflow is **Intent → Spec → Plan → Tasks → Implement → Validate → Integrate**. Plans, tasks, reviews, and validation keep delivery traceable and resumable, with detail proportional to the change. Important human input remains versioned alongside the work.

**ADRs are optional.** Use a separate Architecture Decision Record only when a critical feature or a decision involving substantial research or discussion benefits from a durable record of alternatives, rationale, and consequences. Ordinary technical decisions stay in the plan. Research or discussion alone does not require an ADR, and an ADR does not create product requirements. When an accepted ADR is replaced, supersede it with a linked record to preserve its history.

## Documentation

- [Documentation map](docs/README.md)
- [Recommended spec-driven document layout](docs/reports/recommended-layout.md)
- [`AGENTS.md` template](agents-template.md)
- [Feature-packet and ADR templates](templates/README.md)
- [Case studies](docs/case-studies/README.md)
- [Reports and their case-study inputs](docs/reports/README.md)

## References

- [Source reference index](references/README.md)
