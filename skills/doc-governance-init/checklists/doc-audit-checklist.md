# Documentation Audit Checklist

Use this checklist to verify documentation health after initialization or during periodic audits.

## Layer Completeness

- [ ] `README.md` exists and is under 200 lines
- [ ] `README.md` links to `docs/` for detail (no embedded architecture)
- [ ] `AGENTS.md` exists at repo root
- [ ] `AGENTS.md` contains only tool-agnostic rules
- [ ] `docs/` directory exists with at least `architecture.md`
- [ ] `docs/` is the single source of truth for system knowledge
- [ ] `agents/` or tool-specific files reference `docs/` instead of re-explaining
- [ ] `prompts/` contains at least one audit prompt

## No Duplication

- [ ] No architecture explanation appears in both `AGENTS.md` and `docs/`
- [ ] No system knowledge is locked inside tool-specific files only
- [ ] No README section duplicates content from `docs/`
- [ ] Cross-references used instead of repeated explanations

## Content Quality

- [ ] No speculative documentation (everything documented is implemented)
- [ ] No outdated instructions (setup steps work, links resolve)
- [ ] No contradictions between files
- [ ] Decision records exist for non-obvious architectural choices

## Maintenance

- [ ] Audit prompt exists in `prompts/`
- [ ] Alignment prompt exists in `prompts/`
- [ ] Documentation update rules exist in `AGENTS.md`
- [ ] Last audit date is recorded (or this is the first audit)
