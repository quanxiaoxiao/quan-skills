# Plan: scripts-global-constraints Alignment

Align the newly added `prompts/scripts-global-constraints.md` with the repository's layered documentation structure (rules, checklists, prompts, skills, AGENTS.md, SKILL.md).

---

## Problem

`prompts/scripts-global-constraints.md` is a monolithic file (~500 lines) that mixes three distinct content types into one prompt:

| Content Type | Project Convention | Current State |
|-------------|-------------------|---------------|
| Durable rules (Hard Constraints 1-5, Design Rules 15-21) | `rules/xxx.rule.md` | Embedded in prompt |
| E2E support rules (Rules 6-14) | `rules/` or skill `references/` | Embedded in prompt |
| Review checklist (Default Review Checklist) | `checklists/xxx.checklist.md` | Embedded in prompt |
| Execution guidance (Agent Instructions, templates) | `prompts/xxx.md` | Embedded in prompt |
| AGENTS.md registration | AGENTS.md rules/checklists list | Missing |
| SKILL.md routing | SKILL.md routing table | Missing |
| zx-script skill linkage | `skills/zx-script/SKILL.md` | Not referenced |

This violates the project's core principle: **define once in rules/ -> verify via checklists/ -> reference from prompts/**.

---

## Changes

### 1. New file: `rules/scripts-global.rule.md`

Extract persistent rules from the current prompt.

Content scope (from current prompt sections):

- Hard Constraints 1-5
  - No imports from `src/`
  - Prefer HTTP-based maintenance workflows
  - Third-party dependencies must be minimal and dev-only
  - Use `.mjs` entry files
  - Mandatory usage header comment
- E2E Support Script Rules 6-14
  - E2E scripts are support tools, not runtime logic containers
  - Prefer black-box or controlled gray-box interaction
  - Separate setup/probe/seed/verify/cleanup/orchestrate responsibilities
  - Repeatability and idempotency where practical
  - Explicit environment/config inputs
  - Clear failure signals
  - Bounded polling and readiness patterns
  - Machine-friendly output where useful
  - Do not hide mutable destructive actions
- Design Rules 15-21
  - Scripts must remain standalone and understandable
  - Prefer simple argument handling
  - Prefer JSON/plain-text output suitable for tooling
  - Support safe modes for operational scripts
  - Error handling must be explicit
  - Keep scripts function-oriented, not framework-oriented
  - Local reusable helpers live under `scripts/`, not `src/`

**Excluded from rule file** (stays in prompt):
- Recommended directory structure
- File template / code examples
- "What to Avoid" quick reference
- Agent execution defaults

**Relationship note:** Opening section should state relationship to existing `rules/node-script-vs-app.rule.md`:
- `node-script-vs-app` defines **what qualifies as a script**
- `scripts-global` defines **how scripts must be written**

---

### 2. New file: `checklists/scripts-global.checklist.md`

Extract from current prompt's "Default Review Checklist" section.

Two sections:

**General script checks** (aligned to rules 1-5, 15-21):

- [ ] Fully independent from `src/`?
- [ ] Uses `.mjs` extension?
- [ ] Has top-of-file usage comment with purpose, usage, arguments, examples, notes?
- [ ] Prefers Node.js built-in modules?
- [ ] Extra dependencies truly necessary?
- [ ] Script-only dependencies placed in `devDependencies`?
- [ ] Prefers HTTP/external interfaces over internal code reuse?
- [ ] Execution is explicit and easy to understand?
- [ ] Output suitable for terminal use and model inspection?
- [ ] Fails clearly with non-zero exit code?
- [ ] Argument parsing is simple and explicit?
- [ ] Mutation scripts support `--dry-run` or similar safe mode?

**E2E support script additional checks** (aligned to rules 6-14):

