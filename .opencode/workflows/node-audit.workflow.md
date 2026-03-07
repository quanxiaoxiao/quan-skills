# Node Project Audit Workflow

Audit a Node.js repository using quan-skills standards.

Steps:

1. Load global rules
2. Run node-project-audit prompt
3. Apply node-project-audit-checklist
4. Detect:
   - Runtime
   - Package manager
   - Module system
   - Dependency hygiene
   - Verification coverage
5. Produce an audit report

Output sections:

- Project Type
- Runtime
- Package Manager
- Module System
- Dependency Hygiene
- Verification Baseline
- CI Status
- Consistency Issues
- Risk Notes
- Suggested Actions

Constraints:

- Respect `.nvmrc` if present
- Do not change package manager
- Do not rewrite business logic
