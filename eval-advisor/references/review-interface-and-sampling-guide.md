# Review Interface and Sampling Guide

Use this guide to build sustainable human-in-the-loop review operations.

## Why Custom Review Surfaces Matter

Flat logs and spreadsheets degrade review quality when traces include:
- Intermediate reasoning
- Tool calls and outputs
- Retrieved context
- Multi-turn interactions

A purpose-built review UI increases labeling speed, consistency, and insight density.

## Core Interface Requirements

1. Show full trace context with progressive disclosure
2. Collect structured labels (binary + failure tags)
3. Capture open notes for emerging failure modes
4. Support quick navigation and defer actions
5. Expose key metadata (version, segment, timestamp)

## High-Leverage UX Features

- Keyboard shortcuts for frequent actions
- Inline span annotation for pinpointed failures
- Visual hierarchy for user-visible outcomes
- Saved comment snippets/templates
- Batch review for repetitive clusters
- Cluster and semantic-similarity navigation

## Sampling Strategies for Human Review

### Random sampling
- Baseline coverage for unknown unknowns

### Uncertainty sampling
Prioritize traces where:
- Evaluators disagree
- Judge outputs are unstable
- Signals conflict across checks

### Failure-driven sampling
- Prioritize traces flagged by guardrails, runtime errors, or user complaints

Use a blend of all three for balanced coverage.

## Pattern Discovery

Support exploration by:
- Metadata filters
- Cluster browsing
- Similar-trace search from a known bad example

This shifts review from isolated incidents to systemic pattern detection.

## Workflow Integration

Human review should directly update engineering artifacts:
- Add representative failures to golden dataset
- Feed corrected labels into judge revalidation
- Open prefilled bug tickets with trace links
- Track reviewer-vs-judge disagreement over time

## Failure Modes to Avoid

- Reviewing only traces already flagged by automation
- No defer option (forces low-confidence labels)
- No versioned rubric context in the review tool
- Feedback collected but not wired into CI/CD updates
