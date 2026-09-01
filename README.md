# AI Coding Playbook

Personal canonical development standard for AI-assisted software development and vibe coding.

The goal is not to remove engineering discipline. The goal is to make sound product, implementation, verification, and deployment discipline affordable for an individual using AI agents.

## Canonical standard

Read:

- [`AI_CODING_OPERATING_PRINCIPLES.md`](./AI_CODING_OPERATING_PRINCIPLES.md) — the canonical cross-project standard
- [`AGENTS_TEMPLATE.md`](./AGENTS_TEMPLATE.md) — a short template for repository-specific agent instructions

## Recommended hierarchy

```text
ChatGPT Global Custom Instructions
        ↓
AI_CODING_OPERATING_PRINCIPLES.md  ← canonical personal standard
        ↓
Project AGENTS.md                   ← repository-specific execution rules
        ↓
Project Architecture / Current State
        ↓
Actual implementation
```

The global ChatGPT instructions should remain concise. This repository holds the detailed standard so it can evolve without bloating every conversation or project instruction.

## Core workflow

For meaningful features, default to:

```text
Request
→ Product meaning / scope
→ Existing contract & parity
→ Acceptance criteria
→ Minimal vertical slice
→ Implementation
→ Focused tests
→ Real E2E / live acceptance where appropriate
→ Review
→ Freeze
→ Merge
→ Cutover
→ Predeploy smoke
→ Explicit deploy approval
→ Deploy
→ Production smoke
```

Tiny fixes should use proportionate validation rather than the full workflow.

## Using this in a new project

1. Keep this repository as the canonical cross-project standard.
2. Add a short `AGENTS.md` to the project repository using [`AGENTS_TEMPLATE.md`](./AGENTS_TEMPLATE.md).
3. In that file, identify the project's canonical repo/branch, architecture, state document, deployment boundary, and project-specific exceptions.
4. Do not duplicate this full playbook into every repository unless the execution environment cannot access the canonical standard.
5. Project-specific architecture and explicit project rules override this generic playbook when they conflict.

## Change policy

This playbook should improve from real failures, not from theoretical process expansion.

When the same failure pattern appears repeatedly, promote the lesson into one of:

- a principle
- an architecture invariant
- a test
- a reusable checklist
- an agent instruction

Keep the standard rigorous but proportionate. Process is a risk-control tool, not the product.

## Version

Current standard: **v1.0**
