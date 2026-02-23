# AI Evals Master Reference

This document serves as a comprehensive quick reference for AI evaluation concepts, combining content from Hamel Husain's work with transcribed flashcard content.

---

## 1. Error Analysis

Error analysis is the most important skill you can learn regarding AI Evals. It allows you to decide what metrics and evaluators to write by identifying failure modes unique to your application and data. It prevents you from wasting time on generic metrics and provides product intuition.

### The Four-Stage Process

1. **Creating a dataset:** Gather representative traces of user interactions. If you don't have production data, you can start by generating synthetic data.

2. **Open Coding:** A human annotator (ideally a domain expert) manually reviews traces and writes open-ended notes about every issue. It is most efficient to focus on noting the first failure observed in a multi-step trace.

3. **Axial Coding:** This is the most important step. You categorize the raw open-ended notes, grouping similar issues into a definitive failure taxonomy. You can use an LLM to help organize these clusters, but human review and refinement are essential.

4. **Iterative Refinement:** Continue reviewing traces until you reach theoretical saturation, where new data no longer reveals new failure modes or information. A common rule of thumb is to review at least 100 diverse traces.

### Key Insight

Error analysis reveals what actually matters for your application. Generic metrics like "helpfulness" or "quality" miss application-specific failures that only emerge from looking at real data.

---

## 2. When to Build an Automated Evaluator

Focus automated evaluators on failures that persist after fixing your prompts. Many teams discover their LLM doesn't meet preferences they never actually specified - like wanting short responses, specific formatting, or step-by-step reasoning. Fix these obvious gaps first before building complex evaluation infrastructure.

### The Cost Hierarchy

Consider the cost hierarchy of different evaluator types:

- **Simple assertions and reference-based checks:** Cheap to build and maintain. These include comparing against known correct answers.
- **LLM-as-Judge evaluators:** Require 100+ labeled examples, ongoing weekly maintenance, and coordination between developers, PMs, and domain experts.
- **Expensive infrastructure:** Only build for problems you'll iterate on repeatedly.

### Guidance

Only build expensive evaluators for persistent generalization failures - not issues you can fix trivially. Start with cheap code-based checks where possible: regex patterns, structural validation, or execution tests. Reserve complex evaluation for subjective qualities that can't be captured by simple rules.

---

## 3. Don't Use Generic Metrics

Generic evaluation metrics don't just appear in ads. Eval libraries contain scores like helpfulness, coherence, quality, etc. promising easy evaluation. These metrics measure abstract qualities that likely do not matter for your use case. Good scores on them don't mean your system works.

### The Problem

Generic metrics encourage building evaluations that measure things that don't impact your actual users. They create false confidence - you might have great scores but your product still fails in production.

### The Solution

Conduct error analysis to understand failures. Define binary failure modes based on real problems. Create custom evaluators for those failures and validate them against human judgment.

### Exception for Exploration

Experienced practitioners may use generic metrics as exploration tools to find interesting traces. Once you understand why generic metrics fail as evaluations, you can repurpose them for sampling rather than as quality measures.

---

## 4. AI Eval Mistakes

### Mistake #1: Not looking at data before building evals

Evals often take more time to build and maintain compared to traditional unit tests, which means you need to be more vigilant about ROI. You should prioritize writing evals that measure real failures (as opposed to hypothetical ones).

The best way to find those failures is error analysis. Off-the-shelf metrics like "helpfulness" or "quality" are a bad idea because they skip this step.

### Mistake #2: Not validating your LLM judge against human labels

When using an LLM as a judge, you must measure its agreement with human labels. Skipping this step creates an untrustworthy metric. You should always compare your judge to a human labeled dataset.

### Mistake #3: Having evals with 100% pass rates

In traditional software testing, passing all tests is the goal. In AI evals, a perfect score is a red flag. When all your evals pass, you lose the ability to differentiate between model versions or identify opportunities.

This means you should always have test cases that are hard to pass. For evaluators that are passing consistently, consider a phased approach: reduce frequency to a weekly regression check, and if it still provides no signal, sunset it entirely.

---

## 5. Types of Automated Evals

### 1. Code-based assertions

These check for objective, rule-based failures: valid JSON, SQL syntax, presence of required keywords, tool execution errors. They're fast, cheap, deterministic, and interpretable. Use them whenever possible.

