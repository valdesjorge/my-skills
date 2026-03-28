---
name: review-pr
description: Review a pull request for behavior correctness, test quality, and architectural alignment. Use when user wants a PR reviewed, mentions "review", or shares a PR URL/number.
---

# Review PR

Review a pull request thoroughly, focusing on behavior correctness, test quality, and whether the changes make the codebase deeper or shallower.

## Process

### 1. Load the PR

Accept a PR number, URL, or branch name. Fetch with `gh pr view <number>` and `gh pr diff <number>`.

If the PR references an issue or PRD, fetch that too for context on intent.

### 2. Understand intent before reading code

Before looking at the diff, understand **what the PR is trying to do**:
- Read the PR description
- Read the linked issue/PRD if any
- Understand the acceptance criteria

This prevents you from reviewing against your own expectations instead of the author's intent.

### 3. Explore affected areas

Use the Agent tool (subagent_type=Explore) to understand the codebase context around the changed files. You need to know:
- How the changed code is used by callers
- What patterns exist in the surrounding codebase
- Whether there's prior art for what this PR introduces

### 4. Review the diff

Evaluate across these dimensions:

#### Correctness
- Does the code do what the PR description says?
- Are there edge cases that aren't handled?
- Are there race conditions, null pointer risks, or error paths that fail silently?

#### Test quality
- Do tests verify **behavior** through public interfaces, or are they coupled to implementation?
- Would the tests survive an internal refactor?
- Are the right things tested? (critical paths and complex logic, not trivial getters)
- Are there behaviors described in the PR/issue that aren't tested?

#### Module depth
- Does this change make modules **deeper** (more functionality behind a simple interface) or **shallower** (more surface area for the same capability)?
- Are new abstractions earning their keep, or are they premature?
- Is complexity hidden behind interfaces, or leaking to callers?

#### Consistency
- Does the code follow the patterns already in the codebase?
- Are naming conventions respected?
- If it introduces a new pattern, is there a good reason?

#### Scope
- Does every change in the diff serve the stated goal?
- Are there unrelated formatting changes, refactors, or feature additions mixed in?
- Should this be split into multiple PRs?

### 5. Present the review

Structure your review as:

**Summary**: One sentence — what this PR does and your overall assessment (approve, request changes, or comment).

**Blocking issues** (if any): Things that must change before merge. Be specific — cite the behavior that's wrong or missing, not the code structure you'd prefer.

**Suggestions** (if any): Non-blocking improvements. Things that would make the code better but aren't required for correctness.

**What's good**: Call out things done well — especially good test design, clean interfaces, or thoughtful edge case handling. This is not flattery; it signals which patterns to keep using.

For each item, reference the specific file and behavior (not line numbers, which shift). Explain **why** it matters, not just **what** to change.

### 6. Ask about posting

Ask the user: "Want me to post this as a PR review on GitHub, or just keep it here?"

If they want it posted, use `gh pr review <number>` with the appropriate flag (`--approve`, `--request-changes`, or `--comment`).
