# eval-advisory

`eval-advisor` is a skill for advising, brainstorming, designing, developing, reviewing, and improving AI evaluation systems for LLM applications.

It guides teams through practical evaluation workflows:
- Running error analysis before writing evals
- Choosing the right evaluator type (code assertions, LLM-as-judge, guardrails)
- Validating LLM judges with human labels using TPR/TNR
- Sampling and analyzing traces effectively
- Generating structured synthetic test data when production traces are limited
- Avoiding common eval anti-patterns (generic metrics, Likert scales, unvalidated judges, 100% pass-rate suites)

## Repository Structure

- `eval-advisor/SKILL.md`: Main skill instructions and trigger guidance
- `EVAL_MASTER.md`: Canonical 12-workflow routing and file-loading index
- `eval-advisor/references/`: Deep-dive reference docs (error analysis, evaluator types, judge validation, sampling, synthetic data, anti-patterns)
- `eval-advisor/workflows/`: Actionable checklists and decision trees
- `eval-advisor/templates/`: Reusable templates for failure taxonomies, judge prompts, and synthetic data prompts

## What This Skill Is For

Use this skill when designing new evals, auditing existing eval suites, selecting evaluation strategies, or diagnosing quality failures in AI systems.

The core philosophy is:
1. Look at real failures first (error analysis)
2. Use application-specific, binary pass/fail criteria
3. Prefer the cheapest reliable evaluator
4. Validate any LLM judge rigorously before relying on it

## Source and Attribution

This skill was created from public material released by Hamel Husain.
