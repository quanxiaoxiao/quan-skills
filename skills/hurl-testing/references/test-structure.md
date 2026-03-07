# Test Structure

Use this reference when deciding what each Hurl file should cover and how to keep it rerunnable.

## Directory And File Layout

Preferred layout:

```text
tests/hurl/{tag}/
├── create.hurl
├── query.hurl
├── update.hurl
├── remove.hurl
└── [action].hurl
```

Rules:

- keep one operation family per file
- do not mix create, update, and delete into one catch-all script
- do not depend on execution order across files

## Required Coverage Layers

### Contract Layer

Every relevant file should assert:

- method and path behavior
- expected status codes
- required response fields
- declared error envelopes
- pagination envelope when applicable

### Lifecycle Layer

For resources with a lifecycle, cover the visible state flow:

- create then query
- create then update then query
- create then delete then verify invisibility if the contract requires it

### Resilience Layer

Cover the important negative paths:

- missing required fields
- wrong field types
- invalid enums
- not found cases
- duplicate or business-conflict cases when applicable

## Cleanup Rules

Every file that creates data must:

- capture the created identifier locally
- clean up the resource in the same file
- verify deletion visibility if the contract defines it

Avoid:

- hard-coded shared identifiers
- cross-file capture reuse
- assuming a clean database
- leaving cleanup failures unexamined

## Pagination Rules

When an endpoint supports pagination, cover at least:

- page 1 with explicit parameters
- page 2 or a second slice
- response envelope fields such as `items` and `total`
- negative cases for invalid limits or offsets when the contract defines them

Only assert ordering when the contract promises deterministic ordering.

## Assertion Scope

Good assertions:

- required JSON fields
- machine-readable error codes when documented
- count or limit bounds for paginated responses
- business-critical invariants visible to clients

Avoid:

- full payload snapshots unless the contract requires exact equality
- internal database fields
- undeclared debug fields
- assumptions about implementation-specific ordering
