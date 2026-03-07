---
name: behavior-safe-code-repair
description: Repair ESLint errors and TypeScript build failures using a behavior-safe, risk-aware workflow that preserves business logic and chooses appropriate test strategies based on impact.
---

# Skill: behavior-safe-code-repair

## Purpose

Repair ESLint errors and TypeScript build failures using a behavior-safe, risk-aware workflow.

This skill must not treat lint or build success as the final goal.  
Its real goal is to restore code quality and type correctness **without unintentionally changing business behavior**.

The skill must determine:

- what kind of issue is being fixed
- whether the fix is runtime-neutral or behavior-sensitive
- whether business logic may be affected
- whether existing tests already protect the behavior
- whether tests should be added before the fix, after the fix, or not added
- whether unit tests are appropriate or whether integration / contract / hurl tests are better

---

## Scope

This skill applies when the task involves:

- ESLint failures
- TypeScript compile / typecheck / build failures
- strictness upgrades that surface latent defects
- refactors required only to satisfy lint or types
- code repair flows where passing checks alone is not enough

This skill does **not** assume that every failure should be fixed the same way.

---

## Core Principles

### 1. External correctness matters more than internal neatness
A green ESLint result or successful TypeScript build does not prove the application still behaves correctly.

### 2. Risk classification comes before code edits
Before making any change, classify the repair by behavioral risk.

### 3. Prefer the smallest valid repair
Use the minimal change set that resolves the issue while preserving current contracts and expected behavior.

### 4. Test decisions depend on risk, not habit
Do not blindly add tests for every fix.  
Do not skip tests when business logic may be affected.  
Test strategy must be chosen based on impact.

### 5. Do not mix unrelated refactors into a repair task
If unrelated cleanup opportunities are found, record them separately unless they are required for the current fix.

### 6. Runtime truth is primary
If typings and runtime behavior disagree, first determine the real business behavior.  
Types must model reality, not hide it.

### 7. Never reduce quality just to pass checks
Do not use weak escape hatches as the default repair strategy.

Avoid using these unless explicitly justified:

- `any`
- `as any`
- blind non-null assertions (`!`)
- swallowing errors
- widening types to bypass a real mismatch
- weakening validation to satisfy compile checks

---

## Repair Lifecycle

## Phase 0 - Read and understand the failure

First identify:

- Is the problem from ESLint, TypeScript, or both?
- Is the issue local to one file or spread across multiple modules?
- Is it a purely mechanical failure or a symptom of a deeper mismatch?
- Does the error indicate:
  - formatting or style issues
  - unused imports or variables
  - invalid type assumptions
  - null / undefined handling gaps
  - broken async handling
  - API contract mismatch
  - schema mismatch
  - dead code
  - incorrect generic inference
  - invalid domain assumptions
  - actual business logic defects exposed by stricter checks

The skill must begin with a short classification summary before editing code.

---

## Phase 1 - Risk classification

Each issue must be classified into one of the following levels.

### Level A - Cosmetic / mechanical

Typical examples:

- formatting
- import order
- unused imports
- unused variables
- explicit return types with no runtime impact
- safe lint autofix changes
- rename-only local variable cleanup
- type-only imports

**Business risk:** none or extremely low  
**Runtime impact:** none  
**Default test policy:** no new tests required

---

### Level B - Structural but behavior-preserving

Typical examples:

- adding null guards
- narrowing optional values
- correcting inferred `any`
- tightening function signatures without changing semantics
- repairing generic constraints
- making schema typing consistent with existing runtime behavior
- extracting small helpers for clarity without changing outputs

**Business risk:** low to medium  
**Runtime impact:** possible but intended to preserve behavior  
**Default test policy:** run existing tests; add focused tests if the area is important and uncovered

---

### Level C - Behavior-sensitive / business-sensitive

Typical examples:

- changing branch conditions
- altering default values
- changing validation logic
- changing filtering logic
- changing mapping logic
- changing sorting / pagination behavior
- modifying error handling behavior
- changing async sequencing
- changing request / response shaping
- changing domain rules exposed by compile issues
- removing logic that may still be reachable
- correcting code where the current implementation likely violates business intent

**Business risk:** medium to high  
**Runtime impact:** likely  
**Default test policy:** add protection before or immediately after the fix, depending on confidence and feasibility

---

## Phase 2 - Decide the test strategy

The skill must explicitly choose one of these policies.

