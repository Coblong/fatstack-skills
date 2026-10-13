---
name: fatstack-tickets
description: Break a feature's requirements into demonstrable tickets and optional milestones, agree the dependencies and phases with the user, then create them in the project's tracker.
disable-model-invocation: true
---

# Fatstack Tickets

Slice the work into tickets that each deliver something demonstrable, group larger features into milestones, agree the plan with the user, then create it in the tracker.

## 1. Gather the sources

- Start with the current conversation, then read `docs/fatstack/features/<feature-slug>/requirements.md` to fill gaps. In a new session the requirements document is the main source. Where they disagree, ask which is right. Tickets link back to `requirements.md`, so it must exist before any tickets are created: if it does not, run `fatstack-requirements` first, which can build it from the conversation.
- If `requirements.md` already has a `## Tickets` section, those tickets exist. Read them and compare what they deliver with the current requirements; point out any requirement that has changed since its ticket was written. Slice only the work they do not cover, and update that section rather than replacing it.
- Read `docs/fatstack/tracker.md`. If it does not exist, offer to run `fatstack-setup`, or continue with local markdown tickets: one file per ticket at `docs/fatstack/features/<feature-slug>/tickets/<NN>-<slug>.md`, numbered in dependency order after the highest existing number in that folder (from `01` if it is empty), with `Status:` and `Blocked by:` lines at the top.
- Read the glossary, the decision records, and the code the work touches. Look for prefactoring that would make the change easier.
- For existing tickets, read their current milestone assignments and the feature's milestone records in both the tracker and `requirements.md`, including completed tickets and closed milestones. Keep existing assignments by default. If the sources disagree, show the discrepancy and ask which is right before writing. A tracker configuration without milestone operations needs those operations agreed and recorded before milestone writes; offer setup or an explicitly chosen local fallback instead of guessing a tool or migrating tickets silently.

## 2. Slice into tracer bullets

Each ticket is a **tracer bullet**: a thin slice through every layer it needs (data, logic, interface, tests) that works end to end and can be demonstrated or verified on its own. Prefer several thin slices to one thick layer.

- Put any prefactoring in its own ticket first.
- Size each ticket so one agent can finish it in a single fresh session.
- Give each ticket its **blockers**: the tickets that must be done before it can start. A ticket with no blockers can start at once.
- Link each ticket to the requirements it delivers (`R1`, `R2`, ...), and carry the constraints and edge cases that apply to it, with their status. Write applicable edge cases into the acceptance criteria.

The breakdown is complete when every requirement that is not withdrawn is delivered by at least one ticket, and every constraint and edge case is carried by the tickets it applies to, or each is explicitly left for later with the user's agreement.

### Group into milestones

A milestone is an ordered phase with a number, short name, and goal stating what works or can be demonstrated after its tickets are done. Each phase must leave the product working and be independently reviewable and shippable. Put prefactoring in milestone 1 where possible, with a verifiable structural outcome and preserved behaviour. If the whole feature fits in one phase, offer a single milestone or none rather than forcing a grouping.

Number phases within the feature and name new tracker milestones `<feature-slug> M<N>: <name>`. The feature plus phase number identifies the intended phase; native tracker IDs and numbers are separate identities. Record both in `requirements.md`. When a user says "milestone 3" without a feature and several features match, ask which one. Preserve existing names and identities unless a change is approved; new milestones use unused phase numbers in the agreed order.

For every blocker edge within the feature, the blocking ticket must be in the same or an earlier milestone than the blocked ticket. Resolve blockers outside this feature and flag unfinished work outside the proposed batch; phase numbers from different features cannot be compared directly, so obtain an explicit dependency order across those features. If phase order cannot be determined, or a dependency cycle exists, surface it for correction before approval. Same-milestone blockers still require dependency order during implementation.

Place new tickets into existing or new milestones. Propose any moves of existing tickets separately, showing their old and proposed assignments. Retain completed phases and their records. Unassigned existing tickets remain unassigned unless the user approves an assignment; resolve any unassigned blocker that prevents checking phase order before creating the affected work.

