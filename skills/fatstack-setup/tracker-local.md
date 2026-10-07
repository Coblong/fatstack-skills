# Tracker: Local markdown

Tickets for this project are markdown files in this repository.

## Operations

- **Create a ticket**: write `docs/fatstack/features/<feature-slug>/tickets/<NN>-<slug>.md`, numbered in dependency order after the highest existing number in that folder (from `01` if it is empty). One ticket per file.
- **Read a ticket**: read the file. Users usually refer to a ticket by path or by feature and number.
- **List tickets**: list the files in `docs/fatstack/features/*/tickets/`.
- **Comment**: append to a `## Comments` section at the end of the file.
- **Change status**: edit the `Status:` line near the top of the file.

## Dependencies

Each ticket has a `Blocked by:` line near the top listing the numbers of the tickets it depends on, or `None`.

## Statuses

| Fatstack status | In this tracker |
| --- | --- |
| ready | `Status: ready` |
| in progress | `Status: in progress` |
| in review | `Status: in review` |
| done | `Status: done` |

## Pull requests

- Base branch: `main`
- Branch name: `<feature-slug>-<NN>-<slug>`
- Mention the ticket path in the pull request description.
