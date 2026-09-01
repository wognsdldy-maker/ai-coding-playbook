# AI Coding Operating Principles

**Version:** 1.0  
**Status:** Canonical Personal Development Standard  
**Purpose:** AI-assisted software development / vibe coding across projects

---

## 0. Core Principle

AI coding is not a way to eliminate software-development discipline.

It is a way to make good product-development discipline fast and affordable enough for an individual to use.

The goal is not to generate the most code as quickly as possible. The goal is to produce software that a user can actually trust and use, with the least unnecessary cost and complexity.

Core rules:

- Product completion over code generation.
- Verification over assumption.
- Real behaviour over visible UI.
- Explicit evidence over “it probably works”.
- Safe completion over premature speed.
- Minimal process for tiny fixes; rigorous process where failure is costly.

---

## 1. Classify the Task Before Coding

Do not automatically start implementation when a coding request arrives.

Classify the request first.

### A. Tiny Fix

Typical examples:

- typo or copy change
- button label
- obvious small CSS value
- simple display-order change
- clearly located one-file fix
- low-risk bug with an obvious cause

Characteristics:

- location is already known
- impact surface is narrow
- no data/API/storage/auth/automation/calculation semantics change
- no architecture decision is required

Action:

- implement directly
- run proportionate validation
- do not impose a heavyweight workflow

### B. Implementation

Use when:

- product direction is already clear
- acceptance can be stated precisely
- actual implementation work is non-trivial
- multiple files/components may be involved

Action:

- confirm scope
- define acceptance criteria
- check existing contracts and dependencies
- implement
- validate

### C. Product / Architecture Decision

Use when any of the following is true:

- requirement is ambiguous
- several UX/product directions are plausible
- data meaning must be inferred
- API/storage/schema/auth/automation semantics may change
- financial/calculation meaning may change
- impact surface is unknown
- current architecture and requested behaviour may conflict

Action:

- do not code yet
- resolve the product/architecture decision first
- then hand a bounded implementation specification to the implementation model/agent

When uncertain between B and C, prefer C.

The process is a risk-control mechanism, not bureaucracy. Do not use a large-feature workflow for trivial changes.

---

## 2. Establish One Source of Truth

Before implementation, identify the canonical source.

Confirm:

- canonical repository
- canonical branch
- current main/source head
- production version and whether it differs from source
- relevant architecture/product contract
- current work state
- whether local uncommitted work exists

Rules:

- never treat multiple local copies as equally canonical
- never silently overwrite/reset/discard unrelated dirty work
- if documentation conflicts with actual repository state, repository truth wins
- local folders are working copies unless explicitly designated otherwise
- production and development source must be treated as distinct states when they differ

---

## 3. Define Acceptance Before Large Implementation

For non-trivial work, define what “done” means before coding.

Acceptance should cover, where applicable:

- user-visible result
- exact data source and semantics
- input/output behaviour
- empty/loading/error/stale states
- fail-closed conditions
- permissions and approval boundaries
- existing-feature compatibility
- responsive behaviour
- write-safety expectations
- tests required
- E2E completion conditions
- rollback expectations

If acceptance is ambiguous, implementation completion is also ambiguous.

---

## 4. Vertical Slice First

For a large feature, do not begin by building the entire UI or all components.

First complete the smallest meaningful real user flow end to end.

Preferred sequence:

User action  
→ real UI  
→ real read/data source  
→ real API/backend  
→ real persistence where required  
→ exact read-back  
→ user-visible result

Once one vertical slice works in the real intended architecture, expand horizontally.

A mock, fixture, or isolated component success is not proof that the feature works end to end.

---

## 5. Build a Parity Matrix Before Replacing Existing Functionality

When replacing an existing screen, owner, service, workflow, or backend path, inspect what already exists before implementation.

Create a parity matrix covering:

| Existing capability | New owner/location | Data contract | Test evidence | Legacy handling | Rollback |
|---|---|---|---|---|---|

Check:

- visible features
- hidden/secondary flows
- deep links
- history/back-forward behaviour
- legacy records
- read-only archives
- error states
- approval/write paths
- accessibility/responsive behaviour
- rollback path

A visually improved replacement must not silently delete existing capability or historical data.

---

## 6. Fail Closed by Default for High-Cost Errors

For finance, data, approvals, storage, authentication, automation, and other high-consequence flows, default to fail-closed behaviour.

Examples:

- wrong ID → do not fall back to the first object
- failed read → do not reinterpret as “no data”
- stale data → do not present as current
- ambiguous relation → do not infer one
- unavailable policy/rulebook → do not invent a judgment
- incomplete approval → do not write
- missing exact snapshot → do not silently use a nearby one
- retry → must not create duplicate writes

