# Example Research Project CLAUDE.md

This is an example project-level CLAUDE.md file. Place this in your research project root.

## Project Overview

Statistical Theory Project: Minimax rates in high-dimensional sparse regression
Tech stack: R, Python (numpy, scipy), LaTeX

## Domain

使用領域知識：domains/stats-theory/

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

## 假說陳述
[Clear, testable statement]

## 理論基礎
[Why this might be true]

## 驗證計畫
[How to test it]

## 預期結果
[What would support/refute this]

## 狀態
[提案/審查中/實驗中/已驗證/擱置]

## Elo 排名
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
- `/lab-meeting` - Virtual lab meeting
- `/review-hypothesis H-XXX` - Deep hypothesis review

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

## Notes

- This project uses R 4.3+ with {tidyverse}
- Python 3.10+ for numerical computations
- LaTeX for writeups (TeXLive 2023)
- Results are reproducible with `renv` lockfile
