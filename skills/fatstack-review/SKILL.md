---
name: fatstack-review
description: Review a ticket's implementation against its acceptance criteria, linked requirements, and project standards. Use for a pull request, branch, or local change review, or when an implementation coordinator needs a review before hand-off.
---

# Fatstack Review

Produce an evidence-backed review with requirements gaps separate from standards issues. Review only: leave project files, commits, ticket statuses, and external comments unchanged unless the user explicitly asks for those actions. Use read-only inspection and existing check results by default; obtain authorization before checks that modify files or external state.

Required: access to the change and surrounding source, the ticket and linked requirements, and the project's standards. Use `docs/fatstack/tracker.md` for ticket lookup when present. PR access may use a different tool from the tracker: identify the host from the repository remote and available tools. This skill does not depend on another review skill or parallel agents.

## 1. Establish the change and sources

Start with the current conversation and any supplied ticket, PR, branch, or local changes. Inspect the working tree without switching branches or altering it. State the review scope: repository, ticket, base revision, target revision, and whether staged, unstaged, or untracked files are included. Resolve Git refs to commit IDs so moving branch names do not change the comparison during review. For a PR, resolve its actual base and head and review the PR diff against their merge-base; for a branch, compare its head against the merge-base with the intended base. For local work, include the requested working-tree changes as well as any branch commits. Read relevant untracked files explicitly, since Git diff omits them. If the intended comparison is ambiguous, ask before drawing conclusions. An empty diff is an empty review, not evidence that the ticket is complete.

Find the ticket from the supplied reference or its links in the PR, branch, commits, or project files, then read its acceptance criteria, linked requirements, constraints, and blockers. Read `docs/fatstack/tracker.md` and use its lookup operations; without it, use a supplied local ticket or an unambiguous issue link. Ask for any missing or ambiguous ticket or requirements source. Continue independent standards review while awaiting sources, but label requirements review incomplete rather than inventing requirements. If the user confirms there is no requirements document, review the ticket criteria and state that limitation.

Read applicable `AGENTS.md` / `CLAUDE.md`, contribution and coding standards, and the glossary and relevant decision records at the project's recorded locations. A conversation correction may supersede a saved source: show conflicts and clarify them before treating the affected rule as confirmed. Distinguish documented rules from reviewer suggestions. Record unavailable sources and checks as limitations.

## 2. Review requirements and behaviour

Account for every acceptance criterion and applicable requirement as satisfied, partial, missing, or unverified. Trace each to changed code, surrounding callers, and tests. Follow the meaningful success and failure paths, including permissions and integration boundaries where relevant. Check scope additions and whether constraints and blockers have been respected. For a prefactoring ticket, assess its structural outcome and evidence that existing behaviour is preserved.

Report actionable mismatches with a concrete trigger and consequence. Cite the criterion or requirement and the code that causes the mismatch. Tests or reported passing checks alone do not prove every requirement; inspect what they exercise. Separate a demonstrated gap from an unverified outcome or an open question. If the target changes during inspection, report the reviewed revision and inspect the new changes before extending the conclusion to them.

## 3. Review project standards

Compare the change with the applicable project rules, including domain terminology, module boundaries, security, testing conventions, and documentation obligations. Cite the rule's source for each standards violation. Inspect enough surrounding code to distinguish a regression introduced by the change from an existing problem. Keep optional improvements clearly labelled and out of the actionable finding count unless they address a concrete defect or documented requirement. Avoid duplicating a single issue across both review categories; choose its primary category and note any related rule there.

## 4. Report the review

Present requirements and acceptance findings separately from project standards findings. Within each category, order findings by severity:

- **P0:** an immediate blocker with broad impact, such as demonstrated data loss or a critical security failure.
- **P1:** a serious failure on an important supported path that should block hand-off.
- **P2:** a concrete defect or unmet criterion that needs correction.
- **P3:** a minor documented standards violation with limited impact.

Each finding includes severity, a short title, a verified file and one-based line or tight line range, the source requirement or rule, the triggering case, its consequence, and the smallest useful correction. Link to the reviewed PR revision or use an absolute local file path. For missing implementation, anchor to the nearest relevant code and explain what is absent; if no code location exists, cite the exact ticket or requirements location rather than inventing a line.

Include a compact coverage account for the criteria and requirements, check evidence actually inspected, and remaining questions or limitations. Say "no findings" for a category only if it was reviewed; say "not reviewed" when its sources or scope were unavailable. Never equate missing evidence with a pass. End with the highest severity in each category and whether the change is ready for hand-off, needs corrections, or cannot yet be assessed. Return the review in the conversation; proposed fixes or tracker replies remain proposals until requested.
