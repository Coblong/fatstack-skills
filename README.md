# Fatstack Skills

For years engineers have aspired to become Full Stack. A futile ambition for human engineers.

Fat Stack refers to the process of taking a feature from idea through analysis, design, technical architecture, implementation, quality assurance, deployment, and maintenance in production. 

This ownership of that whole lifecycle has always been far more valuable than the usual frontend-to-backend meaning of "full stack". Now, with the advent of AI assisted engineering, the full stack belongs to AI. 

Fatstack Skills are reusable agent skills for turning product ideas into clear requirements, delivery-ready tickets, and reviewed code.

The intended workflow is:

```text
idea → challenge → requirements → tickets → implementation → review
```

Fatstack Skills takes inspiration from the capabilities in [Matt Pocock's skills](https://github.com/mattpocock/skills). Its instructions and resources are independently authored. This project is not affiliated with Matt Pocock and does not depend on his skills.

## Skills

| Skill | Purpose |
| --- | --- |
| `fatstack-setup` | Choose and record the tracker, its statuses, and pull request conventions. |
| `fatstack-context` | Maintain the project's glossary and decision records. |
| `fatstack-challenge` | Interview you about a feature to draw out constraints, edge cases, and open questions. |
| `fatstack-requirements` | Turn a challenge into an AI-friendly requirements document. |
| `fatstack-tickets` | Break requirements into demonstrable tickets and optional milestones, agree them with you, then create them in your tracker. |
| `fatstack-tdd` | Build behaviour test-first, one red-green slice at a time. |
| `fatstack-implement` | Implement a ticket test-first, managing ticket transitions and the pull request. |
| `fatstack-review` | Review work against its ticket, requirements, and project standards. |
| `fatstack-vibe` | Make small changes with focused checks, then run full checks on wrap-up. |
| `fatstack-chief-of-staff` | Coordinate a ticket batch or milestone through implementation workers and review each result. |

Tracker guides cover GitHub, Jira, and Linear, with local markdown files as a fallback. Generated outputs must remain reviewable and editable, with assumptions and open questions made explicit. Implementing a ticket includes routine status transitions, agent-selected test boundaries, scoped commits, branch pushes, and a linked ready PR without separate approval. Explicit user limits and project rules still apply. Ticket creation, merges, and deployments require separate user intent.

## Installation

Install the skills using the NPM-distributed [skills CLI](https://github.com/vercel-labs/skills):

```bash
npx skills add Coblong/fatstack-skills
```

For Codex specifically:

```bash
npx skills add Coblong/fatstack-skills --agent codex
```

Add `-g` to install globally. The default installation scope is the current project.

The repository is the distribution source. It does not currently publish its own NPM package or installer.

## How to use

Open your coding agent in the project you want to work on. Start by running `fatstack-setup` once for that project to choose the tracker and record its statuses, milestone operations, and branch and PR conventions. Make sure the project's `AGENTS.md` or `CLAUDE.md` documents the checks an implementation must pass.

Send these prompts one at a time in Codex. In Claude Code, replace the leading `$` with `/`:

```text
$fatstack-setup Configure Fatstack for this project.
$fatstack-challenge I want users to invite team members by email.
$fatstack-requirements Write requirements from our discussion.
$fatstack-tickets Turn those requirements into tickets and propose milestones if useful.
$fatstack-implement Implement #123.
$fatstack-review Review #123 and its pull request.
```

Replace the idea with your feature and `#123` with a real ticket reference, such as a GitHub issue number, Jira or Linear key, or local ticket path. Answer the challenge questions, review the requirements, and approve the ticket and milestone plan before anything is created in the tracker.

You can continue through the workflow in one chat. The skills save their records under `docs/fatstack/`, so a new chat can resume from the ticket reference and linked requirements. If you already have a clear ticket and requirements, start with `fatstack-implement`. It runs the project's checks, commits and pushes the ticket branch, and opens a ready PR; ask for local-only work if you do not want publishing. Review findings are reported without changing files, and merging remains your decision.

For several tickets or a milestone, use `fatstack-chief-of-staff` as described below. For small edits with full checks deferred until wrap-up, use `fatstack-vibe`.

### Plan milestones

`fatstack-tickets` groups larger features into ordered milestones with names and demonstrable goals. You can move tickets or merge, split, rename, and reorder phases before approval; dependencies are checked again after changes. Small features can use one milestone or none. Re-runs preserve existing assignments and propose moves for approval.

New milestone names are scoped to a feature, such as `checkout M2: Payment`. The phase number is separate from the tracker's native ID. Plans and ticket assignments are recorded in `requirements.md` and the tracker: GitHub milestones, confirmed Jira fix versions or a project-specific epic configuration, Linear project milestones, or local `Milestone:` fields. Existing tracker configurations need milestone operations added through setup before milestone writes.

### Implement a ticket

Ask to implement a ticket or invoke `fatstack-implement` explicitly, for example `$fatstack-implement implement #6` in Codex or `/fatstack-implement implement #6` in Claude Code. Automatic selection is enabled for requested implementation tasks. Install `fatstack-tdd` alongside it. The skill reads the ticket, linked requirements, `docs/fatstack/tracker.md`, and project check instructions, then implements test-first, runs full checks, commits, pushes its branch, and creates or updates a linked ready PR without another authorization prompt. It asks when information is missing or conflicting and honors explicit limits such as local-only work. Delegated runs follow the same rules.

### Review a ticket

Use `fatstack-review` with a ticket and PR, branch, or local change reference. It reports requirements and acceptance gaps separately from project standards findings, ranks each finding by severity, and cites the affected files and lines. Missing sources and unverified outcomes remain explicit. Reviews leave files and tracker state unchanged unless you request changes.

### Vibe mode

Invoke `$fatstack-vibe` in Codex or `/fatstack-vibe` in Claude Code to start a "let's vibe" session. Install `fatstack-implement` alongside it for the shared verification instructions. Small requested changes use focused checks where useful, while repository rules and authorization still apply. Say "let's wrap it up", "wrap it up", or "back to normal" to exit and run the full project checks before the work is called complete.

In a long session, summarization may lose the mode instructions. If wrap-up does not trigger the checks, invoke the skill again or explicitly ask for the full checks.

### Coordinate a batch

Invoke `$fatstack-chief-of-staff` in Codex or `/fatstack-chief-of-staff` in Claude Code with a goal, ticket batch, or feature milestone. Install `fatstack-implement`, `fatstack-tdd`, and `fatstack-review` alongside it. The coordinator agrees the batch and dependency order, explains each worker's model and effort, delegates implementation to isolated background workers, and reviews the results. Tickets wait for verified blockers; an open PR does not count as done. Workers update statuses, select test boundaries, commit, push, and create ready PRs without asking again, while respecting explicit user limits. Merging remains a separate decision.

This skill takes capability inspiration from [Matt Pocock's chief-of-staff](https://github.com/mattpocock/skills/blob/main/skills/in-progress/chief-of-staff/SKILL.md). It adds ticket and milestone delivery, portable model tiers, and review against project requirements. Its instructions are independently authored. It requires a harness with background sub-agents; limited model or effort controls are disclosed before dispatch and need an agreed fallback.

## Repository structure

```text
fatstack-skills/
├── README.md
├── AGENTS.md
├── LICENSE
├── CONTRIBUTING.md
└── skills/
    └── fatstack-<name>/
        └── SKILL.md
```

Each skill belongs in `skills/fatstack-<name>/SKILL.md`. Add scripts, references, or assets only when the skill needs them.

In your project, the skills keep their shared files under `docs/fatstack/`. See [AGENTS.md](AGENTS.md#project-file-layout) for the layout.

## Contributing

Read [CONTRIBUTING.md](CONTRIBUTING.md) for authoring and validation guidance. Agents working in this repository should follow [AGENTS.md](AGENTS.md).

## Licence

MIT. See [LICENSE](LICENSE).
