# Failure Taxonomy Template

Use this template to organize failure modes discovered during error analysis.

This template is intentionally observation-first:
- Start from raw trace review and open coding
- Build categories from observed failures (not predefined lists)
- Separate diagnosis and fix planning until after taxonomy stabilization

## Phase 1: Open Coding Output (Input to Taxonomy)

For each reviewed trace, capture:
1. `trace_id`
2. `pass_fail` (binary)
3. `first_failure_note` (raw observation, no diagnosis)

Example row format:

| trace_id | pass_fail | first_failure_note |
|---|---|---|
| `<trace_001>` | `FAIL` | `<assistant answered a different question than user asked>` |

## Phase 2: Failure Taxonomy (Primary Template)

After open coding, cluster first-failure notes into categories.

| Field | Required | Description |
|---|---|---|
| Category | Yes | Clear, specific failure mode name derived from your notes |
| Definition | Yes | One-line rule for what belongs in this category |
| Inclusion Rule | Yes | Binary boundary: when this is a `PASS` vs `FAIL` for this mode |
| Exclusion Rule | Yes | Binary boundary for what looks similar but must **not** be labeled as this mode |
| Example First-Failure Notes | Yes | 2-5 representative notes from open coding |
| Confusable Neighbor | Yes | Most likely overlapping mode to compare against during labeling |
| Count (`n`) | Yes | Number of traces where this is the first failure |
| Percent of failed traces (`n / total_failed`) | Optional | Include denominator explicitly |
| Impact | Optional | User/business severity (`High`, `Medium`, `Low`) |
| Priority | Optional | Sequence for investigation/fixing |

Template rows:

| Category | Definition | Inclusion Rule | Exclusion Rule | Confusable Neighbor | Example First-Failure Notes | Count (`n`) | Percent of failed traces | Impact | Priority |
|---|---|---|---|---|---|---:|---:|---|---|
| `<mode_1>` | `<short definition>` | `<binary boundary>` | `<what is not this mode>` | `<nearest mode>` | `<note1>; <note2>` | `<n1>` | `<p1>%` | `<H/M/L>` | `<1..N>` |
| `<mode_2>` | `<short definition>` | `<binary boundary>` | `<what is not this mode>` | `<nearest mode>` | `<note1>; <note2>` | `<n2>` | `<p2>%` | `<H/M/L>` | `<1..N>` |
| `<mode_3>` | `<short definition>` | `<binary boundary>` | `<what is not this mode>` | `<nearest mode>` | `<note1>; <note2>` | `<n3>` | `<p3>%` | `<H/M/L>` | `<1..N>` |

## Phase 2.5: Trace-by-Failure-Mode Matrix (for Quantification)

After defining failure modes, re-code traces into a binary matrix for measurement.

Rules:
1. One row per trace.
2. One binary column per failure mode (`1` = present, `0` = absent).
3. Keep `first_failure_mode` for fast upstream analysis.
4. For high-priority modes, add exhaustive occurrence columns in a second pass.

Example matrix:

| trace_id | first_failure_mode | mode_constraint_violation | mode_retrieval_failure | mode_inappropriate_tone | mode_hallucination | mode_inappropriate_tone_exhaustive_count |
|---|---|---:|---:|---:|---:|---:|
| `<trace_001>` | `<mode_constraint_violation>` | 1 | 0 | 0 | 0 | 0 |
| `<trace_002>` | `<mode_retrieval_failure>` | 0 | 1 | 1 | 0 | 2 |
| `<trace_003>` | `<mode_hallucination>` | 0 | 0 | 0 | 1 | 0 |

Use this matrix to compute prevalence and prioritize work:
- First-failure prevalence (broad discovery)
- Any-occurrence prevalence for high-priority modes (impact measurement)

This matrix is a required completion artifact for error-analysis closure.

## Phase 2.6: Non-Overlap Audit (Required)

Run a quick overlap test on confusable mode pairs before finalizing taxonomy.

| Mode A | Mode B | Distinguishing Question | Example That Should Be A | Example That Should Be B | Resolved (`Yes/No`) |
|---|---|---|---|---|---|
| `<mode_a>` | `<mode_b>` | `<single binary question>` | `<short example>` | `<short example>` | `<Yes/No>` |
| `<mode_c>` | `<mode_d>` | `<single binary question>` | `<short example>` | `<short example>` | `<Yes/No>` |

## Phase 3: Post-Taxonomy Action Plan (Separate From Taxonomy)

Only after taxonomy review, add hypotheses for remediation.

| Category | Suspected Root Cause (Hypothesis) | Candidate Fixes | Evaluation Signal to Watch |
|---|---|---|---|
| `<mode_1>` | `<hypothesis, can be wrong>` | `<prompt change / assertion / retrieval change / etc.>` | `<metric or targeted evaluator>` |
| `<mode_2>` | `<hypothesis, can be wrong>` | `<candidate fix>` | `<metric>` |

## Phase 3.5: Quick Wins Before Full Evaluator Build (Optional)

If early analysis shows one dominant failure, apply a fast fix first, then re-sample.

| Category | Why Immediate | Fast Fix Candidate | Recheck Signal |
|---|---|---|---|
| `<mode_1>` | `<dominates early failures>` | `<prompt/spec/tool fix>` | `<drop in first-failure prevalence>` |

## Guardrails for Quality

1. Do not start with predefined category names.
2. Use first-failure annotation initially to avoid downstream-noise inflation.
3. Keep open-coding notes descriptive; avoid diagnosing during annotation.
4. If you report percentages, always state the denominator.
5. If each failed trace has one first-failure label, category percentages should sum to ~100% (rounding aside).
6. Require inclusion + exclusion rules for every mode before quantification.
7. Do not finalize taxonomy while overlap audit rows remain unresolved.

## Maintenance

Re-run and refine taxonomy as product behavior changes. Merge/split categories when new traces show overlap or new failure patterns.
