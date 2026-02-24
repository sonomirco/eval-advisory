# Collaborative Annotation Checklist

Use this checklist when multiple reviewers define or apply an evaluation rubric.

## Setup

- [ ] Defined a single quality owner (or clear escalation owner)
- [ ] Selected annotators with domain context
- [ ] Drafted binary rubric with Pass/Fail criteria
- [ ] Added at least 4-8 clear edge-case examples

## First Round

- [ ] Selected shared set of 20-50 representative traces
- [ ] Enforced independent labeling (no discussion during labeling)
- [ ] Captured per-trace rationale where rubric ambiguity appears

## Agreement

- [ ] Computed observed agreement
- [ ] Computed chance-corrected agreement metric (two-rater or multi-rater as appropriate)
- [ ] Agreement met target threshold (for example, `kappa >= 0.60`) or proceeded to alignment

## Alignment Sessions

- [ ] Reviewed only disagreement cases
- [ ] Identified root cause: wording, missing rule, missing example, label confusion
- [ ] Updated rubric definitions and decision rules
- [ ] Added new edge-case examples to rubric
- [ ] Logged rubric version and rationale for changes

## Iteration

- [ ] Repeated independent labeling on fresh traces
- [ ] Recomputed agreement after rubric update
- [ ] Repeated until agreement stabilized

## Hand-off

- [ ] Published frozen rubric version for automation
- [ ] Produced consensus-labeled set for judge validation
- [ ] Linked disagreement classes to few-shot examples
- [ ] Documented known unresolved ambiguities
