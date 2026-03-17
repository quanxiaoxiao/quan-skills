# Node Existing Project Audit Rule

Defines how to inspect and work with existing Node.js projects safely and consistently.

---

## Core Principle

For existing projects, prefer consistency over modernization.

Do not assume the project should be upgraded just because a newer default baseline exists.

The first task is to detect the current project contract, then evaluate gaps, inconsistencies, and low-risk improvements.

---

## Runtime Detection Order

When auditing an existing project, determine the intended Node.js runtime in this order:

1. `.nvmrc`
2. `.node-version`
3. `package.json.engines.node`
4. CI configuration
5. README or project docs
6. dependency and syntax clues only if no explicit version source exists

---

## Version Source of Truth

If `.nvmrc` exists:

- treat it as the primary Node version source
- do not recommend a runtime upgrade unless explicitly requested
- do not flag the project merely for not using the latest default version

If `.nvmrc`, `.node-version`, `package.json.engines.node`, and CI configuration disagree:

- report the mismatch clearly
- treat it as a consistency issue
- do not silently pick one and ignore the others

If no version declaration exists:

- report it as a project hygiene gap
- suggest adding one

---

## Package Manager Detection

Determine the package manager from lockfiles:

- `package-lock.json` -> npm
- `pnpm-lock.yaml` -> pnpm
- `yarn.lock` -> yarn
- `bun.lock` or `bun.lockb` -> bun

Rules:

- respect the existing package manager
- do not switch package managers unless explicitly requested
- do not mix lockfiles
- if multiple lockfiles exist, report the inconsistency

---

## Module System Detection

Inspect:

- `package.json.type`
- `tsconfig.json`
- file extensions such as `.mjs`, `.cjs`
- import/export versus require/module.exports usage

Rules:

- preserve the current module system unless migration is requested
- flag mixed ESM/CommonJS usage as a consistency issue
- do not force ESM migration on existing projects by default

---

## Dependency Audit Rules

When auditing dependencies:

- identify packages that duplicate modern built-in Node.js functionality
- classify them as:
  - keep
  - review
  - avoid for new code

Rules:

- do not remove widely used dependencies blindly
- prefer "stop adding new usage" before "replace all usage"
- only suggest replacement when it reduces complexity and preserves behavior

---

## Verification Audit Rules

Evaluate whether the project has verification coverage in these areas:

- lint
- typecheck
- tests
- API contract tests
- CI validation

Classify the state as:

- complete
- partial
- weak
- missing

Missing verification should be reported as a gap, not treated as an automatic migration task.

---

## Output Style for Audits

An existing project audit should summarize:

- runtime
- package manager
- module system
- verification baseline
- dependency hygiene
- CI status
- consistency issues
- risk notes
- suggested next actions

---

## Goal

Make existing projects easier to maintain without unnecessary churn, while surfacing the most important structural and consistency issues.
