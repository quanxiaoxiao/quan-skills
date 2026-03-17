# Doc Init / Refactor Prompt

Use this prompt to bootstrap or refactor documentation in a repository.

---

## Prompt

You are a documentation systems agent. Your task is to analyze this repository's documentation and restructure it into a five-layer governance model.

### Layer Model

| Layer | Location | Purpose |
|-------|----------|---------|
| 1 — Entry | `README.md` | Human entry point |
| 2 — Collaboration | `AGENTS.md` | Shared agent rules |
| 3 — System knowledge | `docs/` | Architecture, workflows, constraints |
| 4 — Tool guidance | `agents/` | Tool-specific instructions |
| 5 — Prompts | `prompts/` | Reusable maintenance prompts |

### Instructions

1. List every documentation file in this repository.
2. Classify each file by layer (1–5) or mark as `unclassified`.
3. Identify: duplications, missing layers, tool-coupled knowledge, outdated content.
4. Propose a target directory structure.
5. Generate a migration plan with actions: `keep`, `move`, `merge`, `split`, `rewrite`, `remove`, `create`.
6. Present the plan for approval before executing.

### Rules

- Project truth belongs in `docs/`, not in tool-specific files.
- Do not duplicate explanations across layers.
- Do not generate speculative documentation.
- Use cross-references instead of repeated content.

### Output Format

1. Assessment table: `File | Current Layer | Problems | Action Needed`
2. Target structure: annotated directory tree
3. Migration plan: `Source | Action | Target | Notes`
4. Wait for user approval before executing.
