---
name: hypothesize
description: AI Co-Scientist hypothesis generation with automatic multi-candidate generation and optional debate refinement
argument-hint: <research-goal>
allowed-tools: Read, WebSearch, Bash, Glob, Grep, Write, Task
---

# /hypothesize Command

Generate research hypotheses using AI Co-Scientist methodology with automatic multi-candidate generation and debate-based refinement.

## Purpose

This command automates the complete hypothesis generation workflow from Google's AI Co-Scientist paper:
1. **Generate** 3-5 competing hypotheses from different theoretical angles
2. **Compare** candidates based on novelty, feasibility, and impact
3. **Debate** (optional) - Refine top candidate through simulated expert panel
4. **Document** all hypotheses with structured metadata

## Usage

```bash
# Basic: Generate multiple hypotheses
/hypothesize "Identify optimal minimax rates for sparse high-dimensional regression"

# With debate refinement for top candidate
/hypothesize "Derive fundamental limits of causal inference from observational data" --debate

# Generate only (skip comparison)
/hypothesize "Explain policy adoption patterns in federal systems" --generate-only
```

## Workflow

```
User: /hypothesize [research goal]
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 1: Context Loading                │
    │  - Load DOMAIN.md (domain standards)    │
    │  - Load PROJECT.md (constraints)        │
    │  - Load existing HYPOTHESES.md          │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 2: Multi-Hypothesis Generation    │
    │  (Automatically spawns theorist-enhanced)│
    │  - Generate 3-5 distinct hypotheses     │
    │  - Different theoretical angles         │
    │  - Novelty-feasibility spectrum         │
    │  - Domain-specific compliance           │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 3: Automatic Comparison           │
    │  - Evaluate each on 5 dimensions        │
    │  - Identify top candidate               │
    │  - Generate comparison summary          │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 4: Optional Debate (if --debate)  │
    │  (Automatically spawns theorist-enhanced)│
    │  - 5-turn expert panel simulation       │
    │  - Theory/Methods/Domain experts        │
    │  - Refine based on critiques            │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 5: Documentation                  │
    │  - Create hypothesis files              │
    │  - Update HYPOTHESES.md                 │
    │  - Set initial Elo (1200)               │
    │  - Update STATE.md                      │
    └─────────────────────────────────────────┘
```

## What Happens Automatically

### 1. Domain Knowledge Injection
The command automatically:
- Reads your project's DOMAIN.md file
- Injects complete domain standards into theorist context
- Ensures hypotheses comply with publication requirements

