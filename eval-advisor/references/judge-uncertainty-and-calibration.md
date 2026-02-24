# Judge Uncertainty and Calibration

Use this guide when judge metrics are used for production monitoring, alerting, or SLO decisions.

## Scope

Judge quality is not just prompt quality. You need:
- Held-out judge validation (`TPR`, `TNR`)
- Bias correction for production pass-rate estimates
- Confidence intervals for uncertainty-aware decisions
- Periodic revalidation for drift

## 1. Validate Judge on Held-Out Labels

On a held-out test set, compute:
- `TPR = TP / (TP + FN)`
- `TNR = TN / (TN + FP)`
- `FPR = 1 - TNR`
- `FNR = 1 - TPR`

If `TPR` and `TNR` are weak, production estimates derived from this judge are unreliable.

## 2. Observe Raw Production Success Rate

On an unlabeled production batch:
- Let `m` = number of evaluated traces
- Let `k` = traces judged `Pass`
- Raw observed rate: `p_obs = k / m`

`p_obs` is biased because the judge is imperfect.

## 3. Correct Bias in Observed Rate

Use judge accuracy estimates to correct the observed rate:

`theta_hat = (p_obs + TNR - 1) / (TPR + TNR - 1)`

Operational notes:
- Clip to `[0, 1]` for numerical stability
- If denominator `TPR + TNR - 1` is near zero, the judge is too weak for correction

## 4. Quantify Uncertainty with Bootstrap

Use bootstrap resampling over held-out `(human_label, judge_prediction)` pairs:
1. Resample pairs with replacement
2. Recompute `TPR*`, `TNR*`
3. Recompute `theta_hat*`
4. Repeat `B` times (for example `B=20,000`)
5. Use 2.5th/97.5th percentiles as 95% CI

Output:
- Point estimate `theta_hat`
- Interval `[L, U]`

## 5. Use CI in Alerting

Prefer threshold checks on interval bounds, not only point estimates.

Common pattern:
- Trigger investigation when lower bound crosses SLO threshold
- Escalate urgency when both point estimate and lower bound are degraded

## 6. Revalidation Loop

Judge alignment can drift from:
- Rubric evolution
- Domain shift in traffic
- Foundation model updates

Recommended loop:
1. Sample fresh production traces
2. Collect new human labels under current rubric
3. Recompute `TPR/TNR`
4. If below thresholds, update prompt/few-shot and revalidate
5. Redeploy only after held-out checks pass

## 7. Data Discipline

Avoid these errors:
- Leakage from test labels into prompt iteration
- Reporting only dev-set judge performance
- Mixing rubric versions without relabeling audit
- Comparing CI/CD metrics across unpinned judge model versions

## 8. Decision Guidance

- Use raw `p_obs` only for rough diagnostics
- Use corrected `theta_hat` for operational reporting
- Use CI for SLO/risk decisions
- Use periodic recalibration as a required maintenance task, not optional cleanup
