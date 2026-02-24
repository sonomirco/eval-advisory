# RAG Evaluation Checklist

Use this checklist to evaluate retrieval and generation without conflating failure sources.

## Data

- [ ] Query set includes realistic production-like questions
- [ ] Relevant chunk mapping exists for retrieval evaluation
- [ ] Synthetic queries (if used) were filtered for realism
- [ ] Real trace sampling added to avoid synthetic overfitting

## Retrieval

- [ ] Measured `Recall@k`
- [ ] Measured `Precision@k` (where useful)
- [ ] Measured ranking quality (`MRR`, `NDCG@k`) as needed
- [ ] Metric choice matches workload (single-hit vs multi-hop vs broad evidence)

## Chunking

- [ ] Tested chunk size variants
- [ ] Tested overlap variants
- [ ] Tested at least one content-aware chunking strategy when fixed windows fail
- [ ] Re-indexed per config before metric comparison

## Generation

- [ ] Measured answer faithfulness to retrieved context
- [ ] Measured answer relevance to user query
- [ ] Logged hallucination, omission, and misinterpretation examples

## Failure Analysis

- [ ] Attributed failures to retrieval vs generation vs interaction
- [ ] Added highest-impact new failures to eval set
- [ ] Updated prompts/retrieval strategy based on observed failure classes
