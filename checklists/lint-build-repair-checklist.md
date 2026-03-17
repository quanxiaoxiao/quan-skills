# Checklist: lint-build-repair-checklist

Use this checklist when repairing ESLint issues, TypeScript typecheck failures, or TypeScript build failures.

The purpose is not only to make tooling pass, but to do so safely and with the right level of behavioral protection.

---

## 1. Failure Identification

- [ ] Confirm whether the failure is from:
  - [ ] ESLint
  - [ ] TypeScript typecheck
  - [ ] TypeScript build
  - [ ] multiple sources
- [ ] Identify the exact files involved
- [ ] Identify whether the visible error is the root cause or only a downstream symptom
- [ ] Group failures by category rather than fixing them one by one blindly

---

## 2. Root Cause Classification

- [ ] Classify each issue as one of:
  - [ ] Level A - cosmetic / mechanical
  - [ ] Level B - structural but behavior-preserving
  - [ ] Level C - behavior-sensitive / business-sensitive
- [ ] Check whether the issue involves:
  - [ ] formatting
  - [ ] import cleanup
  - [ ] unused symbols
  - [ ] null / undefined handling
  - [ ] type mismatch
  - [ ] generic mismatch
  - [ ] schema mismatch
  - [ ] API contract mismatch
  - [ ] validation logic
  - [ ] filtering / sorting / pagination
  - [ ] default selection
  - [ ] lifecycle rules
  - [ ] uniqueness rules
  - [ ] invalid / soft-delete rules
  - [ ] async / retry / fallback behavior

---

## 3. Business Impact Check

- [ ] Determine whether the repair can affect runtime behavior
- [ ] Determine whether the repair can affect business logic
- [ ] Determine whether public request / response contracts may be affected
- [ ] Raise risk level if the repair touches:
  - [ ] lifecycle behavior
  - [ ] validation
  - [ ] mapping / transformation
  - [ ] filtering
  - [ ] status derivation
  - [ ] uniqueness
  - [ ] default values
  - [ ] error handling visible to callers

---

## 4. Existing Test Coverage Review

- [ ] Check for relevant existing unit tests
- [ ] Check for relevant integration tests
- [ ] Check for relevant contract tests
- [ ] Check for relevant hurl tests
- [ ] Decide whether existing coverage is already sufficient
- [ ] Avoid adding redundant tests if meaningful coverage already exists

---

## 5. Test Policy Decision

Choose one primary policy per affected area.

- [ ] Policy A - No new tests
- [ ] Policy B - Fix first, then add tests
- [ ] Policy C - Add tests before fix
- [ ] Policy D - Use integration / contract / hurl tests instead of unit tests

Validation of decision:

- [ ] If Policy A, confirm the change is runtime-neutral
- [ ] If Policy B, confirm the intended behavior is obvious and low-risk
- [ ] If Policy C, confirm business logic may change and a safety net is needed
- [ ] If Policy D, confirm higher-level behavior matters more than isolated implementation details

---

## 6. Safe Repair Constraints

Before editing code, confirm:

- [ ] The planned fix is the smallest justified change set
- [ ] Mechanical fixes are separated from logic-affecting fixes
- [ ] Public contracts will be preserved unless change is required
- [ ] Unrelated refactors are excluded
- [ ] Business logic will not be changed silently

Do not use these as default shortcuts:

- [ ] `any`
- [ ] `as any`
- [ ] blind non-null assertions
- [ ] swallowed errors
- [ ] weakened validation
- [ ] unsafe broad casts
- [ ] semantic-changing default values just to satisfy typing

---

## 7. Code Change Execution

- [ ] Fix mechanical issues first
- [ ] Fix root type / contract mismatches before patching downstream errors
- [ ] Keep diffs scoped and explainable
- [ ] Update related tests if behavior-sensitive changes are required
- [ ] Update related docs / rules if public behavior or assumptions changed

---

## 8. Validation

Run all relevant checks for the affected scope.

- [ ] ESLint passes
- [ ] TypeScript typecheck passes
- [ ] TypeScript build passes
- [ ] Existing tests pass
- [ ] New tests pass
- [ ] Integration / hurl / contract tests pass if relevant
- [ ] Public contract has not changed unintentionally

If any check is not run:

- [ ] explicitly record what was not run
- [ ] explain why
- [ ] state the residual risk

---

## 9. Post-Fix Review

- [ ] Confirm the final fix matches the chosen risk level
- [ ] Confirm the chosen test strategy was followed
- [ ] Confirm no unnecessary widening or suppression was introduced
- [ ] Confirm any behavior change is explicit and justified
- [ ] Confirm regression protection exists for behavior-sensitive repairs

---

## 10. Final Output Requirements

The final result should state:

- [ ] issue summary
- [ ] risk level
- [ ] test decision
- [ ] files changed
- [ ] validation results
- [ ] residual risk
- [ ] recommended follow-up if needed

---

## Quick Heuristics

### Usually no new tests
- [ ] formatting-only change
- [ ] import cleanup
- [ ] unused variable removal
- [ ] type-only import conversion
- [ ] explicit typing with no runtime effect

### Usually fix first, then add tests
- [ ] null guard
- [ ] optional chaining correction
- [ ] helper typing correction with preserved semantics
- [ ] defensive safety repair

### Usually add tests before fix
- [ ] uniqueness rule
- [ ] filtering logic
- [ ] validation logic
- [ ] lifecycle rule
- [ ] default-selection rule
- [ ] status derivation
- [ ] business branching

### Usually prefer integration / hurl
- [ ] route validation
- [ ] middleware behavior
- [ ] response shape correction
- [ ] status code mapping
- [ ] endpoint lifecycle behavior

---

## Stop Conditions

Stop and reclassify the task if:

- [ ] a "simple" lint/build issue reveals a business logic defect
- [ ] one small fix causes many downstream errors
- [ ] the safe runtime behavior conflicts with the easiest type-level patch
- [ ] a public contract must change to complete the repair
