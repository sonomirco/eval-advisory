# Improvement Playbook

Use this guide after evaluation reveals persistent failures or cost pressure.

## Accuracy Improvement Ladder

### Quick wins

- Clarify ambiguous prompt instructions
- Add small, targeted few-shot examples
- Use role framing when tone/behavior requires consistency
- Request explicit reasoning structure for multi-step tasks

### Structural changes

- Decompose large tasks into smaller pipeline steps
- Tighten retrieval query/chunking/reranking in RAG flows
- Improve tool descriptions and argument constraints
- Add lightweight post-checks for common output failures

### Heavier interventions

- Fine-tune for stable, high-value recurring failure classes
- Add recurring human-review loops for hard subjective failures
- Use controlled prompt optimization only after criteria stabilize

## Cost Reduction Patterns

### Tiered model selection

Match model capability to sub-task complexity.

### Decomposition for cost

Use cheaper models for intermediate filtering/reranking; reserve expensive models for final synthesis/high-risk decisions.

### Token discipline

- Keep instructions concise
- Control retrieval payload size
- Use compact output formats when acceptable

## Provider Caching

Place stable instructions before variable user content to maximize prefix cache reuse.

Cache-friendly prompts reduce latency and cost for repeated templates.

## Model Cascades

Goal: approximate expensive-model quality at lower cost.

Basic cascade flow:
1. Proxy model responds
2. Compute confidence
3. If confidence >= threshold, return proxy output
4. Otherwise escalate to stronger model

Threshold tuning should optimize cost while meeting target match-rate or quality constraints.

## Experiment Discipline

For every improvement attempt:
- Define hypothesis and target metric
- Keep test set isolated
- Record latency/cost side effects
- Require regression checks on known failures
- Keep only changes that improve target metrics without violating guardrails
