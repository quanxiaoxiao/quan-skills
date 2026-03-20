# Cross-Project Usage

How to use `quan-skills` prompts and skills from other repositories.

## Quick Start

Add this snippet to your project's `AGENTS.md` (or equivalent agent instruction file):

```md
## Shared Prompt Pack

This repository uses the shared `quan-skills` pack installed at:

`~/.codex/skills/quan-skills`

When a prompt references `quan-skills:<subpath>`, resolve it as:

`~/.codex/skills/quan-skills/<subpath>`

When applying a shared prompt:

1. Read the target repository context first
2. Read the referenced prompt / skill / rule / checklist from `quan-skills`
3. Apply the prompt using this repository's local code and docs as runtime context
```

## Path Protocol

All cross-project references use the `quan-skills:` logical path prefix:

```
quan-skills:<subpath>  →  $QUAN_SKILLS_HOME/<subpath>
```

Default: `QUAN_SKILLS_HOME=~/.codex/skills/quan-skills`

Examples:

```
quan-skills:prompts/lint-build-repair.md
quan-skills:skills/doc-evidence-compare/SKILL.md
quan-skills:rules/scripts-global.rule.md
quan-skills:checklists/scripts-global.checklist.md
```

## Context Separation

When applying a shared prompt in a target project:

- **Project context** — use target project local paths (`README.md`, `src/`, `package.json`)
- **Methodology context** — use `quan-skills:` logical paths (prompts, skills, rules, checklists)

## Prompt Header Format

Every reusable prompt includes a header block identifying its source:

```md
> Authoritative prompt: `quan-skills:prompts/<name>.md`
> Authoritative skill: `quan-skills:skills/<skill>/SKILL.md`
> Related rule: `quan-skills:rules/<rule>.rule.md`
> Related checklist: `quan-skills:checklists/<checklist>.checklist.md`
```

- `Authoritative prompt` is always present
- Other lines appear only when relevant

## Vendored Export (Optional)

For self-contained projects that cannot depend on a global path:

1. Export the needed prompts and their dependencies into `docs/shared-prompts/quan-skills/`
2. Rewrite `quan-skills:` references to the vendored path
3. Treat the export as a frozen snapshot — do not maintain it bidirectionally
