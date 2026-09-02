# Common Patterns Across the Four Studies

**Status:** Completed cross-study synthesis
**Reviewed:** 2026-08-31

## Scope

This synthesis compares [Agent Rumble](agent-rumble.md), [DeepSeek Harness](deepseek-harness.md), [OpenAI harness guidance](openai-harness-guidance.md), and [Stripe Sync Engine](stripe-sync-engine.md). It identifies practices supported across the studies, distinguishes them from promising practices that are not yet universal, and translates the common ground into a minimum reusable project structure.

## Central Finding

The common foundation is not a particular specification format or directory name. It is a **closed development-control loop** in which humans make intent and important judgments durable, agents receive bounded repository context, implementation is checked against executable evidence, and failures improve the repository harness.

```text
Human intent and decisions
          ↓
Versioned authoritative artifacts
          ↓
AGENTS.md map → relevant product, architecture, and local instructions
          ↓
Bounded plan and execution state
          ↓
Coding-agent implementation
          ↓
Executable checks and observable evidence
          ↓
Update current truth and improve the harness
          └───────────────────────────────↺
```

A specification folder without the rest of this loop would reproduce only the most visible part of the approach.

## Common Core

### The repository is durable project memory

All four studies make or recommend repository-local, versioned knowledge. Requirements, specifications, architecture, decisions, plans, schemas, commands, and validation evidence must survive beyond one chat or agent session. This makes work discoverable, auditable, resumable, and available to both humans and agents.

### The root agent file is an entry point

Each study uses or recommends a root agent guide containing high-frequency operating information: repository purpose, layout, commands, universal constraints, and links to deeper sources. DeepSeek Harness and OpenAI go further with local instruction layering. Agent Rumble and Stripe Sync Engine show the scaling risk of keeping all specialized guidance at the root.

### Different artifacts own different kinds of truth

The studies repeatedly distinguish current architecture, durable rationale, proposed work, execution state, and validation. Agent Rumble provides the strongest authority model; DeepSeek formalizes one authoritative home per fact; OpenAI warns that repository knowledge still needs ownership; Stripe separates principles, decisions, current architecture, and plans but applies the separation less consistently.

### Plans are first-class agent handoffs

Plans are stored in the repository rather than left in prompts. Strong plans bound the outcome, identify affected interfaces and files, state non-goals and risks, divide work into phases, and name verification commands. Stripe provides the most concrete file-level examples, while its history also demonstrates why a plan committed alongside completed code is not evidence of prior agreement.

### Important rules become executable

All four studies connect written intent to schemas, validators, structural tests, generated artifacts, CI, fixtures, builds, or end-to-end tests. The shared principle is that stable invariants should not depend on an agent remembering prose. Failures should identify the violated rule, point to its owner, and give enough evidence for an agent to correct the change.

### Agents need observable evidence

The harness must expose the behavior it asks an agent to change. Across the studies this includes typed schemas, representative fixtures, test selectors, generated contracts, logs, metrics, traces, browser-visible behavior, snapshots, and runnable environments. A requirement that cannot be observed or tested remains difficult for an agent to implement reliably.

### Human corrections should improve the system

The recurring direction is to turn useful human intervention into durable improvements: clarify the authoritative artifact, add a decision, improve agent instructions, create a fixture, expose better diagnostics, or encode a new check. DeepSeek and OpenAI emphasize recurring entropy control; Stripe automates an agent-assisted repository audit; Agent Rumble preserves human input by placing its outcome in the artifact with the correct authority.

### Humans retain authority over meaning and risk

Agents can investigate, draft, implement, test, and review, but product intent, material scope changes, important trade-offs, and risk acceptance remain human responsibilities unless explicitly delegated. This boundary is clearest in Agent Rumble and OpenAI guidance, present through decision lifecycles in DeepSeek, and only partially recorded in Stripe.

## Comparison Matrix

| Pattern | Agent Rumble | DeepSeek Harness | OpenAI guidance | Stripe Sync Engine |
| --- | --- | --- | --- | --- |
| Versioned repository knowledge | Strong | Strong | Central recommendation | Strong |
| Root agent entry point | Strong but broad | Concise map | Concise map recommended | Concise operational index |
| Local instruction layering | Missing | Strong | Explicitly supported | Missing |
| Artifact ownership and authority | Strongest example | Strong “one home per fact” rule | Identified as necessary | Partial |
| Plans as durable agent input | Strong milestone plans | Notes and repository workflows | First-class harness input | Strong execution plans |
| Proof that specification precedes code | Strong intent, not mechanically gated | Not a uniform feature-spec pipeline | Not required by the case study | Mixed; some plans accompany code |
| Human decision and approval record | Strong | Decision lifecycle | Humans steer intent and risk | Isolated examples only |
| Stable lifecycle and traceability | Partial | Strong decision lifecycle | Recommended conceptually | Inconsistent plan status |
| Executable architecture and quality gates | Strong | Strongest documentation gates | Central recommendation | Strong code and contract gates |
| Agent-observable behavior | Tests, schemas, fixtures | Focused tests and runnable examples | Strongest general model | Tests, mocks, generated APIs |
| Recurring drift control | Identified gap | Strong | Central recommendation | Scheduled AI audit, weak deterministic document checks |

## Shared Direction, but Not Yet Common Practice

Several practices emerge as desirable precisely because the studies implement them unevenly:

