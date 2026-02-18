# ai-agent-skills

A collection of open-source AI agent skills for use with Claude Code and other
skills-compatible AI agents.

## Available Skills

| Skill | Description |
|-------|-------------|
| [business-naming](skills/business-naming/) | Generate and evaluate names for companies, products, and brands using a structured three-phase process |
| [java-code-review](skills/java-code-review/) | Review Java source code for bugs, security vulnerabilities, performance issues, and best practices |
| [openapi-review](skills/openapi-review/) | Review OpenAPI specs for compliance, security, API design best practices, and documentation quality |

## Installation

### Claude Code

```
npx skills add theepan/ai-agent-skills
```

Or install a specific skill directly:

```
npx skills add https://github.com/your-user/your-repo --skill java-code-review -g -y
```

### Manual

Copy the desired skill directory into your project's skills configuration.

## Creating New Skills

Each skill is a self-contained directory under `skills/` with a `SKILL.md` file.
See the [Agent Skills Specification](https://agentskills.io/specification) for
the format definition.

## License

MIT -- see [LICENSE](LICENSE) for details.
