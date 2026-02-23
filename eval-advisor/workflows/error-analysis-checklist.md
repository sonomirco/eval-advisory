# Error Analysis Checklist

Use this checklist to verify you've completed error analysis properly before building evaluators.

## Preparation

- [ ] **Define scope:** What questions are you trying to answer? Which features or workflows?
- [ ] **Set sample size goal:** Target 50-100 traces for initial analysis
- [ ] **Identify data source:** Production traces, synthetic data, or combination
- [ ] **Plan diversity:** What dimensions do you need represented? (user types, scenarios, complexity levels)

## Stage 1: Gather Dataset

- [ ] **Collect diverse traces:** Not just random, but covering different:
  - [ ] User intents
  - [ ] Complexity levels (simple to complex)
  - [ ] Edge cases and happy paths
  - [ ] Different user personas
  - [ ] Various interaction lengths
- [ ] **Minimum 50 traces collected**
- [ ] **Traces include:** User query, system response, tool calls, metadata
- [ ] **Data source documented:** Where did each trace come from?

## Stage 2: Open Coding

- [ ] **Binary scoring applied:** Each trace marked Pass or Fail (no middle ground)
- [ ] **First failure noted:** Focus on most upstream error, not cataloging every problem
- [ ] **Open-ended descriptions:** Qualitative notes without pre-conceived categories
- [ ] **Domain expert involved:** Someone who understands your users and product
- [ ] **No premature categorization:** Don't cluster during open coding phase

## Stage 3: Axial Coding

- [ ] **All open-coded notes reviewed:** Looked at every note from Stage 2
- [ ] **Patterns identified:** Grouped similar issues into categories
- [ ] **Categories defined:** Each category has clear name and description
- [ ] **Human review completed:** Domain expert validated and refined clusters
- [ ] **Taxonomy documented:** Failure modes recorded in organized format
- [ ] **Examples added:** Each category has representative trace examples

## Completion Check

- [ ] **Saturation reached:** New traces no longer reveal new failure modes (last 20-30 traces showed only existing types)
- [ ] **Coverage adequate:** All major failure patterns identified
- [ ] **Actionable taxonomy:** Clear enough to guide evaluator development

## Output Deliverables

- [ ] **Failure taxonomy created:** Document with categories, descriptions, frequencies
- [ ] **Examples documented:** Representative traces for each failure mode
- [ ] **Prioritization:** Impact and frequency assessed
- [ ] **Next steps clear:** Which evaluators to build first?

## Before Building Evaluators

Confirm you have:
- [ ] **Understood what actually fails:** Not hypothetical problems
- [ ] **Application-specific metrics:** Not generic "helpfulness" or "quality"
- [ ] **Concrete failure modes:** Clear definitions of what constitutes each failure
- [ ] **Real examples:** Actual traces from your data, not made-up scenarios

## Common Pitfalls to Avoid

- [ ] **Didn't skip to generic metrics:** Built based on observed failures only
- [ ] **Didn't outsource analysis:** Done by someone who understands your product
- [ ] **Didn't stop too soon:** Reached saturation before building evaluators
- [ ] **Binary decisions:** Used pass/fail, not Likert scales

## Ready for Next Phase

If you checked all items above, you're ready to:
1. Design targeted evaluators for your top failure modes
2. Create templates/failure-taxonomy-template.md for organization
3. Build specific evaluation criteria based on real failures, not assumptions

See also:
- references/error-analysis-guide.md - Detailed methodology
- templates/failure-taxonomy-template.md - For documenting findings
