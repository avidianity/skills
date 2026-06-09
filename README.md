# Skills

Agent skills are folders of instructions, scripts, and resources that an AI agent can load when a task needs specialized behavior. A skill should make repeatable work easier by giving the agent domain-specific steps, constraints, examples, and supporting files.

This repository contains agent skills created and maintained by John Michael Manlupig.

- Author: John Michael Manlupig
- Email: manlupigjohnmichael@gmail.com

For the Agent Skills standard, see [agentskills.io](https://agentskills.io).

## Repository Contents

- [skills/fallow](./skills/fallow): Use Fallow CLI for JavaScript and TypeScript static codebase intelligence, including dead code, unused exports, unused dependencies, duplicate code, complexity hotspots, maintainability issues, refactor targets, and Fallow CI/config/debugging.
- [template](./template): Starter `SKILL.md` template for creating a new skill.
- [spec](./spec): Pointer to the Agent Skills specification.

## Skill Format

Each skill lives in its own folder with a `SKILL.md` file. The file starts with YAML frontmatter followed by instructions for the agent.

```markdown
---
name: my-skill-name
description: A clear description of what this skill does and when an agent should use it.
---

# My Skill Name

Add instructions here.
```

Required frontmatter:

- `name`: Unique skill identifier, usually lowercase with hyphens.
- `description`: Clear trigger description that tells an agent when to use the skill.

The markdown body can include workflow steps, examples, constraints, command references, scripts, resources, or any other guidance the agent needs.

## Creating a Skill

1. Copy [template/SKILL.md](./template/SKILL.md) into a new folder under [skills](./skills).
2. Rename the skill in the frontmatter.
3. Write a specific description that includes when the skill should be used.
4. Add focused instructions and examples.
5. Test the skill in an agent runtime that supports Agent Skills.

Keep skills scoped and practical. A good skill teaches one repeatable workflow instead of becoming a general knowledge dump.
