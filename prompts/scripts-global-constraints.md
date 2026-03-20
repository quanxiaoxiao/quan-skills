# Scripts Global Constraints

This document defines the default engineering constraints for all code under the `scripts/` directory.

Its purpose is to keep scripts independent from application runtime code, easy to run directly with Node.js, safe for diagnostics and maintenance, and suitable for operator workflows, audit workflows, and E2E-related support workflows.

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

## Primary Goals

Scripts in this repository are expected to be:

- standalone
- easy to execute directly
- operationally safe
- low-dependency
- readable and maintainable
- decoupled from app internals
- suitable for model-assisted diagnostics and auditing
- suitable as external tooling for E2E preparation and verification

Scripts are intended for tasks such as:

- diagnostics
- audit
- maintenance
- data verification
- environment inspection
- HTTP-based project operations
- local tooling and operator workflows
- E2E setup and cleanup
- E2E fixture orchestration
- E2E state reset
- E2E health checks and readiness checks
- E2E result verification support

---

## Hard Constraints

### 1. Do not import from `src/`

Scripts must **not** import, reuse, or depend on code from the `src/` directory.

This includes direct and indirect imports such as:

- `../src/...`
- shared runtime utilities from `src/`
- internal business logic from `src/`
- application-only types/constants/helpers from `src/`

#### Rationale

Scripts must remain operationally independent tools, not hidden extensions of runtime code.

They should still work even if runtime internals are heavily refactored.

#### Allowed approach

If a script needs project behavior or state, it should obtain it through stable external surfaces such as:

- HTTP APIs
- CLI interfaces
- files explicitly intended for script consumption
- environment variables
- standalone local utilities defined inside `scripts/`

---

### 2. Prefer HTTP-based maintenance workflows

When a script needs to inspect, verify, reconcile, reset, seed, or maintain project state, prefer interacting through HTTP endpoints or other explicit external interfaces.

Scripts should act like external operators/tools, not like internal runtime callers.

#### Preferred order

1. Node.js built-in libraries
2. HTTP requests to exposed project interfaces
3. CLI invocation of explicit project entrypoints
4. minimal local standalone helpers under `scripts/`
5. dev-only third-party dependencies, only when clearly necessary

---

### 3. Third-party dependencies must be minimal and dev-only

Avoid third-party packages unless there is a strong reason.

Prefer Node.js built-in modules whenever possible, for example:

- `node:fs`
- `node:path`
- `node:process`
- `node:url`
- `node:http`
- `node:https`
- `node:stream`
- `node:crypto`
- `node:child_process`
- `node:util`
- `node:events`
- `node:timers/promises`

If a dependency is necessary for script development or execution, it must be added to:

- `devDependencies`

Do not place script-only dependencies into production dependencies.

#### Decision rule

Before adding a package, first check whether the same result can be achieved with:

- built-in Node.js APIs
- a small local helper
- simple direct HTTP logic
- `fetch` and standard platform primitives

Only introduce a package when it materially improves correctness, safety, or maintainability.

---

### 4. Use `.mjs` entry files

All standalone scripts should use the `.mjs` format.

Scripts should be directly executable in a way similar to:

```bash
node scripts/xxx.mjs
```

Do not default to TypeScript or CommonJS for `scripts/` unless there is a very strong project-level exception.

#### Rationale

This keeps execution simple, explicit, and environment-friendly for:

- local runs
- tmux workflows
- SSH/server usage
- Docker usage
- agent-driven execution
- CI execution
- E2E orchestration

---

### 5. Every script must start with a usage header comment

At the top of every script file, add a clear header comment explaining:

- what the script does
- how to run it
- supported arguments/options
- example commands
- important notes or limitations

#### Required header structure

Each script should begin with a comment block similar to:

```js
/**
 * Purpose:
 *   Briefly describe what this script does.
 *
 * Usage:
 *   node scripts/example.mjs --foo bar
 *
 * Arguments:
 *   --foo <value>    Description of foo
 *   --dry-run        Run without applying changes
 *
 * Examples:
 *   node scripts/example.mjs --foo bar
 *   node scripts/example.mjs --foo bar --dry-run
 *
 * Notes:
 *   - This script does not import from src/.
 *   - This script interacts with the project only through HTTP APIs.
 */
```

This header is mandatory.

---

## E2E Support Script Rules

### 6. E2E scripts are support tools, not runtime logic containers

Scripts used for E2E must remain support-layer tools.

They may be used for:

- environment bootstrap
- readiness checks
- fixture creation
- fixture cleanup
- test account preparation
- state reset
- seed data injection through public/admin/test-only HTTP APIs
- post-run verification
- artifact collection
- local E2E orchestration

They must **not** become a hidden location for core business assertions or runtime implementation reuse.

