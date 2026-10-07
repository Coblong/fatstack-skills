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
| `fatstack-tickets` | Break requirements into demonstrable tickets, agree them with you, then create them in your tracker. | Draft |
| `fatstack-tdd` | Build behaviour test-first. | Planned |
| `fatstack-implement` | Implement a ticket test-first, managing ticket transitions and the pull request. | Planned |
| `fatstack-review` | Review work against its ticket, requirements, and project standards. | Planned |

Supported trackers are planned to include GitHub, Jira, and Linear, with local markdown files as a fallback. Generated outputs must remain reviewable and editable, with assumptions and open questions made explicit. Skills ask before creating tickets, changing ticket status, or opening pull requests.

## Installation

Once skills are available, install them using the NPM-distributed [skills CLI](https://github.com/vercel-labs/skills):

```bash
npx skills add Coblong/fatstack-skills
```

For Codex specifically:

```bash
npx skills add Coblong/fatstack-skills --agent codex
```

Add `-g` to install globally. The default installation scope is the current project. These commands will not install useful skills until this repository contains them.

The repository is the distribution source. It does not currently publish its own NPM package or installer.

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
