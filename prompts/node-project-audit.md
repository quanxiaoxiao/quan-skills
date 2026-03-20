# Node Project Audit Prompt

> Authoritative prompt: `quan-skills:prompts/node-project-audit.md`

Audit the current Node.js project according to the repository rules and checklist.

Your task is to inspect the existing project and produce a structured audit report.

---

## Objectives

1. Detect the project's current runtime contract
2. Detect the current package manager
3. Detect the module system
4. Evaluate dependency hygiene
5. Evaluate verification coverage
6. Identify consistency issues
7. Recommend low-risk improvements
8. Respect existing project constraints instead of forcing modernization

---

## Runtime Detection Rules

Determine the intended runtime in this priority order:

1. `.nvmrc`
2. `.node-version`
3. `package.json.engines.node`
4. CI configuration
5. project documentation
6. syntax/dependency inference only if needed

If `.nvmrc` exists:

- treat it as the primary runtime source
- do not recommend upgrading Node unless explicitly requested

If runtime declarations disagree:

- report the mismatch clearly
- do not silently normalize it

---

## Package Manager Rules

Determine the package manager by lockfile.

- `package-lock.json` -> npm
- `pnpm-lock.yaml` -> pnpm
- `yarn.lock` -> yarn
- `bun.lock` / `bun.lockb` -> bun

Rules:

- respect the existing package manager
- do not recommend switching package managers unless explicitly requested
- flag multiple lockfiles as an inconsistency

---

## Module System Rules

Inspect:

- `package.json.type`
- `tsconfig.json`
- file extensions
- import/export usage
- require/module.exports usage

Rules:

- preserve current module system unless migration is requested
- flag mixed ESM/CommonJS usage as a consistency issue
- do not force ESM migration by default

---

## Dependency Review Rules

Check whether the project uses packages that duplicate built-in Node.js capabilities.

Examples to review:

- `axios`
- `node-fetch`
- `request`
- `fs-extra`
- `lodash`
- `moment`
- `uuid`
- `rimraf`
- `mkdirp`

Classify findings as:

- keep
- review
- avoid for new code

Do not propose sweeping removals unless explicitly requested.

---

## Verification Review Rules

Inspect whether the project has:

- lint
- typecheck
- tests
- integration tests
- API contract tests
- CI verification

Classify verification state as:

- complete
- partial
- weak
- missing

---

## Output Format

Produce a report with the following sections:

1. Project Type
2. Runtime
3. Package Manager
4. Module System
5. Dependency Hygiene
6. Verification Baseline
7. CI and Tooling
8. Consistency Issues
9. Risk Notes
10. Suggested Next Actions

---

## Style Rules

- Be explicit and concrete
- Prefer consistency over modernization for existing projects
- Do not recommend upgrades without justification
- Prioritize low-risk and high-value improvements
- Separate factual findings from recommendations
