# Prompt: lint-build-repair-with-tests

You are repairing a codebase that currently fails in one or more of these areas:

- ESLint
- TypeScript typecheck
- TypeScript build

This task explicitly includes **test strategy and test work** where appropriate.

Your goal is not just to make tooling pass.  
Your goal is to perform a **behavior-safe repair with the right amount of testing**.

That means:

- restore lint / type / build correctness
- preserve intended business behavior by default
- decide whether tests are needed based on risk
- decide whether tests should be added before the fix, after the fix, or not at all
- choose unit tests vs integration / contract / hurl tests appropriately
- avoid weak shortcuts that hide real defects

You must follow all repository rules, especially:

- `behavior-safe-code-repair`
- repository architecture / contract / lifecycle / testing rules
- relevant `.opencode` rules, prompts, and checklists for the affected code paths

---

## Primary Objective

Complete a safe lint/build repair and apply the correct test strategy for the affected areas.

Success means:

1. relevant lint / type / build checks pass
2. business-sensitive behavior is not changed unintentionally
3. tests are added or updated only where justified
4. the chosen test strategy matches the actual risk
5. validation is run and reported clearly

---

## Non-Goals

Do **not**:

- blindly fix lint/build without impact analysis
- add tests mechanically for every change
- skip tests when business-sensitive behavior may change
- perform unrelated refactors
- rewrite modules for style preference
- replace real type problems with `any`
- add blind non-null assertions
- weaken validation or contracts to satisfy tooling
- declare success based only on lint/build passing

---

## Required Workflow

## Step 1 - Inspect failures first

Identify:

- whether failures come from ESLint, TypeScript, or both
- exact files involved
- whether the root cause is:
  - mechanical
  - structural but behavior-preserving
  - behavior-sensitive / business-sensitive

Do not edit code before making this classification.

---

## Step 2 - Classify risk

Classify each affected area into one or more of:

### Level A - Cosmetic / mechanical
Examples:
- formatting
- import cleanup
- unused variables
- type-only import conversion
- explicit annotations with no runtime effect

### Level B - Structural but behavior-preserving
Examples:
- null guards
- optional handling corrections
- generic fixes
- tighter function signatures with same semantics
- helper extraction that preserves outputs

### Level C - Behavior-sensitive / business-sensitive
Examples:
- validation logic
- filtering logic
- mapping logic
- default values
- uniqueness checks
- lifecycle rules
- request / response shaping
- status derivation
- soft-delete / invalid behavior
- async sequencing
- retry / fallback logic

If the work touches business rules, endpoint contracts, lifecycle behavior, or caller-visible error semantics, treat it as at least Level C unless clearly proven otherwise.

---

## Step 3 - Review existing protection

Before adding tests, inspect whether protection already exists:

- unit tests
- integration tests
- contract tests
- hurl tests
- service-level or route-level tests

Reuse meaningful existing coverage where possible.

Do not add redundant tests that only duplicate existing protection.

---

## Step 4 - Choose the test strategy

For each affected area, choose one primary policy:

### Policy A - No new tests
Use only when the change is runtime-neutral.

### Policy B - Fix first, then add tests
Use when:
- behavior is obvious
- risk is low to moderate
- the fix is small
- a targeted regression test would still add value

### Policy C - Add tests before fix
Use when:
- business logic may change
- branching / validation / filtering / lifecycle behavior is involved
- a safety net is needed before editing

### Policy D - Prefer integration / contract / hurl tests
Use when:
- correctness depends on multiple layers
- request/response behavior matters more than isolated implementation
- route + middleware + schema + service interactions are central

You must explain why the chosen policy fits each affected area.

---

## Step 5 - Implement tests at the right time

Follow these rules:

### When to add tests before fixing
Add protection first when touching:
- uniqueness logic
- validation rules
- filtering logic
- default-selection logic
- lifecycle rules
- status derivation
- business-critical branching

### When to fix first and add tests after
Fix first when:
- intended behavior is obvious
- the fix is defensive
- current behavior is clearly broken
- risk is moderate and the test is mainly regression documentation

