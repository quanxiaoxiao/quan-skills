# Audit: scripts-global-constraints Remediation

Reusable audit baseline for validating the scripts-global governance structure.

---

## Files That Must Exist

| File | Purpose |
|------|---------|
| `rules/scripts-global.rule.md` | Authoritative constraints for `scripts/` |
| `checklists/scripts-global.checklist.md` | Grouped verification checklist |
| `prompts/create-or-review-script.md` | Task-oriented prompt for script creation/review |

## Files That Must Not Exist

| File | Reason |
|------|--------|
| `prompts/scripts-global-constraints.md` | Replaced by rule + checklist + task prompt split |

## Cross-Reference Checks

### AGENTS.md

- [ ] Contains a shared resource note directing `scripts/` work to `rules/scripts-global.rule.md` and `checklists/scripts-global.checklist.md`
- [ ] Does not contain bulky registration tables for individual rules/checklists

### SKILL.md

- [ ] Shared Repository Resources section mentions scripts rule/checklist
- [ ] Context-routing note exists for `scripts/` tasks

### skills/zx-script/SKILL.md

- [ ] Pre-Read Order includes `../../rules/scripts-global.rule.md`
- [ ] Pre-Read Order includes `../../checklists/scripts-global.checklist.md`
- [ ] States inheritance relationship from scripts-global

## Content Boundary Checks

### rules/scripts-global.rule.md

- [ ] Contains Relationship to Existing Rules section
- [ ] References: node-script-vs-app, node-runtime-first, node-dependency-policy, functional-only-js-ts
- [ ] Contains scope, hard constraints (1-5), E2E rules (6-14), design rules (15-21)
- [ ] Does not contain task execution steps, prompt wording, or checklist formatting
- [ ] Does not contain directory structure recommendations or file templates

### checklists/scripts-global.checklist.md

- [ ] Contains sections: Scope and Independence, Entry File and Header, Dependency and Interface Choice, Input/Output/Error Behavior, Safety and Mutation Controls, E2E Support Script Checks, Final Validation
- [ ] Uses grouped checkbox style consistent with other checklists
- [ ] Does not contain long prose or tutorial content

### prompts/create-or-review-script.md

- [ ] Is task-oriented (create or review mode)
- [ ] References rule and checklist in Pre-Read section
- [ ] Contains recommended directory structure and file template
- [ ] Contains What to Avoid quick reference
- [ ] Contains output format specification
- [ ] Does not duplicate rule content
- [ ] Does not duplicate checklist content
- [ ] Does not contain agent-specific execution defaults

## Duplication Check

- [ ] No rule text is copied verbatim into the prompt
- [ ] No checklist items are copied verbatim into the prompt
- [ ] The prompt references but does not redefine constraints

## Verification Commands

```bash
# Confirm file presence
ls rules/scripts-global.rule.md checklists/scripts-global.checklist.md prompts/create-or-review-script.md

# Confirm old file removed
test ! -f prompts/scripts-global-constraints.md && echo "OK: old file removed"

# Check cross-references
grep -l "scripts-global" AGENTS.md SKILL.md skills/zx-script/SKILL.md

# Check no rule duplication in prompt
grep -c "Do not import from" prompts/create-or-review-script.md  # should be 0
```
