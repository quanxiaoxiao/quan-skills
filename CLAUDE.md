# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

A personal skill pack (`~/.codex/skills/quan-skills`) containing reusable AI instruction modules for consistent TypeScript/Node.js development. Skills teach AI assistants how to follow specific coding standards, architecture patterns, and workflows.

## Repository Structure

- `SKILL.md` — Root-level skill with shared defaults and routing to sub-skills
- `skills/<name>/SKILL.md` — Individual skill definitions (one per directory)
- `skills/<name>/references/` — Supporting docs loaded conditionally by skills
- `skills/<name>/agents/openai.yaml` — OpenAI Codex agent configs
- `rules/` — Repo-wide coding rules (functional-only, no-OOP, complexity)
- `checklists/` — Quick verification checklists
- `.opencode/` — Prompts, rules, checklists, and workflows for OpenCode tool

## Skill Anatomy

Each skill has a `SKILL.md` with YAML frontmatter (`name`, `description`) and required sections: **Use This Skill**, **Pre-Read Order**, **Workflow**, **Verification**. Keep SKILL.md under ~500 lines; move detail into `references/`.

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

## Key Coding Principles (from `rules/`)

- **No classes in JS/TS**: Use functions, factory functions, closures, composition. Never generate `class`, `abstract class`, inheritance, or constructor-based services.
- **Testable boundaries**: Split logic into small, single-purpose, pure functions. Inject dependencies through parameters.
- **Service layers**: Use factory functions (`createXxxService(deps) => {...}`) not class-based services.
- **Existing classes**: Don't expand them; add new logic as functions. Refactor only when safe and scoped.
- **Exception**: Classes allowed only when user explicitly requires them or framework mandates them.

## Conventions

- **Naming**: Skill directories use kebab-case with tech or action prefix (`ts-`, `py-`, `deploy-`)
- **Commits**: `skill(name): description` (conventional commit style)
- **No duplication**: Reference docs go in `references/`, not pasted into SKILL.md
- **One skill = one responsibility**
- **Prefer runtime-first, minimal, pure-JavaScript dependencies**
- **Verify with the repo's own scripts**, not ad hoc substitutes
