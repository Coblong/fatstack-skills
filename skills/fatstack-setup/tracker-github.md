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

## Milestones

Use native repository milestones. Phase numbers in `<feature-slug> M<N>: <name>` are not GitHub milestone numbers; record the returned number and URL in `requirements.md`. Store the phase goal in the description. All writes require approval of the milestone plan or explicit approved changes.

- **List/reuse**: `gh api --paginate 'repos/{owner}/{repo}/milestones?state=all'`. Match the recorded identity and feature, including closed milestones; do not recreate a closed milestone silently.
- **Create**: `gh api --method POST 'repos/{owner}/{repo}/milestones' -f title='<feature-slug> M<N>: <name>' -f description='<goal>'`. Record the returned `number` and `html_url`.
- **Assign**: `gh issue edit <issue-number> --milestone '<exact-title>'`, or set the numeric `milestone` field through the issue API when titles are ambiguous. Creating an issue can use `--milestone` too.
- **List tickets**: `gh api --paginate 'repos/{owner}/{repo}/issues?milestone=<native-number>&state=all'`; exclude objects containing `pull_request` and retain both open and closed issues. This avoids truncating a milestone to the CLI's default limit.
- **Approved rename/goal change**: PATCH the recorded milestone endpoint with only the changed title/description. Preserve the native number and release-independent state. Reassignment updates only the approved issues, then reads back their milestones.

Use structured arguments or body files for user-authored text. See [GitHub's milestone API](https://docs.github.com/en/rest/issues/milestones) and [issue API](https://docs.github.com/en/rest/issues/issues).

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
