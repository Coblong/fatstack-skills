---
name: fatstack-tickets
description: Break a feature's requirements into tracer-bullet tickets with dependencies, agree the breakdown with the user, then create the tickets in the project's tracker. Use when the user wants requirements turned into tickets, issues, or a delivery plan.
disable-model-invocation: true
---

# Fatstack Tickets

Slice the work into tickets that each deliver something demonstrable, agree them with the user, then create them in the tracker.

## 1. Gather the sources

- Start with the current conversation, then read `docs/fatstack/features/<feature-slug>/requirements.md` to fill gaps. In a new session the requirements document is the main source. Where they disagree, ask which is right. Tickets link back to `requirements.md`, so it must exist before any tickets are created: if it does not, run `fatstack-requirements` first, which can build it from the conversation.
- If `requirements.md` already has a `## Tickets` section, those tickets exist. Read them and compare what they deliver with the current requirements; point out any requirement that has changed since its ticket was written. Slice only the work they do not cover, and update that section rather than replacing it.
- Read `docs/fatstack/tracker.md`. If it does not exist, offer to run `fatstack-setup`, or continue with local markdown tickets: one file per ticket at `docs/fatstack/features/<feature-slug>/tickets/<NN>-<slug>.md`, numbered in dependency order after the highest existing number in that folder (from `01` if it is empty), with `Status:` and `Blocked by:` lines at the top.
- Read the glossary, the decision records, and the code the work touches. Look for prefactoring that would make the change easier.

## 2. Slice into tracer bullets

Each ticket is a **tracer bullet**: a thin slice through every layer it needs (data, logic, interface, tests) that works end to end and can be demonstrated or verified on its own. Prefer several thin slices to one thick layer.

- Put any prefactoring in its own ticket first.
- Size each ticket so one agent can finish it in a single fresh session.
- Give each ticket its **blockers**: the tickets that must be done before it can start. A ticket with no blockers can start at once.
- Link each ticket to the requirements it delivers (`R1`, `R2`, ...), and carry the constraints and edge cases that apply to it, with their status. Write applicable edge cases into the acceptance criteria.

The breakdown is complete when every requirement that is not withdrawn is delivered by at least one ticket, and every constraint and edge case is carried by the tickets it applies to, or each is explicitly left for later with the user's agreement.

## 3. Agree the breakdown

Present the tickets as a numbered list, each showing:

- **Title**
- **Delivers**: the behaviour that works once it is done
- **Requirements**: the `R` numbers it covers, with their status (confirmed or assumed), and the constraints and edge cases it carries, with their status
- **Blocked by**: ticket numbers, or none

Before asking for approval, list the assumed requirements, constraints, and edge cases, and the open questions, that the tickets depend on, so the user can confirm them or accept the risk. Then ask whether the slices are the right size, whether the blockers are right, and whether any tickets should be merged or split. Revise until the user approves. Create nothing until they do.

## 4. Create the tickets

Create the approved tickets using the operations in `tracker.md`, or as local markdown files described in step 1 if there is no `tracker.md`, in dependency order so each ticket can reference the real identifiers of its blockers. Record blockers in the tracker's dependency format and set each ticket to the `ready` status. Leave existing tickets unchanged.

As soon as each ticket is created, add its identifier, title, and requirements to a `## Tickets` section in `requirements.md`, so a failure part-way leaves an accurate record. If any step for a ticket fails after it was created (blockers or status), mark its entry `(setup incomplete: <what failed>)`; on a later run, finish that setup, with the user's agreement, before treating the ticket as covered. If a tracker call fails, stop, tell the user which tickets were created and which were not, and continue only when they say so.

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

Finally, give the user the list of created tickets. Suggest `fatstack-implement` on the first unblocked ticket.