### 2. Multi-Candidate Generation (Internal)
Behind the scenes, this spawns `theorist-enhanced` agent with:
- Mode: Multi-Hypothesis Generation (internal detail, user doesn't see)
- Full domain knowledge context
- Project constraints
- Generates 3-5 distinct hypotheses with different mechanisms

### 3. Automatic Evaluation
Compares candidates on:
- **Novelty** (25%): Unique contribution vs existing literature
- **Importance** (25%): Impact if true
- **Testability** (20%): Feasibility of verification
- **Domain Fit** (15%): Compliance with standards
- **Clarity** (15%): Well-formulated and specific

### 4. Optional Debate Refinement
If `--debate` flag provided:
- Spawns `theorist-enhanced` with debate mode (internal)
- Simulates 5-turn expert panel discussion:
  - Expert A (Theory): Defends hypothesis
  - Expert B (Methods): Challenges feasibility
  - Expert C (Domain): Checks novelty and standards
- Produces refined version: H-XXX-v2

## Output

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 HYPOTHESIZE ► [Research Goal]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## Generated Hypotheses (3)

### H-001-A: [Title] ⭐ Top Candidate
**Angle**: Mechanistic / Information-theoretic / Causal
**Novelty**: 4.5/5 (Novel approach to [aspect])
**Feasibility**: 4/5 (Testable with [method])
**Core Claim**: [One-sentence statement]
**Why This Matters**: [Impact if true]

### H-001-B: [Title]
**Angle**: [Different angle]
**Novelty**: 3.5/5
**Feasibility**: 4.5/5
**Core Claim**: [Statement]

### H-001-C: [Title]
**Angle**: [Yet another angle]
**Novelty**: 5/5 (Highly novel but...)
**Feasibility**: 2/5 (Challenging to test)
**Core Claim**: [Statement]

─────────────────────────────────────────────────────

## Recommendation

**Prioritize**: H-001-A
- Highest combined score (novelty + feasibility)
- Domain standards: Compliant
- Next step: `/stress-test H-001-A` for deep verification

**Alternative**: H-001-B (if primary blocked)
**Archive**: H-001-C (revisit when methodology advances)

─────────────────────────────────────────────────────

## Files Created

- hypotheses/proposals/H-001-A-[slug].md
- hypotheses/proposals/H-001-B-[slug].md
- hypotheses/proposals/H-001-C-[slug].md
- Updated: hypotheses/HYPOTHESES.md
- Updated: STATE.md

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Next: `/stress-test H-001-A` to verify hypothesis quality
```

## Options

| Flag | Effect | When to Use |
|------|--------|-------------|
| `--debate` | Enable debate refinement for top candidate | High-stakes hypotheses (grants, major papers) |
| `--generate-only` | Skip comparison, output all candidates | Exploratory phase, want maximum diversity |
| `--count=N` | Generate N hypotheses (default: 3-5) | Need more/fewer options |
| `--domain=path` | Override domain knowledge path | Multi-domain project |

## Integration with Other Commands

```
/hypothesize [goal]          ← Generate hypotheses automatically
        ↓
/stress-test H-001-A         ← Verify quality (novelty + assumptions + observations)
        ↓
[If gaps found]
/evolve H-001-A              ← Refine based on issues
        ↓
/execute-analysis H-001-A    ← Run verification plan
```

## Examples

### Example 1: Statistical Theory

```bash
/hypothesize "Derive minimax optimal rates for sparse PCA under heavy-tailed noise"

# Automatically generates:
# - H-001-A: Information-theoretic approach (Fano's method)
# - H-001-B: Computational approach (polynomial-time algorithm)
# - H-001-C: Robust approach (contamination model)

# Then compares and recommends H-001-A
```

### Example 2: Policy Research

```bash
/hypothesize "Explain why some states adopt climate policies and others don't" --debate

# Generates 3 hypotheses
# Selects top candidate
# Runs 5-turn expert debate
# Outputs refined H-002-A-v2 ready for testing
```

### Example 3: Biomedical Research

```bash
/hypothesize "Identify novel drug targets for AML treatment"

# Generates:
# - H-003-A: CXCR1/2 inhibitor mechanism
# - H-003-B: MEK pathway targeting
# - H-003-C: Epigenetic modifier approach

# Recommends based on novelty + clinical feasibility
```

## Behind the Scenes

**What You Don't Need to Know** (but happens automatically):

1. Command spawns `theorist-enhanced` agent via Task tool
2. Internally uses "mode: multi" parameter (not exposed to user)
3. Injects complete DOMAIN.md into agent context
4. Agent generates structured YAML output
5. System parses output, creates files, updates indices
6. If --debate: spawns second agent call with "mode: debate"
7. All coordination handled by Lab Manager

**Why This Design?**
- You focus on **intent** ("generate hypotheses")
- System handles **implementation** (which agent, which mode)
- Maintains **thought continuity** (single coherent process)
- Enables **automatic evolution** (results feed into next step)

## Important Notes

⚠️ **This is NOT a substitute for domain expertise**
- Hypotheses require your critical review
- Domain knowledge in DOMAIN.md should be up-to-date
- AI generates candidates; you select and validate

⚠️ **Quality over quantity**
- 3 good hypotheses beat 10 mediocre ones
- Use --debate for high-stakes situations
- Don't generate hypotheses without clear research goals

⚠️ **Follow-up is essential**
- Always run `/stress-test` on selected hypothesis
- Don't skip verification steps
- Expect multiple iterations (use `/evolve`)

## Troubleshooting

**"Hypotheses are too similar"**
- Solution: Provide more specific research goal
- Or: Check DOMAIN.md has diverse theoretical frameworks

**"Output doesn't match domain standards"**
- Solution: Verify DOMAIN.md is loaded correctly
- Check that domain-specific requirements are documented

**"Want more control over generation"**
- Solution: This is intentionally automated for thought continuity
- For manual control: Use `/brainstorm` command instead
- Remember: Co-Scientist value is in automatic coordination

## Comparison: /hypothesize vs /brainstorm

| Feature | /hypothesize | /brainstorm |
|---------|-------------|-------------|
| Approach | Automatic multi-candidate | Structured rounds with team |
| Agent | theorist-enhanced (single) | Multiple agents (Theorist, Experimentalist, etc.) |
| Output | 3-5 candidates ready for testing | 1-3 fully vetted proposals |
| Speed | Faster (automated) | Slower (deliberate) |
| Use When | Exploring theoretical space | Validating specific direction |
| Philosophy | Google AI Co-Scientist | Desktop seminar style |

Both are valid! Use `/hypothesize` for rapid hypothesis generation, `/brainstorm` for thorough team-based development.
