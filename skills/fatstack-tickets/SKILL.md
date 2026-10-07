---
name: fatstack-tickets
description: Break a feature's requirements into tracer-bullet tickets with dependencies, agree the breakdown with the user, then create the tickets in the project's tracker. Use when the user wants requirements turned into tickets, issues, or a delivery plan.
disable-model-invocation: true
---

# Fatstack Tickets

Slice the work into tickets that each deliver something demonstrable, agree them with the user, then create them in the tracker.

## 1. Gather the sources

- Start with the current conversation, then read `docs/fatstack/features/<feature-slug>/requirements.md` to fill gaps. In a new session the requirements document is the main source. Where they disagree, ask which is right. If there are no requirements to work from, suggest `fatstack-requirements` first.
- If `requirements.md` already has a `## Tickets` section, those tickets exist: slice only the work they do not cover, and update that section rather than replacing it.
- Read `docs/fatstack/tracker.md`. If it does not exist, offer to run `fatstack-setup`, or continue with local markdown tickets: one file per ticket at `docs/fatstack/features/<feature-slug>/tickets/<NN>-<slug>.md`, numbered from `01` in dependency order, with `Status:` and `Blocked by:` lines at the top.
- Read the glossary, the decision records, and the code the work touches. Look for prefactoring that would make the change easier.

## 2. Slice into tracer bullets

Each ticket is a **tracer bullet**: a thin slice through every layer it needs (data, logic, interface, tests) that works end to end and can be demonstrated or verified on its own. Prefer several thin slices to one thick layer.

- Put any prefactoring in its own ticket first.
- Size each ticket so one agent can finish it in a single fresh session.
- Give each ticket its **blockers**: the tickets that must be done before it can start. A ticket with no blockers can start at once.
- Link each ticket to the requirements it delivers (`R1`, `R2`, ...).

The breakdown is complete when every requirement that is not withdrawn is delivered by at least one ticket, or is explicitly left for later with the user's agreement.

## 3. Agree the breakdown

Present the tickets as a numbered list, each showing:

- **Title**
- **Delivers**: the behaviour that works once it is done
- **Requirements**: the `R` numbers it covers
- **Blocked by**: ticket numbers, or none

Then ask whether the slices are the right size, whether the blockers are right, and whether any tickets should be merged or split. Revise until the user approves. Create nothing until they do.

## 4. Create the tickets

Create the approved tickets using the operations in `tracker.md`, in dependency order so each ticket can reference the real identifiers of its blockers. Record blockers in the tracker's dependency format and set each ticket to the `ready` status. Leave existing tickets unchanged.

Use this body for each ticket:

```markdown
## What to build

<The behaviour this ticket makes work, from the user's point of view.>

## Acceptance criteria

- [ ] <Observable, testable outcome.>

## Requirements

<R numbers>, from [<feature> requirements](<link or path to requirements.md>)

## Blocked by

<Blocking tickets, or "None">
```

Describe behaviour rather than file paths or code, which go stale.

Finally, add a `## Tickets` section to `requirements.md` listing each ticket's identifier, title, and requirements, and give the user the list of created tickets. Suggest `fatstack-implement` on the first unblocked ticket.
