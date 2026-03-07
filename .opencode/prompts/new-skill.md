# Prompt: Create New Skill

Create a new Codex skill for the quan-skills repository.

## Context

- Repository: `~/.codex/skills/quan-skills`
- Skills live under `skills/<skill-name>/SKILL.md`
- Use existing skills as templates (e.g., `ts-backend-standard/SKILL.md`)

## Skill Requirements

Skill name: {skill_name}
Purpose: {skill_purpose}
Tech stack / domain: {tech_stack}

## Output

Create the skill structure:

1. Directory: `skills/{skill_name}/`
2. File: `SKILL.md` with:
   - YAML frontmatter (name, description)
   - "Use This Skill" section (when to use / when not to use)
   - "Pre-Read Order" section (files to check first)
   - "Workflow" section (step-by-step actions)
   - "Verification" section (how to validate work)
   - Optional: references/ folder for supporting docs

## Constraints

- Keep it minimal and actionable
- Follow existing naming conventions (kebab-case)
- Make triggers clear and specific
- Include concrete examples where helpful
- Avoid over-engineering - start simple, expand later

## References

- README.md for repository conventions
- ts-backend-standard/SKILL.md for example structure
