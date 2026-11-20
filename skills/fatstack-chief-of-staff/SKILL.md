---
name: fatstack-chief-of-staff
description: Coordinate a batch of tickets or a feature milestone through background implementers, choosing model and effort per ticket and reviewing each result.
disable-model-invocation: true
---

# Fatstack Chief of Staff

Coordinate delivery in a long-running session while keeping the user able to steer. Own the agreed goal and improve the conditions for the next ticket as well as progressing the current batch. The coordinator performs only enough read-only investigation to plan, assess complexity, and review results. Delegate every code-changing action, including tests, documentation, configuration, review fixes, and conflict resolution.

Required: project and tracker access, background sub-agent creation and follow-up tools, isolated worker checkouts, [fatstack-implement](../fatstack-implement/SKILL.md), [fatstack-review](../fatstack-review/SKILL.md), and the implementation skill's dependencies. Read the linked instructions directly when a manual-only skill cannot be invoked through the agent's skill tool. If the harness lacks background delegation, report that limitation and ask the user for another workflow; do not silently implement the batch yourself or create user-visible chats as a substitute. If model or effort selection is limited, handle it as described below rather than claiming unavailable controls.

## 1. Define and agree the batch

Read the user's goal and relevant conversation, `docs/fatstack/tracker.md`, applicable `AGENTS.md` / `CLAUDE.md`, the glossary and decisions, candidate tickets, and their linked requirements. Missing tracker configuration requires setup or an explicitly selected local-ticket workflow before tracker actions. Read actual statuses, blockers, existing branches, and linked PRs; a ready label alone does not establish that a ticket can start. Resolve disagreements between conversation and saved sources with the user before dependent work.

For a milestone request, resolve its feature, phase number, native identity, scope, and goal from `requirements.md` and the tracker guide. Use the guide's milestone listing operations and read every page, including non-ready members, to assess the whole milestone. If "milestone 3" matches several features, ask which one. The proposed implementation batch contains its ready tickets; record completed, in-progress, in-review, or incompletely configured members separately rather than dropping them from the goal assessment. Offer to resume existing work with the user's agreement instead of assigning a competing worker.

Build the dependency order from all selected tickets and their blockers, including unfinished earlier milestones and work outside the batch. Identify cycles, missing blockers, incomplete ticket setup, and unclear requirements. Explain their effect and agree whether to resolve prerequisites, defer affected tickets, or narrow the batch. Do not add prerequisite work silently. A ticket is dispatchable only when its blockers are verified done and their changes are present in the worker's intended base. An open or reviewed PR is not a completed blocker. If the user explicitly chooses stacked development, agree and record the alternative base and review/merge order before dispatching dependent work.

Before feature work, inspect for concrete environment impediments: missing checks or standards, inaccessible test data or logs, ambiguous APIs, or unavailable browser tooling. Suggest improvements with their purpose and priority. Delegate approved prerequisite changes before the feature tickets that need them. Fix deviations through the established process rather than bypassing checks or inventing local exceptions; defer unrelated improvements for the user's decision.

Present the proposed batch and order, parallel candidates, blockers, any prerequisite improvements, and model/effort choices. Ask the user to agree the batch before dispatch unless the conversation already explicitly approved it. A requested implementation batch covers routine ticket transitions, agent-selected test seams, scoped commits, branch pushes, and ready PR creation through `fatstack-implement`; do not request separate approval for those steps. Carry forward explicit user limits such as no publishing. Merges and work outside the agreed batch still require separate user intent; project and tool restrictions remain in force.

## 2. Choose the worker settings

Delegation saves cost. The model running this coordinator is the maximum model for the batch: select the cheapest suitable worker model at or below that model's cost tier. Never delegate to a more expensive model, including for reviews, retries, or difficult tickets. Each worker inherits this rule and may delegate only to its own model or a cheaper one, so nested delegation can never raise the ceiling. A task that needs a more expensive model remains blocked for user direction; do not offer an upward delegation override.

Use the harness's advertised model and reasoning controls. Establish the coordinator's actual model and the relative costs of available worker models from reliable runtime information or current pricing before selecting a different model; capability labels alone do not establish cost. If cost ordering is unknown, inherit the coordinator's exact model or use a model verified cheaper; if neither is possible, stop affected dispatch. Map these portable tiers only to eligible models at runtime; never hard-code provider model names or assume that every agent accepts an effort setting.

| Tier | Effort | Suitable work |
| --- | --- | --- |
| Small | Low | Mechanical copy, renames, ticket write-ups, or trivial configuration. |
| Mid | Medium | Default for a well-specified feature or bug in one area, or straightforward review fixes. |
| Mid | High | Multiple interacting files, tests across layers, or tricky UI state. |
| Highest eligible within the ceiling | High or higher | Authorization, money, migrations, ambiguous requirements, hard bugs, or cross-cutting refactoring. |

