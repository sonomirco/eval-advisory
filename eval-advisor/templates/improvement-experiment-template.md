# Improvement Experiment Template

Use this template for controlled quality or cost improvement experiments.

## Experiment Header

- Experiment ID: `<id>`
- Owner: `<owner>`
- Date: `<date>`
- Type: `<prompt/retrieval/tooling/model/cost>`

## Problem and Hypothesis

- Problem statement: `<specific failure or cost issue>`
- Hypothesis: `<change> will improve <metric> because <mechanism>`

## Variants

- Baseline: `<version/config>`
- Candidate: `<version/config>`

## Evaluation Setup

- Dataset/split used: `<dev/test>`
- Leakage check complete: `<yes/no>`
- Primary metric: `<metric + target>`
- Guardrail metrics:
  - `<metric_1 + threshold>`
  - `<metric_2 + threshold>`

## Results

| Metric | Baseline | Candidate | Delta |
|---|---:|---:|---:|
| `<primary>` | `<v>` | `<v>` | `<v>` |
| `<guardrail_1>` | `<v>` | `<v>` | `<v>` |
| `<guardrail_2>` | `<v>` | `<v>` | `<v>` |
| Latency | `<v>` | `<v>` | `<v>` |
| Cost / 1k req | `<v>` | `<v>` | `<v>` |

## Failure Analysis

- Regressed cases:
  - `<case_id + summary>`
- Improved cases:
  - `<case_id + summary>`

## Decision

- Outcome: `<ship / iterate / reject>`
- Reason: `<brief rationale>`
- Follow-up:
  1. `<next_step_1>`
  2. `<next_step_2>`
