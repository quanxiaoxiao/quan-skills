# Audit: docs-alignment Structure

Reusable audit baseline for validating the documentation alignment structure defined in `plans/docs-alignment.md`.

---

## Audit Scope

This audit verifies five areas:

1. active skill health
2. governance artifact presence
3. cross-reference integrity
4. naming and de-duplication
5. completion evidence

---

## Files That Must Exist

### Root Governance

| File | Purpose |
|------|---------|
| `AGENTS.md` | Shared governance rules and active skill list |
| `SKILL.md` | Root routing and shared defaults |
| `plans/docs-alignment.md` | Target structure, execution order, and completion definition |
| `audits/docs-alignment-structure-audit.md` | This audit baseline |

### Active Skill Entry Files

| File | Purpose |
|------|---------|
| `skills/ts-backend-standard/SKILL.md` | Backend standard skill definition |
| `skills/behavior-safe-code-repair/SKILL.md` | Repair workflow skill definition |
| `skills/hurl-testing/SKILL.md` | Hurl testing skill definition |
| `skills/doc-evidence-compare/SKILL.md` | Doc/code evidence skill definition |
| `skills/zx-script/SKILL.md` | zx script skill definition |
| `skills/auto-proxy-detect/SKILL.md` | Proxy diagnosis skill definition |
| `skills/admin-crud-style/SKILL.md` | Admin CRUD skill definition |
| `skills/doc-governance-init/SKILL.md` | Documentation governance skill definition |

### Required Agents Metadata

| File | Purpose |
|------|---------|
| `skills/ts-backend-standard/agents/openai.yaml` | Discovery metadata |
| `skills/behavior-safe-code-repair/agents/openai.yaml` | Discovery metadata |
| `skills/hurl-testing/agents/openai.yaml` | Discovery metadata |
| `skills/doc-evidence-compare/agents/openai.yaml` | Discovery metadata |
| `skills/zx-script/agents/openai.yaml` | Discovery metadata |
| `skills/auto-proxy-detect/agents/openai.yaml` | Discovery metadata |
| `skills/admin-crud-style/agents/openai.yaml` | Discovery metadata after remediation |
| `skills/doc-governance-init/agents/openai.yaml` | Discovery metadata for governance skill |

### Governance Artifacts Expected By Plan

| File | Purpose |
|------|---------|
| `prompts/ts-backend-audit.md` | Task prompt for `ts-backend-standard` |
| `rules/behavior-safe-code-repair.rule.md` | Durable repair constraints |
| `checklists/doc-evidence-compare.checklist.md` | Evidence comparison verification |
| `checklists/functional-only-js-ts.checklist.md` | Renamed functional-style checklist |
| `prompts/auto-proxy-detect.md` | Proxy diagnosis task prompt |
| `checklists/auto-proxy-detect.checklist.md` | Proxy diagnosis verification |
| `prompts/admin-crud-scaffold.md` | CRUD page task prompt |
| `checklists/admin-crud-style.checklist.md` | CRUD style verification |

---

## Files That Must Not Exist

| File | Reason |
|------|--------|
| `rules/forbid-oop-patterns.rule.md` | Replaced by merged `functional-only-js-ts.rule.md` |
| `checklists/no-oop-js-ts.checklist.md` | Replaced by `functional-only-js-ts.checklist.md` |

If either file still exists, the naming and de-duplication phase is incomplete.

---

## Active Skill Health Checks

### All Active Skills

- [ ] Every active skill listed in `AGENTS.md` has a matching `skills/<name>/SKILL.md`
- [ ] Every active skill listed in `AGENTS.md` is routable from root `SKILL.md`
- [ ] No active skill appears in `AGENTS.md` but is missing from root routing

### `skills/admin-crud-style/SKILL.md`

- [ ] Has valid YAML frontmatter with `name` and `description`
- [ ] Contains `Use This Skill`
- [ ] Contains `Pre-Read Order`
- [ ] Contains `Workflow`
- [ ] Contains `Verification`
- [ ] Scope is consistent with React + Tailwind + shadcn/ui CRUD admin work
- [ ] No malformed header or broken frontmatter remains

### Other Active Skills

- [ ] Each active skill keeps the required sections defined by `docs/skill-authoring.md`
- [ ] Any skill with `agents/openai.yaml` has prompt wording aligned with the current skill scope
- [ ] No active skill duplicates large governance content that should live in `rules/`, `checklists/`, or `prompts/`

---

## Cross-Reference Checks

### `AGENTS.md`

- [ ] Active Skills table matches the actual skill set on disk
- [ ] Documentation rules still point to `docs/` as the source of truth
- [ ] No file-name drift remains after rule/checklist renames

### `SKILL.md`

- [ ] Skill Routing includes `auto-proxy-detect`
- [ ] Skill Routing includes `admin-crud-style`
- [ ] Routing descriptions match the current skill purpose
- [ ] Shared Repository Resources still describe `rules/`, `checklists/`, `prompts/`, and `docs/workflows/`

