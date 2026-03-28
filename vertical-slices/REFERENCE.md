---
name: vertical-slices-reference
description: Shared reference for vertical slice (tracer bullet) rules used by prd-to-plan and prd-to-issues
type: reference
---

# Vertical Slices (Tracer Bullets)

A vertical slice cuts through ALL integration layers end-to-end. It is NOT a horizontal slice of one layer.

## Rules

- Each slice delivers a narrow but **COMPLETE** path through every layer (schema, API, UI, tests)
- A completed slice is **demoable or verifiable** on its own
- Prefer **many thin slices** over few thick ones
- Each slice should be independently implementable and mergeable
- Do NOT include specific file names, function names, or implementation details likely to change
- DO include durable decisions: route paths, schema shapes, data model names

## Slice Types

- **HITL** (Human In The Loop): Requires human judgment — architectural decisions, design reviews, UX sign-off
- **AFK** (Away From Keyboard): Can be implemented and merged without human interaction

Prefer AFK over HITL where possible. The more slices that can run in parallel without human bottlenecks, the faster the work completes.

## Anti-Patterns

- **Horizontal slicing**: "Phase 1: all database work, Phase 2: all API work, Phase 3: all UI work" — nothing is demoable until everything is done
- **Kitchen sink slices**: One slice that touches every feature — too big to review, test, or reason about
- **Dependency chains**: Every slice blocks the next — kills parallelism. Minimize blocking relationships; maximize "None — can start immediately"

## Ordering

Create slices in dependency order (blockers first) so you can reference real issue/phase numbers in "Blocked by" fields. Maximize parallelism — the goal is that multiple people or agents can grab different slices simultaneously.

## Granularity Check

When presenting slices to the user, ask:
- Does the granularity feel right? (too coarse / too fine)
- Are the dependency relationships correct?
- Should any slices be merged or split further?

Iterate until the user approves.
