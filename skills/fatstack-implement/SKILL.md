---
name: fatstack-implement
description: Implement a specified ticket test-first, run the project's full checks, and publish a linked ready pull request. Use when the user asks to implement a ticket or issue, or a coordinator delegates its implementation.
disable-model-invocation: false
---

# Fatstack Implement

Take one requested ticket from ready through implementation to in review. This is the default full implementation workflow, whether invoked explicitly or selected by the agent for an implementation request. Use the same checks for any later wrap-up of lighter work.

An implementation request authorizes the ticket's routine ready → in progress → in review transitions, the agent's choice of test seams, commits of the ticket's changes, pushes to its branch, and creation or updating of its linked ready PR. Announce progress and proceed without separate approval for those steps. Honor explicit user limits such as local-only work or no publishing. Ask for missing or conflicting information that affects the work, not permission for routine implementation or publication. Automatic skill selection alone does not authorize work the user has not requested. Merges, deployments, unrelated ticket changes, and destructive operations are outside this authorization; project and tool restrictions still apply.

Required: access to the project checkout, its ticket and linked requirements, Git, [fatstack-tdd](../fatstack-tdd/SKILL.md), the tracker tool recorded in `docs/fatstack/tracker.md`, and a tool for the repository's PR host. Read the linked TDD instructions if the agent cannot invoke that skill directly. Use the project's configured tools rather than assuming a particular tracker or language. If tracker configuration omits the PR tool, identify the repository's host from its Git remote and select an available CLI or connector that supports creating and reading PRs there. If the remote or tool choice is ambiguous, or no suitable tool is available, ask the user before publishing; tracker access alone does not establish PR access.

## 1. Read before planning

Resolve the ticket reference in the project's tracker and state its identifier and title. Inspect the working tree and read the applicable `AGENTS.md` and `CLAUDE.md`, `docs/fatstack/tracker.md`, the ticket, its linked requirements, and its blockers. Read the glossary and relevant decision records at the locations recorded in the project's Fatstack section. Build on the current conversation; if a saved source contradicts it, ask which is right before implementing the affected behaviour.

If tracker configuration is absent, offer `fatstack-setup` or use an existing local markdown ticket supplied by the user. Resolve the repository's branch and PR conventions before publishing. Ask for missing ticket or requirements content rather than inventing it. If a blocker is unfinished or the ticket is not ready, report the actual state and ask how to proceed. On a resumed run, inspect the existing branch, PR, and status first so completed steps are not repeated.

Before writing code, identify the acceptance criteria, the next vertical slice, the selected test seams, and all required checks from `AGENTS.md` / `CLAUDE.md`, following any check-document pointers there. Record an executable command for each required check, including the full test suite. If no checks are documented, or any required check lacks a documented command, ask the user for the missing commands before implementing. Do not silently substitute familiar lint, test, build, or smoke commands. Record this check list for the final report.

## 2. Start the ticket

Before changing status or editing project files, check for an existing ticket branch locally and on the remote, and any linked PR, using the configured naming and linking conventions. For local markdown trackers, read the ticket on that branch too: the base branch may still say ready while the ticket branch records work in progress. If another agent owns ongoing work, report it and ask how to proceed rather than starting a competing implementation. An existing branch is evidence of work, not an exclusive lock.

Enter the ticket's branch using the configured base branch and naming convention. Reuse it on a resumed run, or create it for new work. Preserve unrelated work in its original checkout; use a separate worktree when switching would carry unrelated edits or overwrite existing work. Verify the active branch and working tree before proceeding.

Announce the ready → in progress transition, apply it through the operations in `tracker.md`, and verify the result. For a local markdown tracker, edit the ticket's `Status:` line only in the isolated ticket checkout, run affected checks, then commit and push the status change and verify it on the remote ticket branch. If the user explicitly requested local-only work, retain the status edit with the working changes and report that it is local only. On resumption, inspect local and remote state before retrying so a pending commit or push does not repeat the transition. Report actual tool or transition failures and continue only work that does not depend on their success.

