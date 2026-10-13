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

## Milestones

Use native project milestones in project `<PROJECT name and ID, selected during setup>`. A team alone does not identify a milestone's project. Name milestones `<feature-slug> M<N>: <name>`, store the goal in the description, and record returned IDs and project scope separately from phase numbers. Use the configured connector or CLI only after checking its supported operations.

- **List/reuse**: list all milestones for the selected project, including completed ones when supported, and match recorded IDs and feature names. The Linear connector provides `list_milestones` and `get_milestone`.
- **Create**: use the project's milestone creation operation with project, name, and description. With the Linear connector, `save_milestone` without `id` creates one. Record the returned ID; omit target dates unless the user agreed them.
- **Assign**: update the issue with its project and milestone ID. The connector's `save_issue` accepts `id`, `project`, and `milestone`. Verify the issue belongs to the selected project first; changing an existing issue's project requires explicit approval.
- **List tickets**: list all issues in the selected project, including completed and archived tickets, and filter by their returned project milestone identity. With the connector, request the `projectMilestone` field and follow every cursor; its issue list has no milestone filter. Do not filter to only ready or open issues when reconciling assignments.
- **Approved rename/goal change**: update the existing milestone by recorded ID, preserving the project. With the connector use `save_milestone` with `id`. Approved ticket moves update only their milestone and are read back for verification.

If required operations or archived membership are unavailable, report the limitation and resolve the missing data/tool before treating the plan as reconciled. See [Linear's project milestone guide](https://linear.app/docs/project-milestones).

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
