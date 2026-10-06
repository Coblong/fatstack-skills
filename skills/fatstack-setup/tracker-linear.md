# Tracker: Linear

Tickets for this project are Linear issues in team `<TEAM>`. Use the Linear MCP server's tools; if none is available, use the Linear CLI recorded here: `<cli or "none">`.

## Operations

- **Create a ticket**: create an issue in `<TEAM>` with a title and description.
- **Read a ticket**: fetch the issue by identifier, including comments.
- **List tickets**: list open issues for `<TEAM>`.
- **Comment**: add a comment to the issue.
- **Change status**: set the issue's workflow state to the one in the table below.

## Dependencies

Add a "blocked by" relation from each issue to the issues that block it.

## Statuses

| Fatstack status | In this tracker |
| --- | --- |
| ready | `<Todo>` |
| in progress | `<In Progress>` |
| in review | `<In Review>` |
| done | `<Done>` |

## Pull requests

- Base branch: `main`
- Branch name: `<TEAM-123>-<slug>`
- Put the issue identifier in the pull request title or branch name so Linear links them.