- [ ] Behaves like an external support tool, not internal app code?
- [ ] Avoids importing runtime internals?
- [ ] Uses explicit health/reset/seed/admin/test interfaces?
- [ ] Safe to rerun, or assumptions clearly documented?
- [ ] Timeout/polling behaviors bounded and explicit?
- [ ] Destructive actions clearly signaled?
- [ ] CI or other tools can consume its output?
- [ ] Config inputs exposed via CLI args and/or documented env vars?
- [ ] Responsibilities focused (not mixing setup + verify + cleanup)?

Format: consistent with existing `checklists/` files (checkbox list + short explanation per item).

---

### 3. Rewrite: `prompts/scripts-global-constraints.md`

Slim down to an execution-guidance prompt (~100-150 lines). Structure:

1. **Purpose** — one paragraph stating this prompt guides script creation/review under `scripts/`
2. **Pre-Read** — reference `rules/scripts-global.rule.md` and `checklists/scripts-global.checklist.md`
3. **Recommended directory structure** (kept from current)
   - `scripts/audit/`
   - `scripts/ops/`
   - `scripts/dev/`
   - `scripts/e2e/`
   - `scripts/lib/`
   - `scripts/e2e/setup/`
   - `scripts/e2e/probe/`
   - `scripts/e2e/seed/`
   - `scripts/e2e/verify/`
   - `scripts/e2e/cleanup/`
   - `scripts/e2e/orchestrate/`
4. **Recommended file template** — the `.mjs` boilerplate with header comment (kept from current)
5. **What to Avoid** — quick-reference list (kept from current)
6. **Agent execution defaults** — summary bullet points (kept from current)

Does NOT duplicate any rule or checklist content — references only.

---

### 4. Modify: `skills/zx-script/SKILL.md`

Add to Pre-Read Order:

```
- ../../rules/scripts-global.rule.md
- ../../checklists/scripts-global.checklist.md
```

Relationship:

```
scripts-global.rule.md   (base constraints for all scripts/)
  └── zx-script skill    (zx-specific framework guidance, inherits base constraints)
```

zx-script focuses on zx framework details; scripts-global provides the underlying constraints that apply to all scripts regardless of framework.

---

### 5. Modify: `AGENTS.md`

Add to rules listing:

```
| scripts-global | rules/scripts-global.rule.md | Global constraints for scripts/ directory: independent from src/, minimal deps, .mjs, E2E support rules |
```

Add to checklists listing:

```
| scripts-global | checklists/scripts-global.checklist.md | Review checklist for scripts/ code |
```

---

### 6. Modify: `SKILL.md`

Add routing entry for `scripts/` directory work so Codex auto-loads scripts-global constraints when user works in `scripts/`.

---

## Not in scope

- **No new skill** — scripts-global is a cross-cutting constraint, not an independent workflow. Rule + checklist coverage is sufficient.
- **No CLAUDE.md change** — it only provides pointers, does not list individual rules.
- **No docs/workflows/ change** — no script-specific workflow document needed at this time.
- **No changes to other skills** — only zx-script is directly related to scripts.

---

## File change summary

| Action | File | Description |
|--------|------|-------------|
| Create | `rules/scripts-global.rule.md` | Persistent rules extracted from prompt |
| Create | `checklists/scripts-global.checklist.md` | Review checklist extracted from prompt |
| Rewrite | `prompts/scripts-global-constraints.md` | Slim execution-guidance prompt referencing rule + checklist |
| Modify | `skills/zx-script/SKILL.md` | Add scripts-global to Pre-Read Order |
| Modify | `AGENTS.md` | Register new rule and checklist |
| Modify | `SKILL.md` | Add scripts/ routing entry |

---

## Execution order

1. Create `rules/scripts-global.rule.md`
2. Create `checklists/scripts-global.checklist.md`
3. Rewrite `prompts/scripts-global-constraints.md`
4. Modify `skills/zx-script/SKILL.md`
5. Modify `AGENTS.md`
6. Modify `SKILL.md`
7. Verify cross-references are consistent
