# Search Workflow

Use this reference after loading the repository context and before writing conclusions.

## Search Order

1. Load repository context:
   - `AGENTS.md`
   - `README.md`
   - `docs/opencode-context.md` if present
   - obvious architecture or glossary docs
2. Expand the topic into aliases.
3. Search documentation sources first.
4. Search code sources second.
5. Inspect a small set of highest-signal hits instead of relying on raw grep output.
6. Classify each claim with evidence and confidence.

## Alias Expansion

For each topic, search with:

- the original term from the user
- Chinese and English synonyms
- plural or singular variants
- domain entity names
- field names and JSON keys
- endpoint names or route fragments
- service, model, and function style names

Example expansions:

- `支付退款关系` -> `refund`, `payment refund`, `refund status`, `refundId`, `refund_amount`
- `权限继承` -> `permission inheritance`, `inherit`, `role permission`, `parent role`
- `重试策略` -> `retry`, `backoff`, `retryable`, `maxAttempts`

## Default Search Targets

Search docs first:

- `docs/`
- `README*`
- `AGENTS.md`
- `openapi*`
- `schema*`

Then search code:

- `src/`
- `app/`
- `packages/`
- `services/`
- `tests/`

If the repository uses different roots, adapt to equivalent directories.

## Search Method

Prefer `rg` with focused patterns instead of vague broad browsing.

Typical search pattern flow:

1. search the primary term
2. search alias set
3. inspect the top matching files
4. search likely implementation names derived from the hits

Do not conclude `not_found` after one failed grep. Run the alias pass first.

## Evidence Rules

Treat evidence as one of:

- direct documentary evidence
- direct implementation evidence
- indirect evidence with clear inference

When evidence is indirect, label it as `inference` in the notes column.

## Status Rules

Use exactly one status per claim:

- `documented_and_implemented`: docs and code both support the claim
- `documented_only`: docs mention it, but implementation evidence was not found
- `implemented_only`: code supports it, but documentation evidence was not found
- `contradictory`: docs and code conflict in a meaningful way
- `not_found`: after searching docs and code with aliases, no evidence was found

## Confidence Guidance

Use simple qualitative confidence:

- `high`: direct doc and/or code evidence is strong and specific
- `medium`: evidence exists but needs some inference or is partial
- `low`: sparse evidence, partial naming match, or unresolved ambiguity

Never use `high` when the answer depends on weak inference.
