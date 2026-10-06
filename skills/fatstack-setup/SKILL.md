---
name: fatstack-setup
description: Choose and record the tracker that Fatstack skills use for tickets in this project. Run once before using fatstack-tickets or fatstack-implement, or again to switch tracker.
disable-model-invocation: true
---

# Fatstack Setup

Record which tracker this project uses so the other Fatstack skills know where tickets live and how to work with them. Setup writes instructions for agents, not code.

## 1. Look before asking

Check what already exists:

- `git remote -v`: is the project hosted on GitHub?
- `docs/fatstack/tracker.md`: has setup already run? If so, show the current choice and ask whether to change it.
- `AGENTS.md` or `CLAUDE.md` at the project root, and any existing `## Fatstack` section.
- An existing glossary or decision records (for example `CONTEXT.md`, `docs/adr/`, `docs/decisions/`).
- Which tracker tools are available: the `gh` CLI, a Jira or Linear MCP server, or a Jira or Linear CLI.
- The default branch, and how existing branches and pull requests are named and linked to issues.

## 2. Ask which tracker to use

Ask one question and lead with a recommendation: GitHub if the remote is on GitHub, otherwise local markdown.

- **GitHub**: GitHub Issues through the `gh` CLI.
- **Jira** or **Linear**: through the tracker's MCP server, or its CLI if no MCP server is available. Ask for the project key or team.
- **Local markdown**: one file per ticket in this repository.
- **Other**: ask the user to describe in a short paragraph how an agent should create, read, and update tickets.

If the chosen tracker has no available tool, say so and offer local markdown until it is connected.

For any external tracker, ask for its names for the statuses ready, in progress, in review, and done. Propose pull request conventions from what the repository already does, defaulting to branches named `<ticket-id>-<slug>` off the default branch.

## 3. Check the connection

For an external tracker, make one read-only call, such as viewing the project or listing a few open tickets. If the call fails, report the error and let the user fix access or choose local markdown.

Do not create or change anything in the tracker, with one exception: if statuses are tracked with labels that do not exist yet (for example GitHub's `ready`, `in-progress`, and `in-review`), list the missing labels and create them only if the user agrees.

## 4. Confirm, then write

Show the user a draft of each file, let them correct it, then write:

- `docs/fatstack/tracker.md`, starting from the matching template: [GitHub](tracker-github.md), [Jira](tracker-jira.md), [Linear](tracker-linear.md), or [local markdown](tracker-local.md). For other trackers, write it from the user's description using the same headings. Do not record credentials or tokens.
- A `## Fatstack` section in the root `AGENTS.md`, or in `CLAUDE.md` if that is the file the project uses. Never create one when the other exists; if neither exists, ask which to create. Update an existing `## Fatstack` section in place rather than adding a second.

```markdown
## Fatstack

- Tracker: <one-line summary>. See `docs/fatstack/tracker.md`.
- Glossary: `<path>`. Decision records: `<directory>`.
- Feature work: `docs/fatstack/features/<feature-slug>/`.
```

Use the project's existing glossary and decision record locations if there are any. Otherwise use `docs/fatstack/glossary.md` and `docs/fatstack/decisions/`. Do not create those files now; `fatstack-context` creates them when there is something to record.

## 5. Finish

Tell the user what was written and that they can edit `docs/fatstack/tracker.md` directly. Re-running setup is only needed to switch tracker.
