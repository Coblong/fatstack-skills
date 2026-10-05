# Working on Fatstack Skills

## Purpose and scope

Build reusable agent skills that help users challenge ideas, specify outcomes, shape work, create delivery tickets, and hand off reviewed work.

This is an independent skills repository. Do not introduce the Fat Stack application's Django runtime, database, or deployment infrastructure here. Add executable helpers only when a skill has a concrete need for them.

Use Matt Pocock's skills as a capability reference. Author original instructions and resources. Do not copy third-party prompts or imply affiliation, endorsement, or a dependency on his work.

## Domain language

- An **idea** is an initial desired outcome, problem, or change.
- A **discovery conversation** clarifies an idea through guided questions.
- A **specification** records intended behaviour, success criteria, decisions, and open questions.
- A **ticket** is a delivery-ready unit of work derived from a specification.
- **Export/sync** is a user-approved transfer to an external tracking tool.

Keep generated material reviewable and editable. Distinguish confirmed decisions from assumptions and preserve links to source specifications and decisions.

## Skill authoring

- Put each skill in `skills/<skill-name>/SKILL.md`.
- Use lowercase letters, digits, and hyphens for skill names. Match the directory name to the frontmatter name.
- Include YAML frontmatter with `name` and `description`. Describe the task and when the skill should apply.
- Keep instructions focused on decisions and procedures that improve the task. Avoid generic advice and repeated policies.
- Keep skills usable independently. State required tools and dependencies instead of assuming they exist.
- Link supporting references from the instructions and explain when to read them.
- Add scripts only for concrete automation needs and validate their behaviour.
- Preserve user intent, project conventions, and existing authorization. A skill must not grant itself permission for external actions.
- Require explicit user intent before creating, modifying, or exporting tickets in an external system.
- Never include credentials, private conversations, or private tracker data in examples or fixtures.

## Validation and documentation

Check frontmatter, relative links, resources, and installation discovery for new skills. Test realistic requests, including incomplete information and relevant failure cases. Judge observable behaviour rather than matching exact prose.

Run relevant checks once tooling exists. Report what was checked and any gaps. Keep README status, installation instructions, and the published skill list accurate. Do not claim agent compatibility without testing it.

## Change hygiene

Inspect the working tree before editing and preserve unrelated work. Keep changes focused. Do not commit, push, publish an NPM package, or create external tickets unless explicitly asked.

Create pull requests in ready state, never as drafts. After addressing pull request comments, reply to each addressed comment and resolve it.
