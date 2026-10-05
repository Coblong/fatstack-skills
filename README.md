# Fatstack Skills

Reusable agent skills for turning product ideas into specifications and delivery-ready work.

The intended workflow is:

```text
idea → discovery conversation → specification → tickets → reviewed handoff
```

Fatstack Skills takes inspiration from the capabilities in [Matt Pocock's skills](https://github.com/mattpocock/skills). Its instructions and resources are independently authored. This project is not affiliated with Matt Pocock and does not depend on his skills.

## Status

This repository currently contains project documentation. No skills have been published yet. The `skills/` directory is reserved for the first skills.

Planned capabilities include challenging ideas, specifying outcomes, shaping delivery slices, creating tickets, and handing off reviewed work. Generated outputs must remain reviewable and editable, with assumptions and open questions made explicit.

## Installation

Once skills are available, install them using the NPM-distributed [skills CLI](https://github.com/vercel-labs/skills):

```bash
npx skills add Scale-Or-Sell/fatstack-skills
```

For Codex specifically:

```bash
npx skills add Scale-Or-Sell/fatstack-skills --agent codex
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
    └── .gitkeep
```

Each future skill belongs in `skills/<skill-name>/SKILL.md`. Add scripts, references, or assets only when the skill needs them.

## Contributing

Read [CONTRIBUTING.md](CONTRIBUTING.md) for authoring and validation guidance. Agents working in this repository should follow [AGENTS.md](AGENTS.md).

## Licence

MIT. See [LICENSE](LICENSE).
