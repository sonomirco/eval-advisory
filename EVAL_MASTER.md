# EVAL Master

This is the canonical routing and attachment index for the `eval-advisor` skill.
Load this file with `eval-advisor/SKILL.md` for every request.

## Routing Rules

1. Classify request into one primary workflow domain.
2. Load files in this order: `primary reference -> workflow checklist -> template (if needed)`.
3. Load secondary references only if listed for the selected workflow.
4. If domain is unclear, ask a routing question before loading additional files.

## 12-Workflow Routing Map

| Workflow Domain | Primary Reference | Workflow Checklist | Template (Only if Artifact Needed) | Secondary References | Execution Focus |
|---|---|---|---|---|---|
| 1. Error Analysis | `eval-advisor/references/error-analysis-guide.md` | `eval-advisor/workflows/error-analysis-checklist.md` | `eval-advisor/templates/failure-taxonomy-template.md` | `eval-advisor/references/trace-analysis-guide.md`, `eval-advisor/references/synthetic-data-patterns.md` | Trace-first discovery, open/axial coding, quantification, saturation |
| 2. Eval Design Decisions | `eval-advisor/references/evaluator-types-guide.md` | `eval-advisor/workflows/eval-design-decision-tree.md` | N/A | `eval-advisor/references/anti-patterns-extended.md` | Choose assertions vs judge vs guardrails after failures are known |
| 3. Collaborative Annotation | `eval-advisor/references/collaborative-evaluation-practices.md` | `eval-advisor/workflows/collaborative-annotation-checklist.md` | `eval-advisor/templates/rubric-alignment-template.md` | N/A | Independent labeling, alignment loop, rubric versioning |
| 4. Judge Build/Validation | `eval-advisor/references/judge-validation-reference.md` | `eval-advisor/workflows/judge-validation-checklist.md` | `eval-advisor/templates/judge-prompt-template.md` | N/A | Train/dev/test discipline, confusion matrix, TPR/TNR |
| 5. Judge Monitoring/Drift | `eval-advisor/references/judge-uncertainty-and-calibration.md` | `eval-advisor/workflows/judge-monitoring-and-revalidation-checklist.md` | `eval-advisor/templates/judge-monitoring-report-template.md` | N/A | Corrected production estimates, intervals, drift revalidation |
| 6. Trace Sampling & Multi-Turn Debugging | `eval-advisor/references/trace-analysis-guide.md` | N/A | N/A | N/A | Sampling mix, first-failure attribution, N-1 regression tests |
| 7. RAG Evaluation | `eval-advisor/references/rag-evaluation-guide.md` | `eval-advisor/workflows/rag-evaluation-checklist.md` | `eval-advisor/templates/rag-evaluation-plan-template.md` | N/A | Separate retrieval vs generation evaluation |
| 8. Architecture/Modality Evaluation | `eval-advisor/references/architecture-and-modality-evaluation-guide.md` | `eval-advisor/workflows/agentic-debugging-checklist.md` | `eval-advisor/templates/transition-failure-matrix-template.md` | N/A | Stage-level tool/agent diagnostics and transition failures |
| 9. Human Review Operations | `eval-advisor/references/review-interface-and-sampling-guide.md` | `eval-advisor/workflows/human-review-operations-checklist.md` | `eval-advisor/templates/review-queue-spec-template.md` | N/A | Queue design, sampling policy, feedback-to-engineering loop |
| 10. CI/CD Eval Operations | `eval-advisor/references/ci-cd-evaluation-guide.md` | `eval-advisor/workflows/ci-cd-evaluation-checklist.md` | `eval-advisor/templates/golden-dataset-entry-template.md` | N/A | Regression gates, monitoring linkage, lifecycle updates |
| 11. Improvement & Cost Optimization | `eval-advisor/references/improvement-playbook.md` | `eval-advisor/workflows/improvement-experiment-checklist.md` | `eval-advisor/templates/improvement-experiment-template.md` | N/A | Hypothesis-driven fixes and cost/quality tradeoffs |
| 12. Synthetic Data Generation | `eval-advisor/references/synthetic-data-patterns.md` | N/A | `eval-advisor/templates/synthetic-data-generation-prompt.md` | N/A | Structured tuple-first generation with realism filters |

