# Scripts Global Rule

Authoritative constraints for all code under the `scripts/` directory.

---

## Relationship to Existing Rules

This rule specializes the following repository-wide rules for script-specific work:

- `rules/node-script-vs-app.rule.md` — defines what counts as script-style work versus application code. This rule defines **how** scripts must be written.
- `rules/node-runtime-first.rule.md` — provides general runtime-first dependency preference. This rule narrows it to Node.js built-ins and dev-only packages.
- `rules/node-dependency-policy.rule.md` — provides general dependency evaluation policy. This rule adds the script-specific constraint that script-only packages belong in `devDependencies`.
- `rules/functional-only-js-ts.rule.md` — provides the repository-wide function-only default. This rule reinforces function-oriented structure for scripts.

When guidance overlaps, the shared repo-wide rules remain valid and this rule adds script-specific constraints.

---

## Scope

These rules apply to:

- all new files created under `scripts/`
- all refactors of existing files under `scripts/`
- all generated script code intended to be executed as standalone Node.js entrypoints
- all E2E support scripts under `scripts/` used for setup, seeding, reset, probes, verification, orchestration, or cleanup

These rules do **not** apply to:

- runtime application code under `src/`
- actual application business logic
- unit test source files unless explicitly stated
- dedicated E2E test case files if those are stored elsewhere

---

## Hard Constraints

### 1. Do not import from `src/`

Scripts must **not** import, reuse, or depend on code from the `src/` directory.

This includes direct and indirect imports such as:

- `../src/...`
- shared runtime utilities from `src/`
- internal business logic from `src/`
- application-only types/constants/helpers from `src/`

Scripts must remain operationally independent tools, not hidden extensions of runtime code. They should still work even if runtime internals are heavily refactored.

If a script needs project behavior or state, it should obtain it through stable external surfaces:

- HTTP APIs
- CLI interfaces
- files explicitly intended for script consumption
- environment variables
- standalone local utilities defined inside `scripts/`

### 2. Prefer HTTP-based maintenance workflows

When a script needs to inspect, verify, reconcile, reset, seed, or maintain project state, prefer interacting through HTTP endpoints or other explicit external interfaces.

Scripts should act like external operators/tools, not like internal runtime callers.

Preferred order:

1. Node.js built-in libraries
2. HTTP requests to exposed project interfaces
3. CLI invocation of explicit project entrypoints
4. minimal local standalone helpers under `scripts/`
5. dev-only third-party dependencies, only when clearly necessary

### 3. Third-party dependencies must be minimal and dev-only

Prefer Node.js built-in modules whenever possible (`node:fs`, `node:path`, `node:process`, `node:url`, `node:http`, `node:https`, `node:stream`, `node:crypto`, `node:child_process`, `node:util`, `node:events`, `node:timers/promises`).

If a dependency is necessary for script development or execution, it must be added to `devDependencies`. Do not place script-only dependencies into production dependencies.

Before adding a package, first check whether the same result can be achieved with built-in Node.js APIs, a small local helper, simple direct HTTP logic, or `fetch` and standard platform primitives.

### 4. Use `.mjs` entry files

All standalone scripts should use the `.mjs` format and be directly executable as `node scripts/xxx.mjs`.

Do not default to TypeScript or CommonJS for `scripts/` unless there is a very strong project-level exception.

### 5. Every script must start with a usage header comment

At the top of every script file, add a clear header comment explaining what the script does, how to run it, supported arguments/options, example commands, and important notes or limitations.

Required sections in the header: Purpose, Usage, Arguments, Examples, Notes.

---

## E2E Support Script Rules

### 6. E2E scripts are support tools, not runtime logic containers

Scripts used for E2E must remain support-layer tools for environment bootstrap, readiness checks, fixture creation/cleanup, test account preparation, state reset, seed data injection through public/admin/test-only HTTP APIs, post-run verification, artifact collection, and local orchestration.

They must **not** become a hidden location for core business assertions or runtime implementation reuse.

Good: wait for service health, reset via test endpoint, create test tenant via HTTP, seed data via public interface, clean artifacts, collect logs.

Bad: import application internals, bypass interfaces by calling internal functions, embed business logic that belongs in tests, reimplement runtime flow.

