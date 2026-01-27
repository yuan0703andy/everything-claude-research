# Claude Project Configuration

> Copy this template to a new project directory and rename as CLAUDE.md
> This file tells Claude how to configure the research team for this project
>
> **Important Supporting Files**:
> - `PROJECT.md` - Project definition (Vision, Scope, Goals)
> - `STATE.md` - Cross-session state management
> - `RESEARCH_PLAN.md` - Phase workflow planning

## Project Basic Information

**Project Name**: [Copy from PROJECT.md]
**Project Directory**: [Path to this project]
**Project Documentation**: Refer to PROJECT.md in this directory

## Domain Knowledge Configuration

### Primary Domain
**Domain**: `domains/[domain-name]/`

[Select one]
- [ ] `domains/stats-theory/` - Statistical theory
- [ ] `domains/policy-making/` - Policy research
- [ ] Other: [Specify path]

**Domain Files**:
- Core knowledge: `domains/[domain-name]/DOMAIN.md`
- Terminology: `domains/[domain-name]/terminology.md`
- Literature: `domains/[domain-name]/literature/`

### Secondary Domain (if interdisciplinary)
**Secondary Domain**: [If applicable]

**Domain Integration Explanation**:
[If this is an interdisciplinary project, explain how to integrate knowledge from both domains]
- Main framework from: [Domain A]
- Methodology from: [Domain B]
- When conflicts arise: [How to handle]

## Available Commands

### Phase Workflow (GSD-inspired)

**Phase 1: Context & Ideation**
```
/brainstorm [topic]      # Structured brainstorming (discussion phase)
                         # Output: .planning/phases/phase-N-CONTEXT.md
```

**Phase 2: Planning & Review**
```
/review-hypothesis H-XXX # In-depth hypothesis review (planning phase)
                         # Output: .planning/phases/phase-N-PLAN.md
                         #         reviews/H-XXX_review.md

/eval define H-XXX       # Define verification criteria (planning phase)
```

**Phase 3: Execution**
```
/execute-analysis        # Execute analysis (execution phase)
                         # Output: .planning/phases/phase-N-EXECUTE.md
                         #         results/, figures/
```

**Phase 4: Verification**
```
/verify-results          # Verify analysis results (verification phase)
                         # Output: .planning/phases/phase-N-VERIFY.md
                         #         reviews/H-XXX_verification_report.md

/eval check H-XXX        # Check if criteria met
```

### State Management (Cross-Session Continuity)

```
/progress                # Quick view of research progress
                         # When to use: Session start, need quick overview

/update-state            # Update STATE.md
                         # When to use: Session end, after key decisions
```

### Coordination

```
/lab-meeting             # Virtual lab meeting (weekly)
                         # Output: meeting_notes/YYYY-MM-DD.md
```

### Typical Workflow

```
┌─────────── Session N ───────────┐
│                                  │
│  1. /progress                    │  ← Understand current state
│      ↓                           │
│  2. [Select task]                │
│      ↓                           │
│  3. /brainstorm              OR  │  ← Phase 1-2
│     /review-hypothesis           │
│      ↓                           │
│  4. /execute-analysis            │  ← Phase 3
│      ↓                           │
│  5. /verify-results              │  ← Phase 4
│      ↓                           │
│  6. /update-state                │  ← Record progress
│                                  │
└──────────────────────────────────┘
        ↓
┌─────────── Session N+1 ──────────┐
│  /progress (read STATE.md)       │
│  [Continue work]                 │
└──────────────────────────────────┘
```

### Additional Commands (from plugin)

```
/checkpoint              # Save research checkpoint
/verify                  # 3-phase verification loop (generic)
/tdd                     # Test-driven development (when writing analysis functions)
/code-review             # Code review (analysis scripts)
```

## Research Team Specialization

> Adjust team members' expertise focus based on project needs

### Theorist (Theory Postdoc)
**Base Role**: `~/.claude/agents/senior-postdocs/theorist.md`

**Particularly Familiar With for This Project**:
- [Project-related theory 1]
- [Project-related theory 2]
- [Project-related key literature]

**Key Tasks**:
- [e.g., Develop extension of XX theory in YY context]
- [e.g., Integrate AA and BB theoretical frameworks]

### Experimentalist (Experimental Postdoc)
**Base Role**: `~/.claude/agents/senior-postdocs/experimentalist.md`

