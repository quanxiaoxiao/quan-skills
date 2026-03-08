# Prompt: Doc Code Evidence Compare

Use `$doc-evidence-compare` to compare repository documentation and code for one concrete topic.

## Context

- Topic: `{topic}`
- Keywords or aliases: `{keywords}`
- Expected scope: `{expected_scope}`
- Constraints: `{constraints}`

## Objective

Determine whether the topic is:

- documented
- implemented
- both documented and implemented consistently
- missing on one side
- contradictory between docs and code

## Required Workflow

1. Apply `$doc-evidence-compare`.
2. Read repository context first:
   - `AGENTS.md` if present
   - `README.md`
   - `docs/opencode-context.md` if present
3. Expand the topic into aliases before searching.
4. Search docs first, then code.
5. Do not conclude absence until both searches are complete.
6. Classify each claim using:
   - `documented_and_implemented`
   - `documented_only`
   - `implemented_only`
   - `contradictory`
   - `not_found`

## Output Format

Return:

1. a short top summary
2. the evidence matrix with columns:
   - `Claim / 问题点`
   - `Doc Evidence`
   - `Code Evidence`
   - `Status`
   - `Gap / Notes`
   - `Confidence`
3. a short recommendation block

## Rules

- every positive evidence cell must contain file references
- label inference explicitly
- distinguish `文档没写` from `我没找到`
- do not answer from memory or naming intuition alone
