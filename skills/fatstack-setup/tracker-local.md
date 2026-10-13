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

## Milestones

The feature's `requirements.md` is the milestone registry. Under `## Tickets`, give each phase a heading `<feature-slug> M<N>: <name>`, its goal, and its ticket links. That heading and file path identify the milestone; numbers are scoped to the feature, not the entire repository.

- **Create**: after approval, add the milestone heading and goal to `docs/fatstack/features/<feature-slug>/requirements.md`. Reuse existing headings and retain completed phases instead of creating duplicate records.
- **Assign**: add or update `Milestone: <feature-slug> M<N>` near the top of a ticket alongside `Status:` and `Blocked by:`. Use `Milestone: none` when milestones were declined. Preserve all other ticket fields. Older tickets without a line are unassigned until the user approves an assignment.
- **List tickets**: read every ticket file for the feature and select the exact `Milestone:` value, with no status filter. Reconcile that membership with the requirements headings; report disagreements rather than silently choosing one.
- **Approved moves/metadata changes**: update the affected ticket lines and requirements groups together. Preserve phase identities when renaming; update references consistently if an approved reorder changes numbers. Keep records of completed work and never delete ticket files as part of regrouping.

With an existing ticket branch, inspect its ticket status and milestone fields too before overwriting local records; if checkouts disagree, ask which state is authoritative. These file edits are local tracker writes and require approval just like external assignments.

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