#### Good examples

- wait until a service is healthy
- reset database through explicit test/reset endpoint
- create a test tenant through HTTP API
- seed data needed by E2E through public or test-only interface
- clean generated artifacts after E2E
- collect logs or snapshots after failure

#### Bad examples

- importing application internals to create data directly
- bypassing intended interfaces by calling internal service functions
- embedding large amounts of business logic that should belong in tests
- reimplementing the app's runtime flow inside scripts

---

### 7. Prefer black-box or controlled gray-box E2E support

E2E support scripts should treat the system as an external black box or controlled gray box.

Preferred interaction surfaces:

1. health endpoints
2. reset/seed/admin/test endpoints explicitly exposed for testing
3. CLI commands intended for ops/testing
4. filesystem artifacts explicitly designed for test usage

Avoid direct access to hidden internals.

#### Rationale

This keeps E2E workflows closer to real deployment behavior and reduces coupling to implementation details.

---

### 8. Separate E2E support responsibilities

Do not mix too many unrelated responsibilities into one large script.

Prefer small focused scripts such as:

- `e2e-wait-ready.mjs`
- `e2e-reset-state.mjs`
- `e2e-seed-fixtures.mjs`
- `e2e-verify-state.mjs`
- `e2e-collect-artifacts.mjs`
- `e2e-cleanup.mjs`

If orchestration is needed, a top-level script may compose these smaller scripts.

#### Recommended layering

- setup scripts
- probe/readiness scripts
- seed/reset scripts
- verification scripts
- cleanup scripts
- optional orchestration wrapper

---

### 9. Require repeatability and idempotency where practical

A script should be safe to re-run when practical.

Preferred properties:

- repeated runs do not corrupt state
- reset scripts leave the system in a known baseline
- seed scripts can detect pre-existing fixtures when necessary
- cleanup scripts tolerate partially completed prior runs
- readiness checks time out clearly instead of hanging forever

When strict idempotency is not possible, the script must document its assumptions clearly in the header comment.

---

### 10. Require explicit environment/config inputs

Avoid hidden hardcoded test environments unless they are deliberate defaults.

Support explicit inputs such as:

- `--base-url`
- `--admin-token`
- `--timeout`
- `--project`
- `--fixture`
- `--output`
- `--verbose`

Environment variables are allowed when they are documented clearly.

Example:

- `E2E_BASE_URL`
- `E2E_ADMIN_TOKEN`
- `E2E_TIMEOUT_MS`

The script must make it obvious which configuration source wins when both CLI args and env vars are provided.

Recommended rule:

- CLI args override environment variables
- environment variables override internal defaults

---

### 11. Provide clear failure signals

When an E2E support script fails, it must help diagnosis.

Requirements:

- validate required inputs early
- print actionable failure messages
- return non-zero exit code on failure
- include target URL/resource in errors when safe
- distinguish timeout vs bad response vs validation failure when possible

Avoid silent retry loops without visibility.

---

### 12. Support bounded polling and readiness patterns

For readiness or eventual consistency checks, support bounded polling with explicit parameters, for example:

- `--timeout <ms>`
- `--interval <ms>`
- `--expect-status <code>`

Do not use infinite polling.

A readiness script should clearly state:

- what condition it is waiting for
- how long it will wait
- what caused failure

---

### 13. Produce machine-friendly output where useful

When useful, E2E support scripts should be able to emit JSON so that CI, agents, or other scripts can consume the results.

Recommended options:

- `--json`
- `--output <file>`
- `--quiet`
- `--verbose`

Human-readable summaries are also encouraged, but structured output should be preferred for scripts that feed other tools.

---

### 14. Do not hide mutable destructive actions

For scripts that reset or mutate test state, prefer explicit naming and flags such as:

- `e2e-reset-state.mjs`
- `--yes`
- `--dry-run`

A destructive script must make it obvious that it will:

- delete fixtures
- clear state
- reset environment
- overwrite prior artifacts

---

## Design Rules

### 15. Scripts must remain standalone and understandable

A script should be understandable from its own file and local helpers.

Avoid hidden coupling, magic defaults, or obscure execution paths.

Prefer explicit:

- argument parsing
- environment variable usage
- request targets
- output structure
- exit conditions

---

### 16. Prefer simple argument handling

For small and medium scripts, use lightweight manual argument parsing based on:

- `process.argv`

Do not introduce heavy CLI frameworks unless clearly justified.

If argument parsing becomes repetitive, create a small reusable helper under `scripts/lib/` or similar script-local directory.

That helper must still remain independent from `src/`.

---

### 17. Prefer JSON/plain-text output suitable for tooling

Scripts should produce output that is friendly for:

