# Review Queue Spec Template

Use this template to define daily/weekly human-review queues.

## Queue Metadata

- Queue name: `<name>`
- Owner: `<owner>`
- Cadence: `<daily/weekly>`
- Batch size: `<n>`

## Sampling Mix

| Stream | Percent | Selection Rule |
|---|---:|---|
| Random | `<%>` | `<uniform or stratified rule>` |
| Uncertainty | `<%>` | `<judge disagreement / unstable verdicts / conflicting signals>` |
| Failure-driven | `<%>` | `<guardrail hits / runtime errors / user complaints>` |

## Eligibility Filters

- Time window: `<last_24h / last_7d>`
- Product segment filter: `<segment_rule>`
- Version filter: `<pipeline_version_rule>`

## Review Fields

- Required labels: `<pass_fail + failure_tags>`
- Optional labels: `<severity / confidence>`
- Open-note required on fail: `<yes/no>`
- Defer option enabled: `<yes/no>`

## Routing and Actions

- Route severe failures to: `<channel/oncall>`
- One-click add-to-golden enabled: `<yes/no>`
- One-click bug creation enabled: `<yes/no>`

## Success Metrics

- Avg review time per trace: `<target>`
- Deferred label rate: `<target>`
- Reviewer-vs-judge disagreement rate tracked: `<yes/no>`
