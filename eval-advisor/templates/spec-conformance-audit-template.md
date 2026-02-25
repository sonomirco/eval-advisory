# Spec Conformance Audit Template

Use this template before designing automated evaluators. The goal is to separate specification ambiguity from true model generalization failures.

## 1) Contract Definition

Define the response contract implied by your current prompt/spec.

| Contract ID | Requirement | Type | Binary Check Rule |
|---|---|---|---|
| `C01` | `<required output structure>` | `format` | `<pass/fail rule>` |
| `C02` | `<required content element>` | `content` | `<pass/fail rule>` |
| `C03` | `<behavior constraint>` | `policy` | `<pass/fail rule>` |

## 2) Trace-Level Conformance Table

Sample real traces and score each contract requirement.

| trace_id | query_summary | C01 | C02 | C03 | conformant_overall (`1/0`) | failure_class (`spec_ambiguity` / `generalization`) | note |
|---|---|---:|---:|---:|---:|---|---|
| `<trace_001>` | `<short summary>` | 1 | 0 | 1 | 0 | `spec_ambiguity` | `<missing mandatory instruction>` |
| `<trace_002>` | `<short summary>` | 1 | 1 | 0 | 0 | `generalization` | `<instruction clear but missed>` |

## 3) Aggregate Metrics

Use explicit denominators.

| Metric | Formula | Value |
|---|---|---|
| Overall conformance rate | `n_conformant / n_total` | `<...>` |
| Spec-ambiguity failure share | `n_spec_ambiguity / n_failed` | `<...>` |
| Generalization failure share | `n_generalization / n_failed` | `<...>` |

## 4) Decision Gate

- If spec-ambiguity failures dominate, revise prompt/spec first and rerun this audit.
- Only proceed to evaluator design when failure signal is mostly generalization.

| Decision | Threshold Rule | Result |
|---|---|---|
| Proceed to evaluator design? | `<rule>` | `<Yes/No>` |
| Prompt/spec revision required? | `<rule>` | `<Yes/No>` |

## 5) Revision Log

Track prompt/spec updates and re-audit outcomes.

| revision_id | change_summary | conformance_rate_before | conformance_rate_after | proceed (`Yes/No`) |
|---|---|---:|---:|---|
| `<r1>` | `<what changed>` | `<...>` | `<...>` | `<Yes/No>` |
