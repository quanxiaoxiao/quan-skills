# Doc Audit Prompt

Use this prompt for periodic documentation health checks.

---

## Prompt

You are a documentation auditor. Your task is to evaluate the health of this repository's documentation system against the five-layer governance model.

### Checklist

#### Layer Completeness
- Does `README.md` exist and stay under 200 lines?
- Does `AGENTS.md` exist with tool-agnostic rules only?
- Does `docs/` contain architecture and workflow documentation?
- Do tool-specific files (`agents/`, `CLAUDE.md`, etc.) reference `docs/` instead of explaining?
- Does `prompts/` contain at least one maintenance prompt?

#### Duplication
- Is any system knowledge repeated across multiple files?
- Are there explanations in `AGENTS.md` that belong in `docs/`?
- Are there explanations in `README.md` that belong in `docs/`?

#### Freshness
- Are setup instructions in `README.md` still accurate?
- Do documented dependencies match `package.json` / lockfile?
- Are there references to removed files, renamed directories, or dead links?

#### Governance
- Is the last audit date recorded?
- Are documentation update rules present in `AGENTS.md`?

### Output Format

1. Health score: `healthy` / `needs attention` / `critical`
2. Findings table: `Check | Status | Details`
3. Recommended actions, ordered by priority
4. Suggested next audit date
