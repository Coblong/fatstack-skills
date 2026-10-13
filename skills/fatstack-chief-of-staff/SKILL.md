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

Present the proposed batch and order, parallel candidates, blockers, any prerequisite improvements, and model/effort choices. Ask the user to agree the batch before dispatch unless the conversation already explicitly approved it. Agreement to a batch does not alone authorize ticket transitions, commits, pushes, PRs, or merges. Carry forward existing explicit authorization for specific actions; collect missing approvals through `fatstack-implement` when the action is concrete and reviewable.

## 2. Choose the worker settings

Use the harness's advertised model and reasoning controls. Map these portable tiers to available models at runtime; never hard-code provider model names or assume that every agent accepts an effort setting.

| Tier | Effort | Suitable work |
| --- | --- | --- |
| Small | Low | Mechanical copy, renames, ticket write-ups, or trivial configuration. |
| Mid | Medium | Default for a well-specified feature or bug in one area, or straightforward review fixes. |
| Mid | High | Multiple interacting files, tests across layers, or tricky UI state. |
| Top | High or higher | Authorization, money, migrations, ambiguous requirements, hard bugs, or cross-cutting refactoring. |

Assess each ticket's acceptance criteria, uncertainty, impact, and earlier attempts rather than its title alone. Before dispatch, state the selected tier, actual available model and effort, and a one-line reason so the user can overrule the choice. If the harness only inherits a model or cannot control effort, state the actual setting and the limitation; ask the user to accept that fallback before dispatching affected work. Respect their overrides.

After a struggling attempt, diagnose what blocked it from the worker's evidence. Propose a higher model tier or effort, with the reason and any remaining uncertainty, before retrying. Escalation does not replace missing requirements, permissions, or tools; resolve those separately. Preserve existing work and route the retry to the same worker where supported, or hand its pointers to a replacement. Stop for user direction if no higher setting is available or repeated attempts cannot progress.

## 3. Dispatch and track

Give each ticket one implementation worker running `fatstack-implement`, with its own branch and isolated checkout. Workers prepare their checkouts using project conventions; keep unrelated changes in their original checkout. Dispatch independent tickets in parallel within available slots and tool limits. Serialize tickets sharing a mutable resource, environment, or conflicting files unless safe isolation is established. Do not dispatch dependents merely because their blocker worker has finished its turn.

Keep prompts short and point to authoritative sources. Supply the ticket reference, project/checkout location, tracker configuration path, requirements path or link, relevant glossary/decisions and prior commits, skill path, intended base, model/effort, and the exact user authorization already granted. Include unresolved questions that affect the ticket. Avoid copying full documents into prompts; if a worker cannot access a source, provide a scoped accessible copy with its origin and note the limitation.

For example:

```text
Implement <ticket reference> in <isolated project checkout> using <fatstack-implement path>.
Read <tracker path>, <requirements path>, and the project instructions there.
Base: <verified base revision>. Relevant decisions: <paths>. Prior work: <commit/PR pointers>.
Settings: <available model and effort>. User-authorized actions: <explicit scope or none>.
Return branch, PR or pending action, verified status, check results, and blockers.
```

Use background execution and sparse follow-ups so the user can continue discussing the goal. Maintain a compact conversation ledger per ticket: worker identity, checkout/branch, chosen settings and reason, dependencies, latest verified state, approvals, PR link, check results, and open questions. Use completion notifications or bounded waits rather than repeatedly asking for unchanged progress. Persist this ledger in session summaries; on resumption, verify worker, branch, PR, and tracker state before redispatching.

Workers return approval or missing-information requests to the coordinator when they cannot ask directly. Show the user the concrete pending action and the applicable skill rule, then send only their actual authorization back to the worker. A coordinator's dispatch is not user approval. Continue independent authorized work while an affected ticket waits.

## 4. Review and report

Review each worker's result with `fatstack-review` against the ticket, requirements, and project standards before presenting it as reviewed delivery. Inspect the pinned diff, acceptance coverage, and actual check evidence rather than relying on a worker's success claim. You may acknowledge receipt or a blocker promptly without implying review is complete.

Send corrections and PR feedback to that ticket's existing worker where the harness permits; delegate all fixes and any rebase requiring conflict resolution. The worker reruns affected checks, and the coordinator reviews the resulting revision again. Follow `fatstack-implement` for PR approvals, ready-state publication, status verification, and replies and resolution of addressed PR comments. Never merge without explicit user authorization. After an authorized merge, verify the tracker status and refresh the intended base before reconsidering blocked tickets.

Report a short running summary per ticket: settings and reason, reviewed PR or pending action, check results, verified status, and remaining questions. Raise useful environment improvements as evidence emerges. Suggest recurring review or check schedules only when supported and useful to the agreed goal; creating a schedule requires the user's request or approval.

For a milestone, summarize every member against its goal, including skipped or blocked members. Distinguish implementation reviewed in PRs from merged and shipped work. Call the milestone done only when its configured completion conditions and goal are verified; PR creation alone is insufficient. Suggest the next milestone with its blockers, without starting it until the user agrees. End a batch with the same concise per-ticket summary, pending decisions, and proposed environment improvements, not a transcript of worker conversations.
