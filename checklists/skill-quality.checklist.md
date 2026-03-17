# Skill Quality Checklist

Use before committing a new or updated skill. Full authoring guide: [docs/skill-authoring.md](../docs/skill-authoring.md)

## Structure

- [ ] SKILL.md has YAML frontmatter (`name`, `description`)
- [ ] Has required sections: Use This Skill, Pre-Read Order, Workflow, Verification
- [ ] Directory follows kebab-case naming with tech or action prefix
- [ ] Under ~500 lines (detail moved to `references/`)

## Clarity

- [ ] Description triggers correctly in one sentence
- [ ] "Use This Skill" lists specific scenarios
- [ ] "When NOT to use" prevents scope creep
- [ ] Triggers don't overlap with other skills
- [ ] No ambiguous language ("usually", "often", "might")

## Workflow

- [ ] Steps numbered, ordered, each starts with a verb
- [ ] Pre-Read Order lists files in priority sequence
- [ ] No missing steps for common scenarios

## Verification

- [ ] Verification steps are specific and testable
- [ ] Describes expected outcomes and failure handling

## Governance

- [ ] No duplicated content from other skills
- [ ] `AGENTS.md` active skill table updated (if new skill)
- [ ] Commit message: `skill(name): description`
