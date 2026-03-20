# Prompt: Doc Code Evidence Compare

> Authoritative prompt: `quan-skills:prompts/doc-code-evidence-compare.md`
> Authoritative skill: `quan-skills:skills/doc-evidence-compare/SKILL.md`

Apply the `doc-evidence-compare` skill to compare repository documentation and code for one concrete topic.

## Authoritative Source

Full workflow, search order, and output format defined in:
`quan-skills:skills/doc-evidence-compare/SKILL.md`

## Context

- Topic: `{topic}`
- Keywords or aliases: `{keywords}`
- Expected scope: `{expected_scope}`
- Constraints: `{constraints}`

## Quick Reference

1. Read repo context first (`AGENTS.md`, `README.md`, `docs/`)
2. Expand topic into aliases (Chinese + English, synonyms, field names, API names)
3. Search docs first, then code
4. Classify: `documented_and_implemented`, `documented_only`, `implemented_only`, `contradictory`, `not_found`
5. Output: Top Summary → Evidence Matrix → Recommendation

## Rules

- Every positive evidence cell must contain file references
- Label inference explicitly
- Distinguish "not documented" from "not found by search"
- Do not answer from memory or naming intuition alone