When the system cannot safely know, “cannot confirm” is better than a plausible guess.

---

## 7. Treat Write Safety as a First-Class Contract

For every feature capable of changing business data, define:

- which interactions are read-only
- which actions create writes
- whether writes are automatic or explicit
- whether user approval is required
- idempotency key/fingerprint behaviour
- duplicate/retry semantics
- exact read-back requirement
- rollback or supersede behaviour

Default principle:

Navigation, page load, selection, filtering, tab changes, and disclosures should not create business writes unless explicitly designed to do so.

Never use production business data as disposable QA data when an isolated verification path is available.

---

## 8. Tests Must Prove Something

Do not evaluate quality by test count alone.

Separate evidence by level:

### Unit
A function/module behaves correctly in isolation.

### Contract
Interfaces, schemas, invariants, and fail-closed rules are preserved.

### Integration
Multiple components/services work together.

### Browser / Responsive
Actual rendered interaction and layout work at required viewports.

### E2E
A complete user workflow works from the UI/input boundary to final persistent result/read-back.

### Live / Real-Backend Acceptance
The actual intended backend/runtime/storage path was exercised, not only mocks.

### Production Smoke
After deployment, the deployed environment still works as intended.

Rules:

- focused tests passing does not mean the entire repository is green
- full CI failure does not automatically mean the current product change is broken
- distinguish current-change failures from stale/global/environment debt
- never report PASS for evidence that was not actually collected

---

## 9. E2E Is the Definition of Done for Critical Workflows

For important workflows, “UI renders” and “build succeeds” are insufficient.

Critical E2E should exercise:

request/action  
→ processing  
→ backend/API  
→ persistence  
→ exact read-back  
→ final UI/state

Examples include:

- authentication
- payment
- financial/portfolio decisions
- data writes
- approval flows
- automation triggers
- migrations
- notifications where correctness matters

Not every change needs E2E. Copy changes, obvious CSS adjustments, and similarly trivial fixes should use proportionate validation.

---

## 10. Freeze UX After Acceptance

Once UX direction is approved and acceptance passes, freeze it.

Reopen design only for:

- real usability defect
- accessibility defect
- responsive defect
- data-semantic defect
- newly approved functional requirement

Do not continue subjective polish indefinitely.

“Could look slightly better” is not sufficient reason to reopen a frozen design.

---

## 11. Separate Implementation, Merge, Cutover, and Deploy

Treat these as different events:

Implementation  
→ Validation  
→ PR  
→ Review  
→ Merge  
→ Owner/Cutover  
→ Predeploy Smoke  
→ Explicit Deploy Approval  
→ Deploy  
→ Production Smoke

Important distinctions:

- code merged ≠ production deployed
- source owner changed ≠ deployed owner changed
- deployable ≠ deployment authorized
- deployed ≠ production verified

When replacing an owner/component/service, preserve a rollback implementation/path when practical until the new production path is verified.

---

## 12. Separate Reasoning from Execution

Where possible, split responsibilities.

### Control / Reasoning Role

Responsible for:

- understanding the request
- product decisions
- UX direction
- architecture decisions
- data/financial semantics
- acceptance criteria
- reviewing implementation evidence
- merge/cutover/deploy judgment

### Implementation Role

Responsible for:

- coding
- mechanical/refactoring work
- CSS
- tests
- lint/build
- browser QA
- Git/branch/PR execution
- deployment execution after approval

The implementation model must not silently invent product meaning.

The reasoning model should not waste time repeatedly performing mechanical implementation work when a capable implementation agent can do it.

---

## 13. Distinguish Technical Problems from Product Decisions

The implementation agent should normally solve these without escalation:

- type errors
- lint errors
- build failures
- test failures caused by its implementation
- CSS breakage
- responsive layout issues
- file-location differences
- ordinary browser QA failures
- ordinary Git/branch/PR problems

Stop and return for product/control-plane decision when:

- requirements conflict
- several UX directions require product choice
- user-visible meaning must be newly decided
- financial/investment/calculation semantics could change
- API/schema/storage contract must change
- auth/automation policy must change
- canonical architecture conflicts with the request
- impact surface becomes unexpectedly broad
- implementation would require guessing data meaning

Escalation format:

```text
BLOCKED — CONTROL DECISION REQUIRED

ISSUE
What is blocked?

WHY IT NEEDS A DECISION
Why is this not merely an implementation problem?

OPTIONS
What are the bounded choices?

IMPACT
What changes for product/data/security/financial meaning?

CURRENT STATE
What has been safely completed?
```

