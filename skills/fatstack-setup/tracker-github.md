# Tracker: GitHub

Tickets for this project are GitHub issues. Use the `gh` CLI, which infers the repository from `git remote`.

## Operations

- **Create a ticket**: `gh issue create --title "..." --body "..."`. Use a heredoc for multi-line bodies.
- **Read a ticket**: `gh issue view <number> --comments`.
- **List tickets**: `gh issue list --state open --json number,title,labels`.
- **Comment**: `gh issue comment <number> --body "..."`.
- **Change status**: add or remove the status label with `gh issue edit <number> --add-label "..." --remove-label "..."`.
- **Close**: `gh issue close <number> --comment "..."`.

## Dependencies

List blocking tickets in a `## Blocked by` section of the issue body, for example `- #12`.

## Statuses

| Fatstack status | In this tracker |
| --- | --- |
| ready | label `ready` |
| in progress | label `in-progress` |
| in review | label `in-review` (an open pull request links the issue) |
| done | issue closed |

## Pull requests

- Base branch: `main`
- Branch name: `<number>-<slug>`
- Link a pull request to its ticket with `Closes #<number>` in the description.
