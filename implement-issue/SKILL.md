---
name: implement-issue
description: Implement a GitHub issue using TDD, exploring the codebase first and filing a PR when done. Use when user wants to implement an issue, pick up a ticket, or build a feature from a GitHub issue.
---

# Implement Issue

Take a GitHub issue and implement it end-to-end using TDD, then file a PR.

## Process

### 1. Load the issue

Ask the user for the issue number. Fetch it with `gh issue view <number>` (include comments for context).

If the issue references a parent PRD, fetch that too for broader context.

### 2. Explore the codebase

Use the Agent tool (subagent_type=Explore) to understand the areas you'll need to touch:

- How the relevant feature currently works
- Existing patterns, conventions, and test styles
- Integration boundaries (where layers connect)
- Prior art — similar features or tests already in the codebase

### 3. Confirm the plan with the user

Before writing code, present a brief implementation plan:

- Which behaviors you'll test (from the issue's acceptance criteria)
- Which modules/areas you'll modify
- Any interface changes you're considering
- The order of your tracer bullets (first slice first)

Ask: "Does this approach look right, or should I adjust?"

### 4. Implement via TDD

Follow the [tdd](../tdd/SKILL.md) workflow strictly:

```
For each behavior:
  RED:   Write one test → verify it fails
  GREEN: Write minimal code to pass → verify it passes
  Repeat for next behavior
```

Rules:
- One test at a time — vertical slices, not horizontal
- Tests verify behavior through public interfaces, not implementation details
- Only enough code to pass the current test
- Don't anticipate future tests

### 5. Refactor

After all acceptance criteria pass:

- Extract duplication
- Deepen modules where possible (small interface, deep implementation)
- Run full test suite after each refactor step
- Never refactor while RED

### 6. Self-review

Before filing the PR, review your own changes:

- Read the full diff — does every change serve the issue?
- Are there accidental additions (debug logs, unrelated formatting, commented code)?
- Do tests describe behavior, not implementation?
- Would the tests survive an internal refactor?

### 7. File the PR

Create a PR using `gh pr create` linking back to the issue:

```
## Summary

- <What this PR does, in 1-3 bullets>

## Issue

Closes #<issue-number>

## Test plan

- [ ] <How to verify each acceptance criterion>

🤖 Generated with [Claude Code](https://claude.com/claude-code)
```

Share the PR URL with the user.
