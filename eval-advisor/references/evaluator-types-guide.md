# Evaluator Types Guide

This guide compares the three main types of automated evaluators for AI systems: code-based assertions, LLM-as-judge, and guardrails. Understanding when to use each type is critical for building effective evaluation systems.

## Overview

| Type | Speed | Cost | Best For | Maintenance |
|-------|--------|-------|-----------|-------------|
| Code Assertions | Fast | Low | Objective failures | Minimal |
| LLM-as-Judge | Slow | High | Subjective qualities | High |
| Guardrails | Fast | Medium | High-impact failures | Medium |

## Type 1: Code-Based Assertions

### What They Are

Deterministic checks that validate objective, rule-based failures.

### Examples

- Valid JSON syntax
- SQL syntax validation
- Presence of required keywords
- Regex patterns (phone numbers, email formats)
- Tool execution error detection
- Data type validation (number in range, boolean flag)
- Reference comparison (expected vs actual output)

### Advantages

**Fast:** Run in milliseconds, no LLM inference latency

**Cheap:** No API costs, minimal engineering time

**Deterministic:** Same input always produces same output - reliable for CI/CD

**Interpretable:** Clear why something failed - debugging is straightforward

### When to Use

Use code assertions whenever possible - for:
- Structural validation (JSON, XML, SQL)
- Format checking (dates, phone numbers, URLs)
- Keyword presence or absence
- Simple business rules (price ranges, valid states)

### Limitations

Cannot capture:
- Subjective qualities (tone, helpfulness)
- Semantic correctness beyond simple patterns
- Contextual appropriateness
- Complex reasoning quality

### Implementation Example

```python
def assert_valid_json(response: str) -> bool:
    try:
        json.loads(response)
        return True
    except ValueError:
        return False

def assert_required_keywords_present(response: str, keywords: list) -> bool:
        return all(kw in response.lower() for kw in keywords)

def assert_price_in_range(price: float, min_price: float, max_price: float) -> bool:
        return min_price <= price <= max_price
```

---

## Type 2: LLM-as-Judge

### What It Is

Using an LLM to assess qualities that code cannot capture, typically subjective or nuanced criteria.

### Examples

- **Tone assessment:** Professional, casual, empathetic, curt
- **Helpfulness:** Did the response address the user's question?
- **Completeness:** Were all aspects of the query addressed?
- **Safety check:** Contains harmful content, PII, or inappropriate language
- **Domain-specific correctness:** Legal accuracy, medical appropriateness, code quality
- **Instruction following:** Did the system follow specified constraints?

### Advantages

**Flexible:** Can evaluate complex, nuanced criteria

**Comprehensive:** Can capture qualities impossible to code

**Human-like judgment:** Can approximate human decision-making

### Disadvantages

**Slow:** LLM inference adds latency (100-500ms)

**Expensive:** API costs per evaluation

**Non-deterministic:** Same input may produce different outputs

**Requires validation:** Must align with human labels

### When to Use

Use LLM-as-judge for:
- Subjective qualities requiring nuanced judgment
- Criteria that would require complex rules to code
- Failures where the "why" matters as much as the "what"

### When NOT to Use

Don't use LLM-as-judge when:
- Simple structural checks (use code assertions)
- Fast, objective validation (use code assertions)
- Criteria with clear right/wrong answers (use code assertions or reference comparison)

### Critical Requirement: Validation

LLM judges MUST be validated against human labels:
1. Collect 100-300 human-labeled examples
2. Split: Train 20%, Dev 40%, Test 40%
3. Measure TPR and TNR (not accuracy)
4. Iterate until both metrics > 90%

See references/judge-validation-reference.md for detailed methodology.

### Cost Considerations

For a 10,000 evaluation run:
- **GPT-4o-mini:** ~$0.15 per eval = $1,500
- **GPT-4o:** ~$0.60 per eval = $6,000
- **Claude Sonnet:** ~$1.50 per eval = $15,000

Calculate costs before deploying at scale.

---

## Type 3: Guardrails

### What They Are

Evaluators that run **synchronously in the request/response path**, blocking or modifying outputs before they reach the user.

### Characteristics

**Fast:** Must execute in milliseconds (1-10ms budget)

**Deterministic:** Same input always produces same result

**Low false positive rate:** Incorrectly blocking valid responses is a production bug

**Targeted:** Clear-cut, high-impact failures only

### Examples

- **PII detection:** Block responses containing SSNs, credit cards
- **Profanity filter:** Redact inappropriate language
- **Prompt injection:** Detect and block malicious instructions
- **SQL injection:** Reject dangerous SQL patterns
- **Malformed output:** Block invalid JSON, broken code
- **Policy violations:** Disallowed content (hate speech, violence)

### When to Use

Use guardrails for:
- Safety-critical failures that must be blocked
- Compliance requirements (PII, content policies)
- Objective format violations (malformed JSON)

### When NOT to Use

Don't use guardrails for:
- Subjective quality assessment (too many false positives)
- Complex reasoning evaluation (too slow)
- Nuanced business logic (better handled by application)

### Synchronous vs Asynchronous Use

**Synchronous (guardrail):** Runs before user sees output, can block/regenerate
**Asynchronous (evaluator):** Runs after response, measures quality but doesn't block

A guardrail that blocks 5% of valid responses is likely causing more user harm than it prevents. Choose carefully.

### Implementation Considerations

```python
# Simple regex-based guardrail (fast, deterministic)
def contains_pii(text: str) -> bool:
    ssn_pattern = r'\d{3}-\d{2}-\d{4}'
    return bool(ssn_pattern.search(text))

# LLM-based guardrail (slower, for nuanced cases)
def guardrail_safety_check(response: str) -> bool:
    # Use small, fast model for synchronous check
    result = small_llm.check_for_safety_issues(response)
    return result.is_safe
```

---

## Decision Framework

### Starting Point: Code Assertions

Always start with code assertions for:
- Objective, checkable failures
- Fast, inexpensive validation
- CI/CD compatibility

### Then: LLM-as-Judge

Add LLM judges only for:
- Subjective qualities not capturable by code
- Validated against human labels
- Problems where "why" matters as much as "what"

### Finally: Guardrails (sparingly)

Use guardrails only for:
- Safety-critical, must-block failures
- Low latency requirements
- Cases where false positives are acceptable

### Hybrid Approaches

Many effective systems use combinations:
- **Assertion + Judge:** Assertions for format, judge for quality
- **Guardrail + Evaluator:** Fast guardrail filters, async judge for nuanced cases
- **Cascade:** Cheap assertions first, expensive judges only on failure

## Cost Comparison

Per 10,000 evaluations:

| Method | Cost/eval | Total Cost | Relative Cost |
|--------|-------------|------------|----------------|
| Regex assertion | $0.0001 | $1 | 1x |
| Reference check | $0.0005 | $5 | 5x |
| Small LLM judge | $0.15 | $1,500 | 150x |
| Large LLM judge | $0.60 | $6,000 | 600x |

Choose the cheapest effective method for each evaluation need.

## See Also

- references/judge-validation-reference.md - For validating LLM judges
- references/anti-patterns-extended.md - For common mistakes to avoid
- workflows/eval-design-decision-tree.md - For choosing evaluator types
