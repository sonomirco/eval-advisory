# Failure Taxonomy Template

Use this template to organize failure modes discovered during error analysis. A well-structured taxonomy enables targeted evaluator development and clear prioritization.

## Template Structure

For each failure mode, document:
1. **Category name:** Clear, descriptive title
2. **Description:** What defines this failure
3. **Frequency:** How often it occurs (if known)
4. **Impact:** Severity and user effect
5. **Examples:** Representative traces
6. **Priority:** Suggested fix priority
7. **Fix approach:** Potential solutions

## Example Taxonomy

### 1. Context Retrieval Failure

| Field | Content |
|-------|----------|
| **Category** | Context Retrieval Failure |
| **Description** | System fails to retrieve relevant documents or information from knowledge base. May retrieve wrong document, miss relevant info, or not use retrieved context at all. |
| **Frequency** | 35% of failures |
| **Impact** | High - renders response incorrect or unhelpful |
| **Examples** | - User asks "What's your return policy?" and AI gives generic policy instead of specific product return window<br>- Question about pricing for Product X returns pricing for Product Y |
| **Priority** | 1 (Critical) |
| **Fix approach** | Improve retrieval relevance scoring, add query expansion, verify knowledge base coverage |

### 2. Hallucination

| Field | Content |
|-------|----------|
| **Category** | Hallucination |
| **Description** | AI generates factually incorrect information or details not present in source material. May include non-existent features, wrong pricing, or fabricated capabilities. |
| **Frequency** | 15% of failures |
| **Impact** | High - directly misleads users |
| **Examples** | - States product integrates with X when it doesn't<br>- Claims feature available next week when it's not<br>- Invents pricing that doesn't match actual rates |
| **Priority** | 1 (Critical) |
| **Fix approach** | Add RAG with verified sources, add verification for factual claims, constrain response to known information |

### 3. Tone Mismatch

| Field | Content |
|-------|----------|
| **Category** | Tone Mismatch |
| **Description** | Response style is inappropriate for user persona or context. May be too formal for casual users, too casual for professional contexts, or lacks required empathy. |
| **Frequency** | 12% of failures |
| **Impact** | Medium - user dissatisfaction, brand damage |
| **Examples** | - Uses slang when addressing executive<br>- Overly formal language for student queries<br>- robotic responses in emotional support situations |
| **Priority** | 2 (High) |
| **Fix approach** | Add user persona detection, tone examples to system prompt, allow tone configuration |

### 4. Incomplete Response

| Field | Content |
|-------|----------|
| **Category** | Incomplete Response |
| **Description** | Response doesn't fully address user's request. May miss key components, provide partial information, or omit required elements. |
| **Frequency** | 22% of failures |
| **Impact** | High - requires user follow-up |
| **Examples** | - Scheduling assistant confirms booking but doesn't provide details<br>- Code assistant gives function but missing error handling<br>- Summary misses key requested sections |
| **Priority** | 1 (Critical) |
| **Fix approach** | Add completeness checks, improve multi-step reasoning, verify all constraints are addressed |

### 5. Format/Structure Issues

| Field | Content |
|-------|----------|
| **Category** | Format/Structure Issues |
| **Description** | Output has structural problems that affect usability or parsing. May include broken code blocks, malformed JSON, missing formatting, or wrong structure. |
| **Frequency** | 8% of failures |
| **Impact** | Medium - unusable output, parsing errors |
| **Examples** | - JSON with unescaped quotes<br>- Code block with wrong language tag<br>- Missing required fields in structured output |
| **Priority** | 2 (High) |
| **Fix approach** | Add output validation, fix templates, add schema validation |

### 6. Tool Use Errors

| Field | Content |
|-------|----------|
| **Category** | Tool Use Errors |
| **Description** | AI incorrectly selects or uses tools. May call wrong function, pass malformed parameters, or fail to use required tool. |
| **Frequency** | 10% of failures |
| **Impact** | High - workflow breakage |
| **Examples** | - Calls search when user provides specific ID<br>- Generates SQL with wrong column names<br>- Attempts to modify non-existent record |
| **Priority** | 1 (Critical) |
| **Fix approach** | Improve tool selection logic, add parameter validation, better tool descriptions in prompt |

### 7. Misinterpreted Intent

| Field | Content |
|-------|----------|
| **Category** | Misinterpreted Intent |
| **Description** | AI misunderstands what user is asking for. May respond to different question than asked, make incorrect assumptions about context, or miss key constraints. |
| **Frequency** | 18% of failures |
| **Impact** | High - unhelpful responses, user frustration |
| **Examples** | - User asks "cancel X" and AI cancels Y instead<br>- User wants to compare items and AI lists them<br>- User specifies constraint that AI ignores |
| **Priority** | 1 (Critical) |
| **Fix approach** | Improve query clarification, add confirmation for ambiguous requests, better context understanding |

### 8. Generic/Unhelpful Response

| Field | Content |
|-------|----------|
| **Category** | Generic/Unhelpful Response |
| **Description** | Response is too vague, boilerplate, or doesn't actually help. May redirect to support unnecessarily, provide generic information, or lack specific actionable guidance. |
| **Frequency** | 14% of failures |
| **Impact** | Medium - poor user experience |
| **Examples** | - "Please contact support" for answerable question<br>- Generic "I can help with that" without specifics<br>- Repeating information from user's query back to them |
| **Priority** | 2 (High) |
| **Fix approach** | Add specificity checks, improve context use, detect when response could be more specific |

## Using This Template

### During Error Analysis

1. Add new categories as you discover them
2. Fill in frequency counts as you review more traces
3. Document representative examples for each pattern
4. Update impact assessments as you learn more

### For Evaluator Development

1. Build targeted binary evaluators for high-priority failures
2. Use taxonomy to ensure coverage of all important modes
3. Track frequency changes as you improve the system
4. Reference examples when debugging new issues

### Maintenance

Review and update your taxonomy quarterly or as your application evolves significantly. New features may introduce new failure modes, and fixes may eliminate old ones.
