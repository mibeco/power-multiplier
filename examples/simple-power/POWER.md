---
name: "code-review-tips"
displayName: "Code Review Tips"
description: "Provides best practices and guidelines for conducting effective code reviews"
keywords: ["code review", "review", "pr review", "pull request", "code quality"]
author: "Example Author"
---

# Code Review Tips

## Overview

This power provides guidance on conducting effective code reviews. It helps developers understand best practices for reviewing code, giving constructive feedback, and improving code quality through the review process.

## When to Use This Power

Activate this power when you need help with:
- Reviewing a pull request or code change
- Writing constructive feedback on code
- Understanding what to look for in a code review
- Improving your code review process

## Code Review Checklist

### Functionality
- Does the code do what it's supposed to do?
- Are edge cases handled appropriately?
- Is error handling implemented correctly?

### Readability
- Is the code easy to understand?
- Are variable and function names descriptive?
- Are comments helpful and not redundant?

### Maintainability
- Is the code DRY (Don't Repeat Yourself)?
- Are functions and classes appropriately sized?
- Is the code modular and loosely coupled?

### Performance
- Are there any obvious performance issues?
- Are database queries optimized?
- Is caching used appropriately?

### Security
- Is user input validated and sanitized?
- Are sensitive data handled securely?
- Are authentication and authorization correct?

## Giving Constructive Feedback

### Do
- Be specific about what needs to change
- Explain why a change is needed
- Suggest alternatives when pointing out issues
- Acknowledge good code and clever solutions
- Ask questions instead of making demands

### Don't
- Be condescending or dismissive
- Focus only on negatives
- Make it personal
- Nitpick on style when there's a linter
- Block PRs for minor issues

## Example Feedback Templates

### Suggesting an Improvement
```
Consider using `Array.map()` here instead of a for loop - it would make the 
intent clearer and reduce the chance of off-by-one errors.
```

### Asking for Clarification
```
I'm not sure I understand the purpose of this function. Could you add a 
comment explaining when it should be called?
```

### Pointing Out a Bug
```
This will throw a NullPointerException if `user` is null. We should add a 
null check here, or use Optional to handle this case.
```

## Best Practices

1. **Review in small batches** - Large PRs are harder to review thoroughly
2. **Take your time** - Rushed reviews miss bugs
3. **Use checklists** - Consistency improves quality
4. **Automate what you can** - Let linters handle style
5. **Follow up** - Ensure feedback is addressed