## 3. Agree the breakdown

Present the tickets as a numbered list grouped by milestone, with each milestone's number, name, goal, and existing tracker identity if any. If milestones were declined, show a flat list. Each ticket shows:

- **Title**
- **Delivers**: the behaviour that works once it is done
- **Requirements**: the `R` numbers it covers, with their status (confirmed or assumed), and the constraints and edge cases it carries, with their status
- **Blocked by**: ticket numbers, or none

Before asking for approval, list the assumed requirements, constraints, and edge cases, and the open questions, that the tickets depend on, so the user can confirm them or accept the risk. Let the user move tickets and merge, split, rename, or reorder milestones, as well as adjust the tickets and blockers. Re-check every dependency after each change and point out invalid phase order. Show any proposed changes to existing assignments or milestone metadata explicitly. Revise until the user approves both the tickets and milestones, including any moves. Create or update nothing in the tracker until they do.

## 4. Create the tickets

When milestones were declined, skip native milestone creation and assignment, use a flat requirements list, and set new local tickets to `Milestone: none`. Otherwise use the milestone operations in `tracker.md` to find and reuse agreed existing milestones or create approved new ones, with the agreed names and goals. For a local fallback without configuration, use the [local milestone operations](../fatstack-setup/tracker-local.md#milestones). Record each verified milestone identity and goal in `requirements.md` immediately, even before its tickets are created. Never assume a name match is the same milestone: check its feature, scope, and recorded identity; ask about ambiguous matches. Do not reopen closed phases, delete milestones, or change release state as an incidental part of ticket creation.

Create the approved new tickets using the operations in `tracker.md`, or as local markdown files described in step 1 if there is no `tracker.md`, in dependency order so each ticket can reference the real identifiers of its blockers. Record blockers in the tracker's dependency format, set each ticket to `ready`, and assign its approved milestone. Leave existing ticket content and status unchanged; apply only explicitly approved milestone moves to existing tickets. Verify assignments by reading back the tickets or listing the milestone's tickets, including all pages.

As soon as each ticket is created, add its identifier, title, and requirements to a `## Tickets` section in `requirements.md`, under its milestone heading with that milestone's goal and tracker identity. Keep a flat list when milestones were declined and an explicit unassigned group for existing unassigned tickets. Preserve existing entries and completed phases. If any step for a ticket fails after creation, including milestone assignment, mark its entry `(setup incomplete: <what failed>)` with the actual known assignment. On a later run, finish that setup with the user's agreement before treating the ticket as covered. If any tracker call fails or its result is uncertain, stop and report the verified milestones, tickets, assignments, and pending actions. On resumption, read back recorded identities and reconcile uncertain results before retrying so partial success does not create duplicates.

When milestones are used, record them like this under `## Tickets`:

```markdown
### <feature-slug> M1: <name>

Goal: <What works or can be demonstrated.>
Tracker: <milestone ID/link, or this requirements heading for local markdown>

- <ticket ID/link>: <title>; requirements <R numbers and status>
```

Use this body for each ticket:

```markdown
## What to build

<The behaviour this ticket makes work, from the user's point of view.>

## Acceptance criteria

- [ ] <Observable, testable outcome.>

## Requirements

<R numbers with status, e.g. R1 (confirmed), R3 (assumed)>, from [<feature> requirements](<link or path to requirements.md>)

Constraints: <constraints this ticket must respect, with status, or "none">

## Blocked by

<Blocking tickets, or "None">
```

Describe behaviour rather than file paths or code, which go stale.

Finally, list the tickets by milestone with each phase's goal and any incomplete setup. Suggest implementing milestone 1, or the next unfinished phase on a re-run, with `fatstack-chief-of-staff` once available, or `fatstack-implement` on its first unblocked ticket. If milestones were declined, suggest `fatstack-implement` on the first unblocked ticket. Never suggest starting a blocked ticket.
