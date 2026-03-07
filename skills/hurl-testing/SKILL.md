---
name: hurl-testing
description: Guidelines for generating, maintaining, and repairing Hurl tests for HTTP APIs. Covers contract testing, lifecycle management, cleanup discipline, and incremental maintenance from OpenAPI specs.
---

# Hurl Testing Framework - Complete Contract and Lifecycle Guidelines

## Purpose

This document defines the rules for generating, maintaining, and repairing Hurl tests for HTTP APIs.

Hurl tests are external contract tests. They must validate observable API behavior without relying on internal implementation details.

This framework is intended to support:

- Hurl generation from OpenAPI
- lifecycle-aware API contract testing
- incremental maintenance when APIs evolve
- stable cleanup and rerunnable execution
- repair discipline when tests fail
- regression coverage after bug fixes

---

## Use This Skill

Use this skill when:

- Generating Hurl tests from an OpenAPI specification
- Writing new Hurl tests for HTTP API endpoints
- Repairing failing Hurl tests after API changes
- Adding regression tests for API bug fixes
- Refactoring Hurl tests for better maintainability
- Reviewing Hurl test coverage for completeness

---

## When NOT to use

Do NOT use this skill when:

- The task involves internal implementation testing (unit tests, database tests)
- Testing non-HTTP protocols (gRPC, WebSocket, GraphQL subscriptions)
- The API has no documented contract or OpenAPI spec
- The testing requires complex UI interactions or browser automation
- Load testing or performance benchmarking is the primary goal
- The project does not use Hurl (use the appropriate testing framework instead)

---

## Pre-Read Order

Before generating or repairing Hurl tests, read these sources in order:

1. `openapi.json` or `openapi.yaml` - The API contract specification
2. Existing Hurl tests in `tests/hurl/` directory
3. API documentation or README for business rules
4. `.opencode/rules/hurl-testing.rule.md` - Repository Hurl testing rules
5. `.opencode/checklists/hurl-quality.checklist.md` - Quality verification checklist

---

# 1. Core Principles

## 1.1 External Behavior Contracts Only

Hurl files define external behavior contracts, not internal implementation details.

### Must NOT assume
- internal architecture
- database structure
- service names
- module names
- storage strategy
- implementation shortcuts
- internal state fields unless explicitly exposed by contract

### Must define
- endpoints and HTTP methods
- request shape
- expected status codes
- response payload shape
- required response fields
- declared error scenarios
- resource lifecycle expectations visible at transport layer

---

## 1.2 Contract-First Testing

The purpose of Hurl is to verify what the API promises to callers.

Hurl tests must be derived from:
1. OpenAPI specification
2. explicit business rules exposed at API level
3. stable transport behavior
4. documented lifecycle expectations

Hurl tests must NOT drift into implementation testing.

---

## 1.3 Source of Truth Priority

When behavior and tests disagree, use this order of verification:

1. OpenAPI specification
2. business contract / documented API rules
3. actual server behavior
4. Hurl assertions

Never weaken a valid contract test just to make a broken implementation pass.

---

## 1.4 Strict but Useful Assertions

Hurl tests should be strict enough to catch contract regressions, but must not assert unspecified behavior.

### Good assertions
- status code
- required fields
- required top-level structure
- declared error envelope
- business-critical changes
- deletion visibility rules
- pagination structure
- deterministic ordering requirements where relevant

### Bad assertions
- internal-only fields
- undeclared debug fields
- exact full payload snapshots when contract only guarantees partial shape
- assumptions about storage ordering unless explicitly declared
- assumptions about internal IDs or timestamps beyond contract

---

# 2. Test Layer Model

Hurl coverage should be organized into three conceptual layers.

## 2.1 Contract Layer
Verifies baseline HTTP contract:
- method
- path
- status code
- request validation
- response shape
- required fields
- declared error responses
- pagination envelope

## 2.2 Lifecycle Layer
Verifies externally visible resource state flow:
- create → query
- create → update → query
- create → remove → query 404
- create → action → query
- invalid request → no unintended resource creation

