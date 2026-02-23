# Anti-Patterns and Common Mistakes in AI Evaluation

This guide covers the most common mistakes teams make when building and using AI evaluation systems, and how to avoid them.

## Anti-Pattern 1: Building Evals Before Error Analysis

### The Mistake

Teams write automated evaluators before understanding what actually fails in their application. This leads to:
- Measuring things that don't matter
- Missing the most important failure modes
- Wasting engineering time on irrelevant metrics
- False confidence from high scores on meaningless tests

### Why It's a Problem

LLMs have infinite surface area for potential failures. You can't anticipate what will break without observing real behavior. Generic metrics like "helpfulness" or "quality" don't capture application-specific failures.

### The Solution

**Always start with error analysis:**
1. Gather 100+ diverse traces
2. Open code: Note each failure without categories
3. Axial code: Group into a failure taxonomy
4. Build evaluators for observed failures, not hypothetical ones

### How to Detect This Anti-Pattern

**Warning signs:**
- Evals defined before looking at any user data
- Metrics copied from other teams or libraries
- Generic names like "helpfulness_score" or "quality_rating"
- Team can't describe what specific failures each eval targets

---

## Anti-Pattern 2: Using Generic Metrics

### The Mistake

Using off-the-shelf evaluation metrics like helpfulness, coherence, quality, or hallucination scores.

### Why It Fails

Generic metrics measure abstract qualities that:
- Likely do NOT matter for your specific use case
- Don't capture the failures your users actually experience
- Create false confidence - high scores don't mean your system works
- Are designed for benchmarks, not production applications

### Example: Generic vs Specific

**Generic (Bad):**
- "Measure helpfulness on 1-5 scale"
- "Calculate coherence score"
- "Detect hallucinations"

**Application-Specific (Good):**
- "Did the assistant provide the correct return policy?"
- "Did the response include all required disclosures?"
- "Was the appropriate tone used for this customer segment?"
- "Did the scheduling respect actual availability?"

### When Generic Metrics Are Acceptable

After you understand why generic metrics fail, you can repurpose them as:
- **Exploration tools:** Use to find interesting traces in large datasets
- **Initial triage:** Quick signals during development when you have nothing better

But never use them as your primary quality measure.

---

## Anti-Pattern 3: Using Likert Scales Instead of Binary

### The Mistake

Using 1-5 or 1-7 rating scales for evaluation instead of binary pass/fail.

### Why It Fails

**Expensive to align:**
- Need agreement on what each number means
- Domain experts struggle with consistent application
- Annotators default to middle values to avoid hard calls

**Encourages vague criteria:**
- "Overall quality" is hard to define operationally
- Favors broad categories over specific failure modes
- Creates interpretation difficulties

**Not actionable:**
- A score of 3/5 doesn't tell you what to fix
- Averages of ratings hide specific patterns
- Doesn't mirror shipping decisions (pass/fail)

### The Binary Alternative

**Why binary works better:**
- **Clear decision:** Pass or fail - no ambiguity
- **Actionable:** When something fails, you know exactly what to fix
- **Progress tracking:** 10% improvement is immediately meaningful
- **Mirrors reality:** You ship features or you don't - binary decisions
- **Enables specificity:** Forces targeted evaluators: `is_polite`, `correct_return_policy`, not `quality`

### Multiple Binary Evals Over One Likert Scale

Instead of one evaluator outputting "3/5," build multiple targeted binary evaluators:
- `is_factual` - Pass/Fail
- `is_complete` - Pass/Fail
- `has_required_disclosures` - Pass/Fail
- `appropriate_tone` - Pass/Fail
- `correct_formatting` - Pass/Fail

Each gives specific, actionable feedback.

### Nuance Isn't Lost

Binary doesn't mean losing nuance. Pair binary decisions with:
- Detailed qualitative critiques explaining why
- Specific examples of what went wrong
- Context for each judgment

---

## Anti-Pattern 4: Not Validating LLM Judges

### The Mistake

Building and deploying LLM-as-judge evaluators without comparing to human labels.

### Why It's Critical

Without validation, you don't know if:
- Judge is too strict (rejecting good work)
- Judge is too lenient (missing real problems)
- Judge has systematic biases
- Judge criteria align with human expectations

**Unvalidated judges can actively harm your product:**
- Over-optimization toward a flawed metric
- Missing real user-facing issues
- Wasted engineering effort on non-problems

### The Validation Requirement

**Always validate against human labels:**

1. Collect human-labeled examples (100-300 minimum)
2. Split into Train/Dev/Test sets (20/40/40)
3. Report TPR and TNR, not accuracy
4. Iterate until both metrics are above 90%

See references/judge-validation-reference.md for detailed methodology.

---

## Anti-Pattern 5: Reporting Accuracy Instead of TPR/TNR

### The Mistake

Reporting overall accuracy for an LLM judge, especially on imbalanced datasets.

