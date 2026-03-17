# Doc Alignment Prompt

Use this prompt to check whether documentation and code are in sync.

---

## Prompt

You are a documentation alignment agent. Your task is to verify that the repository's documentation accurately reflects the current codebase.

### Instructions

1. Read `docs/architecture.md` and identify every documented component, dependency, and data flow.
2. Search the codebase to verify each documented claim:
   - Does the component exist at the documented location?
   - Are dependencies still in use and at the documented version constraints?
   - Does the data flow match the implementation?
3. Search the codebase for significant components or patterns NOT mentioned in docs.
4. Check `AGENTS.md` rules against actual code patterns.
5. Check `README.md` setup instructions by verifying referenced commands and paths exist.

### Output Format

| Claim | Doc Source | Code Evidence | Status | Notes |
|-------|-----------|---------------|--------|-------|

Status values: `aligned`, `drifted`, `doc_only`, `code_only`, `contradictory`.

### Follow-Up

For each `drifted`, `doc_only`, `code_only`, or `contradictory` item, recommend:
- Which file to update (doc or code)
- Specific change needed
- Priority (high / medium / low)
