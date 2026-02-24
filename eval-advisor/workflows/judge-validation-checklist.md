# LLM Judge Validation Checklist

Use this checklist before relying on a judge for CI gating or production metrics.

## Labels and Splits

- [ ] Human labels collected for target failure mode
- [ ] Binary labeling criteria defined
- [ ] Train/Dev/Test split isolated
- [ ] No test leakage into prompt iteration

## Prompt Iteration (Dev)

- [ ] Baseline prompt created from train examples
- [ ] Judge run on dev set
- [ ] `TPR` and `TNR` computed on dev
- [ ] Confusion matrix reviewed
- [ ] Prompt/examples iterated based on disagreement patterns

## Held-Out Validation (Test)

- [ ] Judge run once on test set after dev iteration is finalized
- [ ] Final `TPR` and `TNR` reported
- [ ] Final confusion matrix reported
- [ ] Accuracy may be reported, but not used alone for acceptance

## Acceptance Decision

- [ ] Performance thresholds defined and met
- [ ] Error asymmetry acceptable for use case (false pass vs false fail)
- [ ] Known limitations documented

## Deployment Readiness

- [ ] Monitoring/revalidation cadence defined
- [ ] Judge model version pinned
- [ ] Rollback/recalibration path defined

## Pointers

- Methods: `references/judge-validation-reference.md`
- Production calibration: `references/judge-uncertainty-and-calibration.md`
- Prompt format: `templates/judge-prompt-template.md`