### Why It's Misleading

Consider:
- 95% of your examples are "pass"
- A judge that always says "pass" achieves 95% accuracy
- But it's completely useless at catching failures!

**Accuracy rewards pass-always behavior** on imbalanced data.

### The Solution: TPR and TNR

Report two metrics separately:
- **TPR (True Positive Rate):** How well judge catches actual failures
- **TNR (True Negative Rate):** How well judge recognizes actual successes

Both metrics need to be high (>90%) for the judge to be trustworthy.

### Confusion Matrix Reporting

Always include:
```
                Human Pass    Human Fail
Judge Pass     TN=95           FP=5
Judge Fail     FN=3           TP=47
```

From this, calculate:
- TPR = 47 / (47 + 3) = 94%
- TNR = 95 / (95 + 5) = 95%
- Accuracy = 142 / 150 = 95% (misleading!)

---

## Anti-Pattern 6: Having 100% Pass Rate

### The Mistake

All evaluators passing consistently, or test suites with no hard cases.

### Why It's a Problem

In traditional software, passing all tests is the goal. In AI evals, perfect scores indicate:
- Tests aren't challenging the system
- Missing real-world edge cases
- Can't differentiate between model versions
- No signal for improvement opportunities

### The Solution: Hard Test Cases

**Always include cases that should fail:**
- Known edge cases
- Previously observed failure modes
- Ambiguous inputs
- Multi-turn context challenges
- Low-frequency scenarios

### Managing Evaluator Lifecycle

**For consistently passing evaluators:**
- Reduce frequency to weekly regression check
- If still provides no signal, sunset the evaluator
- Focus on areas where the system actually struggles

**For consistently failing features:**
- These indicate important problems to fix
- Prioritize improvements in these areas
- Track whether failures reduce over time

A healthy eval suite should have a mix of pass rates - some high (90%+) for stable features, some lower (50-70%) for areas being actively improved.

---

## Anti-Pattern 7: Not Sampling Production Data

### The Mistake

Making decisions about your AI system based on anecdote, vibes, or small samples of data rather than systematic analysis.

### Why It's a Problem

**You are not looking at data.** This is the most fundamental failure mode in AI development.

Without systematic data review:
- Your intuition about what works is often wrong
- You optimize for problems that don't exist
- You miss the problems that actually affect users
- You can't measure progress effectively

### The Solution: Continuous Error Analysis

Make data review a recurring activity:
- Review 20-50 traces weekly
- Sample randomly across different user segments
- Use feedback signals to prioritize
- Track failure modes over time

### Building Data Intuition

The most successful AI practitioners have strong product intuition because they:
- Look at data regularly
- See patterns of failures
- Understand what their users actually experience
- Can predict where changes will have impact

---

## Anti-Pattern 8: Automating Prompt Engineering Too Early

### The Mistake

Delegating prompt writing to automated optimization tools before understanding requirements through manual iteration.

### Why It's Risky

When you write a prompt, you:
- Are forced to clarify assumptions and requirements
- Build intuition about model behavior
- Learn which patterns actually work

Automated tools:
- Hill-climb on a predefined metric
- Can't discover new failure modes
- May optimize for wrong objectives
- Hide the learning process

### The Solution: Manual Prompt Iteration

1. Start with manual prompt engineering
2. Iterate based on error analysis findings
3. Use tools for acceleration once criteria are stable
4. Keep humans in the loop for prompt changes

Automated optimization has its place for last-mile tuning, not for initial prompt development.

---

## Anti-Pattern 9: Building Complex Infrastructure Too Early

### The Mistake

Investing in sophisticated evaluation platforms and tooling before understanding what you actually need.

### The Problem

Complex infrastructure:
- Takes time to build and maintain
- Creates switching costs
- May not match your actual workflows
- Adds cognitive overhead to simple tasks

### The Solution: Start Simple

**Minimum viable eval setup:**
- Error analysis in notebooks
- Simple data viewer (spreadsheet or basic UI)
- Manual annotation with pass/fail
- Basic assertions where possible

**Add complexity only when:**
- You've validated the need through usage
- Simple tools can't scale to your needs
- The ROI on additional investment is clear

Many successful teams use notebooks + custom data viewers for a long time before adopting expensive platforms.

---

## Summary: Core Principles

1. **Error analysis first** - Always look at data before building evals
2. **Binary metrics** - Use pass/fail instead of Likert scales
3. **Application-specific** - Custom metrics over generic ones
4. **Validate judges** - Compare to human labels with TPR/TNR
5. **Include hard cases** - Test suites should have some failures
6. **Continuous review** - Make data examination an ongoing practice

Following these principles prevents the most common and costly mistakes in AI evaluation.

## See Also

- references/error-analysis-guide.md - For understanding what to evaluate
- references/judge-validation-reference.md - For proper validation methodology
- workflows/eval-design-decision-tree.md - For avoiding these anti-patterns
