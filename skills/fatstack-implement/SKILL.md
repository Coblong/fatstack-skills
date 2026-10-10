---
name: fatstack-implement
description: Implement a specified ticket test-first, run the project's full checks, and manage ticket status changes and a linked pull request.
disable-model-invocation: true
---

# Fatstack Implement

Take one ticket from ready through implementation to in review. This is the default full implementation workflow when explicitly invoked. Use the same checks for any later wrap-up of lighter work.

Required: access to the project checkout, its ticket and linked requirements, Git, [fatstack-tdd](../fatstack-tdd/SKILL.md), the tracker tool recorded in `docs/fatstack/tracker.md`, and a tool for the repository's PR host. Read the linked TDD instructions if the agent cannot invoke that skill directly. Use the project's configured tools rather than assuming a particular tracker or language. If tracker configuration omits the PR tool, identify the repository's host from its Git remote and select an available CLI or connector that supports creating and reading PRs there. If the remote or tool choice is ambiguous, or no suitable tool is available, ask the user before publishing; tracker access alone does not establish PR access.

## 1. Read before planning

Resolve the ticket reference in the project's tracker and state its identifier and title. Inspect the working tree and read the applicable `AGENTS.md` and `CLAUDE.md`, `docs/fatstack/tracker.md`, the ticket, its linked requirements, and its blockers. Read the glossary and relevant decision records at the locations recorded in the project's Fatstack section. Build on the current conversation; if a saved source contradicts it, ask which is right before implementing the affected behaviour.

If tracker configuration is absent, offer `fatstack-setup` or use an existing local markdown ticket supplied by the user. For local tickets, update their `Status:` line only with authorization. Resolve the repository's branch and PR conventions before publishing. Ask for missing ticket or requirements content rather than inventing it. If a blocker is unfinished or the ticket is not ready, report the actual state and ask how to proceed. On a resumed run, inspect the existing branch, PR, and status first so completed steps are not repeated.

Before writing code, identify the acceptance criteria, the next vertical slice, the agreed test seams, and all required checks from `AGENTS.md` / `CLAUDE.md`, following any check-document pointers there. Record an executable command for each required check, including the full test suite. If no checks are documented, or any required check lacks a documented command, ask the user for the missing commands before implementing. Do not silently substitute familiar lint, test, build, or smoke commands. Record this check list for the final report.

## 2. Start the ticket

Before changing status or editing project files, check for an existing ticket branch locally and on the remote, and any linked PR, using the configured naming and linking conventions. For local markdown trackers, read the ticket on that branch too: the base branch may still say ready while the ticket branch records work in progress. If another agent owns ongoing work, report it and ask how to proceed rather than starting a competing implementation. An existing branch is evidence of work, not an exclusive lock.

Enter the ticket's branch using the configured base branch and naming convention. Reuse it on a resumed run, or create it for new work. Preserve unrelated work in its original checkout; use a separate worktree when switching would carry unrelated edits or overwrite existing work. Verify the active branch and working tree before proceeding.

Present the intended ready → in progress transition and ask before changing status unless the user already explicitly authorized that transition. For a local markdown tracker, include the status-only commit and push in this approval, reusing any explicit authorization already given. Edit the ticket's `Status:` line only in the isolated ticket checkout, run checks affected by that edit, then commit only the ticket status change and push the ticket branch. Verify that the remote branch contains the in progress status before implementation proceeds. If authorization, remote access, checks, commit, or push is blocked, report the last verified state and return the pending action; on resumption, finish any pending commit or push without repeating the edit. For external trackers, apply the transition through the operations in `tracker.md` and verify the result. Report a transition failure before proceeding with any dependent tracker action.

Authorization for implementation does not by itself authorize status changes, commits, pushes, or a PR. Track which actions the user has authorized and ask only for those still missing, when their concrete result is ready for review.

## 3. Implement test-first

Use `fatstack-tdd` for every behaviour slice. Confirm test seams through that skill unless the ticket, requirements, or conversation already agreed them. Write one test, observe it fail for the intended reason, then implement enough to pass and run the related tests. Repeat until every acceptance criterion is satisfied. Keep requirement and decision links intact and carry unresolved questions forward instead of choosing answers silently.

If the TDD instructions or necessary tools are unavailable, report the missing dependency and pause the affected work. Keep refactoring in the review workflow described by `fatstack-tdd`.

## 4. Verify the complete change

Run every check identified in step 1 against the completed change, including the full test suite and any documented lint, type, build, or smoke checks. Check the diff for unrelated work and compare the implemented behaviour with every acceptance criterion. Record each check's command and result, including anything that could not run.

Fix failures within this ticket's scope and rerun affected checks. Report unrelated failures or missing prerequisites to the user. Failed or unrun required checks prevent completion and PR creation, including when the failure predates this ticket. If changes follow verification, rerun the checks they affect before publishing. Leave the ticket in progress and report what remains when verification is blocked.

## 5. Hand off for review

Once all required checks pass, prepare a PR title and description following `tracker.md` and project conventions. Link the ticket and requirements, explain the resulting behaviour, and include validation results and any remaining questions. Present the branch, intended base, and PR text for review, then ask for any missing authorization to commit, push, open a ready PR, and move the ticket to in review. Reuse explicit authorization already given for these specific actions. Create PRs in ready state, never as drafts.

Publish only this ticket's changes. Verify the PR exists and links the ticket before moving the ticket to the configured in review status; verify that transition too. For local markdown trackers, the transition edits a tracked ticket file: with authorization for that edit, commit, and push, change its `Status:` line on the PR branch, run the checks affected by the edit, then commit and push the ticket change. Verify that the remote PR branch contains the in review status before reporting hand-off complete. If any of those actions lacks authorization, report the pending action instead. On a resumed run, inspect both the local ticket and remote branch and finish any pending commit or push rather than repeating the transition. Update the existing PR rather than creating another. If publication or a transition fails, report the last verified state and resume from there after the problem is resolved. Do not close the ticket or mark it done at hand-off. When addressing PR comments, reply to each addressed comment and resolve it after verifying the fix.

## Delegated runs and finish

A sub-agent given only a ticket reference must locate the same project instructions, tracker configuration, ticket, and requirements before planning. A coordinator's dispatch alone is not user approval for status changes or publishing. Carry forward explicit user authorization included in the delegated context. If approval or information is missing and the sub-agent cannot ask the user, complete independent authorized work and return the concrete pending actions and questions to the coordinator. Do not invent approval or skip checks to finish the assignment.

End with a short report: ticket, branch, PR link or pending PR action, verified ticket status, check results, and open questions or blockers. Distinguish locally verified implementation from a completed hand-off. Avoid a transcript of the work.
