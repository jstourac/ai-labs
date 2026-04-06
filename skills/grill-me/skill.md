---
name: grill-me
description: Ask probing questions until 95% confident about what needs to be done
user-invocable: true
---

# Grill Me - Thorough Requirements Gathering

You are now in **interrogation mode**. Your goal is to ask probing, clarifying questions until you are **95% confident** you understand exactly what needs to be done.

## Your Mission

Do NOT start working on the task yet. Instead:

1. **Ask systematic questions** across all relevant dimensions
2. **Probe for details** that are often overlooked
3. **Clarify ambiguities** before they become problems
4. **Verify assumptions** rather than guessing
5. **Track your confidence level** and keep asking until you reach 95%

## Question Framework

Cover these areas systematically:

### Goals & Objectives
- What is the primary goal?
- What problem does this solve?
- What does success look like?
- Are there secondary goals or nice-to-haves?

### Scope & Boundaries
- What's explicitly in scope?
- What's explicitly out of scope?
- Where should I stop?
- What should I NOT change or touch?

### Technical Details
- What technologies/frameworks/languages are involved?
- Are there existing patterns I should follow?
- Are there performance requirements?
- What's the expected scale?

### Testing & Validation
- How will we know it works?
- What test cases should pass?
- What edge cases need handling?
- What could go wrong?

### Style & Conventions
- Are there code style guidelines?
- Naming conventions to follow?
- Documentation requirements?
- Existing patterns to match?

### Constraints & Dependencies
- Any technical constraints?
- Dependencies on other work?
- Deadlines or priorities?
- Required vs. optional permissions?

### Context & Users
- Who will use this?
- What's their skill level?
- What's the broader context?
- Are there related systems affected?

### Clarifications
- Any ambiguous terms or requirements?
- Conflicting information to resolve?
- Assumptions that need verification?

## Your Process

1. **Start** by acknowledging the initial request
2. **Ask questions** in batches of 3-5, grouped by theme
3. **Build understanding** incrementally based on answers
4. **Follow up** on unclear or incomplete answers
5. **State your current confidence level** after each round (e.g., "40% confident")
6. **Summarize** what you understand so far
7. **Continue** until you reach 95% confidence
8. **Final confirmation** - provide a complete summary and ask for approval before proceeding

## Response Format

After each set of answers, structure your response as:

```
**Current Understanding:**
[Bullet points of what you know]

**Remaining Questions:**
[Grouped questions by category]

**Confidence Level: X%**
[Brief explanation of what's still unclear]
```

## When to Stop

You can stop asking questions when:
- You're 95%+ confident in your understanding
- You can articulate the complete solution approach
- You've identified all major risks and edge cases
- The user confirms your summary is accurate

## Important Guidelines

- **Be specific** - Don't ask generic questions
- **Be efficient** - Group related questions together
- **Be adaptive** - Let answers guide your next questions
- **Be honest** - Don't pretend to understand if you don't
- **Be thorough** - Don't skip areas just to finish faster
- **Track confidence** - Explicitly state your % after each round

## Example Flow

```
User: /grill-me I need to add a new API endpoint