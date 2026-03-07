# Node Project Audit Checklist

Use this checklist when auditing an existing Node.js project.

---

## 1. Runtime

- Is there a `.nvmrc` file?
- Is there a `.node-version` file?
- Is `package.json.engines.node` defined?
- Does CI pin a Node.js version?
- Do docs mention the runtime version?
- Do declared runtime sources conflict with each other?
- If `.nvmrc` exists, is it being treated as the primary version source?

---

## 2. Package Manager

- Which lockfile exists?
- Is there exactly one lockfile?
- Does the project appear to mix package managers?
- Are installation instructions consistent with the lockfile?

---

## 3. Module System

- Is the project ESM or CommonJS?
- Is `package.json.type` defined?
- Is TypeScript module output aligned with the runtime?
- Are ESM and CommonJS mixed in a confusing way?
- Are file extensions consistent with the chosen module system?

---

## 4. Dependency Hygiene

- Are there dependencies duplicating built-in Node.js functionality?
- Are there native or `node-gyp` dependencies?
- Are there heavy dependencies with low value?
- Are there obvious "avoid for new code" packages?
- Should any package be classified as:
  - keep
  - review
  - avoid for new code

---

## 5. Project Scripts

- Is there a `dev` script?
- Is there a `start` script?
- Is there a `lint` script?
- Is there a `typecheck` script?
- Is there a `test` script?
- Is there a `check` script?
- Is there a `fix` script?
- Do the scripts reflect the real verification workflow?

---

## 6. Verification

- Is linting configured?
- Is type checking configured?
- Are automated tests present?
- Are integration tests present where needed?
- Are API contract tests present where needed?
- Does CI run verification automatically?
- Is verification complete, partial, weak, or missing?

---

## 7. Structure and Boundaries

- Is the repository mainly a script/tool repo or an app repo?
- Are directories reasonably organized?
- Are transport, validation, and business logic separated where relevant?
- Are utilities and shared helpers placed clearly?
- Is the repository easy to navigate?

---

## 8. Error Handling

- Are errors handled consistently?
- Are machine-readable errors stable where needed?
- Is there evidence of swallowed errors?
- Are CLI errors human-readable?
- Are API errors structured consistently?

---

## 9. Documentation and Hygiene

- Is there a README?
- Are setup instructions present?
- Is `.gitignore` present?
- Is `.editorconfig` present?
- Is `.env.example` present when relevant?
- Is the runtime or package manager documented?
- Are maintenance expectations clear?

---

## 10. Risk Notes

- Are there high-risk areas with weak verification?
- Are runtime declarations inconsistent?
- Is the package manager unclear?
- Is module system usage mixed?
- Are there obvious opportunities for low-risk improvements?
- What should be done first with the least behavioral risk?

---

## Audit Summary Template

Summarize the project using:

- Runtime
- Package manager
- Module system
- Verification baseline
- Dependency hygiene
- Structure notes
- CI status
- Consistency issues
- Risk notes
- Suggested next actions
