# LLM Judge Prompt Template

This template provides a structure for building LLM-as-judge evaluators with few-shot examples and clear evaluation criteria.

## Template Structure

### System Prompt

Defines the judge's role, objective, and evaluation approach.

```
You are an expert evaluator for [APPLICATION TYPE]. Your task is to evaluate AI-generated responses based on specific criteria.

You will be given:
- A user query or input
- The AI system's response
- Relevant context (retrieved documents, conversation history, etc.)

Your role is to assess whether the response meets the specified quality criteria and provide a brief explanation for your judgment.
```

### Evaluation Criteria

Clear, binary criteria for pass/fail determination.

```
## Evaluation Criteria

The response must:

**[CRITERION 1]**: [Description]
- What constitutes pass vs fail
- Specific requirements

**[CRITERION 2]**: [Description]
- What constitutes pass vs fail
- Specific requirements

[Add more criteria as needed]
```

### Few-Shot Examples

Draw examples from your train set (20% of labeled data). Include both pass and fail examples.

```
## Few-Shot Examples

### Example 1: PASS
[Query]: [User input]
[Response]: [AI response]
[Judgment]: PASS
[Explanation]: [Brief rationale]

### Example 2: FAIL
[Query]: [User input]
[Response]: [AI response]
[Judgment]: FAIL
[Explanation]: [Brief rationale describing what failed and why]

[Add 4-8 more examples drawn from your train set]
```

### Output Format

```
## Output Format

Provide your judgment in the following format:

{
  "overall_judgment": "PASS" or "FAIL",
  "criterion_scores": {
    "[criterion_1]": "PASS" or "FAIL",
    "[criterion_2]": "PASS" or "FAIL"
  },
  "explanation": "[Brief explanation of your reasoning]",
  "suggestions": "[Optional: specific improvements if failed]"
}
```

### Important Notes

1. **Binary decisions:** Make a clear pass/fail call for each criterion
2. **Explain reasoning:** Brief rationale helps with validation and debugging
3. **Be specific:** Reference exact parts of the response in your explanation
4. **Stay consistent:** Apply criteria uniformly across all evaluations

## Example: Customer Support Response Judge

### System Prompt

```
You are an expert evaluator for customer support responses. Your task is to evaluate AI-generated customer support responses based on quality, accuracy, and helpfulness.

You will be given:
- The customer's question or issue
- The AI system's response
- Relevant conversation history

Your role is to assess whether the response adequately addresses the customer's need and provide a brief explanation for your judgment.
```

### Evaluation Criteria

The response must:

**Accurate**: Response contains factually correct information. No fabricated details, wrong pricing, or incorrect policies.

**Complete**: Response fully addresses the customer's question. No missing key information or partial answers.

**Helpful**: Response provides useful information or appropriate next steps. Not dismissive or boilerplate.

**Professional Tone**: Language is appropriate for customer support - polite, empathetic, not overly casual.

### Few-Shot Examples

### Example 1: PASS
**Query**: "What's your return policy for online orders?"

**Response**: "You can return online orders within 30 days of purchase. Items must be in original condition with tags attached. Return shipping is free for orders over $50."

**Judgment**: PASS
**Explanation**: Response is accurate, complete, and helpful. It addresses the specific question about return policy for online orders with all relevant details.

### Example 2: FAIL
**Query**: "I never received my order confirmation email"

**Response**: "I understand your concern. Order confirmations are typically sent within 24 hours. Please check your spam folder and if you don't see it, contact us at support@example.com with your order number."

**Judgment**: FAIL
**Explanation**: Response is dismissive and doesn't address the specific issue. It suggests checking spam folder which is not appropriate for a missing order confirmation. A better response would offer to look up the order status directly.

## Example: Code Quality Judge

### System Prompt

```
You are an expert evaluator for generated code. Your task is to evaluate code quality based on correctness, style, and best practices.

You will be given:
- The coding task or question
- The generated code
- Relevant requirements or constraints

Your role is to assess whether the code meets quality standards and provide a brief explanation for your judgment.
```

### Evaluation Criteria

The code must:

**Syntactically Correct**: Code has no syntax errors and can execute without errors.

**Follows Requirements**: Implements all specified functionality and constraints.

**Readable**: Clear variable names, appropriate comments, logical structure.

**Secure**: No obvious vulnerabilities (SQL injection, hardcoded secrets, etc.)

### Few-Shot Examples

[Include 3-5 examples of pass and fail cases from your train set]

## Tips for Effective Judge Prompts

1. **Use the most powerful model you can afford** - Judge quality correlates with reasoning ability
2. **Keep prompts focused** - Single purpose judges work better than multi-purpose ones
3. **Iterate on dev set** - Test different prompt variations and measure alignment
4. **Include diverse examples** - Cover edge cases and ambiguity in your few-shot
5. **Ask for reasoning format** - Chain-of-thought improves judgment quality
6. **Validate against held-out test set** - Final validation prevents overfitting

Remember: Always validate your judge's TPR and TNR against human labels before deployment. See references/judge-validation-reference.md for details.
