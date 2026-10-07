---
name: fatstack-challenge
description: Interview the user about a feature or idea to draw out information, constraints, and edge cases until you share an understanding of the work, then record it. Use when the user wants an idea challenged, grilled, stress-tested, or clarified before writing requirements.
---

# Fatstack Challenge

Question the user about a feature until you both understand the work in the same way. The result is a challenge record that `fatstack-requirements` turns into a requirements document.

## Before the first question

- Agree a short feature slug with the user, for example `saved-baskets`.
- Start from what the conversation has already established: treat it as answered, and skip questions it already settles.
- If `docs/fatstack/features/<feature-slug>/challenge.md` exists, read it and continue from its open questions rather than starting again. Where it disagrees with the conversation, ask which is right.
- Read the glossary and decision records (see the `## Fatstack` section in `AGENTS.md` or `CLAUDE.md`) and the parts of the code the feature touches.

## Ask in rounds

Each round, ask up to five numbered questions. Ask only questions you can ask now: leave questions that depend on an unanswered one for a later round. Give your recommended answer with each so the user can accept it quickly.

```markdown
**1. <Short title>**
<The question, with options if there are any.>
Recommended: <your answer and why, in a sentence>
```

Wait for the answers before the next round. Fewer, sharper questions beat a long list.

Find facts yourself. If the code, docs, or tools can answer something, look it up instead of asking. Put decisions to the user.

## Cover the ground

Work through what matters for this feature, skipping what does not apply:

- Who it is for and the problem it solves.
- The outcome, and how you will know it worked.
- What is in and out of scope.
- The main flow, step by step.
- Constraints: technical, legal, time, compatibility.
- Edge cases and failures: empty, duplicate, concurrent, partial, permission denied, external service down.
- Data and integrations it reads or changes.
- Risks and unknowns.

While talking, apply `fatstack-context`: challenge vague or conflicting terms, test ideas with concrete examples, and check claims against the code. Keep a list of proposed glossary terms and decisions.

Track each point as **confirmed** (the user said so), **assumed** (you proposed it and the user has not confirmed it), or **open** (still unknown). Never present an assumption as confirmed.

## Know when to stop

Stop when the remaining unknowns would not change what gets built, or when the user wants to stop. Unresolved points become open questions; they do not block the record.

Summarise the shared understanding in a few lines and ask the user to confirm or correct it. Do not write anything until they confirm.

## Write the record

Write `docs/fatstack/features/<feature-slug>/challenge.md`:

```markdown
# <Feature name>: challenge

## Idea

<The idea in one or two sentences, in the user's terms.>

## Shared understanding

<A short summary of what will be built and why.>

## Confirmed

- <Decision or fact the user confirmed.>

## Assumptions

- <Point the user has not confirmed.>

## Open questions

- <Unknown that requirements or delivery must resolve.>

## Edge cases

- <Situation>: <expected behaviour, or "unknown">. (confirmed | assumed | open)
```

Give every edge case a status, using the same meanings as above.

Then show the proposed glossary terms and decisions, and use `fatstack-context` to write the ones the user agrees to. Suggest `fatstack-requirements` as the next step.
