# Personal Skills Marketplace

A modular collection of skills, commands, and rules to extend Claude's capabilities for specialized tasks.

## Installation

```bash
# Add this marketplace
/plugin marketplace add RealAlexandreAI/personal-skills-marketplace

# Install all skills
/plugin install personal-skills@alexandre-owned
```

## What's Included

### Skills (5)

| Skill | Description |
|-------|-------------|
| `changelog-generator` | Generate user-friendly changelogs from git commits |
| `code-reviewer` | Code review toolkit with PR analysis and quality checks |
| `error-resolver` | First-principles error diagnosis with replay support |
| `frontend-design` | Create production-grade frontend interfaces |
| `skill-creator` | Guide for creating effective skills |

### Commands (8)

| Command | Description |
|---------|-------------|
| `/add-command` | Add a new custom command to workspace |
| `/create-doc` | Create documentation following Diátaxis framework |
| `/git-commit` | Git commit with conventional-changelog style |
| `/git-push` | Complete git workflow with branch and PR |
| `/issue` | Create GitHub issue using CLI |
| `/no-spec` | Skip spec workflow, execute directly |
| `/spec` | Force complete spec workflow |
| `/translate` | Translate documentation to Chinese |

### Rules (4)

| Rule | Description | Auto Apply |
|------|-------------|------------|
| `project-rules` | Project coding standards and git conventions | Yes |
| `documentation-rules` | Diátaxis framework documentation standards | No |
| `interactive-dev` | Interactive development workflow | No |
| `font-awesome-icons` | FontAwesome icons reference | No |

> **Note**: Only skills are installed via the plugin system. Commands and rules need to be manually copied to your project's `.claude/` directory.

## Project Structure

```
skills/                    # Skill implementations
├── <skill-name>/
│   ├── SKILL.md          # Required: YAML frontmatter + instructions
│   ├── scripts/          # Optional: Python/Bash scripts
│   ├── references/       # Optional: On-demand documentation
│   └── assets/           # Optional: Templates, images, fonts
commands/                  # Slash commands (/xxx trigger)
├── <command-name>.md     # Command prompt files
rules/                     # Rule files (auto/manual apply)
├── <rule-name>.mdc       # Rules with frontmatter
.claude-plugin/marketplace.json  # Plugin marketplace config
```

## Creating Your Own Skills

Every skill requires a `SKILL.md` with:

```markdown
---
name: skill-name
description: What the skill does AND when to trigger it
---

# Instructions here
```

Key design principles:
- **Progressive disclosure**: Metadata always loaded → SKILL.md body when triggered → bundled resources on-demand
- **Concise instructions**: Only add context Claude doesn't already have
- **Appropriate freedom**: High (text) for flexible tasks, Low (scripts) for fragile operations

## References

- [What are skills?](https://support.claude.com/en/articles/12512176-what-are-skills)
- [Using skills in Claude](https://support.claude.com/en/articles/12512180-using-skills-in-claude)
- [Creating custom skills](https://support.claude.com/en/articles/12512198-creating-custom-skills)
- [Agent Skills specification](https://agentskills.io)

## License

Apache 2.0
