# Contributing to Fatstack Skills

Contributions should improve a specific part of the journey from an idea to reviewed delivery work.

## Propose a skill

Explain the user problem, when the skill should trigger, its required inputs, and its reviewable outputs. Identify the capability it supports and how you will verify its behaviour.

Prefer a small workflow that users can complete end-to-end. Keep tracker-specific operations separate from specification and ticket authoring where practical.

## Author a skill

Create `skills/<skill-name>/SKILL.md` with frontmatter such as:

```markdown
---
name: fatstack-example
description: Describe the concrete task this skill handles and when to use it.
---

Write the instructions here.
```

Prefix the name with `fatstack-` and match the folder name to the skill name. Use lowercase letters, digits, and hyphens.

State the intended output and the decisions the agent needs to make. Explain how to handle missing information without inventing certainty. Preserve the user's ability to review and correct results.

Add optional resources only when needed:

- `references/` for guidance loaded in relevant situations.
- `assets/` for templates or files used in output.
- `scripts/` for deterministic, reusable automation.
- `agents/openai.yaml` for optional Codex metadata.

Link resources from `SKILL.md` and keep those links relative. Document required tools, credentials, and permissions without including secrets.

Author original content. Use public skill collections as capability references, not as text to reproduce. If a comparable skill exists in Matt Pocock's collection, note in the proposal what ours does differently.

## Validate changes

There is no automated validation tooling in this repository yet. Before proposing a change:

1. Check that frontmatter includes a meaningful `name` and `description`.
2. Verify that referenced files exist and instructions contain no unfinished placeholders.
3. Run the skill against a realistic request in the target agent.
4. Check incomplete inputs, user corrections, and meaningful failure cases.
5. Run any added scripts and verify their results.
6. Confirm that external side effects require user authorization.

Once a skill exists, check installer discovery from the repository root:

```bash
npx skills add . --list
```

Test installation in a temporary project so you do not overwrite your normal skills. Record the agent, installation method, scenarios, and results in the pull request.

## Submit a change

Keep the change focused and update the README when available skills or installation behaviour change. Explain the user problem, resulting behaviour, validation performed, and any limitations.

Create pull requests in ready state. Reply to and resolve review comments after addressing them.
