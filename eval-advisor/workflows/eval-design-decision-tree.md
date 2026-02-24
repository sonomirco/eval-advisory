# Eval Design Decision Tree

Use this workflow to choose the minimum-cost evaluator that still gives reliable signal.

## Gate 1: Error Analysis Complete?

- [ ] Confirm observed failure modes exist from real traces
- [ ] If not complete: stop and run `workflows/error-analysis-checklist.md`

## Gate 2: Prompt/Spec Fix First?

- [ ] Check whether failure is an unstated preference or missing instruction
- [ ] If yes: fix prompt/spec before adding evaluator

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
