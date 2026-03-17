# Prompt: Create New Skill

Create a new Codex skill for the quan-skills repository.

## Authoritative Source

Full authoring guide, template, naming conventions, and maintenance workflow:
[docs/skill-authoring.md](../docs/skill-authoring.md)

## Context

- Skill name: `{skill_name}`
- Purpose: `{skill_purpose}`
- Tech stack / domain: `{tech_stack}`

## Steps

1. Read `docs/skill-authoring.md` for the template and conventions
2. Create directory: `skills/{skill_name}/`
3. Create `SKILL.md` using the template (frontmatter + required sections)
4. Add `references/` folder if supporting docs are needed
5. Update `AGENTS.md` active skill table
6. Commit: `skill({skill_name}): add {brief_description}`

## Constraints

- Keep it minimal and actionable — start simple, expand later
- Follow kebab-case naming with tech or action prefix
- Make triggers clear and specific
- One skill = one responsibility
