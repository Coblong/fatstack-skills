---
name: fatstack-context
description: Keep the project's glossary of domain terms and its decision records accurate. Use when pinning down what a term means, when a conversation uses vague or conflicting language, when recording a hard-to-reverse decision, or when another Fatstack skill needs to update the glossary or decisions.
---

# Fatstack Context

Help the user use one precise word for each domain concept, and record the decisions that a future reader would otherwise question.

## Find the files

Read the `## Fatstack` section of the project's `AGENTS.md` or `CLAUDE.md` for the glossary path and decision records directory. If there is no such section, look for an existing glossary (for example `CONTEXT.md`) or decision records (for example `docs/adr/`). Otherwise use `docs/fatstack/glossary.md` and `docs/fatstack/decisions/`.

Read the glossary and any decisions related to the topic before going further. If the files do not exist yet, carry on; create them only when there is something agreed to write.

## While talking

- **Spot conflicts.** When the user uses a term differently from the glossary, say so at once and ask which meaning is right.
- **Sharpen vague words.** When one word could mean several things, offer a precise term and ask the user to choose.
- **Test with examples.** Describe a concrete situation at the edge of a concept and ask how it should behave.
- **Check the code.** When the user describes how something works, check whether the code agrees and point out any difference.

Keep a running list of proposed glossary changes and decisions. Do not edit files mid-conversation.

## Write after agreement

At a natural pause, or at the end, show the proposed changes and ask the user to confirm or correct them. Then write only what they agreed.

### Glossary entries

Group entries under headings when clusters emerge. Keep definitions to one or two sentences about what the thing is. List rejected synonyms so others avoid them.

```markdown
**Ticket**: A delivery-ready, demonstrable unit of work derived from a requirements document.
_Not_: task, story, issue
```

Include only terms specific to this project. Leave out general programming concepts and implementation details.

### Decision records

Propose a decision record only when the decision is hard to reverse, would surprise someone reading the code later, and came from a real choice between alternatives. If any of these is missing, do not record it.

Name the file `NNNN-<slug>.md`, numbered one above the highest existing record. Keep it short:

```markdown
# <Decision title>

Status: accepted
Source: <link to the challenge, requirements document, or ticket where it was decided>

<What the situation was, what was decided, and why. A few sentences.>
```

Add the alternatives considered only if a future reader is likely to suggest one again. When a new decision replaces an old one, set the old record's status to `superseded by NNNN` rather than deleting it.
