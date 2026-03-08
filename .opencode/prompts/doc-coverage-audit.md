# Prompt: Doc Coverage Audit

Use `$doc-evidence-compare` to audit whether documentation sufficiently covers implemented behavior for a topic.

## Context

- Topic: `{topic}`
- Doc roots: `{doc_roots}`
- Code roots: `{code_roots}`
- Coverage standard: `{coverage_standard}`

## Objective

Inspect the repository and determine:

- which parts of the topic are documented
- which parts are only implemented in code
- whether docs and implementation disagree
- where documentation coverage is missing or drifting

## Required Workflow

1. Apply `$doc-evidence-compare`.
2. Read repo context before searching.
3. Expand the topic into aliases and implementation-oriented terms.
4. Search the provided doc roots first, then the provided code roots.
5. If roots are empty or not provided, fall back to whole-repo `docs + code`.
6. Build the evidence matrix before making recommendations.

## Output Format

Return:

1. a short coverage summary
2. the evidence matrix with columns:
   - `Claim / 问题点`
   - `Doc Evidence`
   - `Code Evidence`
   - `Status`
   - `Gap / Notes`
   - `Confidence`
3. a short gap list
4. a short recommendation block for what documentation should be added, corrected, or split

## Rules

- every positive finding must include file references
- use `not_found` only after searching both sides with aliases
- distinguish `documented_only` from `implemented_only`
- call out documentation drift explicitly when docs and code disagree
