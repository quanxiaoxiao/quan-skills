# Doc Evidence Compare Checklist

Before finalizing a doc-vs-code evidence comparison, verify:

## Search Coverage

- [ ] Both documentation and code directories were searched
- [ ] Chinese and English terms, synonyms, and aliases were included in search queries
- [ ] Entity names, field names, API paths, and implementation identifiers were checked
- [ ] `docs/`, `README*`, `AGENTS.md`, schema files, and OpenAPI specs were covered on the doc side
- [ ] `src/`, `app/`, `packages/`, `services/`, and `tests/` were covered on the code side

## Evidence Quality

- [ ] Every positive claim includes at least one file reference
- [ ] Direct evidence is distinguished from inference
- [ ] `not_found` means searched-but-absent, not assumed-absent
- [ ] No claim relies solely on filename matching without reading content

## Status Classification

- [ ] Each claim uses exactly one status: `documented_and_implemented`, `documented_only`, `implemented_only`, `contradictory`, or `not_found`
- [ ] `contradictory` is used when doc and code disagree on behavior, not just when wording differs
- [ ] `documented_only` and `implemented_only` are kept separate (not collapsed into `not_found`)

## Confidence Rating

- [ ] Each claim has a confidence level: `high`, `medium`, or `low`
- [ ] `high` requires direct file evidence on both doc and code sides
- [ ] `low` is assigned when the conclusion relies on inference or partial matches

## Output Format

- [ ] Evidence matrix includes columns: Claim, Doc Evidence, Code Evidence, Status, Gap/Notes, Confidence
- [ ] Top summary appears before the matrix
- [ ] Recommendation block appears after the matrix
