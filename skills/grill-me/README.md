# Grill Me - Requirements Interrogation Skill

A Claude skill that ensures thorough understanding before starting work by systematically asking clarifying questions until 95% confidence is reached.

## Why Use This?

Stop jumping into solutions with incomplete information. This skill helps you:
- Avoid costly rework from misunderstood requirements
- Surface edge cases and constraints early
- Build shared understanding with stakeholders
- Document assumptions and decisions upfront
- Reduce back-and-forth during implementation

## Installation

```bash
# From this repository
vercel skill add github:odh-ide/ai-labs/skills/grill-me

# Or copy manually
cp -r skills/grill-me ~/.claude/skills/
```

## Usage

Invoke with `/grill-me` followed by your task or requirement:

```
/grill-me I need to add authentication to the API
/grill-me Build a dashboard for monitoring model performance
/grill-me Refactor the data pipeline to handle larger volumes
```

## How It Works

1. **You state your requirement** - Give a high-level task or goal
2. **Claude asks targeted questions** - Systematic questions across 8 key areas:
   - Goals & Objectives
   - Scope & Boundaries
   - Technical Details
   - Testing & Validation
   - Style & Conventions
   - Constraints & Dependencies
   - Context & Users
   - Clarifications

3. **You answer in batches** - Answer 3-5 questions at a time
4. **Claude tracks confidence** - Shows understanding and remaining gaps
5. **Repeat until 95%** - Continues until highly confident
6. **Final summary** - Complete understanding before work begins

## Example Session

```
User: /grill-me I need to add a new API endpoint
Claude: I'll help you build that endpoint! Let me ask some questions to ensure I understand the requirements completely.

**Goals & Objectives**
1. What data will this endpoint expose or accept?
2. What is the primary use case - who/what will call this endpoint?
3. Is this for internal services, external partners, or public API consumers?

**Scope & Boundaries**
4. Should this be a GET, POST, PUT, PATCH, or DELETE endpoint?
5. Are there existing endpoints I should model this after?

**Confidence Level: 20%**
(Need to understand data schema, auth requirements, and technical stack)

---

User: It's a POST endpoint to submit model inference requests. It'll be called by our web dashboard (internal use). We use FastAPI and already have a /models endpoint that lists available models.

Claude: Great! That helps a lot.

**Current Understanding:**
- POST endpoint for model inference requests
- Internal use (web dashboard)
- FastAPI framework
- Existing pattern: /models endpoint to reference

**Technical Details**
1. What should the request payload look like? (model_id, input_data, etc.?)
2. Should this be synchronous (return results immediately) or asynchronous (return job ID)?
3. Where are the models hosted? (local, remote service, KServe, etc.?)
4. What's the expected response format?

**Testing & Validation**
5. How should we handle invalid model IDs or malformed inputs?
6. Are there any rate limits or quotas to enforce?

**Confidence Level: 50%**
(Better, but need to know async vs sync, payload structure, and error handling)

---

[Session continues until 95% confidence is reached]

Claude: **Final Summary - Ready to Proceed**

**Current Understanding:**
- POST /infer endpoint accepting {model_id, input_data, parameters}
- Synchronous with 30s timeout, async fallback for longer jobs
- Uses existing ModelService class pattern
- Validates model_id against /models list
- Returns {prediction, confidence, model_version, latency_ms}
- Error handling: 404 for unknown models, 400 for bad input, 503 for timeout
- Unit tests + integration test with test-model-v1
- Follow existing FastAPI patterns in endpoints/models.py
- Add to API docs with example curl command
- No auth changes needed (uses existing dashboard session)

**Confidence Level: 95%**

Does this match your expectations? Should I proceed with implementation?
```

## When to Use

- **Starting new features** - Ensure you understand requirements fully
- **Refactoring existing code** - Clarify scope and constraints
- **Debugging complex issues** - Understand the problem deeply
- **Working with unfamiliar codebases** - Learn patterns and conventions
- **Stakeholder requests** - Translate business needs to technical specs

## When NOT to Use

- Simple, well-defined tasks ("fix this typo", "update version number")
- Exploratory work where the goal is to investigate
- When you've already discussed requirements in detail
- Time-critical hotfixes (though quick questions still help!)

## Benefits

### For You
- Fewer surprises during implementation
- Less rework from misunderstandings
- Better estimates (you know the full scope)
- More thoughtful architecture decisions

### For Your Team
- Documented requirements and assumptions
- Shared understanding before code review
- Fewer "why did you do it this way?" questions
- Better handoff documentation

## Tips for Best Results

1. **Be patient** - The questions save time overall
2. **Answer thoroughly** - Detailed answers = fewer follow-ups
3. **Say "I don't know"** - Better to clarify than guess
4. **Provide examples** - Show what success looks like
5. **Share context** - Mention related work or constraints

## Configuration

No configuration needed! The skill works out of the box.

For teams, consider creating a `REQUIREMENTS_TEMPLATE.md` to capture common question categories for your domain.

## Contributing

Found a question category we're missing? Open a PR to add it!

Common additions:
- Security & compliance considerations
- Observability & monitoring requirements
- Deployment & rollout strategy
- Documentation needs
- Accessibility requirements

## Related Skills

- `/plan` - Create implementation plan after requirements are clear
- `/review-pr` - Review implementation against requirements
- `/simplify` - Refactor after understanding the full picture

## License

[Add your license here]
