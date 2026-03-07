# Node Script vs App Rule

Defines different standards for automation scripts versus application code.

---

## Principle

Scripts and applications should not be judged by the exact same standards.

Use a practical standard for scripts and a stronger structural standard for app code.

---

## Script Standard

Applies to:

- zx scripts
- Node.js CLI tools
- local automation utilities
- maintenance scripts
- project helper commands

Scripts should prioritize:

- directness
- readability
- operational clarity
- useful terminal output
- idempotence where possible
- clear exit behavior

Allowed characteristics:

- direct `process.argv` usage
- practical shell command integration
- inline procedural flow when it improves clarity
- human-readable logs
- explicit exit codes
- `--dry-run` support when destructive behavior exists

Avoid overengineering scripts into framework-heavy structures.

---

## App Standard

Applies to:

- backend services
- HTTP APIs
- reusable library modules
- business logic code
- long-lived production systems

Application code should prioritize:

- clear boundaries
- typed inputs and outputs
- validation at boundaries
- consistent error behavior
- testability
- maintainability

Expected characteristics:

- schema validation at entry points
- service layer separated from transport layer
- stable response and error shapes
- explicit business logic boundaries
- verification through lint, typecheck, and tests

---

## Audit Guidance

When reviewing a repository, first identify whether the target is mainly:

- a script/tool repository
- an application repository
- a mixed repository

Evaluate code quality according to the dominant type, rather than applying app-level structure rules to small scripts by default.

---

## Goal

Preserve pragmatism in scripts while enforcing stronger consistency in application code.
