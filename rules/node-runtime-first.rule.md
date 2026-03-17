# Node Runtime First Rule

Prefer modern Node.js built-in capabilities over third-party packages whenever possible.

---

## Core Principle

Before adding a dependency, check whether the task can already be solved with:

- Node.js built-in runtime APIs
- Standard JavaScript language features
- Existing project dependencies
- Shell-native tools when appropriate for scripts

Do not introduce libraries that duplicate stable built-in functionality unless there is a clear and justified benefit.

---

## Prefer Built-In APIs

Prefer these built-in capabilities when applicable:

- `fetch`
- `Request`, `Response`, `Headers`
- `URL`, `URLSearchParams`
- `fs/promises`
- `path`
- `os`
- `crypto`
- `stream`
- `zlib`
- `events`
- `util`
- `assert`
- `node:test`
- `timers/promises`

Use runtime-native equivalents such as:

- `crypto.randomUUID()` instead of `uuid`
- `fs.rm(..., { recursive: true, force: true })` instead of `rimraf`
- `fs.mkdir(..., { recursive: true })` instead of `mkdirp`

---

## Avoid These by Default

Avoid adding these unless the project already depends on them for a justified reason:

- `axios`
- `node-fetch`
- `request`
- `fs-extra`
- `lodash`
- `moment`
- `uuid`
- `rimraf`
- `mkdirp`

For new code, do not introduce them by default.

---

## Existing Projects

For existing projects:

- Do not remove mature dependencies blindly
- Prefer "do not add new usage" before "replace everywhere"
- Propose replacement only if it reduces complexity and preserves behavior
- If a dependency is deeply embedded, preserve consistency unless a refactor is explicitly requested

---

## Dependency Decision Order

Before adding a new package, evaluate in this order:

1. Can Node.js built-in APIs solve it?
2. Can standard JavaScript solve it?
3. Can existing project dependencies solve it?
4. Can shell-native tools solve it in scripts?
5. Is a new dependency still justified?

Only add a new dependency if the answer to all earlier steps is no, or if the dependency provides clear value with lower complexity.

---

## Goal

Minimize dependency footprint, improve portability, reduce installation friction, and keep the codebase easier to understand and maintain.
