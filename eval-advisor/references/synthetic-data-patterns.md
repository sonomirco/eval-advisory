# Synthetic Data Generation Patterns

This guide covers structured approaches to generating synthetic test data for AI evaluation when production data is unavailable or insufficient.

## When to Use Synthetic Data

Use synthetic data when:
- You don't have production traces yet
- Your production data is too sparse to cover failure modes you care about
- You need to test specific scenarios not well-represented in production
- You want to bootstrap evals before user launch

## The Wrong Way

**Prompt:** "Generate 50 test cases for my AI system"

**Problems:**
- Generic, repetitive inputs that miss edge cases
- Don't reflect real usage patterns or language
- Miss domain-specific constraints and terminology
- Fail to exercise the scenarios you actually care about

## The Right Way: Use Structure

### Step 1: Define Diversity Dimensions

Identify categories that describe variation in your queries. Common dimensions:

**Feature/Intent:** What does the user want to accomplish?
- For calendar app: schedule meeting, check availability, reschedule, cancel
- For code assistant: write function, debug, explain code, refactor

**Persona:** Who is the user and what's their context?
- For enterprise tool: admin vs end-user vs guest
- For consumer app: new user vs power user vs expert
- Technical sophistication, language preference, urgency

**Scenario:** How is the request expressed?
- Simple vs complex
- Direct vs ambiguous
- Single-task vs multi-step
- Happy path vs edge case

**Example dimensions for a calendar scheduling bot:**
- Feature: (schedule, check_availability, reschedule, cancel, recurring_meeting)
- Persona: (executive_assistant, hr_admin, external_guest, first_time_user)
- Scenario: (single_attendee, multiple_attendees, no_slots, conflict_resolution)

### Step 2: Create Structured Tuples

Generate combinations across your dimensions as structured tuples before converting to natural language.

**Why this works:**
- Ensures coverage of all dimension combinations
- Separates structure from language generation
- Makes patterns obvious
- Prevents repetitive phrasing

**Example:**
```
(executive_assistant, schedule, single_attendee)
(hr_admin, check_availability, multiple_attendees)
(external_guest, cancel, no_slots)
```

### Step 3: Seed with Real Data

Ground your synthetic data in real system constraints whenever possible.

**Sources of grounding:**
- Real user queries (anonymized)
- Actual database entries (for testing retrieval)
- Real business rules and constraints
- Actual edge cases observed in production

**Benefits:**
- Synthetic examples respect system limitations
- Test cases align with actual usage patterns
- You verify scenarios are realistically achievable

**Example:**
Instead of: "Find a meeting at 3pm on Tuesday"
Use real availability windows from your system:
- "Find a meeting at 3pm on Tuesday during [available_slots]"
- This forces synthetic queries to respect actual constraints

### Step 4: Generate Natural Language in Two Steps

**Step 1:** Generate structured tuples representing test cases
**Step 2:** Convert each tuple to natural language in a separate prompt

**Why two steps:**
- Separates specification from expression
- Prevents repetitive phrasing patterns
- Each query can be expressed multiple ways
- Higher diversity in final output

**Example prompts:**

**Step 1 - Tuple Generation:**
```
Generate 50 test case tuples as (persona, feature, scenario) for a
calendar scheduling assistant. Ensure diverse coverage across all dimensions.
```

**Step 2 - Natural Language Conversion:**
```
For each tuple, generate a natural language query that a user would actually say.
Persona: [persona description]
Feature: [feature]
Scenario: [scenario description]
```

### Step 5: Filter Aggressively

Generate many candidates, then keep only the challenging ones.

**Filtering criteria:**
- Easy cases don't help you find problems
- Keep cases that require correct model behavior
- Include edge cases and stress tests
- Remove overly simple or obvious cases

**Example:**
Generate 100 candidates but only keep the 30 that:
- Require multi-step reasoning
- Test specific failure modes you've observed
- Challenge common model weaknesses
- Include tricky edge cases

**Required logging discipline:**
- Track every candidate as `keep` or `drop` (no silent deletions)
- Record drop reasons (duplicate, unrealistic phrasing, constraint conflict, too-easy, out-of-scope)
- Record whether each kept query preserves tuple intent and constraints
- Use `templates/synthetic-data-quality-gate-template.md`

### Step 6: Validate Scenario Coverage

Ensure your generated data actually triggers the scenarios you intend to test.

**Verification:**
- "No matches" scenario should return zero results
- "Multiple matches" should return 2+ results
- Time-based queries should respect actual availability windows

**Process:**
1. Generate test cases
2. Run through your system
3. Verify expected behaviors occur
4. Remove cases that don't trigger intended scenarios

**Coverage checks (required):**
- Compute per-dimension coverage with explicit denominators
- Verify contradictory/ambiguous scenarios are intentionally represented
- Verify kept set is not dominated by one style/intent bucket

## Quality Guidelines

### Diversify Your Dataset

Create examples that cover a wide range of:
- Features and intents
- User personas and contexts
- Complexity levels
- Edge cases and happy paths

### Generate User Inputs, Not Outputs

Use LLMs to generate realistic user queries or inputs, not expected AI responses.

This prevents:
- Inheriting biases of the generating model
- Artificially limiting response diversity
- Overfitting to specific answer patterns

### Incorporate Real System Constraints

Ground synthetic data in actual system limitations and data:
- Business rules and constraints
- Availability windows and capacity limits
- Geographic or domain-specific restrictions
- Data format requirements
- Feasibility constraints (avoid impossible combinations unless testing contradiction handling)

### Start Simple, Then Add Complexity

Begin with straightforward test cases before adding nuance.
- Helps isolate issues
- Establishes a baseline
- Makes debugging easier
- Add complexity iteratively

### Enforce a Quality Gate Artifact

Do not treat generation output as final dataset. Always run a human-reviewed quality gate pass and keep the audit table.

## Common Patterns by Application Type

### RAG/Retrieval Systems
Generate queries that test:
- Correct document retrieval vs missing documents
- Handling of contradictory information
- Partial answers vs complete answers
- Out-of-domain queries

### Conversational Agents
Generate conversations that test:
- Multi-turn context retention
- Instruction following across turns
- Clarification asking behavior
- Handoff detection and escalation

### Code Generation
Generate test cases for:
- Correctness of generated code
- Security vulnerabilities
- Edge cases in logic
- Performance and efficiency
- Handling of ambiguous requirements

### Content Generation
Generate prompts that test:
- Factual accuracy
- Hallucination detection
- Tone and style consistency
- Format adherence
- Length and completeness

## From Synthetic to Production

Once you have real user data:

1. Compare synthetic patterns to real usage
2. Add real edge cases to your test set
3. Validate that synthetic tests still catch real issues
4. Gradually replace synthetic examples with real ones

Synthetic data is for bootstrapping, not long-term replacement for real data.

## See Also

- references/error-analysis-guide.md - For identifying failure modes to test with synthetic data
- EVAL_MASTER.md - For topic routing overview
- templates/synthetic-data-generation-prompt.md - For implementation templates
- templates/synthetic-data-quality-gate-template.md - For keep/drop review and coverage audit
