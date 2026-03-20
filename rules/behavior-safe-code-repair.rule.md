# Behavior-Safe Code Repair Rule

Durable constraints for repairing lint, typecheck, and build failures without silently changing business behavior.

## Core Principle

Green checks are necessary but not sufficient. A repair that makes the build pass but changes runtime behavior is a regression, not a fix.

## Risk Classification

Every repair must be classified before editing:

- **Mechanical** — formatting, import order, unused-variable removal, type narrowing that does not change runtime paths
- **Behavior-preserving** — refactoring that reorganizes code but preserves observable behavior (rename, extract function, inline type)
- **Behavior-sensitive** — any change that touches branching, validation, error handling, lifecycle, caller-visible return values, or side effects

## Constraints

1. **Smallest justified diff.** Fix the root cause, not downstream symptoms. Do not bundle unrelated cleanup into the same change.

2. **Fix upstream first.** Type or contract mismatches should be fixed at the source (schema, interface, shared type) before adjusting consumers.

3. **No pseudo-fixes.** Do not use `any`, `as any`, blanket `@ts-ignore`, `@ts-expect-error` without a comment, swallowed catches, weakened validation, or disabled lint rules to silence errors.

4. **Preserve runtime truth.** If working code contradicts a type definition, investigate which side is wrong. Do not force types to match by weakening them.

5. **Separate mechanical from logic edits.** When a repair includes both formatting/import changes and logic changes, commit them separately or clearly mark the boundary.

6. **Test policy follows risk level.**
   - Mechanical: no new tests required.
   - Behavior-preserving: existing tests must still pass; add tests only if coverage is clearly missing for the touched area.
   - Behavior-sensitive: targeted regression test required before or after the fix, depending on whether the correct behavior can be specified upfront.

7. **Stop conditions.** If a repair would require changing more than 3 files' business logic, or if the root cause is an architectural mismatch, stop and report rather than pushing through.

## Verification

Run the repository's own scripts (`lint`, `typecheck`, `build`, `test`) rather than ad hoc commands. If a script does not exist, report the gap explicitly.
