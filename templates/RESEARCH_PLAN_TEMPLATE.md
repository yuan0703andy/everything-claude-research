# Research Plan

> Project: [Project Name]
> Created: [YYYY-MM-DD]
> Last Updated: [YYYY-MM-DD]

## Phase Overview

| Phase | Status | Start | Complete | Main Objective |
|-------|--------|-------|----------|----------------|
| 1. Context | ✅ | [Date] | [Date] | Literature review, problem definition |
| 2. Hypothesis | 🔄 | [Date] | - | Hypothesis generation and review |
| 3. Analysis | 📝 | - | - | Data analysis and verification |
| 4. Writeup | 📝 | - | - | Paper writing |

## Phase 1: Context (Literature Review & Problem Definition)

### Objectives
- Understand existing literature
- Identify research gaps
- Define research questions

### Plan
1. Systematic literature review
   - Use Theorist + iterative-retrieval
   - Cover [domain/topic]
   - Identify key papers
2. Define research questions
   - Clarify research questions
   - Determine research scope
   - Set success criteria

### Outputs
- [ ] `literature_review.md` - Literature review summary
- [ ] `research_gap.md` - Identified research gaps
- [ ] `PROJECT.md` - Complete project definition
- [ ] `.planning/phases/phase-1-CONTEXT.md` - Discussion record
- [ ] `.planning/phases/phase-1-SUMMARY.md` - Phase summary

### Completion Criteria
- [ ] At least 20 core papers reviewed
- [ ] Research question clear and testable
- [ ] Research gap clearly defined

---

## Phase 2: Hypothesis (Generation & Review)

### Objectives
- Generate multiple research hypotheses
- Assess hypothesis feasibility
- Prioritize hypotheses

### Plan
1. Hypothesis Generation (`/brainstorm`)
   - Theorist divergent thinking
   - Experimentalist feasibility screening
   - Generate 5-10 preliminary hypotheses

2. In-depth Hypothesis Review (`/review-hypothesis`)
   - Theorist: Theoretical foundation
   - Experimentalist: Feasibility
   - Methodologist: Methodology
   - Elo ranking system

3. Verification Design (`/eval define`)
   - Define success criteria
   - Design verification plan
   - Prepare data and tools

### Outputs
- [ ] `hypotheses/HYPOTHESES.md` - Hypothesis master table
- [ ] `hypotheses/H-001.md` through `H-00N.md` - Individual hypotheses
- [ ] `reviews/H-XXX_review.md` - Review reports
- [ ] `.planning/phases/phase-2-PLAN.md` - Execution plan
- [ ] `.planning/phases/phase-2-SUMMARY.md` - Phase summary

### Completion Criteria
- [ ] At least 3 fully reviewed hypotheses
- [ ] Top 2 hypotheses Elo > 1400
- [ ] Each hypothesis has verification design

---

## Phase 3: Analysis (Execute & Verify)

### Objectives
- Execute analysis to verify hypotheses
- Ensure robust and reliable results
- Pass verification checks

### Plan
1. Data Preparation
   - Data cleaning and validation
   - Exploratory data analysis
   - Assumption checking

2. Main Analysis (`/execute-analysis`)
   - Execute pre-specified analysis
   - Record all analysis steps
   - Save intermediate results

3. Robustness Checks
   - Sensitivity analysis
   - Alternative specifications
   - Subsample analysis

4. Verification (`/verify-results`)
   - 3-phase verification
   - pass@k metrics
   - Reproducibility testing

### Outputs
- [ ] `data/processed/` - Cleaned data
- [ ] `analysis/*.R` or `*.py` - Analysis scripts
- [ ] `results/` - Analysis results
- [ ] `figures/` - Figures
- [ ] `reviews/verification_report.md` - Verification report
- [ ] `.planning/phases/phase-3-SUMMARY.md` - Phase summary

### Completion Criteria
- [ ] All hypothesis verification complete
- [ ] pass@3 ≥ 90%
- [ ] Results reproducible in independent environment
- [ ] Robustness checks passed

---

## Phase 4: Writeup (Publication)

### Objectives
- Write paper
- Prepare submission materials
- Pass internal review

### Plan
1. Paper Structure
   - Introduction
   - Literature Review
   - Methodology
   - Results
   - Discussion
   - Conclusion

2. Internal Review
   - Methodologist methodological review
   - Team review
   - Revisions

3. Submission Preparation
   - Select target journal
   - Format adjustments
   - Supplementary materials

### Outputs
- [ ] `writeup/manuscript.tex` - Paper draft
- [ ] `writeup/supplement.pdf` - Supplementary materials
- [ ] `writeup/response_to_reviewers.md` - Reviewer response (if needed)
- [ ] `.planning/phases/phase-4-SUMMARY.md` - Phase summary

### Completion Criteria
- [ ] Complete paper draft
- [ ] Passed internal review
- [ ] All figures complete
- [ ] Submission materials ready

---

## Timeline

```
Phase 1 (Context)        ████████░░░░░░░░░░░░░░░░  4 weeks
Phase 2 (Hypothesis)     ░░░░░░░░████████░░░░░░░░  4 weeks
Phase 3 (Analysis)       ░░░░░░░░░░░░░░░░████████  6 weeks
Phase 4 (Writeup)        ░░░░░░░░░░░░░░░░░░░░░░██  2 weeks

                         Month 1      Month 2      Month 3      Month 4
```

Expected Total Time: [X] months

## Milestones

- [ ] **M1**: Literature review complete - [Target date]
- [ ] **M2**: Hypothesis ranking finalized - [Target date]
- [ ] **M3**: Main analysis complete - [Target date]
- [ ] **M4**: Paper first draft complete - [Target date]
- [ ] **M5**: Submission - [Target date]

## Risks & Mitigation

| Risk | Impact | Probability | Mitigation Strategy |
|------|--------|-------------|---------------------|
| Data unavailable | High | Medium | Backup data sources, adjust hypotheses |
| Insufficient sample size | Medium | Low | Power analysis, adjust expected effect sizes |
| Non-significant results | Medium | Medium | Exploratory analysis, adjust hypotheses |

## Resource Requirements

- **Data**: [List required data]
- **Computing**: [Computing resource requirements]
- **Software**: [Software tools list]
- **Personnel**: [Team members and division of labor]
- **Time**: [Expected time investment]

## Notes

[Other important information and reminders]
