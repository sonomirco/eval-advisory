# CI/CD Evaluation Guide

Use this guide to operationalize evaluation from regression prevention to production monitoring.

## CI: Regression Safety Net

### Purpose

CI checks stability on known failure modes. It does not estimate overall production quality.

### Golden dataset design

Include:
- Core capability coverage
- Previously observed critical failures
- Edge cases from error analysis
- Examples spanning major pipeline paths

Golden sets are curated, purpose-built, and continuously updated.

### CI execution patterns

- Run automated evaluators on each proposed change
- Pin judge model versions to prevent evaluator drift noise
- Handle non-determinism with retry/tolerance policies where appropriate

### CI interpretation rules

- Treat CI pass rate as regression signal only
- Block leakage of golden examples into development prompts

## CD and Online Monitoring

### Observability baseline

Log for sampled production traces:
- Inputs and metadata
- Intermediate calls, tool interactions, retrieved context
- Final outputs
- Evaluator results
- User feedback signals

### Online evaluator strategy

- Run 5-7 key evaluators asynchronously on sampled traffic
- Compute corrected success rates when judge-based
- Attach confidence intervals for operational decisions

### Guardrails

Run synchronously for high-impact objective failures.

Required properties:
- Fast
- Deterministic where possible
- Low false-positive behavior

Failure actions:
- Reject
- Retry
- Fallback

## Judge Drift Management

- Pin judge versions
- Revalidate alignment on fresh labels periodically
- Trigger recalibration when `TPR/TNR` fall below thresholds
- Re-run after any judge-model change

## Continuous Improvement Flywheel

1. Analyze failures
2. Build/refine evaluators
3. Update CI golden set
4. Monitor in production
5. Re-analyze new patterns
6. Ship fixes and repeat

## Operational Pitfalls

- Stagnant golden datasets
- Weak trace instrumentation
- Over-reliance on automated checks without human review
- Conflicting metric trade-offs with no priority policy
- Outdated judge calibration in production dashboards
