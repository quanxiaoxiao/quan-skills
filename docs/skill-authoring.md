# Skill Authoring Guide

How to create, name, structure, and maintain skills in this pack.

## Skill Anatomy

Each skill lives in `skills/<name>/` and has a `SKILL.md` with YAML frontmatter (`name`, `description`) and these required sections:

1. **Use This Skill** — when to use and when not to use
2. **Pre-Read Order** — files to read before starting work
3. **Workflow** — step-by-step process
4. **Verification** — how to verify the work is correct

Optional but recommended:

- **Implementation Rules** — specific coding standards or constraints
- **Output Contract** — expected response format or deliverables
- **References** — links to supporting docs in `references/`
- **agents/openai.yaml** — UI metadata and default prompt for Codex discovery

Keep `SKILL.md` under ~500 lines. Move detail into `references/` and link only what should load conditionally.

## SKILL.md Template

```markdown
---
name: skill-name
description: One-line description of what this skill does and when to use it.
---

# Skill Title

Brief overview of the skill's purpose.

## Use This Skill

When to use this skill:
- Specific scenario 1
- Specific scenario 2

When NOT to use this skill:
- Out-of-scope scenario

## Pre-Read Order

Files to read before starting:
1. `README.md`
2. `package.json`
3. etc.

## Workflow

1. Step one
2. Step two
3. Step three

## Verification

How to verify the work:
1. `npm run typecheck`
2. `npm run test`
```

## Naming Conventions

- Use lowercase with hyphens: `ts-backend-standard`, `react-frontend-patterns`
- Be descriptive but concise
- Use technology prefix for stack-specific skills: `ts-`, `py-`, `go-`
- Use action prefix for workflow skills: `deploy-`, `test-`, `refactor-`
- Avoid generic names like `utils` or `helpers`

Good examples: `ts-backend-standard`, `py-data-pipeline`, `deploy-aws-lambda`, `refactor-legacy-react`

## Adding a New Skill

1. Create directory: `mkdir skills/<skill-name>`
2. Add `SKILL.md` with the skill definition (use template above)
3. Add supporting files in `references/`, `agents/` as needed
4. Update the Active Skills table in `AGENTS.md`
5. Commit: `skill(<name>): add <description>`

## Maintenance

### Regular Updates

When you notice the AI making mistakes or missing context:

1. Identify which skill needs updating
2. Edit the `SKILL.md` with clearer instructions
3. Test the updated skill on a real task
4. Commit: `skill(<name>): <description of change>`

### Adding New Skills

When you find yourself repeating the same instructions:

1. Extract the pattern into a new skill
2. Follow the structure above
3. Reference existing skills as templates
4. Keep the first version minimal, expand based on usage

### Version Control

- One commit per skill change
- Use conventional commits: `skill(name): description`
- Tag major skill versions if needed: `git tag ts-backend-v2`

### Cleanup

- Remove skills that are no longer used
- Archive outdated skills rather than deleting (move to `archive/`)
- Review skills every few months for relevance

## Pack Defaults

For JavaScript and TypeScript work, the pack default is functional programming style plus splitting logic along unit-testable boundaries. The primary source of that guidance lives in `rules/` and `checklists/`, with skill-level wording kept aligned but shorter.
