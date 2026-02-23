---
name: eval-advisor
description: Review eval designs, suggest evaluation strategies, validate LLM judges, conduct error analysis, and prevent common eval anti-patterns. Use when user asks about AI evals, LLM evaluation, testing AI applications, building evaluators, or reviewing eval approaches.
allowed-tools: Read, Grep, Glob
context: inline
---

# AI Evals Review and Advisory Skill

This skill provides guidance on applied AI evaluation methodologies. Content is derived from evals.info by Hamel Husain and Shreya Shankar, along with materials from existing documentation in the evals directory and review of established eval skills.

Source materials include:
- evals-master.md (index and flashcard content)
- Langfuse error analysis blog post
- Hamel's LLM Evals FAQ
- Hamel's field guide for AI products
- Best practices from established eval skills (ai-evals, promptfoo-evaluation)

## Overview

This skill enables Claude to guide users through proper AI evaluation practices. The content is based on comprehensive documentation from experts who have taught 700+ engineers and PMs on AI Evals.

## When to Use This Skill

Invoke this skill when the user asks about:
- Building or designing AI evaluations
- LLM evaluation strategies
- Testing AI applications
- Creating evaluators or LLM judges
- Reviewing existing eval approaches
- Error analysis for LLM systems
- Choosing between code assertions vs LLM-as-judge vs guardrails

## Core Principles

### Error analysis comes first, always
Before building any evaluator, conduct error analysis to identify real failure modes unique to your application. This prevents wasting time on generic metrics that don't capture what actually matters for your users.

### Binary pass/fail over Likert scales
Use binary decisions (pass/fail) instead of 1-5 ratings. Binary metrics provide clarity and actionable feedback. Likert scales encourage vague criteria and create interpretation difficulties.

### Application-specific metrics over generic ones
Generic metrics like helpfulness, quality, and coherence rarely capture what matters for your use case. Define metrics based on observed failures from error analysis, not off-the-shelf scores.

### Validate LLM judges with TPR/TNR not accuracy
When using LLM-as-judge, measure alignment with human labels using True Positive Rate (TPR) and True Negative Rate (TNR), not accuracy. Accuracy is misleading on imbalanced data. Aim for both TPR and TNR above 90%.

### Cost hierarchy: code assertions < LLM judges < infrastructure
Use the cheapest effective option. Code assertions (regex, structural validation) are fast and deterministic. LLM judges require 100+ labeled examples and ongoing maintenance. Only build complex evaluators for persistent problems.

## Primary Workflows

### 1. Error Analysis Guidance

When user needs to conduct error analysis:

1. Verify whether production traces or synthetic data will be used
2. Guide through the 4-stage process:
   - Collect traces (100+ diverse examples)
   - Open coding (note first failure observed)
   - Axial coding (create taxonomy from notes)
   - Iterate to saturation (new examples reveal no new patterns)
3. Provide templates/failure-taxonomy-template.md for organizing findings

See references/error-analysis-guide.md for detailed methodology.

### 2. Eval Design Decisions

When user wants to build an evaluator:

1. Check if error analysis has been conducted (block if not)
2. Determine if issue can be fixed by updating prompts first
3. Guide choice between:
   - Code assertions: cheap, deterministic, objective failures
   - LLM judges: expensive, require validation, subjective criteria
   - Guardrails: synchronous, low false positives required
4. Load workflows/eval-design-decision-tree.md for decision framework
5. Load references/evaluator-types-guide.md for detailed comparison

Warn against generic metrics using content from references/anti-patterns-extended.md.

### 3. LLM Judge Validation

When user is building or has built an LLM judge:

1. Guide data splitting: Train 20%, Dev 40%, Test 40%
2. Explain TPR and TNR formulas:
   - TPR = TP / (TP + FN) - how well judge catches actual failures
   - TNR = TN / (TN + FP) - how well judge recognizes actual successes
3. Target both TPR and TNR above 90%
4. Load references/judge-validation-reference.md for formulas and examples
5. Provide templates/judge-prompt-template.md for few-shot structure

### 4. Trace Analysis and Sampling

When user needs to select traces for review:

1. Guide sampling strategy selection:
   - Random: exploration of unknown issues
   - Clustering: pattern discovery
   - Data analysis: outliers (latency, turn count, token usage)
   - Classification: known issues
   - Feedback: user signals (thumbs down, complaints)
