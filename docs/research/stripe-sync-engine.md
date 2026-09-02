# Stripe Sync Engine Review

**Status:** Completed source and history review

**Reviewed:** 2026-08-23
**Source:** [Stripe Sync Engine at commit `93321ab`](https://github.com/stripe/sync-engine/tree/93321ab3644d5460213725abe0595247c403eb46)

## Scope

This review evaluates Stripe Sync Engine as a substantial product repository developed with extensive coding-agent involvement. It examines whether specifications or plans govern implementation, how agent context is organized, how human decisions are retained, and which rules are enforced mechanically. It does not treat the repository as a spec-driven-development framework.

## Overall Assessment

Stripe Sync Engine is a strong example of **agent-heavy, plan-accompanied engineering**, but it is not a complete example of strict specification-first development. Its plans are unusually concrete: they describe problems, interfaces, files, phases, non-goals, failure behavior, tests, and verification commands. The root agent guide tells contributors to save a plan before or alongside the first implementation commit, and several plans explicitly instruct Claude to execute them task by task.

The important limitation is the word “alongside.” History shows that some plans precede the relevant implementation, while others enter the default branch in the same large commit as the code. The process therefore preserves implementation intent well, but does not consistently prove that humans agreed on a normative specification before an agent began coding. It is most useful to this template as a source for plan quality, executable architecture rules, generated contracts, agent attribution, and repository-audit feedback loops.

## Project Context and Agent Use

Stripe Sync Engine moves Stripe data through a connector-based message protocol into PostgreSQL and other destinations. The reviewed monorepo contains protocol and connector packages, a stateless engine, a Temporal-backed service, a dashboard, deployment integrations, and cross-package tests. Its README warns that the software is under active development, lacks tight access controls, and has some current development occurring in a companion fork; it should therefore be studied as an engineering-process example rather than assumed to be a stable production reference.

At the reviewed commit, the `dev` branch contains 1,407 commits. A local history audit found explicit agent markers in 848 distinct commits, including 778 commits marked `Committed-By-Agent: claude`, 44 marked `codex`, and 33 marked `cursor`; these categories overlap. This is unusually strong public evidence that agents are doing implementation work rather than merely reviewing or documenting it.

## Observed Documentation Model

The repository separates several kinds of engineering knowledge:

- root `AGENTS.md`, with `CLAUDE.md` symlinked to it, provides the project map, common commands, universal constraints, known environment traps, and links to owning documents;
- `docs/architecture/principles.md` declares non-negotiable architectural rules;
- `docs/architecture/decisions.md` contains concise Design Decision Records with decision, rationale, alternatives where relevant, and consequences;
- `docs/architecture/`, `docs/engine/`, and `docs/service/` describe current package and subsystem behavior;
- `docs/design/` contains focused design proposals;
- `docs/plans/`, `docs/plans/active/`, and `docs/plans/completed/` contain plans and RFC-like records;
- `docs/todos.md` holds short-term unscoped work and directs scoped work into dated plans;
- `docs/changelog.md` links shipped changes back to completed plans;
- generated OpenAPI files provide machine-readable API contracts; and
- `docs/architecture/quality.md` is a manually maintained package-level test, type, and documentation scorecard.

The root guide explicitly calls itself an index rather than a rulebook and directs durable rules into principles, decisions, or other owning documents. This is a useful authority-boundary pattern even though the repository does not define a complete precedence order among all artifacts.

## Plan-to-Code Workflow

The strongest plans are implementation-ready handoff documents:

- the monolith-removal plan records explicit user decisions, target package ownership, estimated code movement, dependency-injection seams, ordered phases, and verification conditions;
- the Protocol v2 plan defines complete wire-message shapes, changed interfaces, affected files by phase, deliberate non-goals, and manual plus automated checks;
- the Reverse ETL plan states goals and non-goals, configuration contracts, checkpoint invariants, failure behavior, validation commands, live-test evidence, cleanup, and follow-ups; and
- several implementation plans open with an instruction for Claude to use an execution skill and work through the plan task by task.

This is substantially better than handing an agent a ticket title or an unstructured conversation. The plans bound the work, expose integration surfaces, preserve rejected scope, and tell an agent how to prove the result.

The history nevertheless shows mixed sequencing:

- the Protocol v2 plan entered the repository on 2026-04-03 and related protocol cleanup followed on 2026-04-04, demonstrating a plan-before-code path;
- the remote-engine plan and its implementation were introduced together in commit `36df5d02`; and
- the Reverse ETL plan was introduced in commit `921fe63b`, the same roughly 3,800-line change that added its implementation, tests, generated artifacts, and supporting files.

The repository rule also says non-trivial PRs “should” have a plan and permits it to be saved alongside implementation. There is no deterministic merge check requiring an approved plan before behavior-changing code. Stripe Sync Engine should therefore be described as plan-led or plan-accompanied, not universally spec-gated.

## Agent Harness and Verification

The repository gives agents a practical execution environment:

- canonical install, build, lint, format, unit, integration, and end-to-end commands are visible at the entry point;
- architecture boundaries are encoded in `e2e/layers.test.ts`, which rejects source-to-destination coupling, protocol dependencies, connector-to-application dependencies, and inverted application layering;
- CI checks generated OpenAPI artifacts for drift and verifies that every shell end-to-end test is wired into a workflow;
- integration tests use PostgreSQL and Stripe mocks, while selected paths run under both Node and Bun;
- generated Zod/OpenAPI contracts reduce duplication between validation, types, clients, and published API descriptions; and
- a scheduled repository-audit workflow invokes Claude to compare documentation with code, look for architecture violations and dead code, check the quality scorecard, repair fixable issues, and open a pull request.

The explicit `Committed-By-Agent` trailers make agent participation auditable across Claude, Codex, and Cursor. This is more informative than relying on contributor names or guessing from commit style.

## Human Input and Decision Retention

There are good isolated examples of durable human input. The monolith-removal plan records named user choices such as removing the Sigma subsystem and selecting new homes for retained components. Decision records preserve rationale and consequences, and richer records list rejected alternatives. Plans frequently preserve non-goals and follow-up decisions so an agent does not silently widen scope.

However, this is not standardized. The pull-request template asks only for a summary, optional test steps, and a related issue. Plans do not consistently identify owner, reviewer, approval, status, or the human input that changed them. There is no durable review artifact comparable to the proposed `reviews.md`, so reconstructing who approved scope or accepted a trade-off may still require pull-request or chat history.

## Strengths

### Agent instructions act as a navigation layer

The root guide contains high-frequency operational facts and points elsewhere for architecture and rationale. Its `CLAUDE.md` symlink avoids maintaining separate, divergent instructions for one agent.

### Plans are concrete enough to execute

File-level scope, interface definitions, phases, non-goals, failure tables, and exact verification commands make the better plans suitable for bounded coding-agent handoff.

### Important prose rules become tests

Connector isolation and application-layer ordering are not just documentation. Failing tests identify the violated boundary and point back to the architecture document.

### Schemas are executable contracts

Zod schemas generate validation and OpenAPI outputs, and CI rejects stale generated files. This keeps part of the specification directly connected to implementation and client-facing artifacts.

### The harness includes a feedback loop

The scheduled audit treats stale documentation, architecture drift, dead code, and quality gaps as recurring maintenance work. It is a useful supplement to deterministic checks.

### Agent work is explicitly attributable

Commit trailers provide direct evidence of which coding-agent harness participated and make historical analysis possible.

## Gaps and Transfer Risks

### A plan is not necessarily an approved specification

Plans frequently combine problem definition, design, tasks, and validation. Without a separate reviewed specification and readiness state, implementation choices can become documented as if they were agreed requirements.

### “Before or alongside” is too weak for strict spec-first development

Co-committing a plan with thousands of lines of code preserves an explanation but does not demonstrate that the plan constrained the implementation. The template should require a reviewed `ready` specification before implementation begins, while allowing plan refinement that does not change approved scope.

### Plan lifecycle and placement are inconsistent

At the reviewed snapshot there are plans at the directory root as well as 7 under `active/` and 9 under `completed/`. Some root plans declare themselves implemented, future, approved, or not started; some active plans refer to old branches and pull requests. There is no uniform metadata schema, plan template, generated index, or check that folder placement agrees with status. Moving files between lifecycle directories also makes links less stable.

### Traceability stops at document and file names

There are no stable requirement, acceptance-criterion, task, review, and validation identifiers. Tests are listed, but there is no mechanically checked chain showing which evidence proves each agreed outcome.

### Documentation checks are incomplete

CI checks generated OpenAPI artifacts and code architecture, but it does not validate Markdown links, plan metadata, lifecycle, plan presence, decision supersession, or agreement between prose and code. Repository formatting is also configured as non-blocking in CI.

### A concrete rule has drifted from code

Both `AGENTS.md` and the golden principles say `api_version` is always mandatory. The current source schema deliberately marks it optional, tests confirm omission is accepted, and DDR-008 documents the defaulting behavior. This contradiction demonstrates why critical prose contracts need an owning source and a deterministic consistency check. The scheduled AI audit is valuable, but it is conditional on an API secret and cannot replace structural validation.

### Instructions are not progressively localized

The repository has one root instruction file and no component-level `AGENTS.md` files. The root is still manageable, but a larger or longer-lived monorepo would benefit from placing connector-, service-, and deployment-specific rules nearer their code.

### Current repository status complicates authority

The README points active development to `sync-engine-fork`. Multiple active branches or repositories make it harder for a coding agent to know which plans, code, and decisions are current unless the authority boundary is explicit.

## Practices to Adopt

- Make the root agent file an operational index and link to owning documents.
- Keep durable architectural principles separate from current subsystem documentation and decision rationale.
- Give implementation plans explicit interfaces, affected files, phases, non-goals, failure behavior, and exact verification commands.
- Record consequential human choices and rejected scope in the durable change packet.
- Encode architecture boundaries as tests with actionable failure messages.
- Generate API contracts and derived documentation from typed schemas and reject drift in CI.
- Attribute agent-authored commits explicitly.
- Run a scheduled agent-assisted documentation and architecture audit as a supplement to deterministic checks.

## Practices Not to Copy Unchanged

- Do not allow “plan alongside implementation” for changes that require prior agreement.
- Do not use one plan file as specification, design, task tracker, review record, and acceptance evidence simultaneously.
- Do not move plan files between lifecycle directories; store status in validated metadata at a stable path.
- Do not rely on optional PR fields to retain approvals and important human input.
- Do not make an LLM audit the only defense against documentation drift.
- Do not declare a principle non-negotiable unless code, tests, and other authoritative documents agree with it.

## Influence on the Recommended Layout

Stripe Sync Engine reinforces the value of an agent-facing repository map, durable principles, lightweight decision records, implementation-ready plans, executable architecture boundaries, generated contracts, explicit agent attribution, and recurring harness audits. Its gaps support the stricter parts of this template: separate specification and plan authority, a human-controlled `ready` gate, stable lifecycle metadata, acceptance-level traceability, dedicated review and validation records, and deterministic documentation checks.
