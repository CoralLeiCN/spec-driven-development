# DeepSeek Harness Review

**Status:** Completed source review

**Reviewed:** 2026-08-22
**Source:** [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness)

## Scope

This review examines DeepSeek Harness as a public implementation example of a repository designed for coding-agent contribution. It focuses on its agent-instruction hierarchy, documentation ownership, architecture maps, decision records, testing policy, generated references, and mechanical enforcement. The repository is in developer preview and changes rapidly, so the review records patterns rather than treating its current paths as a permanent standard.

## Observed Documentation Model

DeepSeek Harness uses several distinct documentation tiers:

- root `AGENTS.md` for standing repository-wide orders, layout, commands, core conventions, safety, and links to owning documents;
- subtree `AGENTS.md` files for documentation, packages, examples, and other local rules;
- `docs/architecture.md` for the current high-level composition, core packages, event flow, capability seams, and extension points;
- subsystem pages and package READMEs for lower-level contracts;
- `.agents/notes/` for proposed, implemented, rejected, and archived decision rationale;
- cookbook and user documentation for procedures and product-facing guidance;
- generated catalogs for exhaustive source-derived reference material; and
- scripts and top-level commands for document links, formats, budgets, generated freshness, type equivalence, tests, and repository hygiene.

The documentation standard explicitly assigns one authoritative home to each fact and requires other tiers to link to that home instead of restating it.

## Strengths

### Progressive disclosure by ownership

The root instructions provide a map, while architecture, subsystem, package, testing, and documentation details live closer to their owners. Agents can load the material relevant to a change without consuming the entire repository manual.

### Local agent instructions

Subtree instruction files make specialized commands and constraints available only where they apply. This keeps repository-wide guidance smaller and reduces irrelevant or conflicting instructions.

### One home per fact

The documentation tier table states what each document owns and what does not belong there. This is a strong defense against duplicated contracts, stale inventories, and competing sources of truth.

### Explicit decision lifecycle

Agent Notes encode lifecycle and class in stable conventions, preserve rationale and rejected alternatives, define when a note must be written, and distinguish current authority from frozen historical records. Proposed work does not become implemented truth merely by being documented.

### Mechanically enforced documentation

DeepSeek Harness validates document links, word budgets, note format and placement, generated references, code examples, type equivalence, bilingual pairing, and archive integrity. Documentation errors are part of the same engineering feedback loop as code errors.

### Current architecture plus durable rationale

Architecture documentation explains how the system works now, while Agent Notes explain why significant decisions were made and what alternatives were rejected. This avoids forcing one document to be both current reference and historical narrative.

### Verification matched to the changed surface

The repository instructs contributors to choose focused tests, snapshots, documentation checks, builds, and real-API tests according to the affected behavior. It also requires user- or model-visible changes to update representative runnable examples and snapshots.

## Gaps and Transfer Risks

### High process cost

The repository has extensive custom scripts, document classes, lifecycle rules, bilingual pairing, generated references, and note requirements. Reproducing this system before a smaller project needs it would make the process harder to understand and maintain than the code it governs.

### Agent Notes are broader than conventional ADRs

Requiring a note for every non-trivial behavioral, architecture, process, or testing change fits DeepSeek Harness's goals, but may create low-value records in a smaller project. A general template should require decision records for significant or likely-to-be-revisited choices and keep routine change evidence inside the feature packet.

### Project-specific architecture rules

Plugin seams, event logging, package topology, strict coverage, bilingual documents, and release-preview compatibility choices are DeepSeek-specific. Their enforcement demonstrates a pattern; the constraints themselves are not reusable defaults.

### Mutable implemented notes require careful authority rules

Implemented Agent Notes may update factual locations while preserving the original decision. This is workable with strong gates, but a simpler template can keep ADR decisions immutable and place current paths exclusively in architecture documents.

## Practices to Adopt

- Give each fact one authoritative home and link to it elsewhere.
- Use a short root instruction map plus local subtree instructions.
- Separate current architecture and contracts from durable decision rationale.
- Give proposals, accepted decisions, rejected alternatives, and superseded history explicit states.
- Check documentation structure, metadata, links, and generated freshness mechanically.
- Generate exhaustive catalogs from source instead of maintaining them manually.
- Match verification commands to the behavior changed and expose focused checks to agents.
- Make failure output actionable enough that an agent can find the owner and remediate the problem.
- Run recurring documentation and architecture drift checks rather than relying only on change-time review.

## Practices Not to Generalize Automatically

- Do not require bilingual-document machinery unless the project actually publishes maintained translations.
- Do not impose universal word budgets or 100% per-file coverage without project-specific evidence.
- Do not require a separate decision note for every non-mechanical edit when a reviewed feature packet already preserves the relevant context.
- Do not copy plugin, package, session-log, or pre-release compatibility rules outside a product that has those constraints.

## Influence on the Recommended Layout

DeepSeek Harness contributes the recommended “one home per fact” rule, layered `AGENTS.md` design, separation of current architecture from rationale, explicit artifact lifecycles, generated-reference ownership, focused verification, and documentation-as-code gates. The recommended template keeps these principles while using a smaller default artifact set.
