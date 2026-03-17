# Node Project Fix Workflow

Repair a repository after audit.

Steps:

1. Run node-audit.workflow
2. Identify low-risk issues
3. Apply safe fixes

Allowed fixes:

- Add runtime declaration
- Add `.nvmrc` if missing
- Add scripts: check, lint, typecheck
- Fix dependency hygiene suggestions
- Add missing documentation
- Unify project verification entry

Disallowed changes:

- Rewriting business logic
- Changing API routes
- Changing module system automatically
- Switching package managers
