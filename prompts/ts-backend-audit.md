# TS Backend Audit

> Authoritative prompt: `quan-skills:prompts/ts-backend-audit.md`
> Authoritative skill: `quan-skills:skills/ts-backend-standard/SKILL.md`
> Related rule: `quan-skills:rules/backend-functional-architecture.rule.md`
> Related checklist: `quan-skills:checklists/backend-architecture.checklist.md`

## Task

Audit a TypeScript backend repository against the `ts-backend-standard` skill baseline and produce a structured gap report.

## Steps

1. Read `README.md`, `package.json`, `tsconfig*.json`, `eslint.config.*`, and `src/` layout.
2. Load `quan-skills:skills/ts-backend-standard/SKILL.md` and relevant references.
3. Check layer separation: routes → schemas → services → models.
4. Check that services do not depend on Hono `Context` and routes do not access models directly.
5. Check functional style: no `class`, factory functions for services, composition over inheritance.
6. Check Zod validation boundaries: each endpoint defines a schema, validated data flows forward.
7. Check TypeScript strictness: `strict: true`, no blanket `any`, no `@ts-ignore` without justification.
8. Check ESLint config: strictest feasible rules enabled, no broad rule disabling.
9. Check dependency policy: runtime-first, no native/WASM unless justified.
10. Run `npm run typecheck`, `npm run lint`, `npm run build`, `npm run test` and record results.

## Output

Return a structured report:

1. **Stack Summary** — runtime, framework, key dependencies
2. **Layer Compliance** — routes / schemas / services / models separation
3. **Functional Style** — class usage, factory patterns, composition
4. **Validation** — Zod boundaries, schema coverage
5. **Strictness** — tsconfig and ESLint findings
6. **Dependencies** — policy violations
7. **Script Results** — pass/fail for each verification script
8. **Gaps** — prioritized list of findings with severity (high / medium / low)
9. **Recommendations** — ordered fix suggestions