## 2.3 Resilience Layer
Verifies robustness and negative scenarios:
- invalid payload
- missing required fields
- wrong types
- not found
- duplicate creation
- repeated delete
- invalid state transition
- business conflicts
- auth errors if applicable

All three layers matter. Do not generate success-only tests.

---

# 3. File Organization

## 3.1 Directory Structure

Each OpenAPI tag maps to its own Hurl directory.

```text
tests/hurl/{tag}/
├── create.hurl
├── query.hurl
├── update.hurl
├── remove.hurl
└── [action].hurl
```

Examples of action files:

* reorder.hurl
* move.hurl
* publish.hurl
* archive.hurl
* restore.hurl

---

## 3.2 File Naming Rules

### Mapping

| HTTP Method            | Operation Type | File          |
| ---------------------- | -------------- | ------------- |
| POST                   | create         | create.hurl   |
| GET (list)             | query          | query.hurl    |
| GET (detail)           | query          | query.hurl    |
| PATCH / PUT            | update         | update.hurl   |
| DELETE                 | remove         | remove.hurl   |
| custom business action | action         | [action].hurl |

---

## 3.3 One Operation Family per File

Each file must focus on one operation family.

### Allowed

* `create.hurl` contains create success + create validation + create conflict cases
* `query.hurl` contains list/detail/filter/pagination/query-not-found cases
* `update.hurl` contains update success + update validation + update conflict + update-not-found cases
* `remove.hurl` contains delete success + repeated delete + delete-not-found + post-delete verification
* `[action].hurl` contains one specific business action and its valid/invalid variants

### Forbidden

* mixing unrelated actions in one file
* putting create/update/remove all in one file
* using one file as a catch-all integration script
* relying on execution order across files

---

# 4. Resource Lifecycle State Model

Hurl tests must reflect external lifecycle rules.

## 4.1 Canonical External States

At transport-contract level, resources usually behave as:

* `nonexistent`
* `active`
* `updated`
* `deleted` or `invalidated`

Actual internal implementation may differ, but Hurl should only test externally visible state.

---

## 4.2 Allowed Transitions

Typical allowed transitions:

* nonexistent → active
* active → updated
* active → deleted
* updated → updated
* updated → deleted

---

## 4.3 Forbidden Transitions

Unless explicitly declared by contract, these should be treated as invalid:

* deleted → updated
* deleted → active
* deleted → deleted with inconsistent behavior
* nonexistent → updated
* nonexistent → deleted with unexpected success
* invalidated resource still accessible via normal detail endpoints

---

## 4.4 Transport-Level Deletion Rule

Even if soft-delete is used internally, transport-layer behavior must follow the external contract.

If a deleted resource is not supposed to be visible anymore, normal query endpoints must return 404.

Example expectation:

* resource created
* resource deleted or invalidated
* normal GET by id returns 404

Do not expose internal soft-delete semantics unless the API contract explicitly declares them.

---

# 5. Test Lifecycle and Cleanup

## 5.1 Clean-as-You-Go Principle

Every resource created during a test must be cleaned before the test ends.

Canonical sequence:

1. create resource
2. perform target operation(s)
3. clean up resource
4. verify post-cleanup transport behavior

---

## 5.2 Mandatory Lifecycle Pattern

### Step 1: Resource Creation

```hurl
POST {{base_url}}/api/resources
Content-Type: application/json

{
  "name": "hurl-test-{{$timestamp}}"
}

HTTP 201
[Captures]
resource_id: jsonpath "$._id"
```

### Step 2: Test Operations

```hurl
GET {{base_url}}/api/resources/{{resource_id}}
HTTP 200
```

### Step 3: Mandatory Cleanup

```hurl
DELETE {{base_url}}/api/resources/{{resource_id}}
HTTP 204
```

### Step 4: 404 Verification

```hurl
GET {{base_url}}/api/resources/{{resource_id}}
HTTP 404
```

---

## 5.3 Cleanup Is Mandatory

Cleanup is not optional.

### Required

* every created resource must be removed or invalidated by the end of the file
* deletion must be asserted
* cleanup must happen in the same file where the resource is created
* cleanup failure is a real test failure