### When not to add tests
Do not add tests for:
- formatting-only changes
- import cleanup
- unused symbol cleanup
- type-only annotations
- other runtime-neutral repairs

### When to prefer integration / hurl
Use higher-level tests for:
- route validation
- middleware behavior
- response shapes
- status code mapping
- endpoint lifecycle flows
- multi-layer request handling

---

## Step 6 - Apply minimal repair

When editing:

1. fix mechanical issues first
2. isolate type-only changes from logic-sensitive changes
3. preserve public contracts unless change is required
4. if a public contract changes, update relevant tests and docs in the same task
5. avoid broad refactors
6. keep the patch small, explicit, and explainable

---

## Step 7 - Validate the result

Run the relevant checks for the affected scope:

- ESLint
- TypeScript typecheck
- TypeScript build
- existing tests
- newly added tests
- integration / contract / hurl tests when relevant

Do not claim completion unless you actually ran the relevant validation, or explicitly state what could not be run.

---

## Step 8 - Final report format

Your final output must contain:

## 1. Issue Summary
- source of failure
- affected files
- root cause category

## 2. Risk Classification
- Level A / B / C
- whether business behavior may be affected

## 3. Test Strategy
For each affected area, state:
- chosen policy
- why that policy was chosen
- whether existing coverage was reused
- whether new tests were added
- whether tests were added before or after the fix

## 4. Changes Made
- files touched
- concise description of code changes
- concise description of test changes

## 5. Validation Results
- lint
- typecheck
- build
- tests run
- tests not run

## 6. Residual Risk
- remaining uncertainty
- follow-up recommendations
- any missing coverage that should be added later

---

## Repair Rules

### Rule 1 - Runtime truth comes first
If current runtime behavior and current typing disagree, determine intended business behavior first.  
Do not distort implementation behavior just to make TypeScript happy.

### Rule 2 - Do not hide real defects
Avoid:
- `any`
- `as any`
- broad unsafe casts
- blind `!`
- swallowed errors
- weakened validation
- fake defaults that change semantics

### Rule 3 - Match test depth to risk
The higher the behavioral risk, the stronger the required protection.

### Rule 4 - Do not over-test trivial repairs
Do not create tests whose only purpose is to validate formatting or import cleanup.

### Rule 5 - Protect business-sensitive changes
When touching:
- uniqueness
- filtering
- sorting
- pagination
- defaults
- validation
- lifecycle
- status derivation
- soft-delete / invalid logic
- request / response contracts

you must either:
- rely on meaningful existing coverage, or
- add targeted new tests, or
- justify why integration / hurl tests are the correct protection

### Rule 6 - Separate repair from refactor
Only refactor when needed to make the repair safe, understandable, or testable.

---

## File Selection Guidance

Inspect only the smallest necessary file set, such as:

- failing implementation files
- related types / schemas / contracts
- associated service / route / validation layers
- existing relevant tests
- related `.opencode` rules / checklists / prompts

Do not widen scope unless necessary.

---

## If a Deeper Problem Appears

If a supposedly simple lint/build issue reveals a real logic defect:

1. stop treating it as a mechanical repair
2. reclassify risk
3. adjust the test strategy
4. repair the root cause
5. report the reclassification clearly

---

## Completion Standard

Only consider the task complete when:

- the relevant lint / type / build failures are resolved
- the chosen test strategy matches the actual risk
- required tests are added or updated appropriately
- relevant validations were run
- business-sensitive behavior is protected or explicitly flagged

---

## Output Style

Be concise, technical, and explicit.

Always distinguish between:
- mechanical fixes
- structural fixes
- behavior-sensitive fixes
- test additions or updates

State uncertainty clearly when it remains.

---

## Final Instruction

Perform a **behavior-safe lint/build repair with appropriate tests**.

Start by:
1. identifying failing checks
2. classifying risk
3. reviewing existing coverage
4. choosing the test strategy
5. then making code and test changes
