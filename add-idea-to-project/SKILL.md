---
name: add-idea-to-project
description: Add a new idea to an existing project that already has a PRD and sliced issues. Interviews the user, updates the original PRD with new user stories (as a dated addendum comment), updates the plan file if one exists, and creates new vertical-slice GitHub issues. Use when a new idea or feature request comes in for a project already in progress, user wants to extend an existing PRD, or add stories to an ongoing project.
---

This skill is invoked when a new idea arrives for a project that already has a PRD and sliced issues. You may skip steps you don't consider necessary.

### 1. Locate the PRD

Ask the user for the existing PRD GitHub issue number or URL.

Fetch it with:

```
gh issue view <number> --comments
```

Read the full PRD carefully: understand the problem statement, existing user stories, implementation decisions, testing decisions, and out-of-scope items. Note the number of the last user story so you can continue the numbering in the addendum.

### 2. Capture the idea

Ask the user for a long, detailed description of the new idea and what problem it solves. Also ask for any potential solution ideas they have in mind.

### 3. Interview

Interview the user relentlessly about every aspect of the new idea until you reach a shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one. See [decision-tree-planning/REFERENCE.md](../decision-tree-planning/REFERENCE.md) for the full mental model.

Focus only on what is **new** — skip decisions already settled in the original PRD (existing schema, architecture patterns, auth approach, etc.). Reference those as "already decided in the PRD" and build on top of them.

Sketch out the major modules you will need to build or modify. Identify deep modules that can be tested in isolation. Check with the user that these modules match their expectations and which ones they want tests written for.

### 4. Explore the codebase

Explore the repo to understand what has already been built from the existing issues, verify your assertions from the interview, and identify exactly where the new idea fits into the current codebase.

### 5. Update the PRD

Add a comment to the original PRD GitHub issue with the new user stories and decisions. Use today's date in the header.

```
gh issue comment <number> --body "..."
```

Use the addendum template below:

<addendum-template>
## Addendum — YYYY-MM-DD

### New User Stories

A numbered list continuing from the last user story in the original PRD:

N. As a <actor>, I want <feature>, so that <benefit>

This list should be extensive and cover all aspects of the new idea.

### New Implementation Decisions

Additional decisions introduced by this idea:

- New modules to build or modify
- Interface changes
- Schema additions
- API contract changes
- Specific interactions

Do NOT include specific file paths or code snippets.

### New Testing Decisions

- What makes a good test for this addition
- Which new modules will be tested
- Prior art in the codebase for similar tests

</addendum-template>

### 6. Update the plan file

Check if a plan file exists in `./plans/` for this project.

- If a plan file **exists**: append new phases for this idea following the same tracer-bullet format. Each new phase is a thin vertical slice through ALL layers. See [vertical-slices/REFERENCE.md](../vertical-slices/REFERENCE.md).
- If no plan file exists: skip this step.

Use the phase template below for each new phase:

<phase-template>
---

## Phase N: <Title>

**User stories**: <list new story numbers from the addendum>

### What to build

A concise description of this vertical slice. Describe the end-to-end behavior, not layer-by-layer implementation.

### Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

</phase-template>

### 7. Slice into issues

Break the new idea into **tracer bullet** issues. Follow the rules in [vertical-slices/REFERENCE.md](../vertical-slices/REFERENCE.md) — each issue is a thin vertical slice through ALL layers, not a horizontal slice of one layer.

Present the proposed breakdown as a numbered list. For each slice, show:

- **Title**: short descriptive name
- **Type**: HITL / AFK
- **Blocked by**: which other slices or existing issues (if any) must complete first
- **User stories covered**: which new user story numbers this addresses

Ask the user:

- Does the granularity feel right? (too coarse / too fine)
- Are the dependency relationships correct?
- Should any slices be merged or split further?
- Are the correct slices marked as HITL and AFK?

Iterate until the user approves the breakdown.

Once approved, create GitHub issues in dependency order (blockers first) using `gh issue create`. Use the issue template below:

<issue-template>
## Parent PRD

#<prd-issue-number>

## What to build

A concise description of this vertical slice. Describe the end-to-end behavior, not layer-by-layer implementation. Reference the PRD addendum rather than duplicating content.

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Blocked by

- Blocked by #<issue-number> (if any)

Or "None - can start immediately" if no blockers.

## User stories addressed

Reference by number from the PRD addendum:

- User story N
- User story N+1

</issue-template>

Do NOT close or modify the parent PRD issue body — only add the comment in step 5.