Carry out scoped commits, branch pushes, and ready PR creation as part of implementation without a publication-approval checkpoint. Follow the user's explicit publishing limits and project rules; report actual access or tool blockers without presenting routine publication as awaiting user permission.

## 3. Implement the ticket

Use `fatstack-tdd` for every behaviour slice. Choose test seams through that skill, respecting boundaries already specified by the ticket, requirements, or user, and proceed without a confirmation gate. Write one test, observe it fail for the intended reason, then implement enough to pass and run the related tests. Repeat until every acceptance criterion is satisfied. Keep requirement and decision links intact and carry unresolved questions forward instead of choosing answers silently.

For a standalone prefactoring ticket created to unblock later behaviour tickets, use a behaviour-preserving path instead of requiring a failing test for a new behaviour. Establish the ticket's scope and select test seams, then run existing tests at those seams before changing the implementation. If coverage is missing, add characterization tests for the existing behaviour and confirm they pass before prefactoring. Make small structural changes, rerun those tests after each change, and verify the ticket's acceptance criteria and full project checks in step 4. If a baseline test fails, resolve or report that failure before changing structure. Keep any new behaviour out of this ticket; use `fatstack-tdd` for a separately agreed behaviour change.

If the TDD instructions or necessary tools are unavailable, report the missing dependency and pause the affected work. Defer optional refactoring during behaviour implementation to the review workflow described by `fatstack-tdd`; explicit prefactoring tickets follow the path above.

## 4. Verify the complete change

Run every check identified in step 1 against the completed change, including the full test suite and any documented lint, type, build, or smoke checks. Check the diff for unrelated work and compare the implemented behaviour with every acceptance criterion. Record each check's command and result, including anything that could not run.

Fix failures within this ticket's scope and rerun affected checks. Report unrelated failures or missing prerequisites to the user. Failed or unrun required checks prevent completion and PR creation, including when the failure predates this ticket. If changes follow verification, rerun the checks they affect before publishing. Leave the ticket in progress and report what remains when verification is blocked.

## 5. Hand off for review

Once all required checks pass, prepare a PR title and description following `tracker.md` and project conventions. Link the ticket and requirements, explain the resulting behaviour, and include validation results and any remaining questions. Commit only this ticket's changes, push its branch, and open or update the linked PR against the intended base without asking for publication approval. Create PRs in ready state, never as drafts. If the user explicitly requested no publishing, report the local result instead and respect that limit.

Verify the PR exists and links the ticket before moving the ticket to the configured in review status; verify that transition too. For local markdown trackers, change its `Status:` line on the PR branch, run affected checks, then commit and push that follow-up without a separate approval prompt. Verify that the remote PR branch contains the in review status before reporting hand-off complete. On a resumed run, inspect both the local ticket and remote branch and finish any pending commit or push rather than repeating the transition. Update the existing PR rather than creating another. If publication or a transition fails, report the last verified state and resume from there after the problem is resolved. Do not close the ticket or mark it done at hand-off. When addressing PR comments, reply to each addressed comment and resolve it after verifying the fix.

## Delegated runs and finish

A sub-agent given only a ticket reference must locate the same project instructions, tracker configuration, ticket, and requirements before planning. A coordinator dispatching the user's requested implementation carries the same permission for routine status updates, test-seam selection, scoped commits, branch pushes, and ready PR creation; none requires another approval from the user or coordinator. Carry forward any explicit user limits. If required information or tool access is missing and the sub-agent cannot ask the user, complete independent work and return the concrete blockers and questions to the coordinator. Do not skip checks or broaden the requested scope to finish the assignment.

End with a short report: ticket, branch, PR link or pending PR action, verified ticket status, check results, and open questions or blockers. Distinguish locally verified implementation from a completed hand-off. Avoid a transcript of the work.
