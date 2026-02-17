# ai-agent-skills

A collection of open-source AI agent skills for use with Claude Code and other
skills-compatible AI agents.

## Available Skills

| Skill | Description |
|-------|-------------|
| [java-code-review](skills/java-code-review/) | Review Java source code for bugs, security vulnerabilities, performance issues, and best practices |

## Installation

### Claude Code

```
npx skills add theepan/ai-agent-skills
```

### Manual

Copy the desired skill directory into your project's skills configuration.

## Creating New Skills

Each skill is a self-contained directory under `skills/` with a `SKILL.md` file.
See the [Agent Skills Specification](https://agentskills.io/specification) for
the format definition.

## License

MIT -- see [LICENSE](LICENSE) for details.
