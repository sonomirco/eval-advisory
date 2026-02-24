# Agentic Debugging Checklist

Use this checklist for multi-step pipelines with tools and dynamic decision paths.

## Foundations

- [ ] Defined expected autonomy policy (high-initiative vs cautious escalation)
- [ ] Enumerated pipeline states for trace analysis
- [ ] Established state-level failure criteria

## Traceability

- [ ] Captured inputs/outputs for each state
- [ ] Logged tool calls, arguments, responses, and errors
- [ ] Logged intermediate reasoning artifacts where available
- [ ] Stored consistent trace IDs across components

## Failure Attribution

- [ ] Labeled end-to-end outcome per trace
- [ ] Identified first failure state for each failed trace
- [ ] Built transition failure matrix (from-state -> first-failure-state)
- [ ] Ranked hotspots by frequency and impact

## Tool Calling Checks

- [ ] Tool selection correctness checked
- [ ] Argument validity and semantics checked
- [ ] Execution failures separated from model reasoning failures
- [ ] Tool output interpretation checked in final response/action

## Iteration

- [ ] Implemented targeted fixes for top transitions
- [ ] Re-ran traces and compared hotspot counts
- [ ] Updated regression set with representative failing transitions
