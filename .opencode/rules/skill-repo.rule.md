# Rule: Skill Repository Structure

Applies to: `~/.codex/skills/quan-skills/`

## Directory Layout

```
quan-skills/
├── README.md                    # Repository overview
├── LICENSE                      # MIT license
├── .opencode/                   # Maintenance tools
│   ├── prompts/                 # Skill generation prompts
│   ├── rules/                   # Repository rules (this file)
│   └── checklists/              # Quality checklists
└── skills/                      # All skills live here
    ├── ts-backend-standard/     # Skill directory
    │   ├── SKILL.md             # Main skill definition
    │   ├── agents/              # Agent configs (optional)
    │   └── references/          # Reference docs (optional)
    └── <skill-name>/            # Other skills follow same pattern
        └── SKILL.md
```

## Constraints

1. **One SKILL.md per skill**: All skill logic in a single file
2. **Top-level skill directories**: No nested skill folders
3. **Naming**: Use kebab-case (e.g., `ts-backend-standard`)
4. **No duplication**: Reference docs in `references/` folder, don't paste into SKILL.md

## SKILL.md Structure

Required frontmatter:
```yaml
---
name: skill-name
description: One-line description of what this skill does and when to use it.
---
```

Required sections:
1. `Use This Skill` - When to use and when NOT to use
2. `Pre-Read Order` - Files to read before starting
3. `Workflow` - Step-by-step process
4. `Verification` - How to verify the work

Optional sections:
- `Implementation Rules` - Specific constraints
- `Output Contract` - Expected response format

## Maintenance

- Keep skills minimal and focused
- One skill = one responsibility
- Update README.md when adding/removing skills
- Use conventional commits: `skill(name): description`
