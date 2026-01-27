# Everything Claude Research

> 🧬 A research-focused adaptation of [everything-claude-code](https://github.com/affaan-m/everything-claude-code)

Transform Claude Code into your AI research team with specialized postdoc agents, hypothesis management, and academic workflows.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## What is this?

**Everything Claude Research** is a framework that turns [Claude Code](https://claude.ai/code) into a virtual research team, complete with:

- 🧑‍🔬 **Senior Postdoc Agents**: Theorist, Experimentalist, Methodologist
- 🔬 **Hypothesis Management**: Elo ranking system for research ideas
- 📊 **Academic Workflows**: Lab meetings, brainstorming sessions, peer review
- 📚 **Domain Knowledge System**: Three-tier architecture (Global/Domain/Project)
- ✅ **Quality Assurance**: Integrated verification loops and eval harness

---

## Relationship to everything-claude-code

This project is **inspired by and builds upon** the excellent work of [@affaanmustafa](https://x.com/affaanmustafa)'s [everything-claude-code](https://github.com/affaan-m/everything-claude-code).

### What we reuse:
- ✅ Core skills: `iterative-retrieval`, `verification-loop`, `eval-harness`, `strategic-compact`
- ✅ Hook system architecture
- ✅ Plugin philosophy

### What's different:
- 🎯 **Target audience**: Academic researchers instead of software developers
- 👥 **Agents**: Research team (Theorist, Experimentalist, Methodologist) instead of dev team
- 🔄 **Workflows**: Hypothesis generation, lab meetings, peer review instead of TDD/code review
- 📚 **Knowledge system**: Three-tier domain knowledge architecture
- 🏆 **Elo ranking**: Hypothesis prioritization system

**If you're a software developer**, use [everything-claude-code](https://github.com/affaan-m/everything-claude-code).
**If you're an academic researcher**, this framework is for you.

---

## Quick Start

### 1. Prerequisites

- [Claude Code CLI](https://claude.ai/code) installed
- [everything-claude-code plugin](https://github.com/affaan-m/everything-claude-code) (for core skills)

### 2. Installation

```bash
# Clone this repository
git clone https://github.com/YOUR_USERNAME/everything-claude-research.git
cd everything-claude-research

# Copy agents to your Claude config
cp -r .claude/agents ~/.claude/

# Copy commands
cp -r .claude/commands ~/.claude/

# Copy rules
cp -r .claude/rules ~/.claude/

# Copy templates (optional)
cp -r shared/templates ~/research-templates/
```

See [SETUP.md](SETUP.md) for detailed installation instructions.

### 3. Your First Research Project

```bash
# Start a new research project
cd ~/research  # or your research directory

# Copy the project template
cp -r ~/research-templates/PROJECT_TEMPLATE.md ./my-project/PROJECT.md
cp -r ~/research-templates/CLAUDE_PROJECT.md ./my-project/CLAUDE.md

# Edit PROJECT.md with your research question

# Start brainstorming
/brainstorm "your research question"
```

---

## Framework Overview

### Research Team Agents

```
                    ┌─────────────┐
                    │     You     │  Direction, final decisions
                    │    (PI)     │
                    └──────┬──────┘
                           │
          ┌────────────────┼────────────────┐
          │                │                │
    ┌─────▼─────┐    ┌─────▼─────┐    ┌─────▼─────┐
    │  Theorist │    │Experiment-│    │ Methodo-  │
    │           │    │  alist    │    │  logist   │
    │ + iterative│   │ + verif.  │    │ + eval    │
    │  retrieval│    │   loop    │    │  harness  │
    └───────────┘    └───────────┘    └───────────┘
                           │
                    ┌──────▼──────┐
                    │ Lab Manager │  Progress tracking
                    │ + strategic │  Elo ranking
                    │   compact   │
                    └─────────────┘
```

#### Theorist (理論博後)
- Generates research hypotheses
- Conducts systematic literature reviews using **iterative-retrieval**
- Develops theoretical frameworks
- Tools: Read, WebSearch, Grep, Glob, Bash
- Model: Opus

#### Experimentalist (實驗博後)
- Evaluates hypothesis feasibility
- Designs verification strategies
- Runs **verification-loop** (Build → Functionality → Quality)
- Tools: Read, WebSearch, Grep, Glob, Bash
- Model: Opus

#### Methodologist (方法論專家)
- Reviews methodology and rigor
- Runs **eval-harness** for formal evaluation
- Calculates pass@k metrics
- Conducts meta-reviews across projects
- Tools: Read, Grep, Glob, Bash
- Model: Opus

#### Lab Manager (實驗室管理員)
- Tracks project progress
- Maintains Elo ranking system for hypotheses
- Suggests **strategic-compact** timing
- Coordinates resources
- Tools: Read, Write, Edit, Bash
- Model: Sonnet

---

## Research Commands

### Core Commands

```bash
/brainstorm [topic]          # Structured brainstorming session
/lab-meeting                 # Virtual lab meeting (weekly)
/review-hypothesis H-001     # Deep hypothesis review
```

### Quality Assurance (from plugin)

```bash
/checkpoint create milestone-1    # Save research checkpoint
/eval define H-001               # Define evaluation criteria
/eval check H-001                # Check evaluation progress
/verify                          # Run verification loop
```

---

## Domain Knowledge System

Three-tier architecture for knowledge management:

### Global Layer (`~/.claude/`)
- Agents, commands, rules
- Universal across all projects

### Domain Layer (`domains/`)
- Field-specific knowledge (e.g., stats-theory, policy-making)
- Shared across projects in same domain

### Project Layer (`projects/`)
- Specific research questions and data
- One project = one research question

```
domains/
├── stats-theory/
│   ├── DOMAIN.md              # Core theories, methods, standards
│   ├── literature/            # Key papers
│   └── terminology.md         # Domain-specific terms
│
└── policy-making/
    ├── DOMAIN.md
    └── literature/
```

---

## Research Workflow Example

```bash
# Week 1: Brainstorming
/brainstorm "minimax theory in high-dimensional statistics"
# → Theorist generates 5-10 ideas
# → Experimentalist filters for feasibility
# → Output: 2-3 structured hypotheses

# Week 2: Hypothesis Review
/review-hypothesis H-001
# → Theorist: theoretical soundness
# → Experimentalist: feasibility assessment
# → Methodologist: methodology review
# → Lab Manager: updates Elo ranking

# Week 3: Verification Design
/eval define H-001              # Define success criteria
# → Experimentalist designs verification
# → Methodologist reviews approach

# Week 4: Execution & Verification
# (Run analysis)
/verify                         # 3-phase verification
/eval check H-001              # Check against criteria

# Week 5: Lab Meeting
/lab-meeting
# → Review all hypotheses
# → Update Elo rankings
# → Assign next steps
# → Lab Manager suggests /compact

# Week 6: Publication Prep
/eval report H-001             # Final evaluation report
# → pass@3 ≥ 90%? Ready to publish
```

---

## Key Features

### 1. Iterative Retrieval (Theorist)
Systematic literature search with progressive refinement:
```
DISPATCH (broad search)
→ EVALUATE (relevance scoring)
→ REFINE (update query)
→ LOOP (max 3 cycles)
```

### 2. Verification Loop (Experimentalist)
Three-phase quality assurance:
```
Phase 1: BUILD (does it run?)
Phase 2: FUNCTIONALITY (is it correct?)
Phase 3: QUALITY (publication-ready?)
```

### 3. Eval Harness (Methodologist)
Formal evaluation framework:
- Define criteria BEFORE verification
- Calculate pass@k metrics
- Regression testing for reproducibility

### 4. Strategic Compact (Lab Manager)
Context management at logical boundaries:
- ✅ After lab meetings (preserve decisions)
- ✅ After brainstorming (preserve hypotheses)
- ❌ Not during active reasoning

### 5. Elo Ranking System
Hypothesis prioritization:
- Initial Elo: 1200
- Updated through pairwise comparisons
- Top-ranked hypotheses get resources

---

## Example Projects

See `projects/example-project/` for a complete example (coming soon).

---

## Directory Structure

```
research/
├── .claude/              # Framework configuration
│   ├── agents/           # 4 research agents
│   ├── commands/         # 3 research commands
│   └── rules/            # Research principles
│
├── domains/              # Domain knowledge
│   ├── stats-theory/
│   └── policy-making/
│
├── projects/             # Your research projects (gitignored)
│   └── [project-name]/
│       ├── CLAUDE.md             # Project config
│       ├── PROJECT.md            # Project info
│       ├── hypotheses/           # Hypothesis files
│       ├── meeting_notes/        # Lab meeting records
│       └── reviews/              # Review reports
│
└── shared/
    └── templates/        # Project templates
```

---

## Credits

This framework builds upon the excellent work of:

- **[everything-claude-code](https://github.com/affaan-m/everything-claude-code)** by [@affaanmustafa](https://x.com/affaanmustafa)
  - Core skills: `iterative-retrieval`, `verification-loop`, `eval-harness`, `strategic-compact`
  - Hook system architecture
  - Plugin philosophy

- **[The Shorthand Guide](https://x.com/affaanmustafa/status/2012378465664745795)** and **[The Longform Guide](https://x.com/affaanmustafa/status/2014040193557471352)** - Essential reading for understanding the underlying principles

---

## Contributing

Contributions are welcome! Please:

1. Fork this repository
2. Create a feature branch
3. Add your contribution (new agents, commands, domain examples)
4. Test with Claude Code
5. Submit a PR

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

---

## License

MIT License - same as [everything-claude-code](https://github.com/affaan-m/everything-claude-code)

This project is based on everything-claude-code by Affaan Mustafa (https://github.com/affaan-m/everything-claude-code), licensed under the MIT License.

---

## Roadmap

- [ ] Example research projects
- [ ] Domain templates (Economics, Psychology, Biology)
- [ ] Integration with reference managers (Zotero, Mendeley)
- [ ] Automated literature review pipelines
- [ ] Jupyter notebook integration for data analysis
- [ ] Paper writing assistant commands

---

## Links

- **Original Framework**: [everything-claude-code](https://github.com/affaan-m/everything-claude-code)
- **Claude Code**: [claude.ai/code](https://claude.ai/code)
- **Author's Guides**:
  - [Shorthand Guide](https://x.com/affaanmustafa/status/2012378465664745795)
  - [Longform Guide](https://x.com/affaanmustafa/status/2014040193557471352)

---

**Built with ❤️ for academic researchers**

If this helps your research, please star ⭐ this repo!
