# Trace Analysis and Sampling Guide

This guide covers strategies for effectively sampling and analyzing traces from your LLM application to conduct error analysis and evaluation.

## What is a Trace?

A trace is a complete record of all actions, messages, tool calls, and data retrievals from a single initial user query through to the final response. It includes every step across all agents, tools, and system components in a session.

**For multi-turn conversations:** A single trace contains all turns from the initial user message through the entire conversation history.

## Sampling Strategies

You have thousands of production traces. Which ones should you review? You can't look at everything. Smart sampling helps you find problems faster.

### Strategy 1: Random Sampling

**When to use:** Always include as part of your approach

**Purpose:** Discover issues you don't know about yet

**Method:**
- Uniform random selection from your trace pool
- Ensures coverage of entire interaction space
- No bias toward known problem areas

**Guidance:**
- Minimum 50-100 random traces for initial analysis
- Combine with targeted strategies for efficiency

### Strategy 2: Clustering

**When to use:** Finding patterns in your data

**Purpose:** Group semantically similar traces to identify problematic clusters

**Method:**
- Use embeddings to cluster traces by semantic similarity
- Review representative examples from each cluster
- Compare error rates across clusters

**Tools:**
- Vector similarity (cosine similarity on embeddings)
- Topic modeling (LDA, clustering algorithms)
- LLM-assisted clustering

**Guidance:**
- If certain clusters have much higher error rates, focus investigation there
- Use clustering to direct manual review, not replace it

### Strategy 3: Data Analysis

**When to use:** Finding outliers and anomalies

**Purpose:** Identify traces with unusual characteristics that may indicate problems

**Metrics to examine:**
- **Latency:** unusually high response times
- **Turn count:** conversations with many back-and-forth exchanges (potential confusion)
- **Token usage:** very low or very high token consumption
- **Tool calls:** traces with many failed or retried tool calls
- **Error codes:** traces containing system errors

**Guidance:**
- Set thresholds (e.g., >10 turns, >30s latency)
- Investigate outliers to understand root causes
- These often reveal systemic issues not visible in individual trace review

### Strategy 4: Classification-Based Sampling

**When to use:** You have existing evals or classifiers

**Purpose:** Use existing signals to find likely problematic traces

**Method:**
- Run existing evaluators on all traces
- Use predictive models to flag problematic traces
- Review traces flagged as high-risk

**Guidance:**
- Only finds issues your classifier already knows about
- Use in combination with random sampling to discover novel failures
- Don't rely on exclusively - you'll miss new failure modes

### Strategy 5: Feedback Signals

**When to use:** You have user feedback mechanisms

**Purpose:** Direct attention to real problems users experience

**Signals to prioritize:**
- **Negative feedback:** Thumbs down, complaints, low ratings
- **Complaints:** Support tickets, user reports of issues
- **Abandonment patterns:** Where users disengage or retry excessively
- **Explicit flags:** User-reported bugs or problems

**Guidance:**
- Strongest signal available - points directly to real user pain
- Prioritize these traces for review
- Use to validate that your error analysis captures what matters to users

## Effective Trace Reading

### Principle 1: Stop at First Failure

When reviewing a trace, don't catalog every problem. Find the most upstream error and stop there.

**Why this matters:**
- First failures often cause downstream issues
- Fixing the root problem resolves multiple symptoms
- Keeps review process tractable and efficient

**Example:**
If the assistant misinterprets the user's intent on turn 2, everything downstream is affected. Fixing the misinterpretation often resolves subsequent issues automatically.

### Principle 2: Simplify Before Testing

When you find a failure in a multi-turn conversation, try to reproduce it with the simplest possible input.

**Process:**
1. Identify the failure
2. Create a minimal test case that reproduces it
3. Remove unnecessary context and complexity
4. Verify the simplified test still fails

**Example:**
A shopping bot gives the wrong return policy on turn 4. Before diving into the full multi-turn complexity, test: "What is the return window for product X?" If it still fails, you've proven the error isn't about conversation context - it's a basic retrieval or knowledge issue.

### Principle 3: Use N-1 Testing for Multi-Turn Issues

For failures that only happen after several turns (like context forgetting), take the first N-1 turns and use them as your test input.

**Process:**
1. Isolate the conversation prefix before the failure occurs
2. Run the model multiple times from that point
3. Observe consistency of handling

**Creates:** Regression tests from real conversation prefixes that catch context retention issues.

### Principle 4: Read User-Visible Parts First

When examining a trace, read what the user saw before diving into technical details.

**Order:**
1. User's initial query
2. Assistant's response (as rendered to user)
3. User's reaction or follow-up
4. Only then: system prompts, tool calls, intermediate steps

**Why this matters:**
The user experience is determined by what they saw, not by what happened internally. Technical issues are only problems insofar as they affect the user-facing outcome.

## Multi-Turn Conversation Analysis

### Annotation Strategy

For multi-turn traces:
1. Annotate only the first failure initially
2. Don't worry about downstream failures - they often cascade from the first issue
3. As you gain experience, annotate independent failure modes within the same trace

### Test Case Generation

Two main approaches:

**1. Simulate with LLM:**
- Use another LLM to play the user role
- Generate realistic multi-turn conversations
- More flexible but less grounded in actual patterns

**2. N-1 Testing:**
- Use first N-1 turns from real conversations
- More representative of actual usage
- Better for regression testing

## Agentic Workflow Analysis

For agent-based systems, additional considerations:

### Session/Trace Logging
Assign a session or trace ID to each user request. Log every message with:
- Source (which agent or tool)
- Trace ID
- Position in sequence

This lets you reconstruct the full path from initial query to final result.

### Outcome vs Process Metrics

**Outcome metrics:** Did we meet the user's goal?
- Task completion status
- Accuracy of final result
- Business objective achieved

**Process metrics:** How efficiently did we get there?
- Step count
- Time taken
- Resource usage

Process failures are often easier to debug since they're more deterministic.

### Transition Failure Matrices

Create a matrix showing:
- Rows: Last successful state
- Columns: Where first failure occurred

This reveals failure hotspots and guides where to invest debugging effort.

**Example:**
|                  | GenSQL | ExecSQL | PlanCal | DecideTool |
|------------------|---------|----------|----------|-------------|
| Success          | 85       | 80        | 78        | 90           |
| First Failure     | 12       | 15        | 20        | 8            |

This shows GenSQL→ExecSQL transitions cause the most failures.

## See Also

- references/error-analysis-guide.md - For conducting error analysis on sampled traces
- references/evals-master.md section 10 - For detailed trace usage guidance
- workflows/error-analysis-checklist.md - Step-by-step verification
