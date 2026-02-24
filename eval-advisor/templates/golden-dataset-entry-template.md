# Golden Dataset Entry Template

Use this entry format for adding regression-critical cases to CI datasets.

## Entry Metadata

- Case ID: `<id>`
- Added date: `<YYYY-MM-DD>`
- Added by: `<name>`
- Failure mode: `<mode_name>`
- Source: `<production/human_review/manual>`

## Input

```text
<input payload or user request>
```

## Expected Behavior

- Must do:
  - `<requirement_1>`
  - `<requirement_2>`
- Must not do:
  - `<prohibited_1>`

## Evaluation Checks

- Code assertion(s):
  - `<assertion_1>`
- Judge check(s):
  - `<judge_criterion_1>`

## Edge-Case Notes

- Why this case matters:
  - `<reason>`
- Related historical incident:
  - `<optional_link_or_id>`

## Maintenance

- Retire condition: `<when this case can be retired>`
- Last reviewed: `<YYYY-MM-DD>`
