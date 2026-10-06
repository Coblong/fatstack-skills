# Tracker: Jira

Tickets for this project are Jira issues in project `<KEY>`. Use the Jira MCP server's tools; if none is available, use the Jira CLI recorded here: `<cli or "none">`.

## Operations

- **Create a ticket**: create an issue of type `<Story/Task>` in `<KEY>` with a summary and description.
- **Read a ticket**: fetch the issue by key, including comments.
- **List tickets**: search with JQL, for example `project = <KEY> AND statusCategory != Done`.
- **Comment**: add a comment to the issue.
- **Change status**: transition the issue to the status in the table below.

## Dependencies

Add an issue link of type "blocks" from each blocking issue to the issue it blocks.

## Statuses

| Fatstack status | In this tracker |
| --- | --- |
| ready | `<To Do>` |
| in progress | `<In Progress>` |
| in review | `<In Review>` |
| done | `<Done>` |

## Pull requests

- Base branch: `main`
- Branch name: `<KEY-123>-<slug>`
- Put the issue key in the pull request title so Jira links them.
