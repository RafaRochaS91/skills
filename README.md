# RafaRochaS91/Skills

## Overview

This is a compilation of useful skills I use with Claude Code and other Agents that support the Skills standard.

These can be used in conjunction with hooks or just evoked via slash commands.

## Installation

Copy the skill folder you want into your Claude skills directory.

**Global (available in all projects):**

```bash
cp -r skills/<skill-name> ~/.claude/skills/
```

**Project-level:**

```bash
cp -r skills/<skill-name> .claude/skills/
```

Once copied, invoke the skill via slash command in Claude Code, e.g. `/memory-review`.

## Skills

| Skill | Description | Usage |
|---|---|---|
| `memory-review` | Reviews and curates `memory/MEMORY.md` to remove stale, duplicate, or low-signal entries. | `/memory-review` |
| `sw-architecture` | Applies DDD feature-based architecture conventions to a project. Scaffolds structure, audits existing code, or generates a `CLAUDE.md`. | `/sw-architecture` |
| `sw-effect` | Enforces Effect.ts conventions for error modeling and composition across DDD layers. Designed to follow `sw-architecture`. | `/sw-effect` or `/sw-effect --verify-docs` |

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
