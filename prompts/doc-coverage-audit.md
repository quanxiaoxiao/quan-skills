# Prompt: Doc Coverage Audit

Apply the `doc-evidence-compare` skill in audit mode to check documentation coverage for a topic.

## Authoritative Source

Full workflow, search order, and output format defined in:
[skills/doc-evidence-compare/SKILL.md](../skills/doc-evidence-compare/SKILL.md)

## Context

- Topic: `{topic}`
- Doc roots: `{doc_roots}`
- Code roots: `{code_roots}`
- Coverage standard: `{coverage_standard}`

## Quick Reference

1. Read repo context before searching
2. Expand topic into aliases and implementation-oriented terms
3. Search doc roots first, then code roots (fall back to whole repo if not specified)
4. Build evidence matrix before making recommendations
5. Output: Coverage Summary → Evidence Matrix → Gap List → Recommendations

## Rules

- Every positive finding must include file references
- Use `not_found` only after searching both sides with aliases
- Distinguish `documented_only` from `implemented_only`
- Call out documentation drift explicitly when docs and code disagree
