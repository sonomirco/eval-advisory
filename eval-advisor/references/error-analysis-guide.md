# Error Analysis Guide

Error analysis is the foundation of effective AI evaluation. This guide provides a detailed workflow for conducting error analysis on your LLM application.

## What is Error Analysis?

Error analysis is the systematic process of identifying, categorizing, and analyzing failure modes in LLM applications. It is the prerequisite to building meaningful evaluation systems.

## Why Error Analysis Matters

- **Prevents wasted time:** Generic metrics like "helpfulness" or "quality" don't capture what actually fails in your application
- **Reveals real problems:** You discover failure modes you never anticipated
- **Builds product intuition:** Direct exposure to user interactions teaches you what matters
- **Enables targeted metrics:** Evaluators based on observed failures are actionable and relevant

## The Four-Stage Process

### Stage 1: Gather a Diverse Dataset

**Goal:** Collect representative traces of user interactions

**Sources:**
- Production traces from real users (preferred)
- Synthetic data if no production data available
- Focus on diversity, not random sampling

**Sample Size Guidance:**
- Minimum: 50-100 traces for initial analysis
- Saturation: Continue until new examples reveal no new patterns (often 100+ traces)

**Diversity Dimensions to Consider:**
- Different user intents
- Various complexity levels
- Edge cases and happy paths
- Different user personas
- Various interaction lengths

### Stage 2: Open Coding

**Goal:** Surface failure patterns without preconceived categories

**Process:**
1. Review each trace completely
2. Assign binary score: Pass or Fail
3. Write open-ended notes describing the FIRST failure observed
4. Don't diagnose or categorize yet - just describe what you see

**Why First Failure Matters:**

When reviewing a multi-turn trace, finding the first upstream error is crucial. If the assistant misinterprets the user's intent on turn 2, everything downstream is affected. Fixing that first failure often resolves multiple downstream issues automatically.

**Example Open Coding Notes:**
- "Misunderstood user's question as X when they meant Y"
- "Retrieved wrong document - info was present but not retrieved"
- "Response too verbose for simple query"
- "Tool called with wrong parameters"
- "Hallucinated additional details not in source"

### Stage 3: Axial Coding

**Goal:** Structure the raw notes into a coherent failure taxonomy

**Process:**
1. Export all open-coded notes
2. Look for patterns and group similar issues
3. Create category names that describe the pattern
4. Define each category with a clear description
5. Review and refine - human judgment is essential here

**Using LLMs for Assistance:**

You can use an LLM to help with initial clustering:
- Prompt: "Here are open-coded notes from error analysis. Group these into coherent failure categories. For each category, provide a descriptive title and one-line definition. Only cluster based on issues in the notes - do not invent new failure types."

**Important:** Always review and refine the LLM's output. The human-in-the-loop ensures categories make sense for your domain.

**Example Failure Taxonomy:**

| Category | Description | Example |
|-----------|-------------|---------|
| Hallucination/Incorrect Info | Assistant provides factually wrong answers | States wrong pricing |
| Context Retrieval Failure | Fails to find or use relevant documents | Ignores retrieved context |
| Irrelevant Response | Content doesn't address user's question | Talks about unrelated feature |
| Generic/Unhelpful | Too broad or vague to be useful | "Contact support" boilerplate |
| Format Issues | Problems with output structure | Missing code blocks, broken links |
| Tool Use Errors | Incorrect tool selection or parameters | Wrong SQL syntax generated |

### Stage 4: Iterative Refinement to Saturation

**Goal:** Continue until no new failure modes emerge

**Process:**
1. Continue reviewing and categorizing new traces
2. Add new categories if distinct patterns emerge
3. Expand existing category descriptions as understanding deepens
4. Stop when you reach theoretical saturation

**Signs of Saturation:**
- Last 20-30 traces show only existing failure types
- You can predict which category a new failure will fall into
- New traces don't surprise you with novel issues

## Common Pitfalls to Avoid

### Don't Skip to Generic Metrics

Starting with pre-built metrics like "hallucination score" or "helpfulness rating" skips the discovery process. Each application has unique failure modes that only emerge from looking at your data.

### Don't Outsource Initial Analysis

While you can use external help for scaling later, the initial error analysis should be done by someone who understands your product and users. Domain expertise matters more than scale for this foundational step.

### Don't Stop Too Soon

Reviewing fewer than 50 traces often misses important failure modes. Going deep enough to reach saturation is worth the extra time - it prevents building evals for problems that don't exist.

## From Error Analysis to Action

Once you have a failure taxonomy:

1. **Prioritize by impact:** Which failures affect most users or critical workflows?
2. **Assess fixability:** Can these be fixed with prompt changes, or do they require system changes?
3. **Design evaluators:** Create targeted binary tests for each important failure mode
4. **Validate:** Before deploying, verify evaluators catch the failures they're designed to catch

Error analysis is not a one-time activity. As your application evolves, new failure modes will emerge. Make this a recurring practice in your development cycle.

## See Also

- references/trace-analysis-guide.md - For detailed sampling strategies
- references/synthetic-data-patterns.md - For generating test data without production traces
- templates/failure-taxonomy-template.md - For organizing your findings
