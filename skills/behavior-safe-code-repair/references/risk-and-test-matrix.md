# Risk And Test Matrix

Use this reference after you understand the failing checks and before you edit code.

## Risk Levels

### Level A: Mechanical

Typical cases:

- formatting
- import cleanup
- unused symbols
- type-only import conversion
- explicit type annotations with no runtime effect

Expected impact:

- runtime impact: none
- business risk: none or negligible
- default test policy: `Policy A`

### Level B: Behavior-Preserving Structural Repair

Typical cases:

- null guards
- optional handling corrections
- generic fixes
- schema typing aligned to existing runtime behavior
- helper extraction that should preserve outputs

Expected impact:

- runtime impact: possible but intended to preserve behavior
- business risk: low to medium
- default test policy: `Policy B`

### Level C: Behavior-Sensitive Repair

Typical cases:

- validation logic
- filtering, sorting, or pagination rules
- branch conditions
- default values
- lifecycle rules
- request or response shaping
- caller-visible error handling
- async sequencing or fallback behavior

Expected impact:

- runtime impact: likely
- business risk: medium to high
- default test policy: `Policy C` or `Policy D`

## Test Policies

### Policy A: No New Tests

Use when the change is runtime-neutral and existing coverage already protects the area or the change does not need protection.

Common fits:

- import cleanup
- unused local removal
- formatting-only fixes
- type-only refactors

### Policy B: Fix First, Then Add Tests

Use when the intended behavior is obvious, the change is low risk, and a targeted regression test still adds value.

Common fits:

- defensive null guard
- optional chaining correction
- helper typing correction that preserves current outputs

### Policy C: Add Tests Before Fix

Use when business logic may change and a safety net is cheap enough to add first.

Common fits:

- uniqueness rules
- filtering logic
- validation rules
- default-selection logic
- lifecycle transitions

### Policy D: Prefer Higher-Level Tests

Use when correctness matters most at the request or integration boundary rather than inside one function.

Common fits:

- route validation
- middleware behavior
- response shape fixes
- status code mapping
- multi-layer endpoint flows

## Practical Heuristics

- If the easiest type fix changes runtime semantics, stop and re-evaluate.
- If one small fix causes many downstream errors, find the shared root cause before editing more files.
- If public contracts or business rules might change, treat the work as at least Level C.
- If the repository already has meaningful protection, extend it instead of adding a second overlapping test style.

## Stop Conditions

Stop and reclassify the task if:

- a lint or type failure reveals a real business bug
- a compile-safe patch conflicts with observed runtime behavior
- the repair requires a public contract change
- tests cannot be added yet but the change is clearly behavior-sensitive
