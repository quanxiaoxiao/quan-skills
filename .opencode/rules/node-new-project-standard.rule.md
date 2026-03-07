# Node New Project Standard Rule

Defines the default baseline for newly created Node.js projects.

---

## Runtime Baseline

For new projects:

- default to Node.js 24+
- declare the runtime in `package.json.engines.node`
- include a version file such as `.nvmrc` or `.node-version`

The runtime version must be explicit rather than implicit.

---

## Module System

For new projects:

- default to ESM
- use `type: "module"` unless there is a strong reason not to
- prefer standard `import` / `export`
- avoid introducing CommonJS in new code unless required by tooling compatibility

---

## Dependency Style

For new projects:

- prefer built-in Node.js APIs first
- prefer minimal dependencies
- prefer pure JavaScript packages
- avoid native build dependencies unless truly necessary

---

## Package Manager

For new projects:

- choose one package manager and keep it consistent
- generate and keep exactly one lockfile
- do not mix package managers

---

## Recommended Scripts

New projects should provide a clear baseline in `package.json.scripts` when applicable:

- `dev`
- `start`
- `lint`
- `typecheck`
- `test`
- `check`
- `fix`

Suggested behavior:

- `check` should aggregate the main verification steps
- `fix` should apply safe formatting or lint repairs only

---

## Testing Baseline

Prefer the following:

- `node:test` and `assert` for simple projects
- integration tests for HTTP behavior and route contracts
- hurl for external API behavior contracts when relevant

Bug fixes should add regression coverage where appropriate.

---

## Structure Guidance

Use practical and explicit project structure.

For application-style repositories, prefer directories such as:

- `src/`
- `tests/`
- `scripts/`
- `docs/`

For backend repositories, common structure may include:

- `src/routes`
- `src/schemas`
- `src/services`
- `src/models`
- `src/utils`
- `src/types`

For CLI or toolbox repositories, common structure may include:

- `scripts/`
- `lib/`
- `fixtures/`
- `README.md`

---

## Goal

Create modern Node.js projects that are explicit, minimal, maintainable, and aligned with a runtime-first philosophy.
