# Architecture and Modality Evaluation Guide

Use this guide for tool-calling pipelines, agentic systems, and non-trivial input modalities.

## Tool Calling: Stage-by-Stage Checks

Evaluate each stage independently.

### 1) Tool selection
- Correct tool chosen for intent
- No hallucinated tools
- Tool use triggered when required

### 2) Argument generation
- Schema/type validity
- Required fields present
- Value semantics correct
- Safety constraints enforced

### 3) Execution success
- Runtime/API/database errors captured
- Silent failures detected (empty/invalid but syntactically valid outcomes)

### 4) Tool output handling
- Output interpreted correctly
- Critical details preserved
- Results integrated into final response/action

## Agentic Systems: Evaluate Strategy, Not Only Outcome

Define expected autonomy level before judging behavior.

Check for:
- Reasonable decision sequence
- Progress toward goal without looping
- Appropriate initiative vs escalation behavior
- Adherence to intended autonomy policy

## Multi-Step Debugging with Transition Failure Analysis

For failed traces:
1. Define pipeline states
2. Identify first failure state per trace
3. Build transition failure matrix:
   - Rows: last successful state
   - Columns: first failure state
4. Prioritize hotspots by column totals and transition frequency

Use this to focus error analysis on highest-leverage transitions.

## Modality-Specific Considerations

### Images / vision-language
- Spatial reasoning errors
- Counting errors
- OCR/read-text errors
- Hallucinated visual details

### Long documents
- Lost-in-context failures
- Chunking/task mismatch
- Chunk-level correctness vs whole-document consistency

### PDF pipelines
- Extraction/OCR quality issues
- Table/layout parsing errors
- Reading-order errors
- Misattribution of extraction errors to reasoning model

Always inspect what the model actually received, not only the source artifact.

## Common Pitfalls

- Declaring success from end-to-end pass rates while stage metrics fail
- Poor traceability across tool calls and reasoning steps
- Undefined autonomy expectations in agentic systems
- Skipping component-level checks before integrated evaluation
