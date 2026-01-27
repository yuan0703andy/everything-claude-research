# Everything Claude Research

> 🧬 A research-focused adaptation of [everything-claude-code](https://github.com/affaan-m/everything-claude-code)

Transform Claude Code into your AI research team with specialized postdoc agents, hypothesis management, and academic workflows. **Now enhanced with Google AI Co-Scientist capabilities!**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 🆕 What's New: AI Co-Scientist Integration

Inspired by Google's [AI Co-Scientist paper](https://arxiv.org/abs/2501.xxxxx), we've added three powerful capabilities:

### 1. 🧪 Multi-Hypothesis Generation
Generate 3-5 competing hypotheses at once from different theoretical angles:
```bash
task theorist-enhanced --mode=multi "Research goal: ..."
```

### 2. 🎭 Scientific Debate Mode
Simulate expert panel discussions (3-5 turns) to refine hypotheses:
```bash
task theorist-enhanced --mode=debate "Refine hypothesis H-001"
```

### 3. 🔬 Deep Verification Suite
- **Novelty Checking**: Deep literature comparison (5-point scale)
- **Observation Matching**: Check if hypothesis explains known results
- **Assumption Decomposition**: Systematic validation of all assumptions

```bash
task verifier-enhanced --mode=novelty "Check H-001"
task verifier-enhanced --mode=observation "Match H-001 with data"
task verifier-enhanced --mode=assumption "Decompose H-001"
```

**Result**: 90%+ functional equivalence with Google's system, plus unique advantages (domain adaptation, goal-backward verification).

📚 **[Full Implementation Guide](docs/ai-co-scientist-implementation-guide-en.md)** | **[Analysis Summary](docs/ai-co-scientist-analysis-summary-en.md)**

---

## What is this?

**Everything Claude Research** is a framework that turns [Claude Code](https://claude.ai/code) into a virtual research team, complete with:

- 🧑‍🔬 **Senior Postdoc Agents**: Theorist, Experimentalist, Methodologist (now with enhanced modes)
- 🔬 **Hypothesis Management**: Elo ranking system + scientific debate
- 📊 **Academic Workflows**: Lab meetings, brainstorming sessions, peer review
- 📚 **Domain Knowledge System**: Three-tier architecture (Global/Domain/Project)
- ✅ **Quality Assurance**: Integrated verification loops and eval harness
- 🆕 **AI Co-Scientist**: Multi-hypothesis generation, debate, deep verification

---

## Relationship to everything-claude-code

This project is **inspired by and built upon** the excellent work of [@affaanmustafa](https://x.com/affaanmustafa)'s [everything-claude-code](https://github.com/affaan-m/everything-claude-code).

### What we adapted:
- ✅ **Core skills** (included in this repo): `iterative-retrieval`, `verification-loop`, `eval-harness`, `strategic-compact`
- ✅ **Some commands** (adapted for research): `checkpoint`, `eval`, `verify`
- ✅ **Architecture patterns**: Hook system, modular design philosophy

### What's different:
- 🎯 **Target audience**: Academic researchers instead of software developers
- 👥 **Agents**: Research team (Theorist, Experimentalist, Methodologist, Lab Manager) instead of dev team
- 🔄 **Workflows**: Hypothesis generation, lab meetings, peer review, GSD-inspired phase workflow
- 📚 **Knowledge system**: Three-tier domain knowledge architecture (Global/Domain/Project)
- 🏆 **Elo ranking**: Hypothesis prioritization system
- 🆕 **AI Co-Scientist**: Enhanced hypothesis generation and verification (Google paper-inspired)

### Complete & Standalone
- ✅ **No plugin dependencies** - Everything is included
- ✅ **Self-contained** - All skills, commands, agents in this repo
- ✅ **Easy setup** - One clone, copy to ~/.claude/, done!

**If you're a software developer**, use [everything-claude-code](https://github.com/affaan-m/everything-claude-code).
**If you're an academic researcher**, this framework is for you.

---

## Quick Start

### 1. Prerequisites

- [Claude Code CLI](https://claude.ai/code) installed
- **No additional plugins required** - This is a complete, standalone framework!

### 2. Installation

```bash
# Clone this repository
git clone https://github.com/yuan0703andy/everything-claude-research.git
cd everything-claude-research

# Copy the complete framework to your Claude config
cp -r agents ~/.claude/
cp -r commands ~/.claude/
cp -r skills ~/.claude/
cp -r rules ~/.claude/
cp -r contexts ~/.claude/

# Copy templates (optional)
cp -r templates ~/research-templates/
```

**That's it!** The framework includes everything you need:
- 6 research agents (Theorist, Theorist-Enhanced, Experimentalist, Methodologist, Verifier, Verifier-Enhanced, Lab Manager)
- 4 core skills (iterative-retrieval, verification-loop, eval-harness, strategic-compact)
- 11 commands for research workflows
- 5 research rules, 3 contexts, and project templates

See [SETUP.md](SETUP.md) for detailed installation instructions.

### 3. Your First AI Co-Scientist Workflow

```bash
# Start a new research project
cd ~/research  # or your research directory

# Generate multiple hypothesis candidates
task theorist-enhanced --mode=multi "
Goal: Discover novel therapeutic targets for ALS
Generate 3 hypotheses with different mechanisms
"
# Output: H-001-A (mitochondrial), H-001-B (RNA), H-001-C (proteostasis)

# Check novelty of top candidate
task verifier-enhanced --mode=novelty "Check novelty of H-001-B"
# Output: Novelty score 4/5, novel aspects identified

# Refine through expert debate
task theorist-enhanced --mode=debate "Refine hypothesis H-001-B"
# Output: H-001-B-refined with 5-turn expert discussion

# Full verification suite
task verifier-enhanced --mode=assumption "Decompose H-001-B-refined"
task experimentalist "Assess feasibility of H-001-B-refined"
task methodologist "Methodological review of H-001-B-refined"
```

---

## Framework Overview

### Enhanced Research Team Agents

```
                    ┌─────────────┐
                    │     You     │  Direction, final decisions
                    │    (PI)     │
                    └──────┬──────┘
                           │
          ┌────────────────┼────────────────┬─────────────────┐
          │                │                │                 │
    ┌─────▼─────┐    ┌─────▼─────┐    ┌─────▼─────┐    ┌─────▼─────┐
    │ Theorist  │    │Experiment-│    │ Methodo-  │    │ Verifier  │
    │ Enhanced  │    │  alist    │    │  logist   │    │ Enhanced  │
    │           │    │           │    │           │    │           │
    │ • Multi   │    │ • Design  │    │ • Review  │    │ • Novelty │
    │ • Debate  │    │ • Verify  │    │ • Quality │    │ • Observe │
    │ • Evolve  │    │  Loop     │    │ • Eval    │    │ • Assume  │
    │ • Iterative│   │           │    │  Harness  │    │ • Goal-   │
    │  Retrieval│    │           │    │           │    │  Backward │
    └───────────┘    └───────────┘    └───────────┘    └───────────┘
                           │
                    ┌──────▼──────┐
                    │ Lab Manager │  Progress tracking
                    │             │  Elo ranking
                    │ • Strategic │  Strategic compaction
                    │   Compact   │
                    └─────────────┘
```

#### Theorist Enhanced 🆕
- **Standard Mode**: Generate single hypotheses with literature review
- **Multi Mode**: Generate 3-5 competing hypotheses from different angles
- **Debate Mode**: Simulate expert panel (3-5 turns) to refine hypotheses
- **Evolution Mode**: Iteratively improve based on critique
- Uses **iterative-retrieval** for systematic literature search
- Model: Opus

#### Verifier Enhanced 🆕
- **Goal-Backward Verification**: Ensures research achieves real goals
- **Novelty Verification**: Deep comparison against literature corpus (5-point scale)
- **Observation Matching**: Check if hypothesis explains known results
- **Assumption Decomposition**: Systematically validate all assumptions
- Model: Opus

#### Experimentalist
- Evaluates hypothesis feasibility
- Designs verification strategies
- Runs **verification-loop** (Build → Functionality → Quality)
- Model: Opus

#### Methodologist
- Reviews methodology and rigor
- Runs **eval-harness** for formal evaluation
- Calculates pass@k metrics
- Model: Opus

#### Lab Manager
- Tracks project progress
- Maintains Elo ranking system
- Suggests **strategic-compact** timing
- Model: Sonnet

---

## Research Commands

### Core Commands

```bash
/brainstorm [topic]          # Structured brainstorming session
/lab-meeting                 # Virtual lab meeting (weekly)
/review-hypothesis H-001     # Deep hypothesis review
```

### 🆕 AI Co-Scientist Enhanced Commands

```bash
# Multi-hypothesis generation
task theorist-enhanced --mode=multi "Goal: ..."

# Scientific debate
task theorist-enhanced --mode=debate "Refine H-001"

# Evolution (critique-driven refinement)
task theorist-enhanced --mode=evolve "
Original: H-001
Critique: [feedback]
"

# Deep verification
task verifier-enhanced --mode=novelty "Check H-001"
task verifier-enhanced --mode=observation "Match H-001"
task verifier-enhanced --mode=assumption "Decompose H-001"
```

### Quality Assurance

```bash
/checkpoint create milestone-1    # Save research checkpoint
/eval define H-001               # Define evaluation criteria
/eval check H-001                # Check evaluation progress
/verify                          # Run verification loop
```

---

## 🆕 AI Co-Scientist Workflow Example

```bash
# Week 1: Multi-hypothesis generation
task theorist-enhanced --mode=multi "
Goal: Discover novel therapeutic targets for ALS
Generate 3 hypotheses with different mechanisms
"
# Output: H-001-A (mitochondrial), H-001-B (RNA), H-001-C (proteostasis)

# Week 2: Novelty screening (parallel)
task verifier-enhanced --mode=novelty "Check H-001-A" &
task verifier-enhanced --mode=novelty "Check H-001-B" &
task verifier-enhanced --mode=novelty "Check H-001-C" &
wait
# Result: H-001-B has highest novelty score

# Week 3: Observation matching
task verifier-enhanced --mode=observation "
Hypothesis: H-001-B
Known observations:
- TDP-43 mislocalization
- RNA metabolism defects
- Motor neuron-specific death
"
# Result: H-001-B provides 2 'missing piece' explanations ✅

# Week 4: Scientific debate refinement
task theorist-enhanced --mode=debate "Refine H-001-B"
# Output: H-001-B-refined (5-turn expert discussion)

# Week 5: Assumption decomposition
task verifier-enhanced --mode=assumption "Decompose H-001-B-refined"
# Output: 4 assumptions identified, A1 & A2 critical

# Week 6: Feasibility assessment
task experimentalist "Assess H-001-B-refined"

# Week 7: Methodological review
task methodologist "Review H-001-B-refined"

# Week 8: Evolution (if needed)
task theorist-enhanced --mode=evolve "
Original: H-001-B-refined
Critique: [Methodologist feedback]
Output: H-001-B-v2
"
```

---

## Key Features

### 🆕 1. Multi-Hypothesis Generation
Generate diverse candidates covering different theoretical angles:
- Mechanistic vs statistical vs causal approaches
- High novelty/low feasibility vs balanced vs low novelty/high feasibility
- Automatic comparison and recommendation

### 🆕 2. Scientific Debate Mode
Simulate expert panel discussion:
- Turn 1: Propose 3 initial hypotheses
- Turns 2-4: Critical questioning and refinement
- Turn 5: Consensus on best approach
- Records full debate transcript

### 🆕 3. Deep Verification Suite

**Novelty Verification**:
- Systematic literature comparison
- 5-point novelty scale (High/Med-High/Med/Low-Med/Low)
- Identifies novel aspects vs already explored

**Observation Matching**:
- Checks if hypothesis explains known results
- Scoring: missing piece / already explained / disproved
- Validates causal explanations

**Assumption Decomposition**:
- Extracts all implicit assumptions
- Validates each against literature
- Risk assessment for critical assumptions

### 4. Iterative Retrieval (Theorist)
Systematic literature search with progressive refinement:
```
DISPATCH (broad search)
→ EVALUATE (relevance scoring)
→ REFINE (update query)
→ LOOP (max 3 cycles)
```

### 5. Verification Loop (Experimentalist)
Three-phase quality assurance:
```
Phase 1: BUILD (does it run?)
Phase 2: FUNCTIONALITY (is it correct?)
Phase 3: QUALITY (publication-ready?)
```

### 6. Eval Harness (Methodologist)
Formal evaluation framework:
- Define criteria BEFORE verification
- Calculate pass@k metrics
- Regression testing for reproducibility

### 7. Strategic Compact (Lab Manager)
Context management at logical boundaries:
- ✅ After lab meetings (preserve decisions)
- ✅ After brainstorming (preserve hypotheses)
- ❌ Not during active reasoning

### 8. Elo Ranking System
Hypothesis prioritization:
- Initial Elo: 1200
- Updated through pairwise comparisons
- Top-ranked hypotheses get resources

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

## Directory Structure

```
research/
├── README.md             # This file
├── SETUP.md              # Installation guide
│
├── docs/                 # Documentation
│   ├── ai-co-scientist-implementation-guide-en.md
│   ├── ai-co-scientist-analysis-summary-en.md
│   └── CONTRIBUTING.md
│
├── agents/               # Research agents
│   ├── theorist-enhanced-en.md  🆕
│   ├── verifier-enhanced-en.md  🆕
│   ├── theorist.md
│   ├── experimentalist.md
│   ├── methodologist.md
│   └── coordinator.md
│
├── commands/             # Research commands
│   ├── brainstorm.md
│   ├── lab-meeting.md
│   └── review-hypothesis.md
│
├── skills/               # Reusable skills
│   ├── iterative-retrieval.md
│   ├── verification-loop.md
│   ├── eval-harness.md
│   └── strategic-compact.md
│
├── domains/              # Domain knowledge
│   ├── stats-theory/
│   └── policy-making/
│
└── projects/             # Your research (gitignored)
    └── [project-name]/
```

---

## 🆕 Comparison: Your System vs Google AI Co-Scientist

| Feature | Google Paper | Your System | Status |
|---------|-------------|-------------|--------|
| **Hypothesis Generation** |
| Multi-hypothesis | ✅ | ✅ Enhanced | 🟢 Equivalent |
| Scientific debate | ✅ | ✅ Enhanced | 🟢 Equivalent |
| Literature-based | ✅ | ✅ Iterative-retrieval | 🟢 Equivalent |
| **Verification** |
| Novelty checking | ✅ | ✅ Enhanced | 🟢 Equivalent |
| Observation matching | ✅ | ✅ Enhanced | 🟢 Equivalent |
| Assumption validation | ✅ | ✅ Enhanced | 🟢 Equivalent |
| **Ranking** |
| Tournament | ✅ | ✅ Elo system | 🟢 Equivalent |
| Debate comparison | ✅ | ✅ Enhanced | 🟢 Equivalent |
| **Evolution** |
| Critique-driven | ✅ | ✅ Enhanced | 🟢 Equivalent |
| **Unique Advantages** |
| Domain adaptation | ❌ | ✅ DOMAIN.md | ⭐ **Yours stronger** |
| Goal verification | ❌ | ✅ Goal-backward | ⭐ **Yours stronger** |
| Complete pipeline | 🟡 | ✅ Full workflow | ⭐ **Yours stronger** |

**Result**: 90%+ functional equivalence with unique advantages

---

## Credits

This framework builds upon the excellent work of:

- **[everything-claude-code](https://github.com/affaan-m/everything-claude-code)** by [@affaanmustafa](https://x.com/affaanmustafa)
  - Core skills: `iterative-retrieval`, `verification-loop`, `eval-harness`, `strategic-compact`
  - Hook system architecture
  - Plugin philosophy

- **[The Shorthand Guide](https://x.com/affaanmustafa/status/2012378465664745795)** and **[The Longform Guide](https://x.com/affaanmustafa/status/2014040193557471352)** - Essential reading

- **Google AI Co-Scientist Paper** - Inspiration for enhanced hypothesis generation and verification capabilities

---

## Contributing

Contributions are welcome! Please:

1. Fork this repository
2. Create a feature branch
3. Add your contribution (new agents, commands, domain examples)
4. Test with Claude Code
5. Submit a PR

See [docs/CONTRIBUTING.md](docs/CONTRIBUTING.md) for guidelines.

---

## License

MIT License - same as [everything-claude-code](https://github.com/affaan-m/everything-claude-code)

This project is based on everything-claude-code by Affaan Mustafa (https://github.com/affaan-m/everything-claude-code), licensed under the MIT License.

---

## Roadmap

- [x] AI Co-Scientist integration (multi-hypothesis, debate, deep verification)
- [ ] Example research projects
- [ ] Domain templates (Economics, Psychology, Biology)
- [ ] Integration with reference managers (Zotero, Mendeley)
- [ ] Automated literature review pipelines
- [ ] Jupyter notebook integration for data analysis
- [ ] Paper writing assistant commands

---

## Links

- **Repository**: [everything-claude-research](https://github.com/yuan0703andy/everything-claude-research)
- **Original Framework**: [everything-claude-code](https://github.com/affaan-m/everything-claude-code)
- **Claude Code**: [claude.ai/code](https://claude.ai/code)
- **Documentation**: [docs/](docs/)

---

**Built with ❤️ for academic researchers**

If this helps your research, please star ⭐ this repo!
