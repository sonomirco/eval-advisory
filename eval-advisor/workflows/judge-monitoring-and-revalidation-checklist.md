# Judge Monitoring and Revalidation Checklist

Use this checklist for production monitoring when a judge score drives quality decisions.

## Validation Baseline

- [ ] Held-out test set exists and is isolated from prompt iteration
- [ ] Test-set `TPR` and `TNR` computed and documented
- [ ] `FPR` and `FNR` computed for debugging error asymmetry
- [ ] Judge model version pinned in config

## Production Estimation

- [ ] Sample strategy for production traces is defined
- [ ] Raw observed pass rate (`p_obs`) is computed
- [ ] Corrected success estimate (`theta_hat`) is computed
- [ ] Denominator safety check passed (`TPR + TNR - 1` not near zero)

## Uncertainty

- [ ] Bootstrap procedure defined and repeat count set
- [ ] 95% confidence interval computed (`[L, U]`)
- [ ] Alert thresholds defined on interval bounds and/or point estimate

## Drift Control

- [ ] Revalidation cadence set (for example, every few weeks)
- [ ] Fresh human labels collected under current rubric
- [ ] New `TPR/TNR` compared against thresholds
- [ ] Recalibration trigger path defined (prompt/few-shot revision)
- [ ] Revalidation required after any judge model change

## Reporting

- [ ] Dashboard shows point estimate and confidence interval
- [ ] Dashboard includes last revalidation date and rubric version
- [ ] Error asymmetry trends tracked (false pass vs false fail)
