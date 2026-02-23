# LLM Judge Validation Reference

When using an LLM as a judge for evaluating your AI system, proper validation is essential. This guide explains how to validate LLM judges against human labels.

## Why Validation Matters

An LLM judge without validation is untrustworthy. Research shows people tend to over-rely and over-trust AI systems. Without rigorous comparison to human judgment, these automated evaluations may drift from actual quality standards or harbor biases that silently undermine your evaluation system.

## Data Splitting Strategy

### Three-Way Split

Start by collecting human-labeled examples for your failure mode. Split this data into three sets:

1. **Train Set (~20%):**
   - Purpose: Draw few-shot examples for the judge's prompt
   - Selection: Representative examples covering your failure modes
   - Usage: Include these as examples in your system prompt

2. **Dev Set (~40%):**
   - Purpose: Iterate on your prompt and approach
   - Selection: Diverse examples for testing and refinement
   - Usage: Run judge, compare to human labels, refine, repeat
   - Process: This is where you debug alignment issues

3. **Test Set (~40%):**
   - Purpose: Final validation of your judge
   - Selection: Held-out examples not used in development
   - Usage: Single evaluation pass to report final metrics
   - Critical: This tells you whether you've overfit to your dev set

## The Problem with Accuracy

Accuracy is misleading on imbalanced data. Consider this scenario:

- 95% of your examples are "pass"
- A judge that always outputs "pass" achieves 95% accuracy
- But it's completely useless - it never catches any failures!

This is why you must use TPR and TNR instead.

## True Positive Rate (TPR)

**Definition:** Measures how well your judge catches actual failures

**Formula:**
```
TPR = TP / (TP + FN)
```

**Where:**
- TP (True Positive) = Judge correctly says "fail" when human says "fail"
- FN (False Negative) = Judge incorrectly says "pass" when human says "fail"

**Interpretation:**
- TPR = 90% means your judge catches 90% of actual failures
- Low TPR means your judge is missing real problems
- Target: Above 90%

**Example:**
If you have 100 actual failures and your judge identifies 85 of them:
- TP = 85, FN = 15
- TPR = 85 / (85 + 15) = 85%

## True Negative Rate (TNR)

**Definition:** Measures how well your judge recognizes actual successes

**Formula:**
```
TNR = TN / (TN + FP)
```

**Where:**
- TN (True Negative) = Judge correctly says "pass" when human says "pass"
- FP (False Positive) = Judge incorrectly says "fail" when human says "pass"

**Interpretation:**
- TNR = 90% means your judge correctly approves 90% of good outputs
- Low TNR means your judge is rejecting good work too often
- Target: Above 90%

**Example:**
If you have 100 actual successes and your judge correctly passes 92 of them:
- TN = 92, FP = 8
- TNR = 92 / (92 + 8) = 92%

## Alignment Targets

Aim for both TPR and TNR above 90%. This ensures:

1. Your judge catches most real problems (high TPR)
2. Your judge doesn't falsely flag good work too often (high TNR)

If one metric is low, iterate on your judge prompt before deploying.

## Validation Workflow

1. **Collect human labels:** Have a domain expert label 100-300 examples
2. **Split data:** Train/Dev/Test (20/40/40)
3. **Build initial judge:** Use train set examples for few-shot
4. **Iterate on dev set:** Run judge, compare to labels, refine prompt
5. **Final validation:** Run on test set and report TPR/TNR

## Common Alignment Issues

### Issue: Judge Too Strict

**Symptoms:** High TNR (>95%), low TPR (<70%)

**Meaning:** Judge rarely fails good outputs but misses many real failures

**Fix:** Add more permissive examples to few-shot, clarify failure criteria

### Issue: Judge Too Lenient

**Symptoms:** Low TNR (<80%), high TPR (>95%)

**Meaning:** Judge catches failures but falsely flags good work too often

**Fix:** Add examples of edge cases that should pass, refine "pass" criteria

### Issue: Inconsistent Criteria

**Symptoms:** Both TPR and TNR are low (<80%)

**Meaning:** Judge's internal logic is inconsistent or criteria unclear

**Fix:** Clarify evaluation rubric, add more specific examples to few-shot

## Reporting Metrics

When reporting your judge's performance:

1. **Always report TPR and TNR separately** - don't average them
2. **Include confusion matrix** - shows TP, TN, FP, FN counts
3. **Report on held-out test set** - not dev set performance
4. **Track over time** - metrics may drift as your system evolves

## Example Output Format

```
LLM Judge Performance Report
===========================

Test Set: N=250 examples
Human Labeler: [Domain Expert Name]

Confusion Matrix:
                Human Pass  Human Fail
Judge Pass       TN=92         FP=8
Judge Fail       FN=12         TP=138

Metrics:
- True Positive Rate (TPR): 138 / (138 + 12) = 92%
- True Negative Rate (TNR): 92 / (92 + 8) = 92%
- Overall Accuracy: 230 / 250 = 92%

Status: PASS - Both metrics above 90% threshold
```

## Integration with Development

Once validated:
1. Run judge on CI for relevant test suites
2. Sample production traces periodically for monitoring
3. Re-validate monthly as system evolves
4. Track agreement rates over time for drift detection

## See Also

- references/error-analysis-guide.md - For identifying failure modes to evaluate
- templates/judge-prompt-template.md - For structuring your judge prompt
- workflows/judge-validation-checklist.md - Step-by-step validation process
