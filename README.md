# Quan's Skills

Personal Codex skills repository stored at `~/.codex/skills/quan-skills`.

## What This Repository Is For

Reusable skill modules that teach AI assistants how to work with specific tech stacks, coding standards, and workflows. Each skill is a self-contained instruction set for consistent, high-quality assistance across projects.

Use cases:
- Standardize how AI handles TypeScript backend work
- Maintain consistent architecture patterns across repositories
- Avoid repeating setup instructions for common tasks
- Keep institutional knowledge versioned and portable

## Documentation

- [AGENTS.md](AGENTS.md) — shared rules for all coding agents, active skill list
- [Skill Authoring Guide](docs/skill-authoring.md) — how to create, name, and maintain skills
- [Cross-Project Usage](docs/cross-project-usage.md) — how to reuse `quan-skills:` paths from other repositories
- [CLAUDE.md](CLAUDE.md) — Claude Code specific guidance
- [SKILL.md](SKILL.md) — Codex entry point and skill routing

## Active Skills

See [AGENTS.md](AGENTS.md#active-skills) for the current skill list.

## Quick Start

```bash
# Skills are auto-loaded by Codex CLI from ~/.codex/skills/
# To add a new skill:
mkdir skills/<skill-name>
# Then follow docs/skill-authoring.md
```

## Cross-Project Reuse

Use this pack from other repositories through the logical `quan-skills:` path prefix.

```text
quan-skills:<subpath> -> ~/.codex/skills/quan-skills/<subpath>
```

Default:

```text
QUAN_SKILLS_HOME=~/.codex/skills/quan-skills
```

This protocol is tool-agnostic. Codex, OpenCode, and Claude can all use the same reference pattern as long as the target project instructions tell them how to resolve `quan-skills:`.

### Copy Snippet For Other Projects

Add this to the target repository's `AGENTS.md` or equivalent agent instruction file:

```md
## Shared Prompt Pack

This repository uses the shared `quan-skills` pack installed at:

`~/.codex/skills/quan-skills`

When a prompt, skill, rule, or checklist references `quan-skills:<subpath>`, resolve it as:

`~/.codex/skills/quan-skills/<subpath>`

Working rule:

- Project context uses this repository's local files (`README.md`, `src/`, `package.json`)
- Methodology context uses `quan-skills:` logical paths
```

### Common References

Prompt-first examples for fast reuse:

```text
quan-skills:prompts/create-or-review-script.md
quan-skills:prompts/doc-code-evidence-compare.md
quan-skills:rules/scripts-global.rule.md
quan-skills:checklists/scripts-global.checklist.md
```

Typical usage:

- use `quan-skills:prompts/create-or-review-script.md` when a project needs a reusable script creation or script review workflow
- use `quan-skills:prompts/doc-code-evidence-compare.md` when you want doc-vs-code evidence gathering in another repository
- use `quan-skills:rules/scripts-global.rule.md` and `quan-skills:checklists/scripts-global.checklist.md` when a shared prompt points to the script constraints pack

For the full protocol, header format, and vendoring guidance, see [docs/cross-project-usage.md](docs/cross-project-usage.md) and [AGENTS.md](AGENTS.md).

## Future Skill Ideas

- `go-microservice-standard` — Go microservice patterns
- `react-frontend-patterns` — React component and state management standards
- `db-migration-workflow` — Database migration best practices

## License

[MIT](LICENSE)
