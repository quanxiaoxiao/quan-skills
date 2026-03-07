# Quan's Skills

Personal Codex skills repository stored at `~/.codex/skills/quan-skills`.

## What This Repository Is For

This repo stores reusable skill modules that teach AI assistants how to work with my specific tech stacks, coding standards, and workflows. Each skill is a self-contained instruction set for consistent, high-quality assistance across projects.

Use cases:
- Standardize how AI handles TypeScript backend work
- Maintain consistent architecture patterns across repositories
- Avoid repeating setup instructions for common tasks
- Keep institutional knowledge versioned and portable

## Directory Structure

```
quan-skills/
├── README.md                    # This file
├── LICENSE                      # MIT license
├── .opencode/                   # Maintenance tools
│   ├── prompts/                 # Skill generation prompts
│   ├── rules/                   # Repository rules
│   └── checklists/              # Quality checklists
└── skills/                      # All skills live here
    ├── ts-backend-standard/     # TypeScript backend skill
    │   └── SKILL.md
    ├── hurl-testing/            # Hurl API testing skill
    │   └── SKILL.md
    ├── zx-script/               # ZX CLI scripting skill
    │   └── SKILL.md
    └── <skill-name>/            # Future skills follow same pattern
        └── SKILL.md
```

Skills live under `skills/` directory. Each skill is independent and contains everything needed to execute that skill.

## How to Add a New Skill

1. Create a new directory: `mkdir skills/<skill-name>`
2. Add `SKILL.md` with the skill definition (see template below)
3. Add any supporting files in subdirectories (`references/`, `agents/`, etc.)
4. Update the Example Skill List section in this README
5. Commit with a descriptive message

### Quick Template for SKILL.md

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

## Skill Naming Conventions

- Use lowercase with hyphens: `ts-backend-standard`, `react-frontend-patterns`
- Be descriptive but concise
- Use technology prefix for stack-specific skills: `ts-`, `py-`, `go-`
- Use action prefix for workflow skills: `deploy-`, `test-`, `refactor-`
- Avoid generic names like `utils` or `helpers`

Good examples:
- `ts-backend-standard`
- `py-data-pipeline`
- `deploy-aws-lambda`
- `refactor-legacy-react`

## What a Good SKILL.md Should Contain

Required sections:

1. **Frontmatter** - YAML metadata with `name` and `description`
2. **Use This Skill** - Clear when-to-use and when-not-to-use guidance
3. **Pre-Read Order** - What files to read before starting work
4. **Workflow** - Step-by-step process for the AI to follow
5. **Verification** - How to verify the work is correct

Optional but recommended:

- **Implementation Rules** - Specific coding standards or constraints
- **Output Contract** - Expected response format or deliverables
- **References** - Links to supporting docs in `references/` folder

Keep it practical and executable. The AI should be able to follow the instructions without asking clarifying questions.

## Maintenance Workflow

### Regular Updates

When you notice the AI making mistakes or missing context:

1. Identify which skill needs updating
2. Edit the `SKILL.md` with clearer instructions
3. Test the updated skill on a real task
4. Commit: `git commit -m "skill(ts-backend): clarify error handling rules"`

### Adding New Skills

When you find yourself repeating the same instructions:

1. Extract the pattern into a new skill
2. Follow the directory structure above
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

## Example Skill List

| Skill | Description | Status |
|-------|-------------|--------|
| [ts-backend-standard](./skills/ts-backend-standard/SKILL.md) | TypeScript backend with Hono, Zod, strict TS/ESLint | Active |
| [hurl-testing](./skills/hurl-testing/SKILL.md) | Hurl-based API testing patterns with lifecycle and contract testing | Active |
| [zx-script](./skills/zx-script/SKILL.md) | CLI automation scripts using zx with minimal dependencies | Active |

### Future Skill Ideas

- `go-microservice-standard` - Go microservice patterns
- `react-frontend-patterns` - React component and state management standards
- `db-migration-workflow` - Database migration best practices

## License

[MIT](LICENSE)
