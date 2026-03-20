# Scripts Global Checklist

Verification checklist for creating or reviewing code under `scripts/`.

Reference rule: `rules/scripts-global.rule.md`

---

## 1. Scope and Independence

* [ ] No imports from `src/` (direct or indirect)
* [ ] No reuse of runtime business logic, types, constants, or internal helpers from `src/`
* [ ] Project state obtained only through external surfaces (HTTP, CLI, env vars, script-local helpers)

## 2. Entry File and Header

* [ ] Uses `.mjs` extension
* [ ] Directly executable as `node scripts/xxx.mjs`
* [ ] Header comment includes: Purpose, Usage, Arguments, Examples, Notes

## 3. Dependency and Interface Choice

* [ ] Prefers Node.js built-in modules
* [ ] Extra dependencies are truly necessary and justified
* [ ] Script-only dependencies placed in `devDependencies`, not production
* [ ] Prefers HTTP/external interfaces over internal code reuse
* [ ] Interaction preference order followed: built-ins > HTTP > CLI > local helpers > dev deps

## 4. Input, Output, and Error Behavior

* [ ] Argument parsing is simple and explicit (`process.argv` or lightweight helper)
* [ ] Config precedence is clear: CLI args > env vars > internal defaults
* [ ] Inputs validated early with actionable error messages
* [ ] Non-zero exit code on failure
* [ ] Output suitable for terminal reading, file redirection, and model inspection
* [ ] Machine-friendly output available when needed (`--json`, `--output`)

## 5. Safety and Mutation Controls

* [ ] Mutation scripts support `--dry-run` or equivalent safe mode
* [ ] Destructive actions clearly named and signaled
* [ ] Inspection, planning, and execution separated when reasonable
* [ ] Idempotent or safe to re-run, or assumptions documented in header

## 6. E2E Support Script Checks

Skip this section if the script is not E2E-related.

* [ ] Behaves like an external support tool, not internal app code
* [ ] Uses explicit health/reset/seed/admin/test interfaces (black-box or gray-box)
* [ ] Does not contain hidden runtime business logic
* [ ] Responsibilities are focused (not mixing setup + verify + cleanup)
* [ ] Timeout/polling behaviors bounded and explicit
* [ ] Destructive actions require explicit confirmation or flags
* [ ] CI or other tools can consume output
* [ ] Config inputs exposed via CLI args and/or documented env vars

## 7. Final Validation

* [ ] Script is understandable from its own file and local helpers
* [ ] No hidden coupling, magic defaults, or obscure execution paths
* [ ] Function-oriented structure (not overengineered into a framework)
* [ ] Shared helpers live under `scripts/`, not `src/`
* [ ] Fails clearly and predictably
