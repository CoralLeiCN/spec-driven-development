# OpenAI Harness Guidance Review

**Status:** Completed source review
**Reviewed:** 2026-08-22

## Sources and Scope

This review combines two complementary OpenAI sources:

- [Harness engineering: leveraging Codex in an agent-first world](https://openai.com/index/harness-engineering/), an engineering case study about building a product through coding agents; and
- [Custom instructions with `AGENTS.md`](https://learn.chatgpt.com/docs/agent-configuration/agents-md), official OpenAI documentation describing how Codex discovers and layers repository instructions.

The review focuses on repository knowledge, progressive disclosure, agent legibility, feedback loops, mechanical enforcement, observability, and instruction placement. It does not assume that the case study's team structure, throughput, or merge policy applies to every project.

## Observed Harness Model

The engineering case study describes a repository in which humans steer at the level of intent, environment design, tools, abstractions, and feedback loops while agents execute implementation and review work. Repository-local documentation, plans, code, schemas, tools, logs, metrics, and runnable environments form the working context available to the agent.

The official Codex documentation explains that Codex builds an instruction chain from global guidance and then from the repository root down to the current working directory. Instructions closer to the working directory take precedence, and the combined instruction chain has a configured size limit. This provides a concrete mechanism for repository-wide rules plus component-specific guidance.

## Strengths

### Repository knowledge as the system of record

Information that exists only in chats, external documents, or people's memories is not reliably available during an agent run. Keeping intent, architecture, plans, decisions, and operating knowledge in versioned repository artifacts makes work discoverable and resumable for humans and agents.

### `AGENTS.md` as a map

The case study reports that a very large instruction file crowded out task context, became difficult to verify, and decayed quickly. Treating the root instruction file as a concise map allows deeper sources to remain authoritative and load only when relevant.

### Progressive disclosure

Agents begin with a small stable entry point and follow links into relevant product, architecture, plan, or subsystem documents. This preserves context capacity while still making deep information available.

### Agent-legible application behavior

The harness exposes user interfaces, logs, metrics, traces, worktree-local application instances, and browser-driving capabilities to the agent. Acceptance criteria such as performance limits or visible UI behavior become tractable when the agent can observe them directly.

### Mechanical architecture and quality enforcement

The case study favors enforcing boundaries and invariants through custom linters and structural tests while leaving agents freedom inside those constraints. Human preferences become scalable when captured in documentation or executable tooling.

### Feedback improves the harness

When an agent fails, the team asks which capability, context, tool, abstraction, or check is missing. Human corrections are promoted into reusable repository mechanisms instead of being repeated as one-off prompting advice.

### Continuous entropy control

Agents reproduce patterns already present in a repository. Recurring checks and cleanup prevent weak patterns, documentation drift, and technical debt from compounding across high-throughput changes.

## Gaps and Transfer Risks

### Case-study results are context-specific

The reported autonomy and throughput depended on substantial investment in repository structure, tools, isolated environments, observability, and review automation. A new project should not expect equivalent behavior from a specification folder alone.

### Minimal blocking merge gates are not a default best practice

The article describes a high-throughput environment in which corrections are cheap relative to waiting. That trade-off may be inappropriate for security-sensitive, regulated, destructive, costly, or difficult-to-reverse changes. This template should scale review and release gates according to risk.

### Repository-local knowledge still needs authority rules

Moving information into a repository does not by itself resolve conflicts, duplication, or stale documents. The harness also needs one authoritative home per fact, explicit lifecycles, ownership, and drift checks.

### Agent-first legibility can overlook other contributors

Optimizing for agents should also improve onboarding for humans. Generated catalogs, strict structure, and automated remediation should remain understandable to maintainers who need to inspect or override the automation.

## Practices to Adopt

- Make repository-local, versioned artifacts the durable source of development context.
- Keep the root `AGENTS.md` concise and link to deeper authoritative documents.
- Use Codex's documented instruction layering for component-specific commands and review rules.
- Treat plans and execution state as first-class repository artifacts.
- Make applications, logs, metrics, traces, schemas, and test evidence directly accessible to coding agents.
- Encode architecture boundaries and recurring quality rules as executable checks.
- Turn repeated human corrections into improvements to documentation, tools, fixtures, or enforcement.
- Support isolated, reproducible worktree environments and direct validation of user-visible outcomes.
- Schedule drift detection and small continuous cleanup rather than allowing weak patterns to spread.

## Practices Not to Generalize Automatically

- Do not adopt a no-human-code constraint unless it is an explicit experiment or organizational choice.
- Do not remove blocking review or release gates merely to increase agent throughput.
- Do not reimplement dependencies solely because an agent might understand local code more easily; evaluate maintenance, security, and ecosystem costs.
- Do not place product truth in `AGENTS.md`; use it to route the agent to the owning requirement, specification, architecture, or decision.

## Influence on the Recommended Layout

OpenAI's guidance contributes the repository-as-system-of-record principle, concise root instruction map, layered local instructions, progressive disclosure, agent-legible verification environment, executable invariants, feedback-driven harness improvement, and recurring drift control. These mechanisms make the spec-first artifact chain usable during real agent execution rather than leaving it as passive documentation.