---

## 14. Keep Documentation Minimal and Purpose-Specific

Avoid creating a new long Markdown file for every change.

Prefer a minimal durable set:

- current state / handoff index
- canonical architecture/product contract
- short agent execution instructions
- this global coding playbook where appropriate

Use Git history and PR descriptions for ordinary change history.

Current-state documents should describe the current state, not accumulate the entire project history.

---

## 15. Report Completion Precisely

Never blur these states:

- PLANNED
- SPECIFIED
- IMPLEMENTED
- VERIFIED
- E2E VERIFIED
- MERGED
- CUTOVER
- DEPLOYABLE
- DEPLOYMENT AUTHORIZED
- DEPLOYED
- PRODUCTION VERIFIED

Examples:

Bad:
> “완료됐습니다.”

Better:
> “구현 및 focused tests PASS. 아직 merge/deploy되지 않았습니다.”

Bad:
> “CI PASS.”

Better:
> “Focused suite PASS. Repository-wide CI still has unrelated failures.”

Evidence language must match what was actually observed.

---

## 16. Git and Branch Discipline

Unless a project explicitly defines a different policy:

- do not commit directly to main
- branch from latest canonical main
- preserve unrelated local work
- keep scope bounded
- validate before PR
- review diff before merge
- use exact expected head SHA for high-risk merges when practical
- keep deployment separate from merge

A branch is a working proposal, not a second source of truth.

---

## 17. Deployment and Rollback

Before deployment:

- verify exact source/main
- run proportionate predeploy tests/build
- run site-wide smoke for shared-shell/global changes
- verify write-safety boundaries
- confirm rollback target/path
- require explicit deployment authorization if the project policy requires it

After deployment:

- smoke the actual production environment
- verify key routes
- verify responsive behaviour where relevant
- verify console/fatal errors
- verify unintended writes = 0 for read-only smoke
- rollback promptly if a production-blocking regression is found

Do not use “deployment succeeded” as proof that the product is healthy.

---

## 18. Default Workflow by Complexity

### Tiny Fix

Request  
→ locate exact change  
→ implement  
→ focused validation  
→ PR/merge according to project policy

### Normal Feature

Request  
→ scope/meaning  
→ acceptance  
→ implementation  
→ focused tests  
→ browser/integration verification  
→ review  
→ merge  
→ deploy when separately approved

### Large / High-Risk Feature

Request  
→ product meaning  
→ existing contract + parity matrix  
→ acceptance  
→ minimal vertical slice  
→ implementation expansion  
→ contract/integration tests  
→ real E2E/live acceptance  
→ review  
→ freeze  
→ merge  
→ cutover  
→ site-wide predeploy smoke  
→ explicit deploy approval  
→ deploy  
→ production smoke

---

## 19. Continuous Improvement Rule

When the same failure pattern is encountered twice, do not keep it as an informal lesson.

Promote it into one of:

- a coding principle
- an architecture invariant
- a test
- a reusable checklist
- an agent instruction

The development system should become better after each project.

---

## 20. Recommended Task Packet

For implementation handoff:

```text
GOAL
What final outcome must be achieved?

SCOPE
What screens/functions/files are in scope?

DO NOT CHANGE
What product/data/API/storage/auth/calculation behaviour must remain untouched?

ACCEPTANCE
What observable conditions define completion?

VERIFY
What tests/build/browser/E2E evidence is required?

STOP
What discoveries require returning for product/control-plane judgment?
```

---

## 21. Recommended Final Verification Summary

```text
SOURCE / HEAD
Exact source used for verification

IMPLEMENTED
What changed

FOCUSED TESTS
PASS / FAIL

INTEGRATION
PASS / FAIL / NOT REQUIRED

E2E
PASS / FAIL / NOT REQUIRED

RESPONSIVE / BROWSER
Viewport and interaction evidence

WRITE SAFETY
Expected writes vs observed writes

BUILD / LINT / DIFF
PASS / FAIL

KNOWN UNRELATED DEBT
Failures not caused by this change

ROLLBACK
Available path

CURRENT STATUS
IMPLEMENTED / VERIFIED / MERGED / DEPLOYABLE / DEPLOYED / PRODUCTION VERIFIED
```

---

# Final Rule

The purpose of AI-assisted development is not:

> “Ask once and receive code.”

It is:

> “Use AI to apply sound product, engineering, verification, and deployment discipline at a scale an individual can realistically afford.”

**Code is an intermediate artifact. Trusted behaviour is the product.**
