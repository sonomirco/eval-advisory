# RAG Evaluation Plan Template

Use this template to define retrieval, generation, and chunking evaluation in one plan.

## Scope

- Application: `<name>`
- Corpus type: `<docs/tables/web/hybrid>`
- Primary user tasks: `<task_list>`

## Dataset

| Field | Value |
|---|---|
| Total queries | `<n_queries>` |
| Real queries | `<n_real>` |
| Synthetic queries | `<n_synth>` |
| Gold chunk mapping complete | `<yes/no>` |
| Multi-hop subset | `<yes/no + size>` |

## Retrieval Evaluation

- Top-k values tested: `<k_values>`
- Metrics: `<Recall@k / Precision@k / MRR / NDCG@k>`
- Success thresholds:
  - `<metric_1>: <target>`
  - `<metric_2>: <target>`

## Chunking Experiment Grid

| Config ID | Chunk Size | Overlap | Boundary Method | Recall@k | NDCG@k | Notes |
|---|---:|---:|---|---:|---:|---|
| `<cfg_1>` | `<n>` | `<n>` | `<fixed/section/semantic>` | `<val>` | `<val>` | `<notes>` |
| `<cfg_2>` | `<n>` | `<n>` | `<method>` | `<val>` | `<val>` | `<notes>` |

## Generation Evaluation

- Faithfulness check method: `<code/judge/human>`
- Relevance check method: `<code/judge/human>`
- Failure tags tracked:
  - `<hallucination>`
  - `<omission>`
  - `<misinterpretation>`

## Attribution Summary

| Failure Class | Retrieval | Generation | Interaction | Priority |
|---|---:|---:|---:|---:|
| `<class_1>` | `<count>` | `<count>` | `<count>` | `<1..N>` |
| `<class_2>` | `<count>` | `<count>` | `<count>` | `<1..N>` |

## Next Actions

1. `<action_1>`
2. `<action_2>`
3. `<action_3>`
