# Prompt: Improve Existing Skill

Refine and improve an existing Codex skill.

## Context

- Skill to improve: {skill_path}
- Issue or improvement area: {improvement_area}

## Analysis

1. Read the current SKILL.md
2. Identify problems:
   - Unclear triggers (when to use the skill)
   - Missing steps in workflow
   - Vague instructions
   - Overly complex or ambiguous
   - Missing verification steps

3. Check if references/ docs need updates

## Improvements to Make

Apply these fixes:

1. **Clarify triggers**: Make "Use This Skill" section specific
2. **Fix workflow**: Ensure steps are actionable and ordered correctly
3. **Add constraints**: Include "Do not..." statements to prevent scope creep
4. **Verify completeness**: Check Pre-Read Order and Verification sections
5. **Remove fluff**: Delete generic advice, keep concrete instructions

## Output

Update SKILL.md with:
- Clearer description
- Better structured workflow
- Specific examples
- Improved maintainability

Commit message format: `skill({skill_name}): {brief_description_of_changes}`
