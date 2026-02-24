---
name: eval-advisor
description: Advise and support AI eval brainstorming, learning, design, development, and review; including error analysis, evaluator strategy, judge validation, RAG/agentic evaluation, CI/CD monitoring, and quality/cost improvement.
allowed-tools: Read, Grep, Glob
context: inline
---

# AI Evals Review and Advisory Skill

This skill provides applied guidance for application-centric AI evaluation.

## When to Use This Skill

Invoke this skill when the user asks about:
- Brainstorming or learning AI eval concepts and strategy
- Designing, developing, or reviewing AI evals
- Error analysis and failure taxonomy creation
- Collaborative annotation and rubric alignment
- LLM-as-judge prompting, validation, calibration, or drift
- RAG evaluation (retrieval + generation)
- Tool-calling/agentic pipeline evaluation and debugging
- CI/CD evaluation pipelines and production monitoring
- Human review operations and trace sampling
- Accuracy/cost improvement strategy for LLM pipelines

## Core Principles

### Error analysis first
Do not build evaluators before understanding actual failures in traces.

### Binary application-specific criteria
Use pass/fail criteria tied to observed product failures, not generic quality scores.

### Cheapest effective evaluator first
Prefer assertions for objective checks; use LLM judges only where needed.

### Validate judges against human labels
Use held-out validation and report TPR/TNR (not accuracy-only).

### Monitor with uncertainty-aware metrics
When judge outputs drive production decisions, track corrected estimates and confidence intervals.

### Continuous human-in-the-loop
Automated evaluators scale, but humans are required for drift detection and new failure discovery.

### Deterministic loading protocol (mandatory)

For every user request, follow this exact order:
1. Always load `EVAL_MASTER.md` as the routing index (with `SKILL.md`).
2. Classify request into one primary workflow domain.
3. Use `EVAL_MASTER.md` to select the exact files for that workflow.
4. Load exactly one primary reference for that domain first.
5. Load the corresponding workflow checklist.
6. Load a template only if user needs an artifact/deliverable.
7. Load secondary references only when the selected workflow explicitly requires them.

## Quick Reference

- `references/` = source of truth for concepts, definitions, tradeoffs
- `workflows/` = execution checklists and sequencing
- `templates/` = output artifact formats only

### Conflict resolution rule

If two references appear to define the same concept differently:
1. Treat the routing/ownership mapping in `EVAL_MASTER.md` as authoritative.
2. Use the other file only as supplemental context.
3. Flag overlap for cleanup in the next skill maintenance pass.

## Workflow Modules

All 12 workflow modules (including exact file attachments per module) are defined in:
- `EVAL_MASTER.md`

## Critical Anti-Patterns to Block

- Building evals before error analysis
- Using generic metrics as primary quality indicators
- Using Likert scales as primary eval output for shipping decisions
- Using unvalidated LLM judges in production
- Reporting accuracy-only on imbalanced judge tasks
- Treating CI pass rate as production quality proxy
- Ignoring judge drift/revalidation
- Running human review without structured sampling and feedback integration

## Instruction Patterns

When user says "let's build evals":
First ask: "What failures did your error analysis find in real traces?"

When user asks about evaluator choice:
Ask: "Can this be checked deterministically with assertions before using an LLM judge?"

When user mentions multi-reviewer disagreement:
Load collaborative workflow and propose independent-labeling + alignment loop.

When user wants production judge dashboards:
Load calibration/uncertainty guide and require corrected estimates + confidence intervals.

When user asks about RAG quality:
Require separate retrieval and generation evaluation plans.

When user asks about agent debugging:
Use first-failure transition matrix workflow.

When user asks about CI/CD:
Enforce distinction between regression safety (CI) and live quality estimation (monitoring).
