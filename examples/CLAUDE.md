# Example Research Project CLAUDE.md

This is an example project-level CLAUDE.md file. Place this in your research project root.

## Project Overview

Statistical Theory Project: Minimax rates in high-dimensional sparse regression
Tech stack: R, Python (numpy, scipy), LaTeX

## Domain

Use domain knowledge: domains/stats-theory/

## Critical Rules

### 1. Reproducibility

- All random seeds fixed and documented
- All analysis scripts version-controlled
- Data processing pipeline documented
- Environment dependencies recorded (renv, conda)

### 2. Code Organization

- Small focused scripts (200-300 lines)
- `functions/` for reusable functions
- `analysis/` for main analyses
- `simulations/` for simulation studies
- Sequential numbering: `01_load.R`, `02_clean.R`, etc.

### 3. Analysis Style

- Set random seed at start of every script
- No magic numbers (use named constants)
- Comment complex derivations
- Save intermediate results
- Validate assumptions explicitly

### 4. Data Management

- Raw data in `data/raw/` (read-only)
- Processed data in `data/processed/`
- Results in `results/`
- Figures in `figures/`
- Never commit data files >10MB

### 5. Hypothesis Management

- Use Elo ranking system
- One hypothesis per file
- Clear status tracking
- Link reviews to hypotheses

## File Structure

```
.
├── CLAUDE.md                # This file
├── PROJECT.md               # Research overview
├── hypotheses/              # Hypothesis files
│   ├── H-001_minimax-rate.md
│   └── H-002_adaptive-estimation.md
├── meeting_notes/           # Lab meeting records
├── reviews/                 # Peer review reports
├── data/
│   ├── raw/                # Original data (gitignored)
│   └── processed/          # Clean data
├── analysis/               # Analysis scripts
│   ├── 01_setup.R
│   ├── 02_simulation.R
│   └── 03_real_data.R
├── functions/              # Reusable functions
│   ├── estimation.R
│   └── plotting.R
├── results/                # Analysis outputs
├── figures/                # Publication figures
└── writeup/                # Paper draft
```

## Key Research Patterns

### Hypothesis File Format

```markdown
# H-001: [Title]

## Hypothesis Statement
[Clear, testable statement]

## Theoretical Basis
[Why this might be true]

## Verification Plan
[How to test it]

## Expected Results
[What would support/refute this]

## Status
[Proposal/Review/Experiment/Verified/Shelved]

## Elo Ranking
[Current score and rank]
```

### Analysis Script Template

```r
# Description: [What this script does]
# Input: [Input files]
# Output: [Output files]
# Author: [Name]
# Date: [YYYY-MM-DD]

# Setup
set.seed(42)  # Always set seed
library(tidyverse)

# Constants
N_SIMULATIONS <- 1000  # Number of Monte Carlo runs
ALPHA <- 0.05          # Significance level

# Load data
data_raw <- read_csv("data/raw/study_data.csv")

# Analysis
# [Your analysis here]

# Save results
write_csv(results, "results/analysis_output.csv")
```

## Available Commands

Research commands:
- `/brainstorm` - Structured hypothesis generation
- `/lab-meeting` - Virtual lab meeting (with Elo tournament)
- `/review-hypothesis H-XXX` - Deep hypothesis review
- `/execute-analysis H-XXX` - Execute analysis with domain standards
- `/verify-results H-XXX` - Goal-backward verification with domain compliance
- `/progress` - Quick status overview
- `/update-state` - Update research state

🆕 AI Co-Scientist workflows:
- Multi-hypothesis: Use Task tool with theorist-enhanced (mode: multi)
- Scientific debate: Use Task tool with theorist-enhanced (mode: debate)
- Evolution: Use Task tool with theorist-enhanced (mode: evolve)
- Novelty check: Use Task tool with verifier-enhanced (mode: novelty)
- Observation matching: Use Task tool with verifier-enhanced (mode: observation)
- Assumption analysis: Use Task tool with verifier-enhanced (mode: assumption)

Quality assurance (from plugin):
- `/checkpoint` - Save research checkpoint
- `/eval define H-XXX` - Define evaluation criteria
- `/eval check H-XXX` - Check evaluation progress
- `/verify` - Run 3-phase verification

## Team Configuration

### Senior Postdocs (Opus)
- **Theorist**: Hypothesis generation, literature review
- **Experimentalist**: Feasibility assessment, verification design
- **Methodologist**: Methodology review, formal evaluation
- **Verifier**: Goal-backward verification, domain compliance checking

### 🆕 AI Co-Scientist Enhanced Agents (Opus)
- **theorist-enhanced**: Multi-hypothesis generation, scientific debate, evolution modes
- **verifier-enhanced**: Novelty verification, observation-hypothesis matching, assumption decomposition

### Lab Manager (Sonnet)
- Progress tracking
- Elo ranking maintenance
- Resource coordination
- Meeting facilitation

## Git Workflow

Conventional commits:
- `hypothesis:` - New or updated hypothesis
- `analysis:` - Analysis work
- `data:` - Data processing
- `fix:` - Bug fixes
- `docs:` - Documentation
- `literature:` - Literature review updates

Example:
```bash
git commit -m "hypothesis: H-001 revised theorem statement based on review"
git commit -m "analysis: Simulation study for H-001 under heavy-tailed errors"
```

## Statistical Standards

- Pre-specify main analyses when possible
- Report all analyses run, not just significant ones
- Check and report assumption violations
- Conduct sensitivity analyses
- Document all data exclusions
- Use appropriate corrections for multiple testing

## Verification Criteria

Before considering hypothesis verified:
- [ ] Code runs without errors (Build phase)
- [ ] Results match theoretical predictions (Functionality phase)
- [ ] Pass@3 ≥ 90% (Quality phase)
- [ ] All assumptions checked
- [ ] Sensitivity analyses complete
- [ ] Results reproducible on clean environment
- [ ] Documentation complete

## Context Management

Use `/compact` after:
- Lab meetings
- Brainstorming sessions
- Major phase completions
- Hypothesis transitions

Avoid compaction during:
- Active hypothesis review
- Multi-round literature search
- Complex theoretical derivations

## 🆕 AI Co-Scientist Workflow Example

### Week 1: Multi-Hypothesis Exploration
```
Use Task tool with theorist-enhanced agent:
Mode: Multi-Hypothesis Generation
Goal: Derive minimax optimal rates for sparse regression

Output: H-001-A, H-001-B, H-001-C (3 competing hypotheses)
```

### Week 2: Verify All Candidates
```
Use Task tool with verifier-enhanced agent (mode: novelty):
- Check H-001-A novelty
- Check H-001-B novelty
- Check H-001-C novelty

Output: H-001-B has highest novelty and feasibility
```

### Week 3: Deep Refinement via Debate
```
Use Task tool with theorist-enhanced agent:
Mode: Scientific Debate
Target: H-001-B

Output: H-001-B-v2 (refined through 5-turn expert panel)
```

### Week 4-5: Execute and Verify
```
/execute-analysis H-001-B-v2 (with domain standards)
/verify-results H-001-B-v2 (domain compliance check)

If gaps found: Use theorist-enhanced (mode: evolve)
```

## Notes

- This project uses R 4.3+ with {tidyverse}
- Python 3.10+ for numerical computations
- LaTeX for writeups (TeXLive 2023)
- Results are reproducible with `renv` lockfile
- AI Co-Scientist features available via enhanced agents (see README.md)
