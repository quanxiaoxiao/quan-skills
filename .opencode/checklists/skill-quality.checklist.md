# Skill Quality Checklist

Use this checklist before committing a new or updated skill.

## Clarity

- [ ] Skill name is descriptive (kebab-case)
- [ ] Description explains when to use the skill in one sentence
- [ ] "Use This Skill" section lists specific scenarios
- [ ] "When NOT to use" is clear to prevent scope creep
- [ ] No ambiguous language (avoid "usually", "often", "might")

## Trigger Quality

- [ ] Description is specific enough to trigger correctly
- [ ] Triggers don't overlap with other skills
- [ ] Examples are concrete, not generic
- [ ] Covers both positive triggers (use when...) and negative (don't use when...)

## Workflow

- [ ] Steps are numbered and ordered correctly
- [ ] Each step is actionable (starts with verb)
- [ ] Pre-Read Order lists files in priority sequence
- [ ] Dependencies between steps are clear
- [ ] No missing steps for common scenarios

## Verification

- [ ] Verification steps are specific and testable
- [ ] Includes commands to run (e.g., `npm run typecheck`)
- [ ] Describes expected outcomes
- [ ] Mentions what to do if verification fails

## Maintainability

- [ ] Skill is focused on one responsibility
- [ ] No duplicated content from other skills
- [ ] References/ folder used for lengthy docs
- [ ] Can be understood without domain expertise
- [ ] Easy to update without breaking existing behavior

## Final Check

- [ ] SKILL.md follows frontmatter format
- [ ] Directory structure follows conventions
- [ ] README.md skill list is updated (if new skill)
- [ ] Commit message follows convention: `skill(name): description`

## Common Issues to Fix

1. **Too generic**: Add specific tech stack or context
2. **Missing negatives**: Add "When NOT to use" section
3. **Vague steps**: Replace with concrete commands or actions
4. **Missing verification**: Add clear success criteria
5. **Scope creep**: Split into multiple skills if doing too much