### Policy A - No new tests

Use this when:

- the issue is purely mechanical
- the change has no runtime effect
- existing tests already sufficiently protect the relevant behavior
- the file is config-only, type-only, generated, or low-risk plumbing

Typical examples:

- import cleanup
- formatting fixes
- removing unused locals
- replacing inline type expressions with aliases
- type-only explicit annotations

---

### Policy B - Fix first, then add tests

Use this when:

- the correct behavior is obvious
- the risk is low to moderate
- the fix is small but worth locking down
- the area lacks tests but is still important
- the behavior can be documented cheaply with one or two targeted tests

Typical examples:

- add a null guard to prevent an obvious crash
- fix optional chaining in a stable code path
- correct a helper's return typing to match existing behavior
- repair fallback logic that is clearly intended

---

### Policy C - Add tests before fix

Use this when:

- business logic may change
- branching, mapping, filtering, validation, or calculations are involved
- you need a safety net before editing
- current expected behavior can be described clearly
- the area is unit-testable at reasonable cost

Typical examples:

- uniqueness checks
- soft-delete filtering
- default resource selection
- upload target resolution
- status derivation
- sorting and pagination rules
- validation edge cases
- domain-specific transformation logic

---

### Policy D - Do not add unit tests; use higher-level tests

Use this when:

- correctness depends on route + middleware + schema + service interaction
- the bug is best observed at HTTP or integration level
- the system is already protected by contract tests or hurl tests
- a unit test would be artificial and low-value

Prefer:

- integration tests
- API contract tests
- hurl tests
- route-level tests

Typical examples:

- request validation mismatch
- response shape mismatch
- status code mapping
- middleware behavior
- multi-step lifecycle corrections

---

## Phase 3 - Apply the repair

Repairs should be applied in this order:

1. fix purely mechanical issues first
2. separate type-only repairs from logic-affecting repairs
3. preserve public contracts unless change is required
4. if a contract change is required, update tests and docs in the same task
5. avoid opportunistic rewrites
6. avoid broad refactors unless they are necessary to make the fix safe and understandable

---

## Phase 4 - Validate the result

After applying the repair, validate all relevant layers:

- ESLint passes for the affected scope
- TypeScript typecheck / build passes for the affected scope
- existing tests still pass
- newly added tests pass
- integration / hurl tests pass if the affected code touches API behavior
- public request / response contracts have not changed unintentionally
- domain rules were not weakened just to satisfy tooling

---

## Phase 5 - Post-fix hardening

If the fix touched business-sensitive code, the skill must:

- add regression tests if still missing
- document any changed assumptions
- explicitly state whether the repair was:
  - type-safety only
  - defensive safety only
  - behavior-preserving structural change
  - actual business behavior correction

---

## Decision Tree

### Step 1
Is the issue runtime-neutral?

- yes -> usually choose Policy A
- no -> continue

### Step 2
Does the repair touch business logic, validation, mapping, filtering, defaults, status transitions, lifecycle rules, async flow, or API contracts?

- yes -> continue
- no -> usually choose Policy B

### Step 3
Are there already meaningful tests protecting this behavior?

- yes -> fix and run them
- no -> continue

### Step 4
Can the behavior be isolated cheaply with unit tests?

- yes -> choose Policy C
- no -> choose Policy D

---

## Practical Heuristics

## When unit tests are usually unnecessary

Do not add unit tests for:

- formatting-only changes
- import cleanup
- unused variable cleanup
- explicit type annotations with no runtime impact
- renaming private local symbols
- converting value imports to type imports
- lint autofixes that do not alter behavior

---

## When unit tests should usually be added before the fix

Add tests before the fix when touching:

- branch conditions
- domain validation
- uniqueness logic
- soft-delete exclusion logic
- default record selection
- upload resolution logic
- status / state derivation
- sorting / pagination logic
- schema normalization / transformation rules
- permission checks
- data filtering rules

---

## When tests can be added after the fix

Add tests after the fix when:

- the intended behavior is obvious
- the repair is defensive, not semantic
- the bug is low-risk
- the change is small and easy to verify
- current behavior is broken in an obvious way and a regression test can be added immediately afterward

---

## When integration or hurl tests are better than unit tests

Prefer higher-level tests when correctness depends on:

- full HTTP lifecycle
- request validation + routing + service collaboration
- middleware execution
- response status code mapping
- serialized response shape
- multi-endpoint workflows
- resource lifecycle rules