**Particularly Familiar With for This Project**:
- [Project-related method 1]
- [Project-related method 2]
- [Project-related data types]

**Key Tasks**:
- [e.g., Design causal inference strategy in XX context]
- [e.g., Handle YY type of data issues]

### Methodologist (Methodology Expert)
**Base Role**: `~/.claude/agents/senior-postdocs/methodologist.md`

**Particular Focus for This Project**:
- [Main methodological challenges of this project]
- [Validity threats for this project]
- [Review standards in this field]

**Review Focus**:
- [e.g., Multiple comparison issues in high-dimensional data]
- [e.g., Causal inference standards in observational studies]

### Lab Manager (Lab Manager)
**Base Role**: `~/.claude/agents/lab-manager/coordinator.md`

**Tracking for This Project**:
- Project progress: According to phases in RESEARCH_PLAN.md
- Hypothesis ranking: Maintain hypothesis Elo system for this project
- State management: Responsible for STATE.md and /progress, /update-state
- Resource allocation: [Specific resource constraints for this project]

## Skills Configuration

### Core Skills (from everything-claude-code)

**Theorist uses**:
- `iterative-retrieval` - 4-phase literature retrieval

**Experimentalist uses**:
- `verification-loop` - 3-phase verification (Build → Functionality → Quality)

**Methodologist uses**:
- `eval-harness` - Formal evaluation framework (pass@k metrics)

**Lab Manager uses**:
- `strategic-compact` - Strategic context compression suggestions

### Project-Specific Skills
[If additional skills needed, list here]

**Need to Load Additionally**:
- [ ] `causal-inference` - Causal inference methods
- [ ] `text-analysis` - Text analysis (if analyzing text)
- [ ] `machine-learning` - Machine learning methods
- [ ] Other: [Specify]

## Rules Configuration

### Global Rules (Must Follow)
- `~/.claude/rules/research-principles.md` - Research fundamental principles
- `~/.claude/rules/analysis-code-style.md` - Analysis code style
- `~/.claude/rules/research-workflow.md` - Research workflow
- `~/.claude/rules/validation.md` - Validation requirements
- `~/.claude/rules/ethics.md` - Research ethics

### Project-Specific Rules
[If this project has special requirements]

```markdown
## [Project Name] Specific Rules

### Data Processing Rules
- [Rule 1]
- [Rule 2]

### Analysis Rules
- [Rule 1]
- [Rule 2]

### Reporting Rules
- [Rule 1]
- [Rule 2]
```

## Project Constraints and Preferences

### Time Constraints
- **Deadline**: [If any]
- **Time Pressure**: [High/Medium/Low]
- **Priority**: [Priority among multiple projects]

### Resource Constraints
- **Computing Resources**: [Constraint description]
- **Data Constraints**: [Constraint description]
- **Personnel Constraints**: [Constraint description]

### Methodological Preferences
- **Preferred Methods**: [If specific preferences]
- **Avoid Methods**: [If specific limitations]
- **Methodological Innovation**: [Whether methodological innovation is encouraged]

### Risk Tolerance
- **Risk Preference**: [Conservative/Neutral/Aggressive]
- **Failure Tolerance**: [High/Medium/Low]
- **Innovation Requirement**: [High/Medium/Low]

## Output and Documentation Standards

### Core State Files (GSD-inspired)
```
STATE.md                 # Cross-session state (update each session)
RESEARCH_PLAN.md         # Phase workflow planning
PROJECT.md               # Project definition and scope
```

### Phase Records (Auto-generated)
```
.planning/
├── phases/
│   ├── phase-1-CONTEXT.md    # /brainstorm output
│   ├── phase-1-SUMMARY.md    # Phase 1 summary
│   ├── phase-2-PLAN.md       # /review-hypothesis output
│   ├── phase-2-SUMMARY.md
│   ├── phase-3-EXECUTE.md    # /execute-analysis output
│   ├── phase-3-VERIFY.md     # /verify-results output
│   └── phase-3-SUMMARY.md
└── research/                 # Literature research records
```

### Hypothesis Files
```
hypotheses/
├── HYPOTHESES.md            # Hypothesis ranking master table
├── H-001.md                 # Individual hypothesis details
├── H-002.md
└── ...
```

### Review Records
```
reviews/
├── H-001_review.md          # /review-hypothesis output
├── H-001_verification_report.md  # /verify-results output
└── ...
```

