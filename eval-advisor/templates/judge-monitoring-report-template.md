# Judge Monitoring Report Template

Use this report for production judge health and corrected success-rate tracking.

## Metadata

- Metric / failure mode: `<name>`
- Judge model version: `<model_version>`
- Rubric version: `<rubric_version>`
- Reporting window: `<start_date>` to `<end_date>`

## Held-Out Judge Accuracy

- Test set size: `<N_test>`
- `TP`: `<tp>`
- `TN`: `<tn>`
- `FP`: `<fp>`
- `FN`: `<fn>`

Computed:
- `TPR = TP / (TP + FN) = <value>`
- `TNR = TN / (TN + FP) = <value>`
- `FPR = 1 - TNR = <value>`
- `FNR = 1 - TPR = <value>`

## Production Batch

- Evaluated traces (`m`): `<m>`
- Judged pass count (`k`): `<k>`
- Raw observed rate: `p_obs = k / m = <value>`

## Bias-Corrected Estimate

Formula:
`theta_hat = (p_obs + TNR - 1) / (TPR + TNR - 1)`

- Denominator check (`TPR + TNR - 1`): `<value>`
- `theta_hat` (clipped to `[0,1]`): `<value>`

## Uncertainty

- Bootstrap iterations (`B`): `<B>`
- 95% CI lower bound (`L`): `<L>`
- 95% CI upper bound (`U`): `<U>`

## Threshold Evaluation

- SLO threshold: `<threshold>`
- Point estimate status: `<pass/fail>`
- Lower-bound status: `<pass/fail>`
- Alert fired: `<yes/no>`

## Drift/Revalidation Status

- Last human relabel date: `<date>`
- Next revalidation due: `<date>`
- Since last report: `<judge_prompt_changed yes/no>`, `<model_changed yes/no>`
- Action required:
  - `<none / recalibrate prompt / recollect labels / rollback>`
