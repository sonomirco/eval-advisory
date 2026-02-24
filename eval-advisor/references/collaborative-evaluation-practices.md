# Collaborative Evaluation Practices

Use this guide when one reviewer is not enough to define quality reliably.

## Why Collaboration Matters

Some criteria are subjective, evolving, and domain-specific. A collaborative process helps teams:
- Make label definitions explicit
- Reduce hidden assumptions in judgment
- Improve consistency before automation
- Build a trustworthy labeled set for judge validation

## Team Design

### Default: Single accountable expert

When possible, use one accountable domain expert as the quality owner.

This works well when:
- Domain scope is narrow
- A clear subject-matter owner exists
- Speed is critical

### Multi-annotator mode

Use multiple annotators when:
- The domain has multiple valid stakeholder perspectives
- Regional/legal variation matters
- One person cannot cover the full scope

## Collaborative Annotation Workflow

1. Assemble annotators with domain context
2. Draft an initial binary rubric with Pass/Fail examples
3. Select a shared set of 20-50 representative traces
4. Label independently (no discussion during this phase)
5. Compute inter-annotator agreement (IAA)
6. Run alignment sessions on disagreements
7. Revise rubric language, rules, and examples
8. Re-run on fresh traces until agreement is acceptable
9. Freeze rubric + consensus labels as gold data

## Agreement Metrics (IAA)

### Percent agreement

Useful as a quick signal, but it can overstate agreement when one label dominates.

### Cohen's Kappa (two annotators)

Use for categorical labels with two raters.

- `kappa = (P_o - P_e) / (1 - P_e)`
- `P_o`: observed agreement
- `P_e`: chance agreement from marginal label distributions

Interpretation bands often used in practice:
- `<0.00`: poor
- `0.00-0.20`: slight
- `0.21-0.40`: fair
- `0.41-0.60`: moderate
- `0.61-0.80`: substantial
- `0.81-1.00`: almost perfect

A practical target for rubric reliability is often `kappa >= 0.60`.

### More than two annotators

For more than two raters, use multi-rater agreement metrics (for example, Fleiss-style approaches for nominal labels).

## Alignment Session Playbook

Goal: improve future consistency, not force retrospective consensus.

Use these techniques:
- Clarify vague terms
- Add concrete Pass/Fail examples for edge cases
- Add explicit decision rules
- Split overloaded criteria into smaller binary checks
- Escalate unresolved disputes to a designated owner
- Record every rubric change and rationale

Avoid overusing majority vote. It may settle labels but does not strengthen the rubric.

## Criteria Drift Management

Judgment criteria naturally evolve as teams review more traces.

Handle this explicitly:
- Version rubrics (`v1`, `v2`, ...)
- Keep a rubric changelog
- Re-check agreement after major changes
- Re-sample previously labeled traces to detect definition drift

## Hand-off to Automated Evaluation

Collaborative outputs should feed automation directly:
- Final rubric becomes judge instruction spec
- Consensus labels become validation data
- Disagreement clusters become few-shot edge cases

Before deployment:
- Confirm held-out judge validation with current rubric
- Confirm no leakage of evaluation examples into prompt development artifacts

## Common Failure Modes

- Skipping independent labeling before alignment
- Treating percent agreement as sufficient
- Using stale rubric versions for new labels
- Mixing annotation and prompt tuning in one uncontrolled loop
- Ignoring disagreement types (strictness vs ambiguity vs missing rule)
