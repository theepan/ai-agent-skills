# ai-agent-skills

A collection of open-source [agent skills](https://agentskills.io) for use with
Claude Code, Claude.ai, and other skills-compatible AI agents.

## Available Skills

### business-naming
Generate and evaluate names for companies, products, and brands using a structured three-phase process.
```
npx skills add theepan/ai-agent-skills --skill business-naming -g -y
```

### domain-name-gen
Generate short, pronounceable, catchy domain names for products, startups, or projects.
```
npx skills add theepan/ai-agent-skills --skill domain-name-gen -g -y
```

### java-code-review
Review Java source code for bugs, security vulnerabilities, performance issues, AI-generated code quality, and dependency licensing risks.
```
npx skills add theepan/ai-agent-skills --skill java-code-review -g -y
```

### learning-guide-creator
Create comprehensive, research-backed ~50-page learning guides (.md) on any topic, written for experienced technical professionals.
```
npx skills add theepan/ai-agent-skills --skill learning-guide-creator -g -y
```

### openapi-review
Review OpenAPI specs for compliance, security, API design best practices, and documentation quality.
```
npx skills add theepan/ai-agent-skills --skill openapi-review -g -y
```

### software-requirements
Write professional software requirements documents (SRS, PRD, BRD, FRD, User Stories, Use Cases) with comprehensive coverage of functional, non-functional, interface, and data requirements.
```
npx skills add theepan/ai-agent-skills --skill software-requirements -g -y
```

### stock-screener
Comprehensive growth stock screening and analysis combining quantitative financial screening with qualitative trend analysis.
```
npx skills add theepan/ai-agent-skills --skill stock-screener -g -y
```

### tariff-guide
Create comprehensive, printable tariff and customs guide documents for any country or customs territory. Produces both Markdown and self-contained HTML with professional styling, worked examples, comparison tables, and print-ready layout.
```
npx skills add theepan/ai-agent-skills --skill tariff-guide -g -y
```

## Installation

### Claude Code

Install all skills from this repo:

```
npx skills add theepan/ai-agent-skills -g -y
```

After installing, just mention the skill in your prompt:

```
"Name my startup that does AI-powered scheduling for trades professionals"
"Review this Java file for bugs and security issues"
"Review my OpenAPI spec for best practices"
```

### Claude.ai

1. Go to **Settings > Profile > Skills**
2. Upload the `SKILL.md` file from the desired skill directory
3. The skill will activate automatically when relevant tasks are detected

Available to paid Claude plans.

### Manual

Copy the desired skill directory into your project's `.claude/skills/`
directory:

```
cp -r skills/business-naming /path/to/your/project/.claude/skills/
```

The agent will discover and use the skill automatically based on the
description in the SKILL.md frontmatter.

## Skill Structure

Each skill follows the [Agent Skills Specification](https://agentskills.io/specification):

```
skill-name/
├── SKILL.md           # Required -- frontmatter + instructions
└── references/        # Optional -- additional docs loaded on demand
```

- **SKILL.md** -- YAML frontmatter (`name`, `description`) followed by
  markdown instructions. The `description` tells agents when to activate the
  skill. The body contains the step-by-step process.
- **references/** -- Detailed reference material that agents read during
  execution. Keeps the main SKILL.md focused while providing depth when needed.

## License

MIT -- see [LICENSE](LICENSE) for details.
