# Eval Design Decision Tree

Use this decision tree to choose the right evaluation approach for your needs.

## Start Here: Have You Conducted Error Analysis?

### If NO → STOP

**Do not build evaluators yet.**

You cannot design effective evaluations without understanding what actually fails in your application. Generic metrics like "helpfulness" or "quality" will miss application-specific failures.

**Action:** Conduct error analysis first
- See references/error-analysis-guide.md for the 4-stage process
- Use workflows/error-analysis-checklist.md to verify completeness

### If YES → CONTINUE

You have identified real failure modes from your data. Now you can design targeted evaluators.

---

## Next: Can the Issue Be Fixed with Prompt Changes?

### If YES → START WITH CODE ASSERTIONS

Many apparent "failures" are actually just missing preferences in prompts.

**Examples:**
- Wanting short responses (never specified)
- Wanting specific formatting (never requested)
- Wanting step-by-step reasoning (never mentioned)

**Action:** Update your prompts first to specify these preferences. They're cheaper to fix than building an evaluator.

**Recommended: Code Assertions**
- Fast, deterministic, cheap
- For format checking, keyword presence, structural validation

### If NO → DETERMINE EVALUATOR TYPE

The failure persists even with good prompts. Now choose what type of evaluator to build.

---

## Decision: What Type of Failure?

### A. Objective, Rule-Based Failures → CODE ASSERTIONS

**Characteristics:**
- Can be checked with code
- Fast (milliseconds)
- Deterministic
- Cheap to build and run
- Easy to debug

**Use cases:**
- Valid JSON/XML/SQL syntax
- Required keywords present/absent
- Data types correct (number in range, boolean)
- Regex patterns (email, phone, URL)
- Tool execution errors
- Reference comparison (expected vs actual)

**Implementation:** Start with regex or simple validation. Add to unit tests.

**See:** references/evaluator-types-guide.md (Code Assertions section)

### B. Subjective or Nuanced Qualities → CONSIDER COST HIERARCHY

These failures require judgment about tone, helpfulness, appropriateness, or other nuanced criteria.

**Cost Hierarchy:**

1. **Try code assertions first** - Sometimes nuanced issues have objective components
2. **Simple LLM judge** - For occasional evaluation, lower cost model
3. **Complex LLM judge** - For critical, frequent evaluation
4. **Human review** - For small scale, just have humans review

**Use code assertions for:**
- Partial objectivity (e.g., "excessive length" can be measured)
- Clear structural requirements (e.g., "must include all 5 components")

**If LLM judge needed → CONTINUE TO LLM JUDGE SECTION**

---

## If LLM JUDGE → Can It Be Synchronous?

### If YES (Guardrail Use) → SPECIAL CONSTRAINTS APPLY

**Synchronous requirements:**
- Must run in milliseconds (1-10ms budget)
- Must have very low false positive rate
- Typically code-based or small classifier, not full LLM

**Use for:**
- PII filtering (SSNs, credit cards, emails)
- Profanity detection
- Prompt injection detection
- Malformed output blocking
- SQL injection prevention

**Warning:** LLM-based guardrails that are too slow or have high false positives will create production bugs. Validate carefully!

**See:** references/evaluator-types-guide.md (Guardrails section)

### If NO (Async Evaluation) → STANDARD LLM JUDGE

**Characteristics:**
- Runs after response generation (asynchronous)
- Can use larger, more capable models
- Higher latency acceptable (100-500ms)
- Requires validation against human labels

**See:** references/evaluator-types-guide.md (LLM-as-Judge section)

---

## For LLM Judges: Is This A One-Time Evaluation or Recurring?

### ONE-TIME → SINGLE JUDGE IS ACCEPTABLE

**When appropriate:**
- Small dataset (<1000 examples)
- Validation-only effort
- One-time assessment or question answering

**See:** references/judge-validation-reference.md for validation methodology

### RECURRING → JUSTIFY INVESTMENT

**Questions to ask:**
1. What's the expected frequency of evaluation?
2. How much does each failure cost if it occurs?
3. Will this evaluator run on every code change or on a schedule?

**Cost considerations for 10,000 evaluations:**
- GPT-4o-mini: ~$1.50 total
- GPT-4o: ~$6.00 total
- Claude Sonnet: ~$15.00 total

If cost doesn't justify expected value, consider:
- Cheaper model for judge
- Lower frequency evaluation
- Code assertions for some criteria

---

## Summary Decision Flow

```
START
  │
  ├─ Conducted error analysis?
  │   │
  │   ├─ NO → Do error analysis first (STOP)
  │   │
  │   └─ YES → Continue
  │       │
  │       ├─ Can fix with prompts?
  │       │   │
  │       │   ├─ YES → Update prompts, use code assertions
  │       │   │
  │       │   └─ NO → What failure type?
  │       │       │
  │       │       ├─ Objective → Code assertions
  │       │       │
  │       │       ├─ Subjective → Synchronous? (guardrails?) → Async LLM judge
  │       │       │                  │
  │       │       └─ One-time or recurring? → Validate investment
  │
  └─ Build targeted evaluators for observed failures
```

## Key Principles

1. **Error analysis first** - Always before building evaluators
2. **Cheapest effective option** - Code assertions before LLM judges
3. **Validate everything** - Especially LLM judges against human labels with TPR/TNR
4. **Consider total cost** - Including API costs at scale
5. **Start simple** - Add complexity only as needed

## See Also

- references/anti-patterns-extended.md - For avoiding common mistakes
- references/evaluator-types-guide.md - Detailed comparison of evaluator types
- workflows/error-analysis-checklist.md - For verifying error analysis completion
- workflows/judge-validation-checklist.md - For validating LLM judges
