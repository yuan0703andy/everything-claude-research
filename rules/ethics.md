# Research Ethics Guidelines

## Mandatory Ethics Checks

Before ANY analysis or publication:

- [ ] IRB approval obtained (if applicable)
- [ ] Informed consent documented
- [ ] Data privacy protected
- [ ] No p-hacking or result manipulation
- [ ] All analyses reported (not just significant)
- [ ] Conflicts of interest disclosed
- [ ] Proper attribution and credit given
- [ ] Reproducibility materials prepared

## Data Privacy

### Personal Data Protection

```r
# NEVER: Keep identifiers with analysis data
analysis_data <- raw_data  # Has names, emails, etc.

# ALWAYS: Strip identifiers, keep mapping separate
analysis_data <- raw_data %>%
  select(-name, -email, -ssn, -phone) %>%
  mutate(participant_id = paste0("P", row_number()))

# Keep ID mapping in SEPARATE secure file
id_mapping <- raw_data %>%
  select(participant_id = row_number(), name, email)
write_csv(id_mapping, "secure/id_mapping.csv")  # Not in git!
```

### De-identification Standards

Minimum requirements:
- Remove direct identifiers (names, emails, phone numbers, addresses)
- Remove indirect identifiers (birth dates, rare characteristics)
- Aggregate geographic data (ZIP → region)
- Top-code/bottom-code extreme values
- Add noise if necessary (differential privacy)

### Secure Storage

```bash
# Analysis data (de-identified): OK in git
data/processed/study_data.csv

# Raw data with identifiers: NEVER in git
# Store in:
# - Institutional secure storage
# - Encrypted local drive
# - Password-protected with 2FA
```

## Result Integrity

### NEVER

```r
# WRONG: p-hacking
for (control in all_possible_controls) {
  model <- lm(y ~ x + control, data = data)
  if (summary(model)$coefficients["x", "Pr(>|t|)"] < 0.05) {
    print("Found significant result!")
    break  # Stop and report only this one
  }
}

# WRONG: HARKing (Hypothesizing After Results Known)
# See result → make up hypothesis → claim it was predicted

# WRONG: Selective reporting
if (p_value < 0.05) {
  write_paper(result)  # Only report significant findings
}
```

### ALWAYS

```r
# CORRECT: Pre-specify analyses
preregistration <- list(
  hypothesis = "X increases Y",
  model = "lm(Y ~ X + age + gender)",
  sample_size = 200,
  alpha = 0.05
)
# Register BEFORE data collection/analysis

# CORRECT: Report all analyses
all_results <- list(
  main_analysis = run_main(),
  sensitivity_1 = run_sensitivity_1(),
  sensitivity_2 = run_sensitivity_2(),
  failed_analysis = run_alternative()  # Even if p > 0.05
)

# CORRECT: Distinguish exploratory vs confirmatory
paper <- list(
  confirmatory = preregistered_results,
  exploratory = post_hoc_findings  # Clearly labeled
)
```

## Reproducibility Ethics

### Transparency Requirements

```markdown
## Transparency Statement

### Data Availability
- De-identified data: [repository link or "available upon request"]
- Raw data: [explain restrictions, e.g., IRB, HIPAA]
- Synthetic data: [if provided as alternative]

### Code Availability
- Analysis code: [GitHub repository]
- Reproduction package: [how to run]
- Software versions: [renv.lock or requirements.txt]

### Pre-registration
- Pre-registered: [Yes/No]
- Registry: [OSF, AsPredicted, ClinicalTrials.gov]
- Registration number: [if applicable]
- Deviations: [explain any deviations from pre-reg]

### Materials
- Experimental materials: [available at...]
- Survey instruments: [available at...]
- Protocols: [available at...]
```

## Attribution and Credit

### Authorship Criteria

MUST meet ALL criteria:
1. Substantial intellectual contribution
2. Drafting or critical revision
3. Final approval of version
4. Accountability for accuracy

### Proper Citation