### Forbidden

* leaving test-created resources behind
* assuming CI environment will be reset later
* relying on manual cleanup
* silently ignoring cleanup failures

---

## 5.4 Cleanup Failure Handling

If cleanup fails:

* the Hurl test is considered failed
* the failure must not be hidden
* the contract must be preserved
* any repair should strengthen cleanup discipline rather than bypass it

---

# 6. Test Data Lifecycle Rules

## 6.1 Unique Test Data

Test data must be uniquely identifiable to avoid collisions across runs.

### Preferred patterns

* timestamp suffix
* random suffix if needed
* clearly marked test prefixes

Example:

```hurl
POST {{base_url}}/api/resources
Content-Type: application/json

{
  "name": "hurl-test-{{$timestamp}}",
  "alias": "alias-{{$timestamp}}"
}
```

---

## 6.2 Test Data Rules

### Required

* test data must be short-lived
* test data must be self-contained
* test data must be created in the same file where it is used
* test data must be cleaned in the same file where it is created
* test data must not depend on mutable shared state

### Forbidden

* hard-coded shared IDs
* assuming database is empty
* depending on leftovers from previous runs
* depending on another Hurl file to prepare state
* depending on execution order between files

---

## 6.3 Isolation Rule

Each Hurl file must be independently executable.

A file must not require another file to run first.

---

# 7. Generation from OpenAPI

## 7.1 OpenAPI as Source of Truth

The OpenAPI specification (`openapi.json`) is the primary source of truth for generating Hurl tests.

Generated Hurl must reflect:

* declared paths
* declared methods
* declared request schemas
* declared response schemas
* declared status codes
* declared error responses
* declared pagination contracts

---

## 7.2 Operation Mapping

| HTTP Method  | Operation Type | File        |
| ------------ | -------------- | ----------- |
| POST         | create         | create.hurl |
| GET (list)   | query          | query.hurl  |
| GET (detail) | query          | query.hurl  |
| PATCH / PUT  | update         | update.hurl |
| DELETE       | remove         | remove.hurl |

Custom business operations should map to dedicated action files.

---

## 7.3 Generated Assertions Must Verify

### Success cases

* status code
* required fields
* identifier presence
* envelope shape
* declared business-critical fields
* pagination structure if applicable

### Error cases

* declared status code
* declared error envelope
* declared machine-readable code if available
* stable error shape

---

## 7.4 Forbidden Generation Behavior

Generated Hurl must NOT:

* assert fields not declared by schema or contract
* invent business rules not declared anywhere
* overwrite manually written tests
* delete user-authored scenarios
* weaken valid assertions to match current server bugs

---

# 8. Pagination Testing

## 8.1 Detection

Treat an endpoint as paginated when query parameters include pagination controls such as:

* `limit`
* `offset`

If another pagination style exists, follow the documented contract.

---

## 8.2 Mandatory Pagination Coverage

### Page 1

```hurl
GET {{base_url}}/api/resources?limit=2&offset=0
HTTP 200
[Asserts]
jsonpath "$.items" exists
jsonpath "$.total" exists
jsonpath "$.items" count <= 2
```

### Page 2

```hurl
GET {{base_url}}/api/resources?limit=2&offset=2
HTTP 200
[Asserts]
jsonpath "$.items" exists
jsonpath "$.total" exists
```

---

## 8.3 Stable Ordering Requirement

Offset-based pagination requires deterministic ordering.

If the contract depends on offset pagination, Hurl must verify stable ordering assumptions.

Example:

```hurl
GET {{base_url}}/api/resources?limit=2&offset=0
HTTP 200
[Captures]
last_id: jsonpath "$.items[-1]._id"

GET {{base_url}}/api/resources?limit=2&offset=2
HTTP 200
[Asserts]
jsonpath "$.items[0]._id" != "{{last_id}}"
```

Do not assume stable ordering without explicit or implied contract support.

If ordering is required for pagination correctness, the API contract must support deterministic ordering.

---

## 8.4 Pagination Negative Cases

Also test invalid pagination inputs when applicable:

* negative offset
* zero or invalid limit
* wrong query type
* out-of-range values if the contract constrains them

