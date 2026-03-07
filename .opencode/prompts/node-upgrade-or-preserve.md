# Node Upgrade or Preserve Prompt

When working on an existing Node.js project, decide whether the project should preserve its current standards or whether an upgrade is justified.

---

## Main Principle

For existing projects, preserve consistency unless there is a clear reason to upgrade.

Do not treat newer defaults as automatic migration targets.

---

## Evaluation Order

Evaluate in this order:

1. What runtime is explicitly declared?
2. Is `.nvmrc` present?
3. What package manager is in use?
4. What module system is in use?
5. Is the project currently stable?
6. Are there clear maintenance or compatibility problems?
7. Is the requested task asking for modernization?

---

## Preserve by Default When

Prefer preserving current standards if:

- `.nvmrc` exists and the project is consistent
- the project is stable and actively working
- the requested task is unrelated to modernization
- upgrading would increase risk without clear value

---

## Upgrade Only When

Recommend upgrade only if one or more are true:

- the user explicitly requested it
- the current runtime is unsupported or blocked
- dependencies require a newer runtime
- the project has unresolved tooling breakage
- consistency is already broken and cleanup is necessary

---

## Output Format

State clearly:

- what should be preserved
- what should be upgraded, if anything
- why the recommendation is low-risk or necessary
- what the smallest safe next step would be

---

## Goal

Avoid unnecessary churn while still allowing practical modernization when justified.