```r
# ALWAYS: Cite methods and software
# In paper:
# "Analyses conducted in R (R Core Team, 2023) using
#  tidyverse (Wickham et al., 2019) and lme4 (Bates et al., 2015)"

# In code:
citation("lme4")
toBibtex(citation("lme4"))
```

### Data Sources

```markdown
## Data Attribution

Original data source: [citation]
Data accessed: [date]
Data version: [if applicable]
Terms of use: [any restrictions]
Modifications: [describe any changes made]
```

## Conflict of Interest

### Disclosure Requirements

```markdown
## Conflicts of Interest

[Author A]: No conflicts to declare
[Author B]: Consultant for [Company], unrelated to this work
[Author C]: Funding from [Source], details: [grant number]
```

### Funding Transparency

```markdown
## Funding

This research was supported by:
- [Funder 1]: [Grant number, amount if required]
- [Funder 2]: [Grant number]

The funders had no role in:
- Study design
- Data collection and analysis
- Decision to publish
- Preparation of the manuscript
```

## Ethical Review Process

### Research with Human Participants

```markdown
## Ethics Statement

This study was approved by [Institution] IRB (Protocol #XXXX-XXX).

Participants:
- Provided written informed consent
- Could withdraw at any time
- Were debriefed after participation
- Data were de-identified before analysis

Risks and Benefits:
- Minimal risk: [describe]
- Compensation: [amount/type]
- Benefits: [individual and/or societal]
```

### Animal Research

```markdown
## Animal Ethics

This study was approved by [Institution] IACUC (Protocol #XXXX).

Procedures followed:
- [Relevant guidelines, e.g., ARRIVE, NIH]
- Housing conditions: [describe]
- Humane endpoints: [criteria for euthanasia]
- Minimization: [efforts to reduce number of animals]
```

## Responsible AI/ML Use

If using AI/ML tools (including Claude):

```markdown
## AI Assistance

AI tools used:
- [Tool name, e.g., Claude, GPT-4]: [specific tasks]
- Usage: [code generation, literature search, writing assistance]
- Verification: [all outputs verified by human researchers]
- Transparency: [what was AI-assisted vs human-generated]

Final responsibility: Authors take full responsibility for all content
```

## Ethics Violations: Response Protocol

If you discover ethical issues:

### CRITICAL: Stop immediately
- Fabricated data
- Plagiarism
- Failure to obtain required approvals
- Privacy breach
- Result manipulation

**Actions:**
1. Stop all related work
2. Notify PI/supervisor immediately
3. Document the issue
4. Consult institution's research integrity office
5. Do NOT publish or share results

### HIGH: Address before proceeding
- Missing informed consent documentation
- Incomplete IRB approval
- Undisclosed conflicts of interest
- Improper attribution

**Actions:**
1. Pause work
2. Correct the issue
3. Document correction
4. Seek guidance if uncertain
5. Update all materials

### MEDIUM: Document and correct
- Minor citation errors
- Incomplete methods documentation
- Small reproducibility gaps

**Actions:**
1. Note the issue
2. Correct promptly
3. Verify correction
4. Update documentation

## Pre-Publication Ethics Checklist

Before submitting manuscript:

- [ ] IRB approval obtained and documented
- [ ] Informed consent procedures described
- [ ] Data privacy protected
- [ ] All analyses reported (not cherry-picked)
- [ ] Exploratory vs confirmatory clearly marked
- [ ] Pre-registration documented (if applicable)
- [ ] Deviations from pre-registration explained
- [ ] Conflicts of interest disclosed
- [ ] Funding sources listed
- [ ] Proper attribution given
- [ ] Data/code availability stated
- [ ] Reproducibility package prepared
- [ ] Co-authors approved final version
- [ ] Ethics statement included

## Resources

- Institution's Research Integrity Office
- IRB/Ethics Committee
- [Your discipline]'s Ethics Guidelines
- COPE (Committee on Publication Ethics)
- FAIR Data Principles
- Open Science Framework (osf.io)

## Notes

Ethics is not just compliance—it's fundamental to good science. When in doubt, consult with your institution's research integrity office.
