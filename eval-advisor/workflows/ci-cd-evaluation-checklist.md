# CI/CD Evaluation Checklist

Use this checklist to keep regression prevention and production monitoring connected.

## CI (Regression)

- [ ] Golden dataset includes core paths, prior failures, edge cases
- [ ] Golden dataset is curated (not random dump)
- [ ] Automated evaluators run on each change
- [ ] Judge model versions pinned
- [ ] Leakage checks in place (no golden examples in dev prompts)
- [ ] Non-determinism policy defined for flaky checks

## CD/Monitoring (Production)

- [ ] Sampling strategy defined for production traces
- [ ] Observability logs include intermediate tool/retrieval steps
- [ ] Key evaluators run asynchronously on sampled traffic
- [ ] Corrected success rates and confidence intervals computed where needed
- [ ] Alerts configured on agreed thresholds

## Guardrails

- [ ] Guardrails limited to objective high-impact failures
- [ ] Guardrail action defined (`reject`, `retry`, `fallback`)
- [ ] False-positive rate monitored

## Continuous Improvement

- [ ] New failure patterns trigger targeted error analysis
- [ ] New representative failures added to golden dataset
- [ ] Evaluators/rubrics updated after failure taxonomy changes
- [ ] Judge revalidation cadence enforced
