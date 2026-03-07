# Prompt: lint-build-repair

You are repairing a codebase that currently fails in one or more of these areas:

- ESLint
- TypeScript typecheck
- TypeScript build

Your job is **not** to blindly make the checks pass.

Your job is to perform a **behavior-safe repair**:
- restore lint / type correctness
- preserve existing business behavior by default
- avoid weakening validation or contracts just to satisfy tooling
- use a risk-aware workflow to decide whether tests are needed, and whether they should be added before the fix, after the fix, or not at all

You must follow the repository skill and rules, especially:

- `behavior-safe-code-repair`
- all repository architecture / testing / contract / lifecycle rules
- existing `.opencode` rules, checklists, and prompts relevant to the affected files

---

## Primary Objective

Fix the current lint / type / build failures with the **smallest justified change set** while preserving intended behavior.

Success means:

1. the relevant lint / type / build checks pass
2. business-sensitive behavior is not changed unintentionally
3. test strategy is explicitly chosen based on risk
4. any required tests are added or updated appropriately
5. no low-quality escape hatches are used without strong justification

---

## Non-Goals

Do **not**:

- perform unrelated refactors
- rewrite large modules for style preference
- change public contracts unless required
- weaken runtime validation just to satisfy TypeScript
- replace real type problems with `any`
- add blind non-null assertions
- suppress lint rules without necessity
- mark work complete based only on lint/build success

---

## Required Workflow

## Step 1 - Inspect the current failures

First identify:

- whether the failures are from ESLint, TypeScript, or both
- the exact files involved
- whether the root cause appears:
  - mechanical
  - structural but behavior-preserving
  - behavior-sensitive / business-sensitive

Produce a short classification before making edits.

---

## Step 2 - Classify risk

Classify the work into one or more of:

### Level A - Cosmetic / mechanical
Examples:
- formatting
- import cleanup
- unused symbols
- type-only import conversion
- explicit annotations with no runtime effect

### Level B - Structural but behavior-preserving
Examples:
- null guards
- optional handling
- generic correction
- signature tightening without semantic change
- helper extraction with same outputs

### Level C - Behavior-sensitive / business-sensitive
Examples:
- validation logic
- filtering logic
- mapping logic
- branch conditions
- default values
- uniqueness checks
- lifecycle rules
- request / response shapes
- async / retry / fallback behavior
- status derivation
- soft-delete / invalid rules

If any touched code affects business rules, lifecycle, API contracts, uniqueness, filtering, defaults, or invalid/soft-delete handling, treat it as at least Level C unless clearly proven otherwise.

---

## Step 3 - Decide the test policy

Choose exactly one primary test policy for each affected area:

### Policy A - No new tests
Use only when the change is runtime-neutral.

### Policy B - Fix first, then add tests
Use when behavior is obvious, risk is low to moderate, and a small regression test is still useful.

### Policy C - Add tests before fix
Use when business logic may change and a safety net is needed before editing.

### Policy D - Prefer integration / contract / hurl tests
Use when correctness is best verified through request/response or multi-layer behavior rather than isolated unit tests.

You must explain why the chosen policy fits.

---

## Step 4 - Review existing protection

Before adding new tests, check whether the repository already has protection in the relevant area:

- unit tests
- integration tests
- contract tests
- hurl tests
- route-level or service-level coverage

If suitable coverage already exists, reuse it instead of adding redundant tests.

If coverage is missing and the risk is meaningful, add the smallest useful protection.

---

## Step 5 - Apply minimal repair

When editing code:

1. fix purely mechanical issues first
2. isolate type-only fixes from logic-affecting fixes
3. preserve public contracts unless a change is required
4. if a public contract changes, update tests and related docs in the same task
5. avoid opportunistic rewrites
6. keep diffs small and explainable

---

## Step 6 - Validate thoroughly

After the repair, run the relevant checks for the affected scope:

- ESLint
- TypeScript typecheck
- TypeScript build
- unit tests if relevant
- integration / contract / hurl tests if relevant

Do not claim success unless the relevant validation actually passes, or unless you explicitly state what could not be run.

---

## Step 7 - Summarize the result

Your final output must include:

## 1. Issue Summary
- lint / type / build source
- affected files
- root cause category

## 2. Risk Level
- Level A / B / C
- whether business behavior may be affected

## 3. Test Decision
- no new tests
- fix first, then add tests
- add tests before fix
- use integration / hurl tests instead

## 4. Changes Made
- exact files touched
- minimal behavior-relevant description

## 5. Validation Results
- lint
- typecheck / build
- tests run
- any checks not run

## 6. Residual Risk
- what is still uncertain
- what follow-up tests or documentation are recommended

---

## Repair Rules

### Rule 1 - Runtime truth beats convenient typing
If runtime behavior and current types disagree, determine the real intended behavior first.  
Do not distort runtime behavior just to make TypeScript happy.

### Rule 2 - Do not hide defects behind weak typing
Avoid:
- `any`
- `as any`
- broad unsafe casts
- blind `!`
- artificial default values that change semantics
- swallowing errors

### Rule 3 - Preserve business behavior by default
Any behavior change must be explicit, justified, and protected by tests when appropriate.

### Rule 4 - Do not over-test trivial fixes
Do not add unit tests for mechanical cleanup with no runtime effect.

### Rule 5 - Add protection around logic-sensitive changes
When touching logic involving:
- uniqueness
- filtering
- sorting
- pagination
- defaults
- validation
- lifecycle
- status derivation
- soft-delete / invalid rules
- request / response contracts

you must either:
- rely on meaningful existing coverage, or
- add targeted tests, or
- justify why higher-level tests are the right protection

### Rule 6 - Separate repair from refactor
Only refactor when required to make the repair safe, understandable, or testable.

---

## Preferred Test Heuristics

### Usually no new tests
- formatting changes
- import cleanup
- unused variable removal
- type-only annotations
- local rename-only cleanup

### Usually fix first, then add tests
- obvious null guards
- optional chaining fixes
- helper typing corrections with stable semantics
- defensive safety fixes

### Usually add tests before fix
- domain validation changes
- uniqueness checks
- filtering behavior
- default selection logic
- lifecycle rules
- status derivation
- business branching

### Usually prefer integration / hurl
- route validation
- middleware behavior
- HTTP status mapping
- serialized response shape
- endpoint lifecycle behavior

---

## File Selection Guidance

Prefer touching only the smallest necessary set of files.

Possible files to inspect depending on the issue:

- implementation files directly failing lint / build
- dependent types / schemas / contracts
- associated service / route / validation layers
- related tests
- relevant `.opencode` rules / checklists / prompts

Do not expand scope without necessity.

---

## If You Encounter a Deeper Problem

If a supposedly simple lint/build repair reveals a real logic problem:

1. stop treating it as a mechanical task
2. reclassify the risk
3. adjust the test strategy
4. repair the root cause instead of patching symptoms
5. explain the reclassification in the output

---

## Completion Standard

Only consider the task complete when:

- the required lint / type / build failures are resolved
- the repair approach matches the risk level
- the test decision is justified
- relevant checks were run
- behavior-sensitive changes are protected or explicitly flagged

---

## Output Style

Be concise, technical, and explicit.

Always separate:
- mechanical fixes
- structural fixes
- behavior-sensitive fixes

When uncertainty remains, state it clearly instead of overstating confidence.

---

## Final Instruction

Do a **behavior-safe lint/build repair**.

Start by identifying the failing checks, classifying risk, and choosing the test policy before making code changes.