- **Prior specification approval:** Stripe shows that a plan can document code without having constrained it. A reusable template should require a human-controlled `ready` state before implementation when prior agreement matters.
- **Stable traceability:** Agent Rumble provides useful requirement mapping, but none of the four demonstrates a complete, mechanically checked requirement-to-acceptance-to-task-to-evidence chain.
- **Dedicated human-review records:** Useful human decisions appear in several forms, but a consistent change-local review and approval artifact is not common.
- **Uniform resumable task state:** Plans are common; stable task IDs, dependencies, blockers, and per-task verification are not.
- **Deterministic documentation gates:** DeepSeek is the strongest example. Stripe's observed prose/code contradiction shows why scheduled LLM review alone is insufficient.
- **Risk-proportional governance:** OpenAI explicitly warns that high-throughput merge policies are context-specific. The other studies also contain project-specific ceremony that should not become a universal default.

These gaps explain why the recommended template is stricter than any single source in some areas: it combines demonstrated practices and closes weaknesses revealed by comparison.

## Cross-Check Against Additional Mature Projects

The four-study synthesis was checked against other large or production-oriented
open-source practices before turning it into the template:

| Project or practice | Relevant evidence | Addition to this template |
| --- | --- | --- |
| [Optimism specifications](https://github.com/ethereum-optimism/specs/discussions/182) and [Optimism `AGENTS.md`](https://github.com/ethereum-optimism/optimism/blob/develop/AGENTS.md) | OP Stack changes are expected to be specified before implementation; specifications include invariants, while agent instructions use progressive disclosure and explicit authority boundaries | Prior spec gate, invariant-oriented criteria, relevant-only context, and untrusted contributor text |
| [Kubernetes Enhancement Proposals](https://github.com/kubernetes/enhancements/blob/master/keps/sig-architecture/0000-kep-process/README.md) and [KEP template](https://github.com/kubernetes/enhancements/blob/master/keps/NNNN-kep-template/README.md) | `provisional` is distinct from approver-authorized `implementable`; higher-risk proposals address goals/non-goals, testing, compatibility, rollout, rollback, monitoring, and production readiness | Human-controlled `ready` state and risk-proportional sections rather than full KEP ceremony for every change |
| [OpenAI Agents Python instructions](https://github.com/openai/openai-agents-python/blob/main/AGENTS.md), [plans](https://github.com/openai/openai-agents-python/blob/main/PLANS.md), and [maintainer references](https://github.com/openai/openai-agents-python/blob/main/.agents/references/README.md) | Complex work uses living plans; narrow long-lived implementation contracts are retained only when stable, easy to violate, and expensive to rediscover | Resumable packets plus optional `.agents/references/`, while keeping the universal root template much smaller |
| [OpenAI ExecPlans](https://github.com/openai/openai-cookbook/blob/main/articles/codex_exec_plans.md) | A complex plan remains usable by a newcomer through purpose, progress, discoveries, decisions, exact commands, acceptance, and recovery | Living plan/task state with resumable context and observable milestone evidence |
| [GitHub Spec Kit](https://github.com/github/spec-kit) | The constitution is read by downstream stages rather than copied, and cross-artifact analysis checks drift | One authoritative home per rule and traceability checks |
| [Apache Camel `AGENTS.md`](https://github.com/apache/camel/blob/main/AGENTS.md) | Investigation includes reproducing the issue and checking history/design evidence before reversing surprising behavior | Baseline reproduction, relevant history inspection, and protection against silently undoing intentional decisions |

These are cross-checks, not additional universal authorities. Their exact
reviewer counts, templates, model choices, project tooling, and release policies
remain project-specific.

## What Is Not a Common Requirement

The following should not be presented as universal parts of agent-led, spec-first development:

- a particular directory name, Markdown template, or documentation generator;
- a separate decision record for every non-mechanical change;
- bilingual documentation, fixed word budgets, or universal coverage targets;
- no-human-code or maximum-throughput merge policies;
- a specific coding agent, model, attribution format, package topology, or deployment stack; or
- moving files between `active` and `completed` directories.

These are implementation choices. The common requirements are durable authority, bounded context, executable feedback, observable evidence, and explicit human responsibility.

## Minimum Shared Repository Skeleton

The four studies support the following minimum conceptual structure, even though their exact paths differ:

```text
README.md                    # Purpose and human entry points
AGENTS.md                    # Short agent operating map
ARCHITECTURE.md              # Current high-level system truth
docs/
  product-or-requirements/   # Intended and current behavior
  architecture/             # Current subsystem contracts
  decisions/                # Durable rationale and consequences
  changes-or-plans/          # Bounded change intent and execution state
  operations/               # Setup, testing, debugging, and release guidance
  generated/                # Source-derived reference artifacts, when useful
tests/                       # Behavioral, contract, integration, and structural evidence
scripts/check               # Canonical local verification entry point
.github/workflows/quality.yml
```

The recommended feature packet adds `spec.md`, `plan.md`, `tasks.md`, `reviews.md`, and `validation.md` because the comparison shows that those responsibilities otherwise become mixed or disappear into chat and pull-request history.

## Implications for This Template

The template should preserve the shared loop while remaining smaller than the most mature source:

1. Keep the repository—not chat history—as the durable record.
2. Make document ownership and conflict resolution explicit.
3. Keep the root agent guide short and localize specialized rules.
4. Require a reviewed specification before implementation for behavior-changing work.
5. Give agents bounded plans, resumable tasks, and direct verification commands.
6. Capture useful human input as decisions and dispositions, not transcripts.
7. Map acceptance criteria to concrete evidence.
8. Encode stable architecture, contract, documentation, and traceability rules in deterministic checks.
9. Make application behavior observable in isolated, reproducible environments.
10. Use recurring agent-assisted audits to find new gaps, then convert repeated findings into deterministic documentation, tooling, or enforcement.
