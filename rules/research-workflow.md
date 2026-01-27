# Research Workflow

## Hypothesis File Naming

```
H-XXX_brief-title.md

Example: H-001_minimax-rates-sparse-models.md
```

## Commit Message Format

```
<type>: <description>

<optional body>

Types:
- hypothesis: New hypothesis or major update
- analysis: New analysis or experiment
- data: Data collection or processing
- docs: Documentation or notes
- fix: Bug fix in analysis code
- refactor: Code refactoring without results change
- literature: Literature review updates
```

## Research Project Workflow

### 1. Hypothesis Generation Phase

Use `/brainstorm` command:
1. **Theorist** generates ideas
2. **Experimentalist** filters for feasibility
3. Create hypothesis files in `hypotheses/`
4. Initial Elo ranking

### 2. Hypothesis Review Phase

Use `/review-hypothesis H-XXX`:
1. **Theorist** reviews theoretical soundness
2. **Experimentalist** assesses feasibility
3. **Methodologist** reviews methodology
4. Update Elo ranking
5. Decision: Proceed / Modify / Shelve

### 3. Verification Design Phase

Before running experiments:
1. Define success criteria with `/eval define H-XXX`
2. **Experimentalist** designs verification plan
3. **Methodologist** reviews approach
4. Document in `hypotheses/H-XXX_verification.md`

### 4. Execution Phase

Run analyses:
1. Follow verification plan
2. Use version-controlled analysis scripts
3. Document as you go in lab notebook
4. Save intermediate results

### 5. Verification Phase

Use `/verify`:
1. **Phase 1: Build** - Does code run?
2. **Phase 2: Functionality** - Are results correct?
3. **Phase 3: Quality** - Is it publication-ready?

Check against criteria: `/eval check H-XXX`

### 6. Lab Meeting Phase

Use `/lab-meeting`:
1. Review all active hypotheses
2. Update Elo rankings
3. Assign next steps
4. Identify blockers
5. **Lab Manager** suggests `/compact` if appropriate

## Git Workflow for Research

### Daily Work

```bash
# Start of day: Check status
git status

# During work: Commit frequently
git add hypotheses/H-001_minimax-rates.md
git commit -m "hypothesis: Add alternative formulation for Theorem 2"

# After analysis
git add analysis/01_simulation.R
git commit -m "analysis: Run simulation for H-001 under normal errors"

# End of day: Push to backup
git push origin main
```

### Important Results

```bash
# Tag important milestones
git tag -a "H-001-verified" -m "H-001 passed verification, ready for writeup"
git push origin --tags
```

### Hypothesis States

Track hypothesis status in git:

```bash
# New hypothesis
git add hypotheses/H-003_new-idea.md
git commit -m "hypothesis: H-003 new convergence rate conjecture"

# Update after review
git add hypotheses/H-003_new-idea.md
git commit -m "hypothesis: H-003 revised based on Methodologist feedback"

# Mark as verified
git add hypotheses/H-003_new-idea.md reviews/H-003_verification_report.md
git commit -m "hypothesis: H-003 verified (pass@3 = 100%)"
```

## Context Management

**Lab Manager** will suggest strategic compaction at:
- ✅ After lab meetings
- ✅ After brainstorming sessions
- ✅ After major phase completions
- ✅ When switching between hypotheses

**DO NOT** compact during:
- ❌ Active hypothesis review
- ❌ Complex theoretical reasoning
- ❌ Multi-round literature search
- ❌ Waiting for critical decisions

## Publication Workflow

### 1. Pre-Publication Checklist

Before starting writeup:
- [ ] Hypothesis verified (pass@3 ≥ 90%)
- [ ] All analyses reproducible
- [ ] Data documentation complete
- [ ] Code cleaned and commented
- [ ] Key results saved
- [ ] Figures publication-ready

### 2. Writing Phase

1. Create `writeup/` directory
2. Use LaTeX for technical papers
3. Keep text in git
4. Link to hypotheses and reviews
5. Include reproducibility section

### 3. Pre-Submission

1. Final `/verify` run
2. **Methodologist** meta-review
3. Check reproducibility on clean machine
4. Archive code and data

## Collaboration Workflow

### Adding Collaborators

1. Document in `PROJECT.md`
2. Assign hypotheses or tasks
3. Use branches for major independent work
4. Coordinate via lab meetings

### Code Review

Even in solo research:
1. **Experimentalist** reviews analysis code
2. **Methodologist** reviews methodology
3. Use review comments for audit trail