- terminal reading
- redirection to files
- model inspection
- downstream shell processing
- CI logs

Preferred output styles:

- structured JSON for machine-readable results
- clear text summaries for human-readable diagnostics

Avoid noisy, decorative output unless it clearly improves usability.

---

### 18. Support safe modes for operational scripts

When a script can mutate state, prefer supporting flags such as:

- `--dry-run`
- `--yes`
- `--output <file>`
- `--timeout <ms>`

Mutation scripts should clearly separate:

- inspection
- planning
- execution

Whenever reasonable, allow users to preview intended actions before applying them.

---

### 19. Error handling must be explicit

Scripts must fail clearly and predictably.

Requirements:

- validate inputs early
- print actionable error messages
- return non-zero exit code on failure
- avoid swallowing exceptions silently

Prefer patterns like:

- input validation at startup
- centralized `main()` + `.catch(...)`
- structured error output when useful

---

### 20. Keep scripts function-oriented, not framework-oriented

Prefer small functions with clear responsibilities.

Avoid overengineering scripts into mini-frameworks.

Good script structure usually looks like:

- parse args
- validate config
- fetch/load data
- transform/evaluate
- print result
- exit

---

### 21. Local reusable helpers should live under `scripts/`, not `src/`

If multiple scripts share logic, place the shared code in a script-local area, such as:

- `scripts/lib/`
- `scripts/shared/`
- `scripts/utils/`

Do not move script helper logic into `src/` just for reuse.

---

## Recommended Script Categories

Suggested script categories under `scripts/`:

- `scripts/audit/`
- `scripts/ops/`
- `scripts/dev/`
- `scripts/e2e/`
- `scripts/lib/`

Suggested E2E support categories:

- `scripts/e2e/setup/`
- `scripts/e2e/probe/`
- `scripts/e2e/seed/`
- `scripts/e2e/verify/`
- `scripts/e2e/cleanup/`
- `scripts/e2e/orchestrate/`

These are suggestions, not strict mandatory paths, but structure should stay clear.

---

## Recommended File Pattern

A typical script should follow this structure:

```js
/**
 * Purpose:
 *   ...
 *
 * Usage:
 *   node scripts/xxx.mjs ...
 *
 * Arguments:
 *   ...
 *
 * Examples:
 *   ...
 *
 * Notes:
 *   ...
 */

import process from 'node:process';

function parseArgs(argv) {
  // explicit parsing
}

async function run(options) {
  // main logic
}

async function main() {
  const options = parseArgs(process.argv.slice(2));
  await run(options);
}

main().catch((error) => {
  console.error('[script:error]', error?.stack || error?.message || String(error));
  process.exit(1);
});
```

---

## What to Avoid

Do not:

- import from `src/`
- couple scripts to internal runtime implementation details
- introduce unnecessary third-party dependencies
- put script-only packages in production dependencies
- omit usage comments at the top of the file
- hide network targets or operational behavior
- make scripts depend on large framework abstractions
- convert simple scripts into overbuilt architectures
- use E2E support scripts as a shortcut to bypass intended interfaces
- mix setup, business assertions, and cleanup into one unclear script

---

## Default Review Checklist

When creating or reviewing a script under `scripts/`, verify:

- Is it fully independent from `src/`?
- Does it use `.mjs`?
- Does it have a top usage comment?
- Does it prefer built-in Node.js modules?
- Are extra dependencies truly necessary?
- Are script-only dependencies placed in `devDependencies`?
- Does it prefer HTTP/external interfaces over internal code reuse?
- Is execution explicit and easy to understand?
- Is output suitable for terminal use and model inspection?
- Does it fail clearly?

For E2E-related support scripts, also verify:

- Does it behave like an external support tool rather than internal app code?
- Does it avoid importing runtime internals?
- Does it use explicit health/reset/seed/admin/test interfaces?
- Is it safe to rerun or at least clearly documented?
- Are timeout/polling behaviors bounded and explicit?
- Are destructive actions clearly signaled?
- Can CI or other tools consume its output?

---

## Instruction for Code Generators and Agents

When generating or modifying code under `scripts/`, always follow this document.

Unless explicitly overridden, treat these rules as mandatory defaults.

Agent execution defaults:

- keep scripts independent from runtime code
- never import from `src/`
- prefer Node.js built-ins first
- prefer HTTP/API interaction over internal code reuse
- use `.mjs` entry files
- place script-only dependencies in `devDependencies`
- add a clear usage comment block at the top of every script
- keep implementation small, explicit, and directly runnable
- prefer maintenance/audit/diagnostic scripts that behave like external operator tools
- for E2E support, treat scripts as black-box or controlled gray-box helpers
- keep setup, probe, seed, verify, cleanup, and orchestration responsibilities clear