These must produce declared 4xx behavior, not 500.

---

# 9. Error Taxonomy

Hurl negative coverage should distinguish between error categories.

## 9.1 Validation Errors

Request structure is invalid.

Examples:

* missing required field
* wrong field type
* malformed JSON
* invalid enum
* invalid query parameter format

Expected:

* 4xx
* stable error envelope
* no resource side effects

---

## 9.2 Business Rule Errors

Request shape is valid, but business constraints are violated.

Examples:

* duplicate alias
* cannot delete default resource
* cannot mark multiple resources as default
* invalid reorder target
* illegal business transition

Expected:

* declared 4xx such as 409 or 422
* stable error envelope
* no unintended state mutation

---

## 9.3 Not Found Errors

Target resource does not exist.

Examples:

* query missing id
* update missing id
* delete missing id
* action on missing id

Expected:

* 404
* stable error envelope
* never 500 for normal missing-resource cases

---

## 9.4 Conflict and Idempotency Errors

Repeated or conflicting operations.

Examples:

* duplicate create
* repeated delete
* repeated publish
* repeated archive
* action applied to already-invalid state

Expected:

* explicitly defined success or explicitly defined conflict
* behavior must be stable across repeated requests

---

## 9.5 Auth or Access Errors

If the API includes authentication/authorization.

Examples:

* missing token
* invalid token
* expired token
* forbidden role

Expected:

* 401 or 403 as declared
* no internal details leaked

---

# 10. File Scope Rules

## 10.1 create.hurl Must Cover

* create success
* create response contract
* create validation failure
* duplicate create or business conflict if applicable

## 10.2 query.hurl Must Cover

* list success
* detail success
* filters
* pagination
* sorting if contract exposes it
* query deleted or missing resource

## 10.3 update.hurl Must Cover

* update success
* update response contract
* invalid update payload
* update missing resource
* update business conflict if applicable

## 10.4 remove.hurl Must Cover

* remove success
* post-delete verification
* repeated remove behavior
* remove missing resource
* transport-level invisibility after delete

## 10.5 [action].hurl Must Cover

* one specific action only
* valid action execution
* invalid action payload
* action-not-found
* illegal action state transition if applicable

---

# 11. Assertion Strength Rules

## 11.1 Minimum Success Assertions

Every success test should assert at least:

* HTTP status code
* required top-level structure
* required identifier or key fields
* fields necessary to prove the contract outcome

Examples:

* `_id` exists after create
* updated field changed after update
* deleted resource returns 404 after remove
* `items` and `total` exist for paginated list

---

## 11.2 Minimum Error Assertions

Every error test should assert at least:

* HTTP status code
* error envelope exists
* machine-readable error code exists if contract declares it
* stable error shape

Do not rely only on the status code unless the contract intentionally defines no body.

---

## 11.3 Forbidden Weak Assertions

Do NOT:

* assert only `HTTP 200` with no body validation
* assert only `HTTP 4xx` with no error shape validation
* treat optional fields as required
* assert exact payload snapshots unless contract requires exact payload
* assert internal debugging fields

---

# 12. Incremental Update Workflow

## 12.1 When OpenAPI Changes

### New Endpoint Added

* generate corresponding Hurl file
* map it to correct tag directory
* add minimum success and error scenarios

### Existing Endpoint Modified

* update Hurl incrementally
* preserve user-authored tests
* append missing assertions or scenarios
* do not erase manual refinements

### Endpoint Removed

* remove obsolete generated coverage
* never delete manually written custom scenarios without explicit intent
* do not remove user-owned files blindly

---

## 12.2 Preserve Manual Tests

Generated tests must coexist with hand-written contract tests.

### Required

* preserve manual scenarios
* preserve domain-specific assertions
* preserve custom negative cases
* append rather than overwrite when possible

### Forbidden

* overwriting manually written Hurl content
* deleting custom action tests
* flattening hand-crafted files into generic generated output

---

# 13. Failure Recovery and Repair Rules

When a Hurl test fails, do not patch blindly.

## 13.1 First Classify the Failure

