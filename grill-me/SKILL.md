---
name: grill-me
description: Stress-test an existing plan, design, or PRD by poking holes and surfacing risks. Use when user has a plan they want challenged, mentions "grill me", or wants to find blind spots before implementation.
---

# Grill Me

Stress-test an existing plan or design. Your job is to find what's wrong, missing, or underspecified — NOT to build the plan (that's what `write-a-prd` and `request-refactor-plan` are for).

## Input

The user should have an existing plan, PRD, design doc, or GitHub issue in context. If they don't, ask them to paste or point to one.

## Process

### 1. Read and understand the plan

Read the full plan. Explore the codebase if needed to verify claims the plan makes about the current state.

### 2. Identify attack surfaces

Look for:

- **Unstated assumptions** — what does the plan take for granted that might not hold?
- **Missing edge cases** — what happens when inputs are unexpected, empty, concurrent, or at scale?
- **Dependency risks** — does this rely on something that could change, break, or not exist yet?
- **Scope creep traps** — which parts sound simple but hide surprising complexity?
- **Integration gaps** — does the plan cover every layer end-to-end, or does it hand-wave at the seams?
- **Testing blind spots** — what's hard to test or not covered by the testing plan?
- **Ordering problems** — would a different sequencing of work reduce risk or reveal issues earlier?

### 3. Grill one branch at a time

Walk through each concern **one question at a time**. For each:

- State the specific risk or gap you see
- Explain why it matters (what breaks if it's wrong)
- Provide your recommended answer or mitigation
- Ask the user if they agree or want to go a different direction

Do NOT dump a list of 15 questions. One at a time, resolve each branch before moving to the next.

### 4. Explore instead of asking when possible

If a question can be answered by reading the codebase, reading a dependency's docs, or checking git history — do that instead of asking the user. Only ask the user about decisions that require human judgment.

### 5. Summarize findings

After all branches are resolved, provide a concise summary:

- **Confirmed decisions** — things that survived the grilling
- **Changes made** — things the user decided to change based on your questions
- **Open risks** — things the user acknowledged but chose to accept
- **Suggested plan updates** — specific edits to make to the plan/PRD if applicable
