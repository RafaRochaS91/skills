# RafaRochaS91/Skills

## Overview

This is a compilation of useful skills I use with Claude Code and other Agents that support the Skills standard.

These can be used in conjunction with hooks or just evoked via slash commands.

## Installation

**All skills (global):**

```bash
npx skills add RafaRochaS91/skills -g
```

**All skills (project-level):**

```bash
npx skills add RafaRochaS91/skills
```

Once installed, invoke skills per the usage instructions in the table below. Not all skills are slash-command invocable — check the Usage column for each.

## Skills

| Skill | Description | Invocation | Usage |
|---|---|---|---|
| `memory-review` | Reviews and curates `memory/MEMORY.md` to remove stale, duplicate, or low-signal entries. | Slash command | `/memory-review` |
| `sw-architecture` | Applies DDD feature-based architecture conventions to a project. Scaffolds structure, audits existing code, or generates a `CLAUDE.md`. | Slash command | `/sw-architecture` |
| `sw-effect` | Enforces Effect.ts conventions for error modeling and composition across DDD layers. Designed to follow `sw-architecture`. | Slash command | `/sw-effect` or `/sw-effect --verify-docs` |

## Contributing

If you have a skill you'd like to contribute, please submit a pull request with the following format:

```markdown
---
name: skill-name
description: A brief description of what the skill does and how to use it.
trigger
---
# Skill Name

A detailed description of the skill, including any parameters it takes and examples of how to use it.
```

For more information about skills structure and best practices, please refer to the [Skills Documentation](https://code.claude.com/docs/en/skills).
