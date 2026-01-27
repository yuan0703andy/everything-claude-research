# Setup Guide

Complete installation guide for Everything Claude Research.

---

## Prerequisites

### Claude Code CLI Only!

Install Claude Code if you haven't already:

```bash
# Follow instructions at https://claude.ai/code
```

Verify installation:
```bash
claude --version
```

**That's it!** No additional plugins needed. This framework is completely self-contained and includes:
- 4 core skills (iterative-retrieval, verification-loop, eval-harness, strategic-compact)
- 12 research commands
- 4 specialized research agents
- Complete documentation and templates

---

## Installation

### Option 1: Fresh Install (Recommended)

```bash
# 1. Clone the repository
git clone https://github.com/YOUR_USERNAME/everything-claude-research.git
cd everything-claude-research

# 2. Copy the complete framework to Claude config
cp -r .claude/agents ~/.claude/
cp -r .claude/commands ~/.claude/
cp -r .claude/skills ~/.claude/
cp -r .claude/rules ~/.claude/
cp -r .claude/contexts ~/.claude/

# 3. Create your research workspace
mkdir -p ~/research
cp -r domains ~/research/
cp -r shared ~/research/

# 4. Verify installation
ls ~/.claude/agents/senior-postdocs/
# Should show: theorist.md, experimentalist.md, methodologist.md

ls ~/.claude/commands/
# Should show: 12 command files including brainstorm.md, lab-meeting.md, etc.

ls ~/.claude/skills/
# Should show: iterative-retrieval, verification-loop, eval-harness, strategic-compact
```

### Option 2: Manual Setup

If you prefer to set up manually or already have a research directory:

#### Step 1: Agents

```bash
# Create agent directories
mkdir -p ~/.claude/agents/senior-postdocs
mkdir -p ~/.claude/agents/lab-manager

# Copy agent definitions
cp .claude/agents/senior-postdocs/*.md ~/.claude/agents/senior-postdocs/
cp .claude/agents/lab-manager/*.md ~/.claude/agents/lab-manager/
```

#### Step 2: Commands

```bash
# Copy all research commands
cp .claude/commands/*.md ~/.claude/commands/
```

#### Step 3: Skills

```bash
# Copy core skills
mkdir -p ~/.claude/skills
cp -r .claude/skills/* ~/.claude/skills/
```

#### Step 4: Rules

```bash
# Copy research rules
mkdir -p ~/.claude/rules
cp .claude/rules/*.md ~/.claude/rules/
```

#### Step 5: Contexts

```bash
# Copy research contexts
mkdir -p ~/.claude/contexts
cp .claude/contexts/*.md ~/.claude/contexts/
```

#### Step 6: Research Workspace

```bash
# Set up research directory
mkdir -p ~/research/domains
mkdir -p ~/research/projects
mkdir -p ~/research/shared/templates

# Copy domains and templates
cp -r domains/* ~/research/domains/
cp -r shared/templates/* ~/research/shared/templates/
```

---

## Verification

### 1. Check Claude Config

```bash
# Verify agents are installed
ls ~/.claude/agents/senior-postdocs/
# Expected: theorist.md, experimentalist.md, methodologist.md

ls ~/.claude/agents/lab-manager/
# Expected: coordinator.md

# Verify commands are installed
ls ~/.claude/commands/
# Expected: 12 files including brainstorm.md, lab-meeting.md, checkpoint.md, eval.md, etc.

# Verify skills are installed
ls ~/.claude/skills/
# Expected: iterative-retrieval, verification-loop, eval-harness, strategic-compact

# Verify contexts are installed
ls ~/.claude/contexts/
# Expected: research.md, hypothesis.md, peer-review.md
```

### 2. Test Commands

Start Claude Code and try:

```bash
# Should show all your commands (type /help to see full list)
/brainstorm           # Research-specific
/lab-meeting          # Research-specific
/review-hypothesis    # Research-specific
/execute-analysis     # GSD-inspired
/verify-results       # GSD-inspired
/progress             # GSD-inspired
/update-state         # GSD-inspired
/checkpoint           # From everything-claude-code
/eval                 # From everything-claude-code
/verify               # From everything-claude-code
```

### 3. Check Research Workspace

```bash
# Verify structure
tree -L 2 ~/research/
# Should show:
# research/
# ├── domains/
# │   ├── stats-theory/
# │   └── policy-making/
# ├── projects/
# └── shared/
#     └── templates/
```

---

## Your First Research Project

### 1. Create Project Directory

```bash
cd ~/research/projects
mkdir my-first-project
cd my-first-project
```

### 2. Copy Templates

```bash
# Copy project templates
cp ~/research/shared/templates/PROJECT_TEMPLATE.md ./PROJECT.md
cp ~/research/shared/templates/CLAUDE_PROJECT.md ./CLAUDE.md

# Create project structure
mkdir -p hypotheses
mkdir -p meeting_notes
mkdir -p reviews
mkdir -p data
mkdir -p results
mkdir -p notes
```

