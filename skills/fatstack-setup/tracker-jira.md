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

## Milestones

Mapping: fix versions in `<KEY>` by default, confirmed during setup. Name each `<feature-slug> M<N>: <name>` and store its goal in the description. Record the version ID and project, independently of the feature's phase number. Use the configured authenticated tool; if it cannot perform these operations, ask for an available tool before writing. Do not invent credentials or silently switch to epics.

- **List/reuse**: get project versions via `GET /rest/api/3/project/<KEY>/versions`, including released and archived versions. Match the recorded version ID and feature; do not silently recreate or reopen a completed version.
- **Create**: resolve the project ID, then `POST /rest/api/3/version` with `project`, `name`, and `description`. Do not supply release dates or change release state unless separately requested. Record the returned ID.
- **Assign**: fetch the issue's `fixVersions`, then add the milestone's version ID through the issue update operation. Preserve unrelated versions. For an approved move, remove only the previously recorded Fatstack phase version and add the new one; read back the field to verify.
- **List tickets**: search JQL `project = <KEY> AND fixVersion = <version-id>`, with all statuses and all result pages.
- **Approved rename/goal change**: `PUT /rest/api/3/version/<id>` with only the changed name/description, preserving the identity and release state.

If the user chooses epics instead, replace these operations during setup with the project's verified create/read epic operations, its editable parent or epic-link field, and a scoped JQL membership query. Preserve unrelated hierarchy and ask before moving an issue out of another parent. A fix-version configuration cannot be used as an epic configuration.

References: [Jira project versions](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-project-versions/) and [issue updates](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-issues/).

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
