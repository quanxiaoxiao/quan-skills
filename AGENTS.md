# AGENTS.md

Shared rules for all coding agents working in this repository. Tool-specific instructions belong in `CLAUDE.md` (Claude Code) or `SKILL.md` (Codex CLI).

## Active Skills

| Skill | Purpose |
|-------|---------|
| `ts-backend-standard` | Hono + Zod + strict TS/ESLint backend patterns |
| `behavior-safe-code-repair` | Risk-aware lint/typecheck/build repair |
| `hurl-testing` | Hurl API test generation, repair, regression |
| `doc-evidence-compare` | Doc vs code evidence gathering and mismatch detection |
| `zx-script` | zx-based CLI automation scripts |
| `auto-proxy-detect` | Network diagnostics and proxy configuration |
| `admin-crud-style` | Admin CRUD UI patterns |
| `doc-governance-init` | Bootstrap or refactor repo documentation into layered governance |

## Coding Principles

These apply to all generated code unless a specific skill or user overrides them:

- **No classes in JS/TS**: Use functions, factory functions, closures, composition. Never generate `class`, `abstract class`, inheritance, or constructor-based services.
- **Testable boundaries**: Split logic into small, single-purpose, pure functions. Inject dependencies through parameters.
- **Service layers**: Use factory functions (`createXxxService(deps) => {...}`) not class-based services.
- **Existing classes**: Don't expand them; add new logic as functions. Refactor only when safe and scoped.
- **Exception**: Classes allowed only when user explicitly requires them or framework mandates them.
- **Runtime-first**: Prefer minimal, pure-JavaScript dependencies.
- **Verify with the repo's own scripts**, not ad hoc substitutes.

Full rule definitions live in `rules/`. Verification checklists live in `checklists/`.

## Conventions

- **Skill naming**: kebab-case with tech or action prefix (`ts-`, `py-`, `deploy-`)
- **Commits**: `skill(name): description` (conventional commit style)
- **No duplication**: Reference docs go in `references/`, not pasted into SKILL.md
- **One skill = one responsibility**
- **Skill anatomy**: See [docs/skill-authoring.md](docs/skill-authoring.md) for the full authoring guide

## Documentation Rules

- Project architecture and authoring guides live in `docs/`. Reference them; do not duplicate.
- Update documentation when changing behavior described in `docs/`.
- Do not add speculative documentation. Document what exists.
- When adding a new skill, update the Active Skills table above.

## Cross-Project Reuse Protocol

This repository uses the `quan-skills:` logical path protocol for cross-project references.

**Resolution rule:**

```
quan-skills:<subpath>  →  ~/.codex/skills/quan-skills/<subpath>
```

- `quan-skills:` is a logical prefix, not a relative path
- It always resolves to `QUAN_SKILLS_HOME` (default: `~/.codex/skills/quan-skills`)
- **Project context** uses target project local paths
- **Methodology context** uses `quan-skills:` logical paths

When referencing resources in prompts, skills, rules, or checklists, use this protocol as the primary reference mechanism. Markdown relative links may be kept as convenience shortcuts for in-repo browsing but are not the authoritative reference.

## Shared Resource Notes

- When a task targets `scripts/`, follow `quan-skills:rules/scripts-global.rule.md` and verify with `quan-skills:checklists/scripts-global.checklist.md` before completing.

## Repository Structure

```
quan-skills/
├── README.md              # Human entry point
├── AGENTS.md              # This file — shared agent rules
├── CLAUDE.md              # Claude Code specific guidance
├── SKILL.md               # Codex entry point and skill routing
├── docs/                  # System knowledge (authoring guides, workflows)
│   ├── skill-authoring.md
│   ├── cross-project-usage.md
│   └── workflows/
├── rules/                 # Durable coding and Node.js rules
├── checklists/            # Verification checklists
├── prompts/               # Reusable maintenance prompts
└── skills/                # Individual skill definitions
    └── <name>/
        ├── SKILL.md
        ├── references/
        └── agents/
```