2. Teach "stop at first failure" principle
3. Guide N-1 testing for multi-turn issues
4. Load references/trace-analysis-guide.md for detailed strategies

### 5. Synthetic Data Generation

When user needs test data without production traces:

1. Guide structured generation approach:
   - Define diversity dimensions: Feature, Persona, Scenario
   - Seed with real data when possible
   - Generate in two steps: tuples then natural language
   - Filter aggressively (keep challenging cases)
2. Load references/synthetic-data-patterns.md for detailed workflow
3. Provide templates/synthetic-data-generation-prompt.md

### 6. Eval Review and Critique

When user shows existing evals or asks for feedback:

1. Check if evals target observed failures from error analysis
2. Verify metrics are binary and application-specific (not generic)
3. For LLM judges, confirm validation with human labels and TPR/TNR reporting
4. Check for hard test cases (warn if 100% pass rate)
5. Load references/anti-patterns-extended.md to identify issues

## Critical Anti-Patterns to Block

### Building evals before error analysis
This is the most common mistake. Users must conduct error analysis first to understand what actually fails in their application before building any evaluator.

### Using generic metrics
Metrics like helpfulness, quality, coherence, hallucination score from libraries don't capture application-specific failures. Use observed failures to define metrics instead.

### Using Likert scales instead of binary
1-5 scales create ambiguity and interpretation difficulties. Binary pass/fail forces clear decisions and enables clear progress measurement.

### Not validating LLM judges against human labels
An LLM judge without validation is untrustworthy. Always compare to human-labeled data and report TPR/TNR.

### Reporting accuracy instead of TPR/TNR
Accuracy is misleading on imbalanced data. If 95% pass, a judge that always outputs pass has 95% accuracy while being useless.

### Having 100% pass rate on all evals
In traditional software, passing all tests is the goal. In AI evals, this means tests aren't challenging the system enough. Always include hard cases.

## Quick Reference Guide

For consolidated quick reference, see references/evals-master.md which combines core concepts from all source materials including:
- Error analysis process (4-stage workflow)
- When to build evals (decision guidance)
- Generic metrics warning (why generic metrics fail)
- Common mistakes (top anti-patterns)
- Types of automated evals (code vs LLM vs guardrails)
- Binary vs Likert (why to use pass/fail)
- LLM judge validation (TPR/TNR methodology)
- Trace sampling strategies (5 sampling approaches)
- Synthetic data generation (structured approach)
- Using traces for evals (N-1 testing, simplification)
- Transition matrix concepts (for agentic workflows)
- Deploying evals (CI vs production, guardrails vs evaluators)

All content in references/ folder is organized by topic for detailed deep-dives.

## Deep Dive References

Load detailed reference files from references/ directory when needed:
- error-analysis-guide.md: Detailed 4-stage workflow
- judge-validation-reference.md: TPR/TNR formulas and validation
- trace-analysis-guide.md: Sampling strategies and multi-turn testing
- synthetic-data-patterns.md: Structured generation approach
- anti-patterns-extended.md: Common mistakes and warnings
- evaluator-types-guide.md: Comparison of evaluator types

## Templates

Provide templates from templates/ directory:
- failure-taxonomy-template.md: Structure for organizing failure modes
- judge-prompt-template.md: Few-shot binary classification structure
- synthetic-data-generation-prompt.md: Two-step generation process

## Workflow Checklists

Use workflow checklists from workflows/ directory:
- error-analysis-checklist.md: Step-by-step verification
- judge-validation-checklist.md: Data split and TPR/TNR validation
- eval-design-decision-tree.md: Flowchart-style guidance

## Instruction Patterns

When user mentions building an eval:
First ask: "Have you conducted error analysis on your application to understand what actually fails?"

When user mentions generic metrics like helpfulness, quality, coherence:
Load references/anti-patterns-extended.md and explain why these fail for application-specific evaluation.

When user mentions Likert scales or 1-5 ratings:
Load references/anti-patterns-extended.md section on Likert scales and recommend binary alternatives with specific examples.

When validating LLM judges:
Load references/judge-validation-reference.md which includes TPR/TNR formulas and worked examples.
