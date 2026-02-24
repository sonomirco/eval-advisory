# Human Review Operations Checklist

Use this checklist to run continuous human review efficiently.

## Interface Readiness

- [ ] Review UI shows complete trace context
- [ ] Binary labels and failure tags are easy to apply
- [ ] Open-note field exists for novel failure modes
- [ ] Defer action exists for uncertain cases
- [ ] Keyboard shortcuts configured for common actions

## Sampling Program

- [ ] Random sampling stream configured
- [ ] Uncertainty sampling stream configured
- [ ] Failure-driven sampling stream configured
- [ ] Daily/weekly review quotas set by stream

## Review Cadence

- [ ] Review batches delivered on schedule
- [ ] High-severity traces routed with alerts
- [ ] Reviewer assignment/ownership defined

## Feedback Integration

- [ ] One-click path to add traces to golden dataset
- [ ] One-click path to open bug with trace context
- [ ] Judge disagreement with human labels tracked
- [ ] New recurring failure tags fed back into taxonomy

## Quality Control

- [ ] Reviewer consistency checks performed periodically
- [ ] Rubric version visible during labeling
- [ ] Criteria drift review meeting scheduled
