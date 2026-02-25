# Synthetic Data Generation Prompt Template

This template provides a two-step approach for generating high-quality synthetic test data for AI evaluation.

## Step 1: Generate Structured Tuples

First, generate structured tuples representing your test dimensions. This ensures coverage across all combinations.

### Prompt Template

```
Generate [NUMBER] test case tuples as (dimension1, dimension2, dimension3) for a [YOUR APPLICATION TYPE].

## Dimensions

1. **[DIMENSION 1]**: [Description and possible values]
   - Values: [list of possible values]

2. **[DIMENSION 2]**: [Description and possible values]
   - Values: [list of possible values]

3. **[DIMENSION 3]**: [Description and possible values]
   - Values: [list of possible values]

## Requirements

- Generate diverse combinations across all dimensions
- Ensure coverage of edge cases and common scenarios
- Include combinations that represent known failure modes
- Avoid overly similar or repetitive combinations

Output format: JSON array of tuples, where each tuple is (dimension1_value, dimension2_value, dimension3_value).
```

### Example: Calendar Scheduling Bot

```
Generate 50 test case tuples as (persona, feature, scenario) for a calendar scheduling assistant.

## Dimensions

1. **Persona**: User type and context
   - Values: executive_assistant, hr_admin, external_guest, first_time_user, power_user

2. **Feature**: What the user wants to accomplish
   - Values: schedule_meeting, check_availability, reschedule, cancel_meeting, recurring_meeting, find_slot

3. **Scenario**: Complexity and constraints
   - Values: single_attendee, multiple_attendees, no_slots_available, time_conflict, location_conflict, last_minute_request

## Requirements

- Generate diverse combinations across all dimensions
- Ensure coverage of edge cases (e.g., no slots, multiple attendees)
- Include scenarios that test system's failure modes
- Avoid overly similar or repetitive combinations

Output format: JSON array of tuples.
```

## Step 2: Convert to Natural Language

Convert each structured tuple into a realistic natural language query that a user would actually say.

### Prompt Template

```
For each test case tuple, generate a natural language query that a user would actually say to interact with a [YOUR APPLICATION TYPE].

## Tuple Format
Each tuple: (persona, feature, scenario)

## Conversion Guidelines

- Use natural language, not stilted or formal
- Reflect how real users speak (not how they write)
- Include relevant context and details
- Vary phrasing and structure
- Keep queries concise but realistic
- Ground in actual system constraints when provided

## Context to Include

For [YOUR APPLICATION], ensure queries include:
- [Relevant detail 1]
- [Relevant detail 2]
- [Relevant detail 3]

## Examples

### Input Tuple
(executive_assistant, check_availability, single_attendee)

### Output Query
"Can you check if there's a time slot open next Tuesday afternoon for a quick sync with our London office calendar?"

### Input Tuple
(hr_admin, recurring_meeting, multiple_attendees, no_slots_available)

### Output Query
"We need to set up a recurring weekly team standup. Can you find a time that works for everyone across the Engineering, Product, and Design teams? We're about 15 people total and the meeting should be around 30 minutes."

## Additional Guidelines

- **Seed with real data**: When possible, use actual user queries, real database entries, or real constraints
- **Filter aggressively**: Remove overly simple or obvious cases
- **Add complexity incrementally**: Start simple, then add constraints, edge cases, and ambiguity
- **Verify scenario triggering**: Ensure generated queries actually trigger the intended scenarios
- **Test with your system**: Run through your application to verify they work as intended

Output one query per line in plain text format.
```

## Complete Workflow Example

### Step 1: Generate Tuples

```python
# Generate the tuples
response = llm_call(generate_tuples_prompt)
tuples = parse_json(response)
```

Result:
```json
[
  ["executive_assistant", "check_availability", "single_attendee"],
  ["hr_admin", "recurring_meeting", "multiple_attendees"],
  ["first_time_user", "find_slot", "no_slots_available"],
  ...
]
```

### Step 2: Convert to Natural Language

```python
# Convert to natural language
queries = []
for tuple in tuples:
    prompt = convert_prompt.replace("(persona, feature, scenario)", str(tuple))
    query = llm_call(prompt)
    queries.append(query)
```

Result:
```
Can you check if there's a time slot open next Tuesday afternoon for a quick sync with our London office calendar?

We need to set up a recurring weekly team standup. Can you find a time that works for everyone across the Engineering, Product, and Design teams? We're about 15 people total and the meeting should be around 30 minutes.
```

### Step 3: Filter and Validate

```python
# Filter for complexity and edge cases
hard_queries = [q for q in queries if is_complex(q)]
edge_cases = [q for q in queries if is_edge_case(q)]

# Verify with your system
validated_queries = []
for query in hard_queries:
    result = run_through_system(query)
    if triggers_intended_scenario(result):
        validated_queries.append(query)
```

### Step 4: Run Human Quality Gate (Required)

Before finalizing the synthetic dataset:

1. Mark each candidate as `keep` or `drop`
2. Record a concrete drop reason
3. Verify tuple intent and constraints are preserved in each kept query
4. Compute per-dimension kept coverage with explicit denominators

Use `templates/synthetic-data-quality-gate-template.md`.

## Best Practices

1. **Define dimensions relevant to your application** - What variations matter for your use case?
2. **Use real data as grounding** - Actual user queries, real database entries, real constraints
3. **Generate in two steps** - Tuples first, then natural language
4. **Filter aggressively** - Keep only challenging, diverse cases
5. **Validate with your system** - Ensure queries actually work as intended
6. **Run a documented keep/drop quality gate** - Keep the audit table
7. **Iterate on the process** - Improve prompts based on validation results

See also:
- references/synthetic-data-patterns.md - Detailed methodology and patterns
- references/error-analysis-guide.md - For identifying failure modes to test
- templates/synthetic-data-quality-gate-template.md - Keep/drop and coverage artifact
