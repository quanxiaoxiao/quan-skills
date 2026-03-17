# Documentation Migration Checklist

Use this checklist when executing a migration plan from Phase 3.

## Pre-Migration

- [ ] Assessment (Phase 1) is complete and reviewed
- [ ] Target structure (Phase 2) is approved by the user
- [ ] Migration plan (Phase 3) is approved by the user
- [ ] Current docs are committed to git (clean working tree)

## During Migration

- [ ] Executing actions in correct order: create → move → merge → split → rewrite → remove
- [ ] Each phase committed separately for rollback
- [ ] Cross-references updated after each move
- [ ] No file deleted that is the sole source of information

## Post-Migration

- [ ] All files map to exactly one layer
- [ ] `README.md` links are valid
- [ ] `AGENTS.md` references `docs/` correctly
- [ ] Tool-specific files reference `docs/` instead of explaining
- [ ] No broken internal links
- [ ] Doc audit checklist passes
- [ ] Migration log is complete: Source → Action → Target → Status
- [ ] Final state committed to git