### 2. LLM-as-a-judge

These use an LLM to assess subjective or nuanced criteria that code can't capture: tone, helpfulness, whether a response adequately addresses the user's concern.

LLM judges are powerful but expensive. They require labeled examples to validate, ongoing maintenance, and coordination between developers and domain experts. Reserve them for failure modes where code-based checks genuinely can't work.

### 3. Guardrails

Guardrails are evaluators that run in the request/response path, blocking failures before they reach the user. If a guardrail triggers, the system can redact, refuse, or regenerate.

Because guardrails run synchronously, they must be fast and have low false positive rates. A guardrail that incorrectly blocks valid responses is a production bug. For this reason, guardrails are typically code-based checks or small classifiers, not LLM judges.

---

## 6. Don't Use Likert Scales

Use binary.

Likert scales (1-5 ratings) have several problems. They're expensive to align with domain experts because you need agreement on what each number means. Annotators tend to default to middle values to avoid making hard calls. And they encourage vague, broad criteria like "overall quality" instead of targeted failure modes.

Binary scores force a decision: does this pass or fail? This mirrors the reality of shipping software. You have to decide whether a feature is good enough to release. A score of 3/5 doesn't help you make that call.

Binary evals are also more actionable. Instead of one judge that outputs "3/5," build multiple targeted evals: `is_polite`, `scheduling`, `human_handoff`. Each returns pass or fail. When something fails, you know exactly what to fix.

---

## 7. How to Trust an LLM Judge

Once you've built an LLM judge, how can you trust it?

The only way to trust an LLM judge is to measure it against human labels.

### Data Splitting

Start by collecting human-labeled examples for your failure mode. Split this data into three sets:

1. **Train (~20%):** Draw your few-shot examples from here to include in the judge's prompt.
2. **Dev (~40%):** Use this to iterate on your prompt. Run the judge, compare to human labels, refine, repeat.
3. **Test (~40%):** A held-out set for final validation. This tells you whether you've overfit to your dev set.

### Measuring Alignment

When measuring alignment, don't report "accuracy." It's misleading on imbalanced data. If 95% of your examples pass, a judge that always outputs "pass" gets 95% accuracy while being useless.

Instead, use True Positive Rate (TPR) and True Negative Rate (TNR).

- **TPR (True Positive Rate):** Measures how well your judge catches actual failures. Formula: TP / (TP + FN)
- **TNR (True Negative Rate):** Measures how well your judge recognizes actual successes. Formula: TN / (TN + FP)

Aim for both above 90%.

---

## 8. Ways to Sample Traces for AI Evals

You have thousands of production traces. Which ones should you review?

You can't look at everything. Smart sampling helps you find problems faster. The strategies fall on a spectrum from pure exploration to using signals you already have:

1. **Random sampling** should always be part of your approach. It's the only way to discover issues you don't know about yet. But random alone is inefficient.

2. **Clustering** groups traces by semantic similarity. Review a few examples from each cluster to see if certain clusters contain more errors than others.

3. **Data analysis** looks at metadata and statistics: latency, number of turns, token counts, tool calls. Outliers often indicate problems. A conversation with 20 back-and-forth turns might signal confusion. A response with unusually high latency might have hit an edge case.

4. **Classification** uses your existing evals, a predictive model, or an LLM to flag traces that are likely problematic. This is powerful but use it with caution: you'll only find issues your classifier already knows about.

5. **Feedback** from customers is the strongest signal. Thumbs down, complaints, or abandonment patterns point directly to real problems.

---

## 9. Tips on Generating Synthetic Data for Bootstrapping Evals

What if you don't have traces yet? Or what if your production data is too sparse to cover failure modes you care about?

This is where synthetic data comes in.

### The Wrong Way

Prompting an LLM with "Generate 50 test cases" produces generic, repetitive inputs that miss edge cases and don't reflect real usage patterns.

### The Right Way: Use Structure

1. **Define dimensions for diversity:** Identify categories that describe variation in your queries. Examples: Feature (what user wants), Persona (who user is), Scenario (how they express it). For a recipe app, this might be Dietary Restriction × Cuisine Type × Query Complexity. Create combinations across these dimensions to ensure coverage.