## Workflow Use Playbooks

### 1) Error Analysis Guidance
When user needs error analysis:
1. Confirm data source (production traces vs synthetic bootstrap).
2. Guide staged workflow:
   - Start small, then expand to a diverse trace set until learning tapers
   - Open coding (first failure + raw notes)
   - Axial coding (cluster into binary failure modes)
   - Re-code traces into binary trace-by-failure-mode matrix for quantification
   - Iterate to saturation and taxonomy refinement
   - For high-priority modes, add targeted exhaustive re-labeling
3. If one failure dominates early review, prioritize quick fix before broad evaluator build.

### 2) Eval Design Decisions
When user asks what evaluator to build:
1. Block if error analysis is missing.
2. Check prompt/spec fixes before evaluator investment.
3. Choose among assertions, judges, and guardrails using cost/risk.

### 3) Collaborative Annotation and Rubric Alignment
When user has multiple labelers, subjective criteria, or disagreement issues:
1. Decide single-owner vs multi-annotator mode.
2. Run independent-labeling + alignment loop.
3. Compute agreement metrics as appropriate.
4. Version rubric and manage criteria drift.

### 4) LLM Judge Build and Validation
When user is creating or tuning a judge:
1. Enforce train/dev/test discipline.
2. Guide prompt iteration on dev set.
3. Report held-out TPR/TNR and confusion matrix.

### 5) Judge Calibration, Uncertainty, and Drift
When user monitors judge-based metrics in production:
1. Compute raw observed rate (`p_obs`).
2. Correct estimate with held-out `TPR/TNR`.
3. Add bootstrap confidence intervals for operational decisions.
4. Define alerting based on bounds/thresholds.
5. Set revalidation cadence and drift triggers.

### 6) Trace Sampling and Multi-Turn Debugging
When user needs to select traces and diagnose interactions:
1. Blend random, uncertainty, and failure-driven sampling.
2. Use first-failure attribution for initial labeling.
3. Use N-1 testing for multi-turn regressions.

### 7) RAG Evaluation
When user asks about retrieval/generation quality:
1. Evaluate retrieval and generation separately.
2. Choose retrieval metrics (`Recall@k`, `Precision@k`, `MRR`, `NDCG@k`) by use case.
3. Tune chunking as a measured variable (size/overlap/method).
4. Evaluate generation faithfulness and relevance to query.

### 8) Architecture and Modality-Specific Evaluation
When user has tool-calling, agentic, or complex modality pipelines:
1. Evaluate tool calling by stage: selection, arguments, execution, output handling.
2. Define intended autonomy policy for agentic behavior.
3. Build transition failure matrix from first-failure state attribution.
4. Separate extraction/input failures from reasoning failures for long docs/PDF/image workflows.

### 9) Human Review Operations and Interfaces
When user asks how to run ongoing human review:
1. Define review UI requirements and high-leverage workflow features.
2. Define queue sampling mix (random + uncertainty + failure-driven).
3. Connect review actions to engineering artifacts (golden set, bug filing, judge revalidation).

### 10) CI/CD and Production Evaluation Operations
When user asks about deployment-time eval operations:
1. Build and maintain curated golden dataset for CI regressions.
2. Run production monitoring on sampled traces with observability coverage.
3. Use guardrails for synchronous high-impact failures only.
4. Keep CI/CD flywheel connected to error analysis and evaluator updates.

### 11) Improvement and Cost Optimization
When user asks how to improve system performance after eval findings:
1. Run hypothesis-driven improvement experiments.
2. Apply accuracy ladder: quick wins -> structural changes -> heavier interventions.
3. Apply cost optimizations: tiered models, token discipline, caching, cascades.

### 12) Synthetic Data Generation
When user needs bootstrap test data:
1. Use structured dimensions and tuple-first generation.
2. Add adversarial/distractor cases and filter for realism.
3. Replace synthetic-heavy sets with real traces over time.

## Guardrails

- Keep conceptual guidance in `eval-advisor/references/`.
- Keep execution logic in `eval-advisor/workflows/`.
- Keep artifact formats in `eval-advisor/templates/`.
- Do not duplicate full theory across files.
