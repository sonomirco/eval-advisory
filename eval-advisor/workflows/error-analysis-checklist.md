# Error Analysis Checklist

Use this checklist before designing evaluators.

## Scope and Data

- [ ] Scope defined (features/workflows under review)
- [ ] Data source defined (production, synthetic bootstrap, or mixed)
- [ ] Started with a manageable sample (no fixed minimum to begin)
- [ ] Diverse sample assembled
- [ ] Sampling expanded until learning tapered (saturation signal checked)

## Open Coding

- [ ] Each trace labeled `Pass/Fail`
- [ ] First failure noted per failed trace (raw observation)
- [ ] No premature category forcing during annotation
- [ ] Annotations captured in a structured table (`trace_id`, `first_failure_note`, optional `pass_fail`)

## Axial Coding

- [ ] Open notes clustered into binary failure modes
- [ ] Category definitions and boundaries clarified
- [ ] Human/domain review completed

## Re-Coding and Quantification

- [ ] Binary trace-by-failure-mode matrix created (`1` present, `0` absent)
- [ ] Failure-mode prevalence computed with explicit denominators
- [ ] LLM-assisted mapping (if used) spot-checked and corrected by humans

## Iteration

- [ ] Additional traces reviewed after first taxonomy pass
- [ ] Saturation check performed (few/no new categories emerging)
- [ ] Taxonomy revised where overlaps or gaps were found
- [ ] If one glaring failure dominates early review, applied fix before scaling evaluator build
- [ ] High-priority modes received targeted exhaustive re-labeling (not first-failure only)

## Output Quality Gate

- [ ] Failure taxonomy is specific enough to drive evaluator decisions
- [ ] Top failure modes are prioritized by impact/frequency
- [ ] Next-step evaluator plan references observed failures only

## Pointers

- Method details: `references/error-analysis-guide.md`
- Taxonomy artifact: `templates/failure-taxonomy-template.md`
