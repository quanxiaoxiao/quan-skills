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

## Future Skill Ideas

- `go-microservice-standard` — Go microservice patterns
- `react-frontend-patterns` — React component and state management standards
- `db-migration-workflow` — Database migration best practices

## License

[MIT](LICENSE)
