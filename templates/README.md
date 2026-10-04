# Reusable Document Templates

Use these files to create a feature packet under
`docs/specs/SPEC-NNNN-short-name/`. Replace every placeholder and remove
sections only when the repository's proportional change policy permits it.

`intent.md` and `spec.md` are required for every behavior-changing feature
packet. Intent owns the problem, desired outcome, and stakeholder constraints;
spec owns detailed behavior and acceptance criteria. Review them together under
the spec's revision and approval. Keep the supporting delivery files
proportionate to the change policy.

Research and ADRs are optional. Create an ADR only when a critical feature or a
decision involving substantial research or discussion needs a standalone record
of rationale, alternatives, and consequences. Research or discussion does not
automatically require one. Keep other technical decisions in the plan; omit
unused optional files rather than creating empty placeholders. ADRs explain
technical choices; requirements remain in intent and spec.

| Template | Destination | Purpose |
| --- | --- | --- |
| [`intent.md`](intent.md) | `intent.md` | Required problem, desired outcome, and stakeholder constraints |
| [`feature-spec.md`](feature-spec.md) | `spec.md` | Required behavior, scope, and observable acceptance criteria |
| [`research.md`](research.md) | `research.md` | Optional evidence, experiments, and unaccepted alternatives |
| [`implementation-plan.md`](implementation-plan.md) | `plan.md` | Technical approach, scope, risks, and verification strategy |
| [`tasks.md`](tasks.md) | `tasks.md` | Dependency-aware, resumable execution state |
| [`reviews.md`](reviews.md) | `reviews.md` | Durable human input, dispositions, and approvals |
| [`validation.md`](validation.md) | `validation.md` | Acceptance evidence, waivers, and residual risks |
| [`adr.md`](adr.md) | `docs/decisions/ADR-NNNN-short-name.md` | Optional decision rationale for critical features or substantial research/discussion |

First adopt and customize the core paths in the
[recommended document layout](../docs/reports/recommended-layout.md), including the
documentation map, change policy, product truth, and canonical operations
commands. Then copy [`../agents-template.md`](../agents-template.md) to the
target repository as `AGENTS.md`, replace its placeholders and links, and keep
only universally applicable instructions in the root file.
