# Synthetic Data Quality Gate Template

Use this template to review generated synthetic queries before they are accepted into an eval dataset.

## 1) Candidate Review Table

Log every generated candidate.

| query_id | tuple_id | tuple_json | query_text | keep_drop (`keep/drop`) | drop_reason | realism_score (`1-3`) | tuple_intent_preserved (`1/0`) | constraint_consistent (`1/0`) | duplicate_of | reviewer_note |
|---|---|---|---|---|---|---:|---:|---:|---|---|
| `<Q001>` | `<T001>` | `<...>` | `<query text>` | `keep` | `` | 3 | 1 | 1 | `` | `` |
| `<Q002>` | `<T001>` | `<...>` | `<query text>` | `drop` | `unrealistic_phrasing` | 1 | 0 | 1 | `` | `<explain>` |

Allowed `drop_reason` values:
- `duplicate`
- `unrealistic_phrasing`
- `constraint_conflict_unintended`
- `too_easy_or_uninformative`
- `out_of_scope`
- `format_or_parse_error`

## 2) Coverage Summary (Kept Set Only)

Report coverage with explicit denominators.

| Dimension | Value | kept_count | kept_rate (`kept_count / total_kept`) | minimum_target_met (`Yes/No`) |
|---|---|---:|---:|---|
| `<dimension_name>` | `<value_1>` | `<...>` | `<...>` | `<Yes/No>` |
| `<dimension_name>` | `<value_2>` | `<...>` | `<...>` | `<Yes/No>` |

## 3) Quality Gate Metrics

| Metric | Formula | Value |
|---|---|---|
| Keep rate | `n_keep / n_generated` | `<...>` |
| Duplicate drop rate | `n_drop_duplicate / n_generated` | `<...>` |
| Constraint-consistency rate (kept) | `n_constraint_consistent_keep / n_keep` | `<...>` |
| Tuple-intent preservation rate (kept) | `n_intent_preserved_keep / n_keep` | `<...>` |

## 4) Acceptance Gate

| Gate | Rule | Result |
|---|---|---|
| No silent filtering | all generated rows logged | `<Pass/Fail>` |
| Coverage diversity | no major dimension value missing unintentionally | `<Pass/Fail>` |
| Constraint quality | kept set is mostly constraint-consistent | `<Pass/Fail>` |
| Realism quality | no large cluster of low-realism kept queries | `<Pass/Fail>` |

Final decision: `<Accept / Regenerate / Partial Accept>`

## 5) Regeneration Backlog (Optional)

| tuple_id | issue_found | regeneration_instruction |
|---|---|---|
| `<T017>` | `<missing conflict scenario>` | `<regenerate with clearer conflict wording>` |
