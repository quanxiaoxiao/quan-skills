# Node Verification Standard Rule

Defines the baseline verification expectations for Node.js projects.

---

## Core Principle

Verification should be explicit and repeatable.

A project should have a clear way to validate code quality, type safety, and behavior.

---

## Preferred Verification Areas

Evaluate the presence and quality of:

- formatting or linting
- type checking
- automated tests
- API contract tests where applicable
- CI execution of verification steps

---

## Minimal Baseline

A healthy Node.js project should ideally support:

- `lint`
- `typecheck`
- `test`
- `check`

Where appropriate:

- `test:hurl`
- integration tests
- regression tests for fixed bugs

---

## Aggregated Verification

Prefer a unified verification entry such as:

- `npm run check`
- `pnpm check`
- `bun run check`

`check` should aggregate the main non-destructive verification steps.

---

## Fix Behavior

A `fix` script may exist, but it should be limited to safe automatic changes such as:

- formatting
- lint autofix
- non-semantic code cleanup

It should not silently rewrite business logic.

---

## Bug Fix Standard

When fixing a real bug:

- add regression coverage if practical
- prefer reproducing the failure before changing behavior
- do not weaken tests just to make them pass
- do not remove meaningful assertions without justification

---

## Audit Reporting

Verification should be reported using clear categories such as:

- complete
- partial
- weak
- missing

Also note whether the missing coverage is:

- low risk
- medium risk
- high risk

depending on the type of project and the area affected.

---

## Goal

Keep verification practical, visible, and trustworthy across both small and large Node.js projects.
