# Working on Fatstack Skills

## Purpose and scope

Build reusable agent skills that take work from an idea to reviewed, delivered code: challenge ideas, record requirements, shape work into tickets, implement tickets test-first, and review the result.

```text
idea → challenge → requirements → tickets → implementation → review
```

This is an independent skills repository. Do not introduce the Fat Stack application's Django runtime, database, or deployment infrastructure here. Add executable helpers only when a skill has a concrete need for them.

Use Matt Pocock's skills as a capability reference. Author original instructions and resources. Do not copy third-party prompts or imply affiliation, endorsement, or a dependency on his work. When building a skill with an equivalent in his collection, review his version for capability gaps first, then write ours independently.

## Domain language

- An **idea** is an initial desired outcome, problem, or change.
- A **challenge** is the guided interview that clarifies an idea, drawing out constraints, edge cases, and open questions. The Fat Stack application calls this a discovery conversation. It produces a **challenge record**.
- A **requirements document** records the work to be done: requirements, constraints, user stories, decisions, and open questions. It is the source of truth that tickets link back to. The Fat Stack application calls this a specification.
- A **ticket** is a delivery-ready, demonstrable unit of work derived from a requirements document, with explicit dependencies on other tickets.
- The **tracker** is where tickets live: an external tool such as GitHub, Jira, or Linear, or local markdown files.
- **Export/sync** is a user-approved transfer of tickets to an external tracker.
- The **glossary** defines the project's domain terms. A **decision record** captures a hard-to-reverse decision and why it was made.

Keep generated material reviewable and editable. Distinguish confirmed decisions from assumptions and open questions, and preserve links to source requirements and decisions.

## Skill set

All skills use the `fatstack-` prefix.

| Skill | Purpose |
| --- | --- |
| `fatstack-setup` | Choose and record the tracker, its statuses, and pull request conventions. |
| `fatstack-context` | Maintain the glossary and decision records. Used directly and by other skills. |
| `fatstack-challenge` | Interview the user about a feature until there is a shared understanding. |
| `fatstack-requirements` | Turn a challenge record into a requirements document. |
| `fatstack-tickets` | Break requirements into demonstrable tickets, agree them with the user, then create them in the tracker. |
| `fatstack-tdd` | Build behaviour test-first in red-green cycles at agreed seams. Refactoring happens in review. |
| `fatstack-implement` | Implement a ticket using `fatstack-tdd`, managing ticket transitions and the pull request. |
| `fatstack-review` | Review work against its ticket, requirements, and the project's standards. |

Skills that need a tracker read the configuration written by `fatstack-setup`. If none exists, they offer to run setup or fall back to local markdown tickets.

## Project file layout

Skills write to these locations in the user's project. Prefer locations the project already uses for a glossary or decision records, and record them during setup.

```text
docs/fatstack/
├── tracker.md                    # fatstack-setup: tracker, statuses, PR conventions
├── glossary.md                   # fatstack-context (default location)
├── decisions/NNNN-<slug>.md      # fatstack-context (default location)
└── features/<feature-slug>/
    ├── challenge.md              # fatstack-challenge
    ├── requirements.md           # fatstack-requirements
    └── tickets/NN-<slug>.md      # fatstack-tickets, local markdown tracker only
```

`fatstack-setup` adds a short `## Fatstack` section to the project's existing `AGENTS.md` or `CLAUDE.md` pointing at these files, so any agent can find them.

## Session first, files second

Users often run the skills one after another in a single session, so the conversation holds context the files may not.

- Treat the current conversation as the primary source. Use everything relevant already said.
- Read the saved files too when they exist. They fill gaps, and in a new session they carry the work forward.
- When the conversation and a file disagree, show the difference and ask the user which is right before writing. The conversation is usually newer.
- Write files so the work survives the session; they record the conversation, never replace it.

## Skill authoring

- Put each skill in `skills/<skill-name>/SKILL.md`.
- Prefix every skill name with `fatstack-`. Use lowercase letters, digits, and hyphens. Match the directory name to the frontmatter name.
- Include YAML frontmatter with `name` and `description`. Describe the task and when the skill should apply.
- For skills that should only run when the user asks, such as setup and skills with external side effects, set `disable-model-invocation: true` in the frontmatter for Claude Code and add `agents/openai.yaml` with `policy.allow_implicit_invocation: false` for Codex.
- Keep instructions focused on decisions and procedures that improve the task. Avoid generic advice and repeated policies.
- Keep skills usable independently. State required tools and dependencies instead of assuming they exist.
- Follow [Session first, files second](#session-first-files-second): build on the current conversation, and read and write the shared project files so a skill also works in a new session.
- Link supporting references from the instructions and explain when to read them.
- Add scripts only for concrete automation needs and validate their behaviour.
- Preserve user intent, project conventions, and existing authorization. A skill must not grant itself permission for external actions.
- Require explicit user intent before creating, modifying, or exporting tickets, changing ticket status, or opening pull requests.
- Never include credentials, private conversations, or private tracker data in examples or fixtures.

## Validation and documentation

Check frontmatter, relative links, resources, and installation discovery for new skills. Test realistic requests, including incomplete information and relevant failure cases. Judge observable behaviour rather than matching exact prose.

Run relevant checks once tooling exists. Report what was checked and any gaps. Keep README status, installation instructions, and the published skill list accurate. Do not claim agent compatibility without testing it.

## Change hygiene

Inspect the working tree before editing and preserve unrelated work. Keep changes focused. Do not commit, push, publish an NPM package, or create external tickets unless explicitly asked.

Create pull requests in ready state, never as drafts. After addressing pull request comments, reply to each addressed comment and resolve it.