Assess each ticket's acceptance criteria, uncertainty, impact, and earlier attempts rather than its title alone. A tier unavailable within the ceiling does not authorize a more expensive model. Before dispatch, state the coordinator's model ceiling, selected tier, actual available model and effort, and a one-line reason so the user can choose another eligible setting. If the harness only inherits a model or cannot control effort, state the actual setting and the limitation; ask the user to accept that fallback before dispatching affected work. Apply user choices within the ceiling.

After a struggling attempt, diagnose what blocked it from the worker's evidence. Resolve missing requirements, permissions, or tools separately. Retry on the same model or a cheaper one, with a narrower task, better context, or a proposed effort adjustment where useful; a retry or replacement must never increase the worker's model cost tier. Preserve existing work and route the retry to the same worker where supported, or hand its pointers to an eligible replacement. Stop for user direction if no suitable setting is available within these limits or repeated attempts cannot progress.

## 3. Dispatch and track

Give each ticket one implementation worker running `fatstack-implement`, with its own branch and isolated checkout. Workers prepare their checkouts using project conventions; keep unrelated changes in their original checkout. Dispatch independent tickets in parallel within available slots and tool limits. Serialize tickets sharing a mutable resource, environment, or conflicting files unless safe isolation is established. Do not dispatch dependents merely because their blocker worker has finished its turn.

Keep prompts short and point to authoritative sources. Supply the ticket reference, project/checkout location, tracker configuration path, requirements path or link, relevant glossary/decisions and prior commits, skill path, intended base, model/effort, and the user's requested implementation scope and explicit limits. Include unresolved questions that affect the ticket. Avoid copying full documents into prompts; if a worker cannot access a source, provide a scoped accessible copy with its origin and note the limitation.

For example:

```text
Implement <ticket reference> in <isolated project checkout> using <fatstack-implement path>.
Read <tracker path>, <requirements path>, and the project instructions there.
Base: <verified base revision>. Relevant decisions: <paths>. Prior work: <commit/PR pointers>.
Settings: <available model and effort>. Delegation ceiling: <this worker's model>; any further delegation must use this model or a cheaper one, including retries and reviews.
Requested implementation: <scope>. Explicit user limits: <limits or none>.
Return branch, PR or pending action, verified status, check results, and blockers.
```

Use background execution and sparse follow-ups so the user can continue discussing the goal. Maintain a compact conversation ledger per ticket: worker identity, checkout/branch, chosen settings and reason, dependencies, latest verified state, approvals, PR link, check results, and open questions. Use completion notifications or bounded waits rather than repeatedly asking for unchanged progress. Persist this ledger in session summaries; on resumption, verify worker, branch, PR, and tracker state before redispatching.

Workers return missing-information or tool-access blockers to the coordinator when they cannot ask directly. Routine status updates, test-seam choices, scoped commits, branch pushes, and ready PR creation are already covered by the requested implementation; direct the worker to proceed rather than forwarding a redundant approval request. For genuinely missing information or an action outside the agreed scope, show the concrete question or action to the user, then send their answer back to the worker. Continue independent work while an affected ticket waits.

## 4. Review and report

Review each worker's result with `fatstack-review` against the ticket, requirements, and project standards before presenting it as reviewed delivery. Inspect the pinned diff, acceptance coverage, and actual check evidence rather than relying on a worker's success claim. You may acknowledge receipt or a blocker promptly without implying review is complete.

Send corrections and PR feedback to that ticket's existing worker where the harness permits; delegate all fixes and any rebase requiring conflict resolution. The worker reruns affected checks, and the coordinator reviews the resulting revision again. Follow `fatstack-implement` for ready-state publication without an approval checkpoint, status verification, and replies and resolution of addressed PR comments. Never merge without explicit user authorization. After an authorized merge, verify the tracker status and refresh the intended base before reconsidering blocked tickets.

Report a short running summary per ticket: settings and reason, reviewed PR or pending action, check results, verified status, and remaining questions. Raise useful environment improvements as evidence emerges. Suggest recurring review or check schedules only when supported and useful to the agreed goal; creating a schedule requires the user's request or approval.

For a milestone, summarize every member against its goal, including skipped or blocked members. Distinguish implementation reviewed in PRs from merged and shipped work. Call the milestone done only when its configured completion conditions and goal are verified; PR creation alone is insufficient. Suggest the next milestone with its blockers, without starting it until the user agrees. End a batch with the same concise per-ticket summary, pending decisions, and proposed environment improvements, not a transcript of worker conversations.
