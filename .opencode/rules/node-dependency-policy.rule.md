# Node Dependency Policy Rule

Defines the dependency selection policy for Node.js projects.

---

## Default Policy

Dependencies should be:

- minimal
- justified
- portable
- maintainable
- preferably pure JavaScript

Avoid adding packages that increase install complexity without clear benefit.

---

## Preferred Dependency Style

Prefer dependencies that are:

- pure JavaScript
- well-maintained
- widely used
- easy to replace later
- free of native build requirements
- free of hidden postinstall complexity

---

## Avoid by Default

Avoid dependencies that require or commonly introduce:

- native compilation
- `node-gyp`
- platform-specific toolchains
- WASM when not necessary
- heavy transitive dependency chains
- runtime duplication of built-in Node.js functionality

---

## Existing Project Considerations

For mature or already-working projects:

- respect current dependency decisions unless there is a reason to change them
- do not churn dependencies just for style preference
- report dependency issues separately from functional issues
- prefer incremental cleanup over sweeping replacement

---

## Allowed Reasons to Add a Dependency

A new dependency may be justified if it provides one or more of the following:

- substantial reduction in implementation complexity
- significantly improved correctness or safety
- stable support for a protocol or format that is not practical to implement in-house
- better maintainability than a custom implementation
- clear team-wide reuse value

---

## Required Evaluation

Before adding a dependency, evaluate:

- Is there a built-in Node.js alternative?
- Is there already an equivalent package in the project?
- Is the package pure JavaScript?
- Does it add native build risk?
- Does it simplify the code enough to justify the dependency?
- Will it make CI, local setup, or portability worse?

---

## Goal

Keep Node.js projects lean, predictable, and easy to set up across macOS and Linux.