Every failure should first be classified as one of:

* outdated OpenAPI
* outdated Hurl assertion
* API implementation regression
* test data collision
* cleanup failure
* unstable ordering
* auth/environment issue
* flaky behavior
* contract ambiguity

Do not modify Hurl until failure type is understood.

---

## 13.2 Repair Order

Use this repair order:

1. verify OpenAPI and documented contract
2. verify business rule expectations
3. inspect server behavior
4. update Hurl only if contract actually changed

Never change Hurl to legitimize broken implementation behavior.

---

## 13.3 Forbidden Repair Behavior

Do NOT:

* change expected 404 to 200 just because deleted resource still returns data
* change expected 4xx to 500 because server currently crashes
* remove assertions because implementation is incomplete
* weaken cleanup requirements because delete is inconvenient
* bypass business rule checks to make tests pass

---

## 13.4 Repair Must Preserve Contract Strictness

A repair should maintain or improve correctness.

Allowed repair actions:

* add missing negative tests
* strengthen weak assertions
* update tests to match real contract changes
* add cleanup validation
* refine pagination verification
* add business conflict coverage

---

## 13.5 Cleanup Failures Are First-Class Failures

If a test creates data and fails to clean it:

* that is a real failure
* it must not be ignored
* cleanup logic must be repaired
* test data collision risk must be addressed
* future runs must not depend on leftover state

---

# 14. Regression Rule

Every bug fix that changes externally visible API behavior must add or strengthen at least one Hurl regression scenario.

## Examples

* duplicate alias bug fixed
  → add duplicate create conflict test
* delete visibility bug fixed
  → add remove + query 404 verification
* unstable pagination fixed
  → add deterministic ordering check
* invalid update returned 500 before
  → add invalid update 4xx regression test

A bug fix without regression coverage is incomplete.

---

# 15. Stability and Rerun Safety

## 15.1 Execution Stability Rules

Hurl tests must be:

* rerunnable
* order-independent
* isolated
* cleanup-safe
* deterministic where the contract requires determinism

---

## 15.2 Captures Must Be Local

Captured values must come from requests in the same file execution flow.

Do not reuse captures across unrelated files.

---

## 15.3 No Hidden Cross-File Dependency

A Hurl file must not depend on:

* another Hurl file having run first
* shared mutable fixture state
* manually seeded records unless explicitly documented
* old leftovers from previous runs

---

## 15.4 Eventual Consistency Rule

Do not add retry loops, sleeps, or timing-based hacks unless eventual consistency is explicitly documented by the API contract.

If eventual consistency is real and intentional, document it before adding waiting behavior.

---

# 16. Priority Rule

Hurl tests MUST pass, but never at the cost of violating repository rules or API contract rules.

Passing tests are not the goal if they validate the wrong behavior.

Correct external contract validation is the goal.

---

# 17. Best Practices Summary

1. Always test external behavior, never internal implementation.
2. Keep one operation family per file.
3. Use OpenAPI as the source of truth.
4. Always clean up created resources.
5. Verify post-delete 404 behavior when contract requires invisibility.
6. Use unique test data to avoid collisions.
7. Ensure every Hurl file is independently executable.
8. Cover validation, business, not-found, and repeated-operation errors.
9. Preserve manual Hurl tests during regeneration.
10. Never repair failures by weakening valid assertions.
11. Every externally visible bug fix must add regression coverage.
12. Pagination tests must verify stable ordering when offset-based paging is used.
13. Cleanup failures are real failures.
14. Repeated operations must have explicit expected behavior.
15. Generated Hurl must strengthen contract safety, not dilute it.

---

# 18. Final Non-Negotiable Rules

The following rules are mandatory:

* Hurl tests validate external contracts only.
* Every created resource must be cleaned in the same file.
* Deleted resources must return 404 if the contract says they are no longer visible.
* Repeated operations must have explicit expected behavior.
* Test files must be independent and rerunnable.
* OpenAPI changes must update Hurl incrementally without destroying manual tests.
* Test failures must never be resolved by weakening valid contract assertions.
* Every bug fix must add or improve at least one regression scenario.
