# Contributing to Everything Claude Research

Thanks for wanting to contribute. This repo is meant to be a community resource for academic researchers using Claude Code.

## What We're Looking For

### Agents

New research-focused agents:
- Domain-specific theorists (Economics, Psychology, Biology, Physics)
- Methodology specialists (Causal inference, Bayesian methods, Experimental design)
- Statistical reviewers (Power analysis, Assumption checking)
- Literature specialists (Systematic review, Meta-analysis)
- Data specialists (Data cleaning, Preprocessing, Visualization)

### Skills

Research workflow definitions and domain knowledge:
- Discipline best practices
- Statistical methodologies
- Research design patterns
- Publication strategies
- Domain-specific knowledge

### Commands

Slash commands that invoke useful research workflows:
- `/systematic-review` - Structured literature review
- `/power-analysis` - Sample size calculation
- `/preregister` - Pre-registration assistant
- `/meta-analysis` - Meta-analysis workflow
- `/replicate` - Replication study design

### Domains

New domain knowledge structures:
- Economics
- Psychology
- Biology
- Computer Science
- Physics
- Sociology
- Education
- Medicine

Each domain should have:
- `DOMAIN.md` - Core theories, methods, standards
- `literature/` - Key foundational papers
- `terminology.md` - Domain-specific terms

### Rules

Research-specific guidelines:
- Statistical rules
- Ethical guidelines
- Reproducibility requirements
- Citation conventions
- Discipline-specific norms

### Examples

Example research projects showing:
- Complete hypothesis workflow
- Literature review process
- Analysis and verification
- Meeting notes
- Review reports

---

## How to Contribute

### 1. Fork the repo

```bash
git clone https://github.com/YOUR_USERNAME/everything-claude-research.git
cd everything-claude-research
```

### 2. Create a branch

```bash
git checkout -b add-economics-theorist
```

### 3. Add your contribution

Place files in the appropriate directory:
- `.claude/agents/` for new agents
- `.claude/commands/` for slash commands
- `.claude/rules/` for rule files
- `domains/` for domain knowledge
- `contexts/` for context configurations
- `shared/templates/` for project templates

### 4. Follow the format

**Agents** should have frontmatter:

```markdown
---
name: agent-name
description: What it does
tools: ["Read", "Bash", "WebSearch", "Grep", "Glob"]
model: opus
---

# Agent Name

## Identity
...

## Core Capabilities
...
```

**Commands** should explain workflow:

```markdown
---
name: command-name
description: Brief description
---

# /command-name

## Purpose
...

## Workflow
...

## Output
...
```

**Domains** should be comprehensive:

```markdown
# [Domain] Domain Knowledge

## Domain Overview
...

## Core Theoretical Frameworks
...

## Methodological Traditions
...

## Quality Standards
...
```

### 5. Test your contribution

Make sure your config works with Claude Code before submitting:
1. Copy files to appropriate locations
2. Test commands with Claude Code
3. Verify agents can be invoked
4. Check that workflows execute correctly

### 6. Submit a PR

```bash
git add .
git commit -m "Add economics theorist agent"
git push origin add-economics-theorist
```

Then open a PR with:
- What you added
- Why it's useful for research
- How you tested it
- What domain/discipline it supports

---

## Guidelines

### Do

- Keep agents focused and specialized
- Include clear research-oriented descriptions
- Test with actual research workflows
- Follow existing patterns
- Document dependencies
- Use research terminology appropriately
- Include citations where relevant
- Provide examples

### Don't

- Include sensitive research data
- Add overly narrow/niche agents (unless domain-specific)
- Submit untested configs
- Create duplicate functionality
- Include proprietary methods without permission
- Copy-paste from software development agents without adaptation

---

## File Naming

- Use lowercase with hyphens: `causal-inference-specialist.md`
- Be descriptive: `bayesian-methods.md` not `methods.md`
- For domains: Use discipline name `psychology/` not `psych/`
- Match the agent/skill name to the filename

---

## Domain Contributions

When adding a new domain, include:

```
domains/
└── your-domain/
    ├── DOMAIN.md              # Required
    ├── literature/            # Required (can be empty)
    │   └── README.md         # List key papers
    ├── terminology.md         # Optional
    └── methods/              # Optional
        └── specific-method.md
```

### DOMAIN.md Structure

```markdown
# [Domain] Domain Knowledge

## Domain Overview
[What this field studies]

## Core Theoretical Frameworks
[Major theoretical traditions]

## Methodological Traditions
[Common methodological approaches]

## Quality Standards
[What makes good research in this field]

## Publication Ecosystem
[Publication venues, review process]

## Common Pitfalls
[Common mistakes or pitfalls]

## Terminology
[Key domain-specific terms]
```

---

## Research Agent Design Principles

Good research agents should:
1. **Understand disciplinary norms** - Know what's acceptable in the field
2. **Be methodologically sound** - Follow rigorous research practices
3. **Support reproducibility** - Encourage documentation and transparency
4. **Respect ethics** - Consider ethical implications
5. **Integrate literature** - Connect to existing knowledge
6. **Communicate clearly** - Use precise academic language

---

## Questions?

- **Issues**: Open an issue on GitHub
- **Discussions**: Use GitHub Discussions for ideas
- **Original Framework**: See [everything-claude-code](https://github.com/affaan-m/everything-claude-code)

---

## Code of Conduct

- Be respectful of different research traditions
- Value methodological diversity
- Support open science and reproducibility
- Credit others' contributions
- Maintain academic integrity

---

Thanks for contributing. Let's build a great resource for the research community.
