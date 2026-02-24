# RAG Evaluation Guide

Use this guide for retrieval-augmented pipelines where failures can come from retrieval, generation, or both.

## Evaluate Components Separately

A single end-to-end score hides root cause.

Track at least:
- Retrieval quality
- Generation groundedness
- End-to-end task success

## 1. Build Retrieval Eval Data

### Gold mapping approach

For each query, identify the relevant chunk(s) that contain answer evidence.

### Synthetic bootstrap approach

When data is limited:
1. Chunk documents
2. Generate question/fact pairs from chunks
3. Add hard distractors by using similar non-answer chunks
4. Filter unrealistic queries before using them

Synthetic sets are for bootstrapping, not long-term replacement for real traffic samples.

## 2. Retrieval Metrics

Use metrics matched to pipeline goals:
- `Recall@k`: coverage of relevant chunks in top-k
- `Precision@k`: concentration of relevance in top-k
- `MRR`: rank quality for first correct hit
- `NDCG@k`: graded ranking quality

Practical guidance:
- Early-stage retrieval often prioritizes recall
- Re-ranking stage prioritizes ranking precision
- Interpret `NDCG@k` with care when user utility depends on specific evidence coverage patterns

## 3. Chunking Strategy Is a First-Class Variable

Tune chunking as a hyperparameter:
- Chunk size
- Overlap
- Boundary method (fixed, section-based, semantic)

Suggested process:
1. Define candidate chunking configs
2. Re-index corpus per config
3. Measure retrieval metrics on same eval set
4. Inspect failed queries to see if chunk boundaries caused misses

Use content-aware chunking when fixed windows split related evidence.

## 4. Generation Metrics

Evaluate against retrieved context and user question:
- Faithfulness: answer is supported by retrieved evidence
- Relevance: answer addresses the query directly

Common generation failure types:
- Hallucination beyond provided evidence
- Omission of key evidence
- Misinterpretation of retrieved content
- Correct-but-irrelevant answers

## 5. Long-Context and Document Tasks

Not all tasks are equal:
- Constant-output tasks: answer uses a small slice of long context
- Variable-output tasks: output scales with number of items in document

For variable-output workloads:
- Smaller chunks usually improve extraction completeness
- Evaluate chunk-level and aggregate consistency

## 6. RAG Pitfalls to Block

- Using only end-to-end correctness metrics
- Overfitting to synthetic QA style
- Treating chunking as fixed infrastructure
- Ignoring retrieval/generation metric mismatch
- Measuring fluent answers without grounding checks

## 7. Operational Loop

1. Run retrieval + generation evals on CI set
2. Monitor sampled production traces
3. Add new high-impact failures to CI set
4. Re-tune chunking/retrieval/prompting as failure patterns evolve
