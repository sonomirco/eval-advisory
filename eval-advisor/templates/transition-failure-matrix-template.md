# Transition Failure Matrix Template

Use this template to localize first-failure hotspots in multi-step pipelines.

## State Definitions

List each pipeline state clearly.

| State | Description | Failure Criterion |
|---|---|---|
| `<state_1>` | `<description>` | `<what counts as failure here>` |
| `<state_2>` | `<description>` | `<criterion>` |
| `<state_3>` | `<description>` | `<criterion>` |

## Matrix

Rows = last successful state (`from_state`)
Columns = state where first failure occurred (`in_state`)

| From \ In | `<state_1>` | `<state_2>` | `<state_3>` | `<state_4>` |
|---|---:|---:|---:|---:|
| `<state_1>` | `<n>` | `<n>` | `<n>` | `<n>` |
| `<state_2>` | `<n>` | `<n>` | `<n>` | `<n>` |
| `<state_3>` | `<n>` | `<n>` | `<n>` | `<n>` |
| `<state_4>` | `<n>` | `<n>` | `<n>` | `<n>` |

## Hotspot Ranking

| Rank | Transition (`from -> in`) | Count | Impact | Suspected Cause |
|---:|---|---:|---|---|
| 1 | `<state_a -> state_b>` | `<n>` | `<H/M/L>` | `<hypothesis>` |
| 2 | `<state_c -> state_d>` | `<n>` | `<H/M/L>` | `<hypothesis>` |

## First Actions

1. `<inspect top transition traces>`
2. `<validate upstream/downstream assumptions>`
3. `<add regression case for representative failures>`
