# Eval Design Decision Tree

Use this workflow to choose the minimum-cost evaluator that still gives reliable signal.

## Gate 1: Error Analysis Complete?

- [ ] Confirm observed failure modes exist from real traces
- [ ] If not complete: stop and run `workflows/error-analysis-checklist.md`

## Gate 2: Specification Conformance Gate (Required)

- [ ] Run a prompt/spec conformance audit against real traces using `templates/spec-conformance-audit-template.md`
- [ ] Measure required-contract adherence with explicit denominator (`n_conformant / n_total`)
- [ ] Classify each failure as either:
  - spec/ambiguity failure (missing or unclear instruction)
  - generalization failure (instruction was clear but behavior still missed)
- [ ] If spec/ambiguity failures dominate, fix prompt/spec and rerun conformance before adding evaluator

## Gate 3: Failure Type

### Objective / deterministic

- [ ] Use code assertions
- [ ] Add to CI checks

### Subjective / nuanced

- [ ] Use LLM judge only if assertions cannot cover it
- [ ] Plan for label collection + validation maintenance

## Gate 4: Synchronous Block Needed?

If failure must be blocked before user sees output:
- [ ] Use guardrail-style check
- [ ] Keep latency low
- [ ] Minimize false positives

If not blocking:
- [ ] Run as asynchronous evaluator

## Gate 5: Investment Justified?

- [ ] Estimate run frequency
- [ ] Estimate failure impact
- [ ] Estimate maintenance burden
- [ ] Proceed only if expected value > evaluator cost

## Required Follow-up

- [ ] For any LLM judge: run `workflows/judge-validation-checklist.md`
- [ ] For production use: run `workflows/judge-monitoring-and-revalidation-checklist.md`

## Pointers

- Concepts: `references/evaluator-types-guide.md`
- Anti-patterns: `references/anti-patterns-extended.md`
- Conformance artifact: `templates/spec-conformance-audit-template.md`
