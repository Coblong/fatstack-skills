# Fatstack Skills

Reusable agent skills for turning product ideas into clear requirements, delivery-ready tickets, and reviewed code.

The intended workflow is:

```text
idea → challenge → requirements → tickets → implementation → review
```

Fatstack Skills takes inspiration from the capabilities in [Matt Pocock's skills](https://github.com/mattpocock/skills). Its instructions and resources are independently authored. This project is not affiliated with Matt Pocock and does not depend on his skills.

## Status

Skills are in early development. Planned skills, in build order:

| Skill | Purpose | Status |
| --- | --- | --- |
| `fatstack-setup` | Choose and record the tracker, its statuses, and pull request conventions. | Draft |
| `fatstack-context` | Maintain the project's glossary and decision records. | Draft |
| `fatstack-challenge` | Interview you about a feature to draw out constraints, edge cases, and open questions. | Draft |
| `fatstack-requirements` | Turn a challenge into an AI-friendly requirements document. | Draft |
| `fatstack-tickets` | Break requirements into demonstrable tickets and optional milestones, agree them with you, then create them in your tracker. | Draft |
| `fatstack-tdd` | Build behaviour test-first, one red-green slice at a time. | Draft |
| `fatstack-implement` | Implement a ticket test-first, managing ticket transitions and the pull request. | Draft |
| `fatstack-review` | Review work against its ticket, requirements, and project standards. | Draft |
| `fatstack-vibe` | Make small changes with focused checks, then run full checks on wrap-up. | Draft |

Supported trackers are planned to include GitHub, Jira, and Linear, with local markdown files as a fallback. Generated outputs must remain reviewable and editable, with assumptions and open questions made explicit. Skills ask before creating tickets, changing ticket status, or opening pull requests.

## Installation

Install the draft skills using the NPM-distributed [skills CLI](https://github.com/vercel-labs/skills):

```bash
npx skills add Coblong/fatstack-skills
```

For Codex specifically:

```bash
npx skills add Coblong/fatstack-skills --agent codex
```

Add `-g` to install globally. The default installation scope is the current project.

The repository is the distribution source. It does not currently publish its own NPM package or installer.

### Plan milestones

`fatstack-tickets` groups larger features into ordered milestones with names and demonstrable goals. You can move tickets or merge, split, rename, and reorder phases before approval; dependencies are checked again after changes. Small features can use one milestone or none. Re-runs preserve existing assignments and propose moves for approval.

New milestone names are scoped to a feature, such as `checkout M2: Payment`. The phase number is separate from the tracker's native ID. Plans and ticket assignments are recorded in `requirements.md` and the tracker: GitHub milestones, confirmed Jira fix versions or a project-specific epic configuration, Linear project milestones, or local `Milestone:` fields. Existing tracker configurations need milestone operations added through setup before milestone writes.

### Implement a ticket

Invoke `fatstack-implement` explicitly with a ticket reference, for example `$fatstack-implement implement #6` in Codex or `/fatstack-implement implement #6` in Claude Code. Install `fatstack-tdd` alongside it. The skill reads the ticket, linked requirements, `docs/fatstack/tracker.md`, and the project's check instructions before planning. It uses test-first slices, requires the full checks to pass, and prepares a linked ready PR with authorization for status changes and publishing. Delegated runs return pending approvals to their coordinator.

### Review a ticket

Use `fatstack-review` with a ticket and PR, branch, or local change reference. It reports requirements and acceptance gaps separately from project standards findings, ranks each finding by severity, and cites the affected files and lines. Missing sources and unverified outcomes remain explicit. Reviews leave files and tracker state unchanged unless you request changes.

### Vibe mode

Invoke `$fatstack-vibe` in Codex or `/fatstack-vibe` in Claude Code to start a "let's vibe" session. Install `fatstack-implement` alongside it for the shared verification instructions. Small requested changes use focused checks where useful, while repository rules and authorization still apply. Say "let's wrap it up", "wrap it up", or "back to normal" to exit and run the full project checks before the work is called complete.

In a long session, summarization may lose the mode instructions. If wrap-up does not trigger the checks, invoke the skill again or explicitly ask for the full checks.

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
