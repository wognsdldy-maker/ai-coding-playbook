# AGENTS.md Template

Use this as a starting point for project-specific agent instructions.

```md
# Agent Instructions

## Global development standard

This project follows the canonical personal AI coding standard:
https://github.com/wognsdldy-maker/ai-coding-playbook/blob/main/AI_CODING_OPERATING_PRINCIPLES.md

Read and apply that standard before non-trivial implementation work when the environment can access it.
If external access is unavailable, use a repository-local snapshot only if the project explicitly maintains one.

Project-specific rules below override the global playbook when they conflict.

## Source of truth

- Canonical repository: <owner/repo>
- Canonical branch: main
- Production: <describe production baseline and whether it may differ from main>
- Current state index: <PROJECT_STATE.md or N/A>
- Canonical architecture/product contract: <path or N/A>

Local folders and agent worktrees are working copies unless explicitly designated otherwise.
Preserve unrelated dirty work. Do not reset/delete/overwrite it.

## Work routing

Tiny Fix:
- exact location and narrow impact
- copy/typo/small CSS/simple value/obvious low-risk bug
- use proportionate validation

Implementation:
- requirements and product meaning are already clear
- define bounded acceptance before implementation

Product / Architecture Decision:
- requirement ambiguity
- competing UX/product choices
- data meaning must be inferred
- API/schema/storage/auth/automation/calculation semantics may change
- architecture conflict or unexpectedly broad impact

For Product / Architecture Decision work, stop implementation until the decision is resolved.

## Git workflow

- Start from latest canonical main.
- Do not commit directly to main.
- Use branch → validation → PR → review → merge.
- Keep merge and deployment separate.

## Validation

Use the minimum evidence appropriate to risk:
- focused tests
- contract/integration tests
- browser/responsive QA
- E2E/live-backend acceptance for critical workflows
- predeploy smoke for shared/global changes
- production smoke after deployment

Never report evidence that was not actually collected.

## Write safety

- Page load/navigation/filter/selection/tab/disclosure should not create business writes unless explicitly designed to do so.
- High-consequence data flows fail closed.
- Writes must preserve idempotency/retry safety and exact read-back where applicable.
- Do not use production business data as disposable QA data when isolation is possible.

## Deployment

- Merge does not equal deployment.
- Deployable does not equal deployment authorized.
- Do not deploy production without explicit approval when the project requires it.
- Preserve/confirm a rollback path for meaningful cutovers.

## Implementation handoff

Use:

GOAL
SCOPE
DO NOT CHANGE
ACCEPTANCE
VERIFY
STOP

If a product/control-plane decision is required, return:

BLOCKED — CONTROL DECISION REQUIRED

ISSUE
WHY IT NEEDS A DECISION
OPTIONS
IMPACT
CURRENT STATE
```

Keep each project's actual `AGENTS.md` short. Do not copy the full global playbook into every repository unless external access is impossible and a local snapshot is intentionally maintained.
