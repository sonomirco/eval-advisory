# LLM Judge Validation Checklist

Use this checklist to ensure your LLM-as-judge is properly validated before deployment.

## Data Preparation

- [ ] **Human labels collected:** Minimum 100-300 examples labeled by domain expert
- [ ] **Labels are binary:** Each example marked Pass or Fail (not Likert scales)
- [ ] **Label consistency verified:** Single expert or clear alignment process for multiple labelers
- [ ] **Ground truth documented:** Clear definition of what each label means

## Data Splitting

- [ ] **Three-way split created:** Train/Dev/Test separation established
- [ ] **Train set (~20%):** Examples for few-shot learning in judge prompt
- [ ] **Dev set (~40%):** Examples for iteration and refinement
- [ ] **Test set (~40%):** Held-out for final validation only
- [ ] **Stratified if needed:** Ensures balanced representation across splits
- [ ] **No leakage:** Test set examples not used in any way during development

## Judge Development

- [ ] **Baseline judge created:** Initial prompt with train set examples
- [ ] **Few-shot format:** Examples included in system prompt for reference
- [ ] **Clear criteria defined:** Binary pass/fail requirements specified
- [ ] **Output format specified:** Structured JSON or format for parsing
- [ ] **Evaluation scope documented:** What the judge assesses and doesn't assess

## Dev Set Iteration

- [ ] **Dev set evaluated:** Judge run on dev examples, results compared to human labels
- [ ] **TPR calculated:** True Positive Rate measured (how well judge catches failures)
- [ ] **TNR calculated:** True Negative Rate measured (how well judge recognizes successes)
- [ ] **Confusion matrix analyzed:** TP, TN, FP, FN counts examined
- [ ] **Failures analyzed:** Specific types of errors (too strict, too lenient, inconsistent)
- [ ] **Prompt refined:** At least one iteration based on dev set performance
- [ ] **Examples updated:** Few-shot examples adjusted based on learnings

## Performance Targets

Both TPR and TNR should be above 90% before deployment.

- [ ] **TPR > 90%:** Judge catches at least 90% of actual failures
- [ ] **TNR > 90%:** Judge correctly passes at least 90% of actual successes
- [ ] **Balanced performance:** Both metrics meet threshold
- [ ] **No severe bias:** Confusion matrix shows balanced errors

## Test Set Validation

- [ ] **Test set evaluated:** Final run on held-out data
- [ ] **Metrics calculated:** Final TPR, TNR, and confusion matrix
- [ ] **No overfitting:** Test set performance similar to dev set (within 5%)
- [ ] **Results documented:** Clear record of final validation metrics

## Validation Report

- [ ] **Confusion matrix included:** Shows TP, TN, FP, FN counts
- [ ] **TPR reported:** True Positive Rate with formula and value
- [ ] **TNR reported:** True Negative Rate with formula and value
- [ ] **Accuracy noted:** Reported for reference but not as primary metric
- [ ] **Examples provided:** Include examples of each error type

## Common Mistakes to Avoid

- [ ] **Didn't report accuracy only:** Included TPR and TNR separately
- [ ] **Validated on held-out set:** Not just dev set performance
- [ ] **No data leakage:** Test set completely isolated from development
- [ ] **Sufficient sample size:** Minimum 100 examples in test set
- [ ] **Binary labels:** Used pass/fail, not 1-5 scales

## Deployment Readiness

Before deploying the judge to CI/CD or production:

- [ ] **Meets performance threshold:** Both TPR and TNR above 90%
- [ ] **Human expert reviewed:** Domain expert signed off on validation
- [ ] **Error analysis documented:** Known failure modes and limitations recorded
- [ ] **Monitoring plan:** How often to re-validate and check for drift
- [ ] **Rollback procedure:** What to do if judge degrades in production

## Ongoing Maintenance

- [ ] **Regular calibration schedule:** Plan for monthly/quarterly re-validation
- [ ] **Drift detection:** Monitor for changes in agreement over time
- [ ] **Feedback loop:** Process for incorporating new human labels as system evolves
- [ ] **Version tracking:** Maintain history of judge prompt versions and metrics

## Ready to Deploy

If you checked all critical items above, your LLM judge is validated and ready for production use.

See also:
- references/judge-validation-reference.md - Detailed methodology and formulas
- templates/judge-prompt-template.md - For structuring your judge
