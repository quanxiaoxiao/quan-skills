# Rule: Hurl Testing Framework

Applies to: All repositories using Hurl for API contract testing

## Directory Structure

```
tests/hurl/{tag}/
├── create.hurl
├── query.hurl
├── update.hurl
├── remove.hurl
└── [action].hurl
```

## Non-Negotiable Rules

1. **External contracts only** - Test observable HTTP behavior, never internal implementation
2. **Clean-as-you-go** - Every created resource must be cleaned in the same file
3. **Post-delete 404 verification** - Deleted resources must return 404 if contract declares them invisible
4. **Independent execution** - Each file must run standalone without cross-file dependencies
5. **Never weaken assertions** - Fix implementation, not tests; never change 404 to 200
6. **OpenAPI is source of truth** - Generated tests must reflect declared schemas and status codes

## Source of Truth Priority

When behavior and tests disagree:

1. OpenAPI specification
2. Business contract / documented API rules
3. Actual server behavior
4. Hurl assertions

Never weaken a valid contract test to make a broken implementation pass.

## File Organization

- **One operation family per file** - No mixing unrelated actions
- **No cross-file dependencies** - No execution order assumptions
- **Unique test data** - Use timestamp-based identifiers to avoid collisions

### Operation Mapping

| HTTP Method | File | Coverage |
|-------------|------|----------|
| POST | create.hurl | Create success, validation errors, conflicts |
| GET | query.hurl | List, detail, filters, pagination, not-found |
| PATCH/PUT | update.hurl | Update success, validation, not-found, conflicts |
| DELETE | remove.hurl | Delete success, repeated delete, 404 verification |
| Custom | [action].hurl | One specific business action with valid/invalid variants |

## Mandatory Lifecycle Pattern

Every test file creating resources must follow this sequence:

```hurl
# 1. Create resource
POST {{base_url}}/api/resources
Content-Type: application/json

{
  "name": "hurl-test-{{$timestamp}}"
}

HTTP 201
[Captures]
resource_id: jsonpath "$._id"

# 2. Test operations (query, update, action)
GET {{base_url}}/api/resources/{{resource_id}}
HTTP 200

# 3. Cleanup
DELETE {{base_url}}/api/resources/{{resource_id}}
HTTP 204

# 4. Verify invisibility
GET {{base_url}}/api/resources/{{resource_id}}
HTTP 404
```

## Test Layer Requirements

### Contract Layer (Required)
- Status code assertions
- Required field assertions
- Error envelope assertions
- Pagination structure (items, total)

### Lifecycle Layer (Required)
- Create → Query flow
- Create → Update → Query flow
- Create → Remove → 404 flow
- No unintended side effects on invalid requests

### Resilience Layer (Required)
- Missing required fields
- Wrong field types
- Invalid enums
- Not found scenarios
- Duplicate/business conflicts
- Repeated operations

## Forbidden Assertions

- Internal-only fields
- Undeclared debug fields
- Exact full payload snapshots unless contract requires
- Assumptions about storage ordering
- Assumptions about internal IDs beyond contract

## Repair Discipline

When tests fail:

1. **Classify** the failure type
2. **Verify** OpenAPI and contract expectations
3. **Inspect** server behavior
4. **Update** Hurl only if contract actually changed

**Never** repair by weakening valid assertions.

## Incremental Maintenance

When OpenAPI changes:

- **New endpoints**: Generate corresponding Hurl files
- **Modified endpoints**: Update incrementally, preserve manual tests
- **Removed endpoints**: Delete generated coverage, preserve manual scenarios

## Regression Rule

Every externally visible bug fix must add or strengthen at least one Hurl regression scenario.

Examples:
- Duplicate alias bug → Add duplicate create conflict test
- Delete visibility bug → Add remove + query 404 verification
- Unstable pagination → Add deterministic ordering check
- Invalid update 500 → Add invalid payload 4xx regression test

## Cleanup Failures Are First-Class Failures

If a test creates data and fails to clean it:
- That is a real failure
- Cleanup logic must be repaired
- Test data collision risk must be addressed
- Future runs must not depend on leftover state
