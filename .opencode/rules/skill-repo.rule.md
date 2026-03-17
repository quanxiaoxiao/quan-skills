# Rule: Skill Repository Structure

Applies to: `~/.codex/skills/quan-skills/`

## Authoritative Sources

- **Skill authoring guide**: [docs/skill-authoring.md](../../docs/skill-authoring.md) — templates, naming, anatomy, maintenance
- **Shared agent rules**: [AGENTS.md](../../AGENTS.md) — coding principles, conventions, active skill list
- **Repository structure**: [AGENTS.md § Repository Structure](../../AGENTS.md#repository-structure)

## Quick Constraints (for OpenCode context)

1. One SKILL.md per skill, all logic in a single file
2. Top-level skill directories only, no nesting
3. Naming: kebab-case with tech or action prefix
4. Reference docs in `references/`, not pasted into SKILL.md
5. Keep SKILL.md under ~500 lines
6. One skill = one responsibility
7. Conventional commits: `skill(name): description`
8. Update `AGENTS.md` active skill table when adding/removing skills
