# Rubric Alignment Template

Use this template to define, test, and version a binary evaluation rubric.

## Rubric Header

- Criterion name: `<criterion_name>`
- Rubric version: `<vX.Y>`
- Owner: `<owner_name>`
- Scope: `<where this rubric applies>`
- Last updated: `<YYYY-MM-DD>`

## Binary Decision Definition

| Label | Definition | Boundary Rule |
|---|---|---|
| `PASS` | `<what must be true>` | `<explicit pass conditions>` |
| `FAIL` | `<what counts as failure>` | `<explicit fail conditions>` |

## Inclusion/Exclusion Rules

- Include as `FAIL` when:
  - `<rule_1>`
  - `<rule_2>`
- Do not mark as `FAIL` when:
  - `<non_failure_case_1>`
  - `<non_failure_case_2>`

## Canonical Examples

| Example ID | Input Summary | Label | Why |
|---|---|---|---|
| `<ex_001>` | `<summary>` | `PASS` | `<reason>` |
| `<ex_002>` | `<summary>` | `FAIL` | `<reason>` |
| `<ex_003>` | `<summary>` | `FAIL` | `<edge-case reason>` |

## Disagreement Log

| Trace ID | Annotator A | Annotator B | Disagreement Type | Resolution | Rubric Change Needed |
|---|---|---|---|---|---|
| `<trace_001>` | `PASS` | `FAIL` | `<ambiguous term / missing rule / unclear example>` | `<decision>` | `<yes/no + change summary>` |

## Alignment Session Notes

- Session date: `<YYYY-MM-DD>`
- Participants: `<names>`
- Key ambiguity found: `<notes>`
- Rule updates agreed:
  - `<update_1>`
  - `<update_2>`

## Revision Changelog

| Version | Date | Change | Reason |
|---|---|---|---|
| `<v1.0>` | `<date>` | `<initial>` | `<initial release>` |
| `<v1.1>` | `<date>` | `<clarified boundary>` | `<resolved disagreement cluster>` |
