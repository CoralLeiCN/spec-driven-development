# Reusable Document Templates

Use these files to create a feature packet under
`docs/specs/SPEC-NNNN-short-name/`. Replace every placeholder and remove
sections only when the repository's proportional change policy permits it.

| Template | Destination | Purpose |
| --- | --- | --- |
| [`feature-spec.md`](feature-spec.md) | `spec.md` | Approved outcome, scope, and observable criteria |
| [`research.md`](research.md) | `research.md` | Optional evidence, experiments, and unaccepted alternatives |
| [`implementation-plan.md`](implementation-plan.md) | `plan.md` | Technical approach, scope, risks, and verification strategy |
| [`tasks.md`](tasks.md) | `tasks.md` | Dependency-aware, resumable execution state |
| [`reviews.md`](reviews.md) | `reviews.md` | Durable human input, dispositions, and approvals |
| [`validation.md`](validation.md) | `validation.md` | Acceptance evidence, waivers, and residual risks |
| [`adr.md`](adr.md) | `docs/decisions/ADR-NNNN-short-name.md` | Significant durable technical decision |

First adopt and customize the core paths in the
[recommended document layout](../docs/reports/recommended-layout.md), including the
documentation map, change policy, product truth, and canonical operations
commands. Then copy [`../agents-template.md`](../agents-template.md) to the
target repository as `AGENTS.md`, replace its placeholders and links, and keep
only universally applicable instructions in the root file.
