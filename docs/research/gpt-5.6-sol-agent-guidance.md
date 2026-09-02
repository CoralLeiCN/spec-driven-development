# GPT-5.6 Sol Guidance and Practitioner Pitfalls

**Status:** Supporting research for the agent template
**Reviewed:** 2026-08-31

## Scope and Evidence Standard

This review asks how a repository should guide GPT-5.6 Sol during spec-driven
development. It combines official OpenAI model and Codex guidance with recent
practitioner reports from public Reddit threads and Codex issues.

Official documentation supports model capabilities and prompting practices.
Codex issues and Reddit posts are first-hand reports from particular repositories,
prompts, tools, plans, and runtime configurations. They reveal useful failure
patterns but do not prove a universal model defect or its cause. The template
therefore adopts low-cost repository guardrails that remain useful even when a
specific report cannot be reproduced.

## Official Guidance That Shapes the Template

OpenAI describes GPT-5.6 Sol as its flagship coding model with a large context
window, high tool-use capability, and several reasoning-effort settings. The
repository should not hard-code a reasoning setting: effort and model selection
are runtime concerns and should be chosen through task-appropriate evaluation,
not treated as authority to expand work. See the
[official model page](https://developers.openai.com/api/docs/models/gpt-5.6-sol)
and [GPT-5.6 prompt guidance](https://developers.openai.com/api/docs/guides/latest-model?model=gpt-5.6).

The prompt guidance recommends giving the model the goal, useful domain context,
hard constraints, approval boundaries, required evidence, success criteria, and
output shape while avoiding unnecessary step-by-step control. It advises stating
each rule once, exposing only relevant tools, and using task-specific concurrency
and retry limits. OpenAI reports that leaner instructions improved its internal
coding-agent evaluation scores by roughly 10–15% while using materially fewer
tokens and lower cost; those figures are directional and should be validated in
each harness, not used as a promised result. The same guidance warns that
repeated “ask first” rules can create unnecessary stops and notes that GPT-5.6 is
concise by default, so a handoff should name required evidence rather than merely
say “be concise.”

Official Codex guidance supports a short root `AGENTS.md` with local files for
subtree-specific rules. Instructions from the repository root to the working
directory are combined, nearer files take precedence, and the documented default
combined size limit is 32 KiB. Formatting rules that a tool can enforce belong
in a formatter or CI, not repeated instruction text. See
[Custom instructions with AGENTS.md](https://learn.chatgpt.com/docs/agent-configuration/agents-md)
and [Codex prompting](https://learn.chatgpt.com/docs/prompting).

These sources lead to a three-layer context design:

1. `AGENTS.md` is the always-loaded operating policy and navigation map.
2. The active feature packet is the task-local, resumable handoff.
3. Product, architecture, decisions, and operations files are stable sources of
   truth loaded through links only when relevant.

## Practitioner Signals

### Instruction debt can amplify overplanning

A [July 11 Reddit report](https://www.reddit.com/r/codex/comments/1utb8zj/before_blaming_gpt56_for_overplanning_check_the/)
describes older compensating instructions such as demands for extreme
thoroughness becoming counterproductive with Sol; a
[discussion comment](https://www.reddit.com/r/codex/comments/1utb8zj/comment/oww1t74/)
also points to injected skills and plugins as hidden instruction sources. This
aligns with the official recommendation to remove repetition and keep examples
only when they address a measured gap.

**Template response:** state rules once, make the root file lean, load task
detail progressively, and avoid vague instructions to be exhaustive, review
repeatedly, or continue until fully confident.

### Persistence can turn scope creep into false governance

Open Codex issue reports
[#39059](https://github.com/openai/codex/issues/39059),
[#40424](https://github.com/openai/codex/issues/40424), and
[#41222](https://github.com/openai/codex/issues/41222) describe Sol adding such
mechanisms as hashes, manifests, gates, or policy and then treating those
agent-created artifacts as new authority. A
[two-run practitioner case study](https://www.reddit.com/r/OpenaiCodex/comments/1uv9u84/a_case_study_on_steering_gpt_56_sol_you_cannot/)
reports a larger redesign even after the user requested a surgical change; a
[comment on that study](https://www.reddit.com/r/OpenaiCodex/comments/1uv9u84/comment/oxbe7dh/)
suggests reviewing scope conformance separately from correctness.

Other anecdotes report similar costs: a
[release megathread](https://www.reddit.com/r/codex/comments/1urw0c3/gpt56_sol_codex_release_discussion_megathread/)
contains both positive reports and individual complaints about substantially
larger pull requests, while an
[August 17 report](https://www.reddit.com/r/codex/comments/1vqlutp/i_must_say_gpt56_sol_is_a_stupidly_intelligent/)
praises the model's intelligence but describes severe overengineering and poor
prioritization. These are anecdotes, not measured population-level behavior.

**Template response:** accepted specs and ADRs alone create requirements and
durable decisions. Plans, reviews, tests, generated files, and agent-authored
notes cannot approve themselves. Define non-goals and expected change surface,
inspect the diff against them, require an independent basis before adding new
infrastructure or policy, and stop when scoped acceptance passes.

### Recursive review and subagent loops can consume the task

A [July 28 report](https://www.reddit.com/r/codex/comments/1v9dq4b/gpt56_sol_gets_stuck_in_implementation_and_review/)
describes an implementation/reviewer loop repeatedly rejecting fixes. In a
separate [July 19 report](https://www.reddit.com/r/codex/comments/1v12oin/gpt56_sol_high_massively_overengineered_my_basic/),
the author initially blamed Sol for an overengineered prototype, then identified
their subagent-per-task TDD/review harness as a major contributor. A
[related Reddit comment](https://www.reddit.com/r/codex/comments/1v0kb45/comment/oyfskqn/)
attributes high usage in one setup to nested agents receiving full context.
Codex issue [#36557](https://github.com/openai/codex/issues/36557) documents large
parent-history duplication in one multi-agent workload, although that issue's
instrumented example used another GPT-5.6 variant.

**Template response:** delegate only genuinely independent, bounded work; pass
the smallest complete packet; prevent overlapping edits and recursive delegation
by default; let one coordinating agent own integration and final validation; do
not repeat reviews without new failing evidence.

### Long sessions and compaction can lose execution state

Codex issues [#35226](https://github.com/openai/codex/issues/35226) and
[#35935](https://github.com/openai/codex/issues/35935) report regression after
context compaction, including repeated reads or tests and lost subagent results.
A [July 16 Reddit report](https://www.reddit.com/r/codex/comments/1uy70sl/context_compactment_is_completely_broken/)
describes similar behavior, and a
[comment](https://www.reddit.com/r/codex/comments/1uy70sl/comment/oxwybfo/)
recommends a durable handoff file. Repository instructions cannot fix product
compaction, but they can reduce the cost of recovery.

**Template response:** make `tasks.md` a durable checkpoint with scope,
decisions, completed/pending work, changed areas, validation receipts, blockers,
and next action. After resumption, reconcile it with the working tree and actual
test state before acting or repeating work.

### Completion language can outrun observable state

Codex issue [#40646](https://github.com/openai/codex/issues/40646) reports a task
conflating inspection, validation, commit, deployment, and live acceptance.
Issue [#32251](https://github.com/openai/codex/issues/32251) gives a reproducible
example of GPT-5.6 family models reporting completion without the required tool
receipt; it concerned Luna/Terra rather than Sol and is included only as a
general harness warning.

**Template response:** keep inspection, implementation, focused validation,
system validation, commit, deployment, and live acceptance as distinct states.
Every completion claim must name direct evidence; every unrun or skipped check
must remain explicit in validation and the final handoff.

### Command brute force may be an environment mismatch

An [August 10 Reddit report](https://www.reddit.com/r/codex/comments/1vkewxl/gpt_56_sol_brute_force_coding/)
describes repeated failing commands and brute-force correction; the discussion
points to Unix-oriented commands in a Windows/PowerShell environment as one
likely contributor. Another
[July 13 report](https://www.reddit.com/r/codex/comments/1uvigpf/is_anyone_elses_codex_gpt56_sol_suddenly/)
attributes a slowdown in that setup to persistent plugin/MCP overhead. Neither
establishes a model-wide cause.

**Template response:** publish canonical environment-specific scripts, inspect
the exact failure, classify code/test/environment/flaky causes, retry only with
a changed hypothesis, and record a persistent blocker instead of making
unrelated edits until a command happens to pass.

### High capability is also a positive signal

The reports are not uniformly negative. A
[June 29 practitioner account](https://www.reddit.com/r/codex/comments/1uihemr/my_experience_with_gpt_56_sol/)
credits Sol with strong one-shot implementation, intent inference, and proactive
edge-case handling. The release megathread also contains positive experiences.
This is why the template gives the agent meaningful autonomy inside approved
scope rather than prescribing every step.

## Guardrails Adopted

| Observed risk | Durable repository control |
| --- | --- |
| Old exhaustive prompts cause overplanning | Lean root instructions; one statement per rule; progressive disclosure |
| Large or speculative changes | Ready spec, non-goals, expected change surface, diff review, explicit stop condition |
| Agent-created governance becomes authority | Ownership map; human approval; specs/ADRs cannot self-promote |
| Adjacent findings consume delivery | Blocker-versus-follow-up rule; no automatic investigation or fix |
| Repeated command/review loops | Evidence-based retries; no repeat without changed state or new failure |
| Nested agents multiply cost/context | Bounded independent delegation; no recursive delegation by default; central integration |
| Compaction loses state | Durable task checkpoint reconciled with actual repository state |
| Optimistic completion claims | Acceptance-to-evidence map and receipt-based final handoff |
| Platform/tool mismatch is mistaken for code failure | Canonical commands and explicit failure classification |
| Concise model omits important handoff detail | Required outcome, files, checks/results, risks, and open questions |

The adopted wording is in [`agents-template.md`](../../agents-template.md) and
the workflow is in the
[`recommended document layout`](../recommended-layout.md).

## What the Template Deliberately Excludes

Do not hard-code a model slug, reasoning level, compaction threshold, context
size, output limit, concurrency count, or quota workaround in `AGENTS.md`.
Those are volatile operator/runtime settings. Also avoid giant anti-loop prompts,
mandatory subagents for ordinary work, per-edit review gates, or a fixed demand
for maximum reasoning. Benchmark such controls against representative repository
tasks and keep only those that improve measured outcomes.
