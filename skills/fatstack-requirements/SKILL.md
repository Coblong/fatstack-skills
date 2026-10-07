---
name: fatstack-requirements
description: Turn a challenge, from this session or a saved challenge record, into a requirements document of user stories, requirements, constraints, and open questions that agents can build from. Use after fatstack-challenge, or when the user asks to write up requirements for a feature.
---

# Fatstack Requirements

Synthesise what is already known into a requirements document. This is a synthesis, not an interview: gaps become open questions.

## 1. Gather the sources

- Start with the current conversation: a challenge or discussion earlier in this session is the freshest source, including anything said after the challenge record was written.
- Read the feature's challenge record at `docs/fatstack/features/<feature-slug>/challenge.md` to fill gaps. In a new session it is the main source. If the slug is unclear, list the feature folders and ask.
- Where the conversation and the record disagree, show the difference and ask which is right.
- If neither gives enough to work from, suggest running `fatstack-challenge` first.
- If `requirements.md` already exists for the feature, treat it as a source too and update it rather than starting again. It may hold decisions made after the challenge record was written: keep its confirmed requirements unless the user changes them, and ask where it disagrees with the conversation or the challenge record.
- Read the glossary and decision records (see the `## Fatstack` section in `AGENTS.md` or `CLAUDE.md`) and the parts of the code the feature touches.

## 2. Draft

Write the document using the template below. Use glossary terms throughout. Describe behaviour, not code: leave out file paths and code, which go stale.

Carry each point's status across from the conversation, the challenge record, and any existing requirements document. A requirement the user confirmed is `confirmed`; anything you inferred or proposed is `assumed`. Anything unknown goes under open questions.

The draft is complete when every confirmed point, assumption, and edge case from the conversation, the challenge record, and any existing requirements document appears in it, either as a requirement, constraint, or open question, or under out of scope.

## 3. Review, then write

Show the draft and ask the user to correct it. Point out the assumed requirements and open questions so they can settle what they can now. When updating an existing document, show what changed.

Write `docs/fatstack/features/<feature-slug>/requirements.md` once the user is happy, then suggest `fatstack-tickets` as the next step.

## Template

Number requirements `R1`, `R2`, and so on, so tickets can link back to them. Keep numbers stable when updating: add new numbers, and mark removed requirements as withdrawn rather than renumbering.

```markdown
# <Feature name>: requirements

Source: <[challenge record](challenge.md), or "conversation on <date>" if there is no record>
Decisions: <links to relevant decision records, or "none">

## Summary

<The problem, who has it, and the outcome that will show it is solved. Three or four sentences.>

## User stories

1. As a <role>, I want <capability>, so that <benefit>.

## Requirements

- **R1** (confirmed): <Observable behaviour, written so it can be tested.> Stories: 1
- **R2** (assumed): <...> Stories: 1, 2

## Constraints

- <Technical, legal, time, or compatibility limit the solution must respect.> (confirmed | assumed)

## Edge cases

- <Situation>: <expected behaviour>. Requirement: R<n> (confirmed | assumed)

## Out of scope

- <What this work does not include.> (confirmed | assumed)

## Open questions

- <Unknown that must be resolved before or during delivery, and who can answer it.>
```
