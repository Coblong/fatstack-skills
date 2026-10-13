---
name: fatstack-vibe
description: Enter a "let's vibe" session for small changes with focused checks, then run the project's full checks when wrapping up.
disable-model-invocation: true
---

# Fatstack Vibe

Enter this mode only when the user invokes the skill. Acknowledge that vibe mode is active and that full verification is pending until wrap-up. The mode applies to small requested changes in this session, not to unrelated chats or future sessions.

## While vibe mode is active

Read the applicable `AGENTS.md` / `CLAUDE.md` and inspect the working tree before editing. Preserve unrelated work. Make the user's requested changes without adding an unrequested planning or ticket workflow. Run focused checks when they help verify the particular change; defer the full suite until exit instead of running it after every change. Repository requirements still take precedence, including any mandatory checks for each change.

Keep a compact account in the conversation of the changes, focused checks and results, failures, and full checks still pending. Describe what changed and what was checked without calling the whole work complete before wrap-up. Fix failures within the requested scope; report unrelated failures and missing prerequisites rather than silently widening the task.

Vibe mode changes the frequency of checks, not safety, authorization, destructive-action, PR, or repository rules. Invocation does not authorize commits, pushes, tracker changes, external messages, or a PR. Preserve explicit authorization already given for those actions and ask only for what is missing when the proposed action is ready for review.

## Exit and verify

Treat "let's wrap it up", "wrap it up", or "back to normal" as exit requests when the user directs them at the session. Exit vibe mode immediately and return to the normal workflow; full verification remains pending until the following steps succeed. Exit does not itself authorize publishing.

Read the check-discovery guidance in step 1 and the verification rules in step 4 of [fatstack-implement](../fatstack-implement/SKILL.md). Use those instructions as the source of truth for full checks, without starting its ticket, status, branch, or PR workflow. Read the linked file directly if the agent cannot invoke that skill. If it is unavailable, report the missing dependency and ask the user for the project's full verification procedure rather than guessing commands.

1. Read the project's current `AGENTS.md` / `CLAUDE.md` and follow their check-document pointers. Resolve a command for every required check, including the full test suite. If none are documented, or any required command is missing, ask the user what to run. Continue only independent work while awaiting the answer; elapsed time is not approval to choose a command.
2. Run the full check list against the accumulated changes, including documented lint, type, build, and smoke checks. Focused checks from earlier in the session do not replace this run. Inspect the final diff for unrelated changes and verify the requested outcomes.
3. Fix failures within the requested scope and rerun affected checks. Apply the linked verification rules to failures, missing prerequisites, and any edits after verification. Report unrelated failures and blocked or unrun checks; keep verification pending and do not call the work done or open a PR while required checks fail or remain unrun.
4. Report the changes, each check's command and result, and any remaining questions or blockers. Confirm that normal mode is active. Call the work complete only when all required checks pass and the requested changes are verified.

## Session continuity

When the conversation is summarized, carry forward whether vibe mode is active or wrap-up verification is pending, the changed scope, check results, and any missing commands or authorization. If that state is unclear, ask rather than silently assuming the work was verified. A user can re-invoke this skill to restore the instructions or explicitly request the full checks after a long session. Full verification outside vibe mode belongs to `fatstack-implement`.