### `skills/*/SKILL.md`

- [ ] `Pre-Read Order` references still resolve
- [ ] Renamed files use new paths
- [ ] Newly introduced governance artifacts are referenced where relevant

### `skills/*/agents/openai.yaml`

- [ ] `display_name` and `short_description` match the skill name and scope
- [ ] `default_prompt` still points at the correct skill trigger
- [ ] No active skill that should support discovery is missing its metadata file

### `prompts/*.md`

- [ ] Each new prompt names its authoritative source
- [ ] Each new prompt references the relevant rule and/or checklist when applicable
- [ ] Prompts stay task-oriented and do not absorb full rule prose

---

## Content Boundary Checks

### Rules

- [ ] `rules/behavior-safe-code-repair.rule.md` contains durable constraints, not task choreography
- [ ] `rules/functional-only-js-ts.rule.md` is the single source for no-class functional JS/TS policy
- [ ] No prompt contains long copied rule prose

### Checklists

- [ ] `checklists/doc-evidence-compare.checklist.md` contains review items, not tutorial prose
- [ ] `checklists/functional-only-js-ts.checklist.md` is the only functional-style checklist name in use
- [ ] `checklists/auto-proxy-detect.checklist.md` and `checklists/admin-crud-style.checklist.md` are grouped verification artifacts, not mini-rules

### Prompts

- [ ] `prompts/ts-backend-audit.md` is task-oriented
- [ ] `prompts/auto-proxy-detect.md` is task-oriented
- [ ] `prompts/admin-crud-scaffold.md` is task-oriented
- [ ] Prompts reference constraints instead of redefining them

### Skill-Internal Governance

- [ ] `doc-governance-init` keeps its internal `prompts/`, `checklists/`, and `templates/` without unnecessary root duplication
- [ ] No root-level artifact duplicates `doc-governance-init` internal product docs

---

## Naming and De-Duplication Checks

- [ ] No active governance or skill file still references `forbid-oop-patterns.rule.md`
- [ ] No active governance or skill file still references `no-oop-js-ts.checklist.md`
- [ ] All surviving references point to `functional-only-js-ts.rule.md`
- [ ] All surviving checklist references point to `functional-only-js-ts.checklist.md`
- [ ] No duplicate OOP prohibition text is maintained in two separate rule files

---

## Completion Evidence Checks

- [ ] `plans/docs-alignment.md` completion criteria can be evaluated from the repo state
- [ ] This audit file reflects the latest target structure
- [ ] At least one manual audit pass has been recorded or reported after implementation

---

## Verification Commands

```bash
# Root governance files
ls AGENTS.md SKILL.md plans/docs-alignment.md audits/docs-alignment-structure-audit.md

# Active skills on disk
find skills -maxdepth 2 -name SKILL.md | sort

# Agent metadata presence
ls skills/*/agents/openai.yaml 2>/dev/null | sort

# Root routing coverage
rg -n "auto-proxy-detect|admin-crud-style|doc-governance-init|behavior-safe-code-repair|ts-backend-standard" AGENTS.md SKILL.md

# Required new governance artifacts
ls \
  prompts/ts-backend-audit.md \
  rules/behavior-safe-code-repair.rule.md \
  checklists/doc-evidence-compare.checklist.md \
  checklists/functional-only-js-ts.checklist.md \
  prompts/auto-proxy-detect.md \
  checklists/auto-proxy-detect.checklist.md \
  prompts/admin-crud-scaffold.md \
  checklists/admin-crud-style.checklist.md

# Old names must be gone
test ! -f rules/forbid-oop-patterns.rule.md && echo "OK: old OOP rule removed"
test ! -f checklists/no-oop-js-ts.checklist.md && echo "OK: old checklist removed"

# Residual references must be gone from active paths
! rg -n \
  --glob 'AGENTS.md' \
  --glob 'SKILL.md' \
  --glob 'rules/**' \
  --glob 'checklists/**' \
  --glob 'prompts/**' \
  --glob 'skills/**' \
  "forbid-oop-patterns\\.rule\\.md|no-oop-js-ts\\.checklist\\.md" .

# Check admin-crud-style basic structure
rg -n "^---$|^## Use This Skill$|^## Pre-Read Order$|^## Workflow$|^## Verification$" skills/admin-crud-style/SKILL.md
```

---

## Audit Result Template

Use this structure when running the audit:

| Check Area | Status | Evidence | Follow-up |
|-----------|--------|----------|-----------|
| Active skill health | `pass` / `fail` | file references | required action |
| Governance artifacts | `pass` / `fail` | file references | required action |
| Cross-references | `pass` / `fail` | file references | required action |
| Naming and de-duplication | `pass` / `fail` | file references | required action |
| Completion evidence | `pass` / `fail` | file references | required action |