### Meeting Notes
```
meeting_notes/
├── 2024-01-27.md            # /lab-meeting output
└── ...
```

### Analysis Results
```
results/                     # /execute-analysis generated
figures/                     # /execute-analysis generated
data/
├── raw/                     # Raw data (gitignored)
└── processed/               # Processed data
```

### Code
```
analysis/                    # Analysis scripts
├── 01_load.R
├── 02_clean.R
├── 03_main_analysis.R
└── 04_robustness.R

functions/                   # Reusable functions
```

## Context Management

### State Files Priority

When starting a new session, read in priority order:
1. **STATE.md** - Most important (where did we leave off?)
2. **RESEARCH_PLAN.md** - Current phase and plan
3. **PROJECT.md** - Project vision and scope
4. **hypotheses/HYPOTHESES.md** - Hypothesis rankings

### Context Refresh Strategy (strategic-compact)

**Lab Manager will suggest compression timing**:
- ✅ After Lab Meeting ends
- ✅ After Brainstorming completes
- ✅ During Phase transitions
- ✅ Tool calls > 50

**Not recommended to compress**:
- ❌ During hypothesis review
- ❌ During complex reasoning
- ❌ Waiting for key decisions

## Collaboration and Communication

### Internal Communication
- **Lab Meeting**: [Frequency, e.g., Weekly]
- **Progress Updates**: Run `/update-state` at end of each session
- **Hypothesis Review**: [When triggered]

### External Communication
[If involving external collaborators]
- [Collaborator role]: [Communication method and frequency]

## Version Control

### Git Setup
- **Repository**: [If applicable]
- **Important**: STATE.md, RESEARCH_PLAN.md, PROJECT.md should all be committed
- **Commit Conventions**:
  - `hypothesis:` - Hypothesis-related
  - `analysis:` - Analysis execution
  - `review:` - Review records
  - `state:` - STATE.md updates

### Data Version Control
- **Strategy**: [How to track data versions]
- **Backup**: [Backup strategy]

## Startup Checklist

Before starting to use this project configuration, confirm:
- [ ] PROJECT.md is fully filled out (including Vision and Scope)
- [ ] STATE.md copied from template and initialized
- [ ] RESEARCH_PLAN.md copied from template and filled out
- [ ] DOMAIN.md is ready (at least has framework)
- [ ] Project directory structure established
  - [ ] hypotheses/, meeting_notes/, reviews/, .planning/
  - [ ] data/, results/, figures/, analysis/
- [ ] All Agents are configured
- [ ] First /brainstorming or hypothesis proposal done
- [ ] Lab Manager has established HYPOTHESES.md

## Startup Process

```bash
# Session 1: Setup
1. Copy templates to project directory
2. Fill out PROJECT.md (Vision, Scope, Goals)
3. Initialize STATE.md
4. Fill out RESEARCH_PLAN.md framework
5. /brainstorm "your research topic"
   → Generates phase-1-CONTEXT.md and preliminary hypotheses

# Session 2: Planning
1. /progress (review Session 1 outcomes)
2. /review-hypothesis H-001
   → Generates phase-2-PLAN.md and review
3. /update-state

# Session 3+: Execution Loop
1. /progress
2. /execute-analysis or /review-hypothesis (new)
3. /verify-results
4. /update-state
```

## Knowledge Accumulation Plan

### Expected Domain Knowledge Updates
After completing this project, expect to update:
- [ ] `domains/[xxx]/DOMAIN.md` - [What sections to update]
- [ ] `domains/[xxx]/literature/` - [What literature to add]
- [ ] `domains/[xxx]/methods.md` - [What methods to add]

### Lessons Learned
[Record at end of STATE.md after project completion]
- What worked well
- What needs improvement
- Recommendations for future similar projects

## Special Instructions

### Key Instructions for Lab Manager
- Strictly maintain STATE.md (this is key to cross-session continuity)
- Remind to run `/update-state` before each session ends
- Track phase transitions
- Suggest `/compact` at appropriate times

### Instructions for All Agents
- Check STATE.md to understand current state
- Follow phase process in RESEARCH_PLAN.md
- Record important decisions (will go into STATE.md)

## Notes

[Any other matters that need explanation]

---

**Configuration Created**: [Date]
**Last Updated**: [Date]
**Configuration Version**: 2.0 (GSD-integrated)
