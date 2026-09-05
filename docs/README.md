# Documentation

The documentation is organized by provenance so readers can distinguish source
analysis from conclusions derived from it.

```text
case-studies/  source-specific observations and limits
      ↓
reports/       comparisons, synthesis, and recommendations
      ↓
../templates/  reusable artifacts produced from those recommendations
```

## Case Studies

The [case-study index](case-studies/README.md) contains one attributable analysis
per source. Four studies examine spec-driven or agent-heavy workflows; a fifth
examines the implementation layout used by the Full Stack FastAPI Template.

Case studies are evidence inputs. They describe what a source does, what is worth
adopting, and what should not be generalized. They are not recommendations for
this template on their own.

## Reports

The [report index](reports/README.md) identifies the inputs and role of every
derived document:

- [Case study synthesis](reports/case-study-synthesis.md) compares the four
  workflow case studies.
- [Common patterns across the four studies](reports/common-patterns.md) develops
  the detailed evidence matrix and shared operating model.
- [Recommended spec-driven document layout](reports/recommended-layout.md)
  turns the findings into the proposed repository structure and workflow.
- [GPT-5.6 Sol guidance and practitioner pitfalls](reports/gpt-5.6-sol-agent-guidance.md)
  is a supporting model/harness report, not a case-study-derived conclusion.

## Reusable Outputs and Sources

- [`agents-template.md`](../agents-template.md) is the lean root instruction
  contract to copy into a target repository as `AGENTS.md`.
- [Reusable document templates](../templates/README.md) provide concrete
  feature-spec, research, plan, task, review, validation, and ADR files.
- The [source reference index](../references/README.md) contains external links
  and citation metadata only.

The reports, `agents-template.md`, and templates become operational only after a
target repository adopts and customizes them. Case studies and reports remain
supporting evidence and do not override an adopted project artifact.