2. **Seed with real data when possible:** If you have any logs or traces, use them as starting points. Ask the model to inject a new constraint or modify a variable to create realistic edge cases. This grounds your synthetic data in actual usage.

3. **Generate in two steps:** First generate structured tuples like (Vegan, Italian, Multi-step). Then in a separate prompt, convert each tuple to natural language: "I need a dairy-free lasagna recipe that I can prep of day before." This separation avoids repetitive phrasing.

4. **Filter aggressively:** Generate many candidates, then keep only challenging ones. Easy cases don't help you find problems.

5. **Increase complexity iteratively:** Start simple, then ask LLM to add constraints, formatting requirements, or ambiguity.

### Important Caveat

Synthetic data is for bootstrapping. It often fails to capture ambiguity and diversity of real queries. As soon as you have production traffic, start sampling real traces to validate and augment your test set.

---

## 10. How to Read and Use Traces for Evals

A trace is a full record of what happened during a request: user query, system prompt, tool calls, intermediate steps, and final response. Multi-turn conversations are a single trace containing all turns.

### 1. Stop at the First Failure

When reviewing a trace, don't catalog every problem. Find the most upstream error and stop there. If the assistant misinterprets the user's intent on turn 2, everything downstream is affected. Fixing that first failure is often the best use of your time.

### 2. Simplify Before You Test

When you find a failure in a multi-turn conversation, try to reproduce it with the simplest possible input. If a shopping bot gives the wrong return policy on turn 4, first try asking directly: "What is the return window for product X?" If it still fails, the problem is retrieval or grounding, not conversation context. This saves you from debugging multi-turn complexity when the issue is simpler.

### 3. Use N-1 Testing for True Multi-Turn Issues

For failures that only happen after several turns (like the bot forgetting earlier context), take the first N-1 turns and use them as your test input. Run the model multiple times from that point to see how consistently it handles the situation. This creates regression tests from real conversations.

### 4. Perturb Traces for Coverage

Once you have working test cases, you can expand coverage by modifying them: rephrase the user question, add a constraint mid-conversation, change the persona. This is like data augmentation for evals.

### 5. Simulate Users When Needed

You can use an LLM to play the user role and generate full conversations. This is harder to do well, but becoming more viable as models improve. Put detailed persona information in the simulating LLM's system prompt.

---

## 11. Transition Matrix Concepts

Transition failure matrices help you understand where workflows break. Create a matrix where rows represent the last successful state and columns represent where the first failure occurred.

This reveals failure hotspots and guides where to invest debugging effort. For example, if GenSQL → ExecSQL transitions cause 12 failures while DecideTool → PlanCal causes only 2, you know where to focus your effort.

Transition matrices transform overwhelming agent complexity into actionable insights. Instead of drowning in individual trace reviews, you can immediately see patterns and prioritize improvements.

---

## 12. Deploying Evals

### CI vs Production Evaluation

The most important difference between CI vs production evaluation is the data used for testing.

**Test datasets for CI** are small (often 100+ examples) and purpose-built. Examples cover core features, regression tests for past bugs, and known edge cases. Since CI tests are run frequently, the cost of each test has to be carefully considered. Favor assertions or other deterministic checks over LLM-as-judge evaluators.

**Production evaluation** can sample live traces and run evaluators against them asynchronously. Since you usually lack reference outputs on production data, you might rely more on expensive reference-free evaluators like LLM-as-judge.

These two systems are complementary: when production monitoring reveals new failure patterns through error analysis and evals, add representative examples to your CI dataset. This mitigates regressions on new issues.

### Guardrails vs Evaluators

**Guardrails** are inline safety checks that sit directly in the request/response path. They validate inputs or outputs before anything reaches the user, so they typically are:
- Fast and deterministic – typically a few milliseconds
- Simple and explainable – regexes, keyword blocklists, schema validators
- Targeted at clear-cut, high-impact failures – PII leaks, profanity, disallowed instructions

If a guardrail triggers, the system can redact, refuse, or regenerate.

**Evaluators** typically run after a response is produced. They measure qualities that simple rules cannot, such as factual correctness, completeness, etc. Their verdicts feed dashboards, regression tests, and model-improvement loops, but they do not block the original answer.

Apply guardrails for immediate protection against objective failures requiring intervention. Use evaluators for monitoring and improving subjective or nuanced criteria. Together, they create layered protection.
