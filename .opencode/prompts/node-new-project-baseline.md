# Node New Project Baseline Prompt

Create or adjust a Node.js project to follow the repository's default new-project standard.

---

## Default Baseline

Unless the task explicitly says otherwise:

- Node.js 24+
- explicit runtime declaration
- ESM by default
- runtime-first dependency policy
- minimal dependency footprint
- pure JavaScript dependencies preferred
- one package manager only
- one lockfile only
- explicit verification scripts

---

## Required Decisions

When generating or adjusting a project, ensure:

- `package.json.engines.node` is defined
- `.nvmrc` or `.node-version` is present
- module system is consistent
- scripts are practical and minimal
- dependencies do not duplicate modern built-in Node.js features unnecessarily

---

## Script Baseline

Where appropriate, include:

- `dev`
- `start`
- `lint`
- `typecheck`
- `test`
- `check`
- `fix`

`check` should aggregate the main verification commands.

---

## Dependency Rules

Prefer built-ins first:

- `fetch`
- `fs/promises`
- `path`
- `crypto`
- `URL`
- `node:test`
- `assert`

Avoid adding unnecessary third-party packages that replicate built-in functionality.

---

## Testing Rules

Prefer practical coverage:

- unit tests for pure logic
- integration tests for boundary behavior
- hurl for external API contracts when relevant

Bug fixes should add regression coverage where practical.

---

## Output Expectations

When generating project structure or code, keep it:

- minimal
- explicit
- maintainable
- aligned with macOS + Linux development
- consistent with the repository rules
