# Improvement Experiment Checklist

Use this checklist for prompt, retrieval, architecture, or cost optimization experiments.

## Experiment Definition

- [ ] Problem statement is specific and measurable
- [ ] Hypothesis states expected mechanism of improvement
- [ ] Primary metric and guardrail metrics defined
- [ ] Baseline version and candidate version pinned

## Data Discipline

- [ ] Dev/test splits respected
- [ ] No test leakage into prompt revisions
- [ ] Evaluation set includes known failure cases

## Execution

- [ ] Ran baseline and candidate under same conditions
- [ ] Recorded quality metrics
- [ ] Recorded latency and cost metrics
- [ ] Logged notable error-class shifts

## Decision

- [ ] Candidate improves primary metric
- [ ] Candidate does not violate guardrails
- [ ] Candidate does not regress key known failures
- [ ] Rollback plan documented

## Adoption

- [ ] Promoted winning change into CI regression checks
- [ ] Updated monitoring to track affected failure mode(s)
- [ ] Documented follow-up experiments