### 3. Edit PROJECT.md

Fill in your research question:

```markdown
# My First Research Project

## 研究問題
探索高維統計中的理論界限...

## 研究目標
1. ...
2. ...
```

### 4. Configure CLAUDE.md

Specify which domain to use:

```markdown
## Domain
使用領域知識：domains/stats-theory/

## 專案資訊
參考本目錄下的 PROJECT.md
```

### 5. Start Research

```bash
# Start Claude Code in your project directory
cd ~/research/projects/my-first-project

# Run your first brainstorming
/brainstorm "your research question"
```

---

## Directory Structure After Setup

```
~/
├── .claude/                    # Claude global config
│   ├── agents/
│   │   ├── senior-postdocs/
│   │   │   ├── theorist.md              ✅
│   │   │   ├── experimentalist.md       ✅
│   │   │   └── methodologist.md         ✅
│   │   └── lab-manager/
│   │       └── coordinator.md           ✅
│   │
│   ├── commands/
│   │   ├── brainstorm.md                ✅
│   │   ├── lab-meeting.md               ✅
│   │   └── review-hypothesis.md         ✅
│   │
│   ├── rules/
│   │   └── research-principles.md       ✅
│   │
│   └── plugins/
│       └── marketplaces/
│           └── everything-claude-code/  ✅ (plugin)
│
└── research/                   # Your research workspace
    ├── domains/
    │   ├── stats-theory/
    │   │   ├── DOMAIN.md                ✅
    │   │   └── literature/
    │   └── policy-making/
    │       ├── DOMAIN.md                (optional)
    │       └── literature/
    │
    ├── projects/
    │   └── my-first-project/
    │       ├── CLAUDE.md                ✅
    │       ├── PROJECT.md               ✅
    │       ├── hypotheses/
    │       ├── meeting_notes/
    │       └── reviews/
    │
    └── shared/
        └── templates/
            ├── PROJECT_TEMPLATE.md      ✅
            └── CLAUDE_PROJECT.md        ✅
```

---

## Customization

### Adding a New Domain

```bash
cd ~/research/domains
mkdir my-domain
cd my-domain

# Create domain knowledge file
touch DOMAIN.md
mkdir literature
```

Edit `DOMAIN.md`:
```markdown
# My Domain Knowledge

## 領域概述
...

## 核心理論框架
...

## 方法論傳統
...
```

### Modifying Agents

Agents are at `~/.claude/agents/`. You can:

1. Edit existing agents to fit your research style
2. Add new agents for specific tasks
3. Adjust tool permissions

Example:
```bash
# Edit Theorist
nano ~/.claude/agents/senior-postdocs/theorist.md
```

### Creating Custom Commands

```bash
# Create new command
nano ~/.claude/commands/my-command.md
```

Format:
```markdown
---
name: my-command
description: What it does
---

# /my-command

## 目的
...

## 流程
...
```

---

## Troubleshooting

### Commands not showing up

```bash
# Restart Claude Code
# Or check command files are in the right location
ls ~/.claude/commands/
```

### Skills not loading

```bash
# Verify skills are installed
ls ~/.claude/skills/

# Should show:
# - iterative-retrieval
# - verification-loop
# - eval-harness
# - strategic-compact

# If missing, copy them:
cp -r .claude/skills/* ~/.claude/skills/
```

### Domain knowledge not loading

Make sure your project's `CLAUDE.md` correctly points to the domain:

```markdown
## Domain
使用領域知識：domains/stats-theory/  # ← Check this path
```

---

## Updating

To update to the latest version:

```bash
cd /path/to/everything-claude-research
git pull origin main

# Re-copy agents, commands, rules
cp -r .claude/agents ~/.claude/
cp -r .claude/commands ~/.claude/
cp -r .claude/rules ~/.claude/
```

---

## Uninstalling

```bash
# Remove agents
rm -rf ~/.claude/agents/senior-postdocs
rm -rf ~/.claude/agents/lab-manager

# Remove commands
rm ~/.claude/commands/brainstorm.md
rm ~/.claude/commands/lab-meeting.md
rm ~/.claude/commands/review-hypothesis.md

# Remove rules
rm ~/.claude/rules/research-principles.md

# Your research data in ~/research/ is preserved
```

---

## Next Steps

1. Read the [README](README.md) for an overview
2. Review the example domain: `domains/stats-theory/DOMAIN.md`
3. Create your first project using templates
4. Run `/brainstorm` to generate hypotheses
5. Hold your first `/lab-meeting`

---

## Getting Help

- **Issues**: Open an issue on GitHub
- **Discussions**: Use GitHub Discussions
- **Original Framework**: Refer to [everything-claude-code docs](https://github.com/affaan-m/everything-claude-code)

---

Happy researching! 🔬