---

## Business-Impact Signals

The skill must raise the risk level if the repair touches any of these areas:

- create / update / remove lifecycle
- invalid / soft-delete rules
- alias or unique-field checks
- default resource resolution
- upload target selection
- API request / response contracts
- schema validation and transformation
- filtering, sorting, pagination
- state transitions
- retry / fallback / async behavior
- error semantics visible to callers

---

## Unit Test Strategy Rules

### Add tests before the fix when:
- there is a realistic chance of changing intended business behavior
- current behavior is known and should be preserved
- the area is cheap to isolate

### Add tests after the fix when:
- the fix is obviously correct
- the change is primarily defensive
- the code path is moderately important but currently uncovered

### Do not add tests when:
- the change is purely mechanical
- runtime behavior cannot change
- cost is high and value is near zero

### Prefer integration / hurl when:
- the behavior is best verified through actual request / response flow
- route/middleware/schema interactions are central
- unit tests would duplicate implementation details instead of protecting behavior

---

## Exception and Recovery Rules

### If a "simple" fix reveals deeper business logic problems
Reclassify the task immediately.  
Do not continue treating it as a cosmetic or mechanical repair.

### If one change triggers many new type errors
Stop patching downstream blindly.  
Find the root contract or type mismatch and repair it at the source.

### If the compile-safe fix conflicts with runtime truth
Preserve runtime/business truth first, then make types match it.

### If the fastest repair weakens validation or hides errors
Reject it unless the task explicitly requires that behavior change.

### If tests are expensive and cannot be added immediately
At minimum:
- apply the smallest safe repair
- state residual risk
- recommend the exact missing tests that should be added next

---

## Output Contract

Whenever this skill runs, the result must be structured in the following format.

## 1. Issue Summary
- source of failure
- affected files
- root cause category

## 2. Risk Level
- Level A / B / C
- whether business behavior may be affected

## 3. Test Decision
Choose one:
- no new tests
- fix first, then add tests
- add tests before fix
- use integration / hurl tests instead

## 4. Fix Plan
- minimal intended change set
- files to touch
- what must not be changed

## 5. Validation Plan
- lint
- typecheck / build
- unit / integration / hurl tests as applicable
- contract verification if applicable

## 6. Follow-up
- whether regression tests should be added later
- whether docs / rules / contracts should be updated

---

## Behavior Constraints

The skill must **not**:

- blindly autofix everything
- suppress errors without analysis
- convert real mismatches into `any`
- add non-null assertions without proof
- rewrite modules just because they are inconvenient
- change public behavior without explicitly flagging it
- claim safety based only on lint/build success

The skill should:

- preserve business behavior by default
- prefer minimal diffs
- separate mechanical and logical edits
- state uncertainty clearly
- document residual risk when full certainty is not possible

---

## Recommended Default Style For This Repository Pattern

For repositories with service / schema / router / hurl style architecture:

- treat lint-only changes as low risk by default
- treat schema / service / route interaction changes as medium or high risk
- treat lifecycle rules, uniqueness rules, default-selection rules, and invalid/soft-delete rules as business-sensitive
- prefer hurl or integration tests for endpoint contract protection
- prefer unit tests for isolated domain logic, validation helpers, mapping helpers, and filtering logic

---

## Example Classification Guide

### Example 1
Issue: unused imports, formatting, explicit type-only imports  
Risk: Level A  
Test policy: Policy A

### Example 2
Issue: TypeScript error because optional field may be undefined; safe guard needed  
Risk: Level B  
Test policy: Policy B

### Example 3
Issue: build failure reveals broken alias uniqueness check in update flow  
Risk: Level C  
Test policy: Policy C or D depending on architecture

### Example 4
Issue: response type mismatch caused by route returning wrong error shape  
Risk: Level C  
Test policy: Policy D

---

## Short Execution Template

Use this template when performing a repair:

1. identify error source and affected files
2. classify risk level
3. determine whether business behavior may change
4. inspect existing tests
5. choose test policy
6. apply minimal repair
7. run lint / typecheck / build
8. run relevant tests
9. add regression protection if needed
10. summarize what changed and what did not

---

## Final Goal

The final goal is **not** merely:

- zero lint errors
- successful TypeScript build

The final goal is:

**behavior-safe repair with the smallest justified change and the right level of test protection**