### 7. Prefer black-box or controlled gray-box interaction

E2E support scripts should treat the system as an external black box or controlled gray box.

Preferred interaction surfaces:

1. health endpoints
2. reset/seed/admin/test endpoints explicitly exposed for testing
3. CLI commands intended for ops/testing
4. filesystem artifacts explicitly designed for test usage

### 8. Separate responsibilities

Do not mix unrelated responsibilities into one large script. Prefer small focused scripts for setup, probe/readiness, seed/reset, verification, cleanup, and orchestration. If orchestration is needed, a top-level script may compose smaller scripts.

### 9. Repeatability and idempotency

Scripts should be safe to re-run when practical. Reset scripts should leave the system in a known baseline. Seed scripts should detect pre-existing fixtures when necessary. Cleanup scripts should tolerate partially completed prior runs. Readiness checks should time out clearly instead of hanging forever.

When strict idempotency is not possible, the script must document its assumptions clearly in the header comment.

### 10. Explicit environment/config inputs

Support explicit inputs such as `--base-url`, `--admin-token`, `--timeout`, `--project`, `--fixture`, `--output`, `--verbose`. Environment variables are allowed when documented clearly (e.g. `E2E_BASE_URL`, `E2E_ADMIN_TOKEN`, `E2E_TIMEOUT_MS`).

Precedence rule: CLI args override environment variables; environment variables override internal defaults.

### 11. Clear failure signals

Validate required inputs early. Print actionable failure messages. Return non-zero exit code on failure. Include target URL/resource in errors when safe. Distinguish timeout vs bad response vs validation failure when possible. Avoid silent retry loops.

### 12. Bounded polling and readiness patterns

For readiness or eventual consistency checks, support bounded polling with explicit `--timeout`, `--interval`, `--expect-status` parameters. Do not use infinite polling. A readiness script should clearly state what condition it waits for, how long, and what caused failure.

### 13. Machine-friendly output

When useful, E2E support scripts should emit JSON for CI/agent/tool consumption. Support options like `--json`, `--output <file>`, `--quiet`, `--verbose`. Human-readable summaries are also encouraged.

### 14. Visible destructive actions

For scripts that reset or mutate test state, prefer explicit naming (e.g. `e2e-reset-state.mjs`) and flags (`--yes`, `--dry-run`). A destructive script must make it obvious that it will delete fixtures, clear state, reset environment, or overwrite artifacts.

---

## Design Rules

### 15. Standalone and understandable

A script should be understandable from its own file and local helpers. Avoid hidden coupling, magic defaults, or obscure execution paths. Prefer explicit argument parsing, environment variable usage, request targets, output structure, and exit conditions.

### 16. Simple argument handling

Use `process.argv` for small and medium scripts. Do not introduce heavy CLI frameworks unless clearly justified. Shared argument-parsing helpers may live under `scripts/lib/` but must remain independent from `src/`.

### 17. Tooling-friendly output

Scripts should produce output friendly for terminal reading, file redirection, model inspection, downstream shell processing, and CI logs. Prefer structured JSON for machine-readable results and clear text summaries for diagnostics. Avoid noisy decorative output.

### 18. Safe modes for operational scripts

When a script can mutate state, support `--dry-run`, `--yes`, `--output <file>`, `--timeout <ms>`. Separate inspection, planning, and execution. Allow previewing intended actions before applying them.

### 19. Explicit error handling

Validate inputs early. Print actionable error messages. Return non-zero exit code on failure. Avoid swallowing exceptions silently. Prefer input validation at startup, centralized `main()` + `.catch(...)`, and structured error output.

### 20. Function-oriented, not framework-oriented

Prefer small functions with clear responsibilities. Avoid overengineering scripts into mini-frameworks. Good structure: parse args, validate config, fetch/load data, transform/evaluate, print result, exit.

### 21. Local reusable helpers under `scripts/`

If multiple scripts share logic, place shared code in `scripts/lib/`, `scripts/shared/`, or `scripts/utils/`. Do not move script helper logic into `src/` for reuse.

---

## Non-Goals

This rule does not prescribe:

- mandatory directory structure for `scripts/` (see prompts for recommendations)
- specific file templates or boilerplate (see prompts for recommendations)
- agent-specific execution behavior
