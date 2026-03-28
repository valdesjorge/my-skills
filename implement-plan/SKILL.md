---
name: implement-plan
description: Execute a multi-phase implementation plan from ./plans/ using TDD, working through phases sequentially. Use when user wants to implement a plan, execute phases, or build from a plan file.
---

# Implement Plan

Execute a multi-phase implementation plan, working through phases sequentially using TDD.

## Process

### 1. Load the plan

Ask the user which plan to implement. Look in `./plans/` for available plan files, or accept a path.

Read the full plan. Pay special attention to:
- **Architectural decisions** in the header — these are constraints for all phases
- **Phase ordering** — respect the sequence
- **Acceptance criteria** — these become your test targets

### 2. Explore the codebase

Use the Agent tool (subagent_type=Explore) to understand the current state. Check whether any phases have already been partially implemented (the plan may have been started in a prior session).

### 3. Work through phases sequentially

For each phase:

#### 3a. Confirm with the user

Before starting a phase, present:
- Which phase you're about to implement
- The acceptance criteria you'll target
- Any questions or ambiguities you see

Ask: "Ready to start Phase N, or do you want to adjust anything?"

#### 3b. Implement via TDD

Follow the [tdd](../tdd/SKILL.md) workflow:

```
For each acceptance criterion:
  RED:   Write one test → verify it fails
  GREEN: Write minimal code to pass → verify it passes
  Repeat for next criterion
```

Rules:
- One test at a time — vertical slices, not horizontal
- Tests verify behavior through public interfaces
- Only enough code to pass the current test
- Respect the architectural decisions from the plan header

#### 3c. Refactor

After all acceptance criteria for this phase pass:
- Extract duplication
- Deepen modules where possible
- Run full test suite after each refactor step

#### 3d. Checkpoint

After completing a phase:
- Run the full test suite (not just this phase's tests)
- Summarize what was built and any deviations from the plan
- Ask the user if they want to commit, adjust, or continue to the next phase

### 4. Update the plan

As you complete phases, check off the acceptance criteria in the plan file. If you discover that a future phase needs adjustment based on what you learned, note it but do NOT modify future phases without user approval.

### 5. Wrap up

After all phases are complete (or the user stops):
- Run the full test suite one final time
- Summarize what was implemented vs what remains
- Offer to file a PR if the work is ready
