# Research Review and Synthesis

**Status:** Completed research informing the current layout recommendation
**Reviewed:** 2026-09-02

## Review Scope

The research is split into focused reviews so that observations, recommendations, and limitations remain attributable to the source that supports them:

- [Agent Rumble](agent-rumble.md)
- [DeepSeek Harness](deepseek-harness.md)
- [OpenAI harness guidance](openai-harness-guidance.md)
- [Stripe Sync Engine](stripe-sync-engine.md)

The cross-study conclusions are developed in [Common patterns across the four studies](common-patterns.md).

[GPT-5.6 Sol guidance and practitioner pitfalls](gpt-5.6-sol-agent-guidance.md)
is supporting model/harness research rather than a fifth project case study. It
separates official OpenAI recommendations from public practitioner reports and
explains the guardrails adopted in the reusable agent template.

The Agent Rumble review uses local commit [`56024b7`](https://github.com/CoralLeiCN/Agent-Rumble/tree/56024b70244ff4f76ff137a37360a9d422c09471). The Stripe Sync Engine review uses commit [`93321ab`](https://github.com/stripe/sync-engine/tree/93321ab3644d5460213725abe0595247c403eb46). DeepSeek Harness and the online OpenAI material were reviewed as available on 2026-08-22. Each review distinguishes practices worth adopting from context-specific choices that should not be copied automatically.

The [Full Stack FastAPI Template layout review](full-stack-fastapi-template.md)
is an additional implementation-layout reference, not a fifth workflow case
study. It records the `backend/`, `frontend/`, package, generated-contract,
testing, and root orchestration boundaries observed at upstream commit
[`cb740b6`](https://github.com/fastapi/full-stack-fastapi-template/tree/cb740b656d7a0a6c5e12c7bf8e50343ec94ee9c7)
and supports the Python backend full-stack default in the recommended layout.

## Comparative Summary

| Concern | Agent Rumble | DeepSeek Harness | OpenAI guidance | Stripe Sync Engine |
| --- | --- | --- | --- | --- |
| Primary strength | Clear authority boundaries among requirements, specifications, designs, decisions, plans, and code | One authoritative home per fact, local agent instructions, and extensive mechanical documentation gates | Repository knowledge as the system of record, progressive disclosure, and agent-legible feedback loops | Implementation-ready plans, executable architecture rules, generated contracts, and extensive attributable agent work |
| Specification model | Strong current product specification with requirement traceability | Decision- and contract-led documentation rather than a uniform feature-spec pipeline | Plans and repository context are first-class inputs to agent execution | Plans mix problem, design, tasks, and evidence; some precede code and some arrive alongside it |
| Agent instructions | Strong root guidance, but some product constraints are duplicated there | Root operational map plus specialized subtree instructions | Official Codex documentation defines root-to-working-directory instruction layering | Root index shared through `AGENTS.md` and `CLAUDE.md`; no subtree instruction layering |
| Execution state | Detailed milestone plans, but no uniform task-level state format | Agent Notes and repository workflows have explicit lifecycles and validation | Agents benefit from bounded work, direct tools, review loops, and observable outcomes | Dated plans with concrete phases and commands, but inconsistent status and `active`/`completed` placement |
| Mechanical enforcement | Strong code, schema, fixture, and build checks; documentation traceability can be strengthened | Strong document format, link, freshness, generated-reference, test, and architecture checks | Important invariants should be encoded in tools and checks rather than prose alone | Strong architecture, generated OpenAPI, and test-wiring gates; weak plan and prose-document validation |
| Main caution | Large topic files and heading-based links will become harder to maintain as the project grows | The mature custom process is too heavy to copy wholesale into a general template | High-throughput merge practices are context-specific and not a universal risk policy | “Before or alongside implementation” does not establish prior human approval, and documentation drift is visible |

## Cross-Source Synthesis

The four studies converge on a closed loop rather than one prescribed folder structure: durable repository intent, an agent-facing navigation layer, bounded plans, executable checks, observable evidence, and feedback that updates current truth and the harness. The synthesis was also cross-checked against Optimism specifications, Kubernetes KEPs, OpenAI Agents Python and ExecPlans, GitHub Spec Kit, and Apache Camel. See the [common-pattern analysis](common-patterns.md) for the evidence matrix, cross-checks, important non-common practices, and minimum shared repository skeleton.

The strongest combined design is a hybrid:

1. Preserve global, stable sources of truth for principles, stakeholder requirements, current product behavior, accepted architecture decisions, and current architecture.
2. Give every behavior-changing change a self-contained feature packet containing its reviewed spec, plan, resumable tasks, durable human review outcomes, and validation evidence.
3. Keep the root `AGENTS.md` short and operational. Point to authoritative documents and put component-specific rules in local instruction files.
4. Use stable identifiers to trace requirements to acceptance criteria, tasks, tests, and validation evidence.
5. Treat the repository as the durable record. Capture the outcome of useful human input, not full conversation transcripts.
6. Make scope approval and high-impact trade-off authority explicit. Agents may draft and execute but do not silently approve product changes.
7. Turn prose rules into executable checks where practical, including document metadata, links, traceability, generated artifacts, architecture boundaries, and tests.
8. Give agents deterministic setup, focused commands, fixtures, logs, metrics, screenshots, and actionable failure messages.
9. Feed repeated agent failures back into the harness as improved documentation, tooling, abstractions, or checks.
10. Keep ceremony proportional to risk and exempt genuinely mechanical changes through a narrow, explicit policy.
11. Treat agent-assisted repository audits as a useful recurring feedback loop, but keep normative metadata, traceability, generated artifacts, and architecture boundaries under deterministic checks.
12. Require specification readiness before implementation when prior agreement matters; a plan committed alongside code is supporting explanation, not evidence of spec-first authorization.

This synthesis is implemented in the [recommended spec-driven document layout](../recommended-layout.md).

## Supporting Sources

[GitHub Spec Kit](https://github.github.com/spec-kit/) provides the core **Spec → Plan → Tasks → Implement** flow and useful clarification, checklist, and cross-artifact analysis stages. [AWS architecture decision record guidance](https://docs.aws.amazon.com/prescriptive-guidance/latest/architectural-decision-records/adr-process.html) supports recording a decision's context and consequences, reviewing it through an explicit lifecycle, and superseding accepted records instead of silently rewriting history.
