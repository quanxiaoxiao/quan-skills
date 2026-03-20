# Prompt: Create or Review Script

Create a new script or review an existing script under `scripts/`.

---

## When to Use

- creating a new standalone Node.js script
- reviewing or refactoring an existing script under `scripts/`
- adding E2E support scripts (setup, seed, reset, probe, verify, cleanup, orchestrate)
- converting inline automation into a proper script file

---

## Pre-Read

Before starting, read:

1. `rules/scripts-global.rule.md` — authoritative constraints for all script work
2. `checklists/scripts-global.checklist.md` — verification checklist

---

## Inputs

- **Target path**: where the script will live (e.g. `scripts/ops/check-health.mjs`)
- **Mode**: create or review
- **Purpose**: what the script should do
- **Runtime constraints**: Node version, existing dependencies, project conventions

---

## Steps

### Create mode

1. Inspect existing patterns under `scripts/` for local conventions
2. Choose the appropriate category directory (or create one if justified)
3. Write the script following the rule file:
   - `.mjs` extension
   - mandatory header comment with Purpose, Usage, Arguments, Examples, Notes
   - no imports from `src/`
   - prefer Node.js built-ins, then HTTP, then CLI, then local helpers, then dev deps
   - function-oriented structure: parseArgs → validate → run → main + catch
   - explicit error handling with non-zero exit codes
   - `--dry-run` for mutation scripts
4. Run the checklist before finishing

### Review mode

1. Read the target script
2. Walk through every section of the checklist
3. Flag violations with the specific rule number
4. Suggest minimal fixes for each violation
5. Verify the script still runs after suggested changes

---

## Recommended Directory Structure

```
scripts/
├── audit/
├── ops/
├── dev/
├── e2e/
│   ├── setup/
│   ├── probe/
│   ├── seed/
│   ├── verify/
│   ├── cleanup/
│   └── orchestrate/
└── lib/
```

---

## Recommended File Template

```js
/**
 * Purpose:
 *   Briefly describe what this script does.
 *
 * Usage:
 *   node scripts/example.mjs --foo bar
 *
 * Arguments:
 *   --foo <value>    Description of foo
 *   --dry-run        Run without applying changes
 *
 * Examples:
 *   node scripts/example.mjs --foo bar
 *   node scripts/example.mjs --foo bar --dry-run
 *
 * Notes:
 *   - This script does not import from src/.
 *   - This script interacts with the project only through HTTP APIs.
 */

import process from 'node:process';

function parseArgs(argv) {
  // explicit parsing
}

async function run(options) {
  // main logic
}

async function main() {
  const options = parseArgs(process.argv.slice(2));
  await run(options);
}

main().catch((error) => {
  console.error('[script:error]', error?.stack || error?.message || String(error));
  process.exit(1);
});
```

---

## What to Avoid

- import from `src/`
- couple scripts to internal runtime implementation details
- introduce unnecessary third-party dependencies
- put script-only packages in production dependencies
- omit usage comments at the top of the file
- hide network targets or operational behavior
- make scripts depend on large framework abstractions
- convert simple scripts into overbuilt architectures
- use E2E support scripts to bypass intended interfaces
- mix setup, business assertions, and cleanup into one unclear script

---

## Output Format

1. **Summary** — what was created or reviewed
2. **Files changed or audited** — list with paths
3. **Checklist result** — pass/fail per section, with rule numbers for failures
4. **Verification** — how to test the script
5. **Residual risks** — anything not addressed
