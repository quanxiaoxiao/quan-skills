# Hurl Quality Checklist

Use this checklist before committing Hurl test changes.

## Contract Layer

- [ ] Status code assertions present for all requests
- [ ] Required fields asserted in success responses
- [ ] Top-level response structure asserted
- [ ] Error envelope asserted for 4xx responses
- [ ] Machine-readable error codes asserted if declared in contract
- [ ] Pagination structure verified (items, total fields exist)
- [ ] Items count assertion for paginated endpoints
- [ ] No assertions on internal-only fields
- [ ] No assertions on undeclared debug fields
- [ ] No assertions on optional fields as if they were required
- [ ] No exact full payload snapshots unless contract requires exact match

## Lifecycle Layer

- [ ] Create → Query flow tested
- [ ] Create → Update → Query flow tested
- [ ] Create → Remove → 404 flow tested
- [ ] Resource ID captured after creation using jsonpath
- [ ] Captured ID used in subsequent operations
- [ ] Post-delete 404 verification present for deleted resources
- [ ] Invalid request → no unintended resource creation verified
- [ ] State transitions follow allowed paths only

## Resilience Layer

- [ ] Missing required field tested
- [ ] Wrong field type tested
- [ ] Malformed JSON tested
- [ ] Invalid enum value tested
- [ ] Invalid query parameter format tested
- [ ] Duplicate create conflict tested (if applicable)
- [ ] Update not-found tested
- [ ] Delete not-found tested
- [ ] Repeated delete behavior defined and tested
- [ ] Invalid state transition tested
- [ ] Business rule conflict tested
- [ ] Action on nonexistent resource tested

## Pagination (If Applicable)

- [ ] Page 1 query with limit/offset tested
- [ ] Page 2 query tested
- [ ] Total count field asserted
- [ ] Items array asserted
- [ ] Items count <= limit asserted
- [ ] Stable ordering verified for offset-based pagination
- [ ] First item of page 2 != last item of page 1
- [ ] Invalid pagination params tested (negative offset)
- [ ] Invalid pagination params tested (zero/invalid limit)
- [ ] Out-of-range values tested if contract constrains them

## Cleanup & Isolation

- [ ] Every created resource has corresponding cleanup
- [ ] Cleanup happens in same file as resource creation
- [ ] Cleanup assertions present (HTTP 204 or appropriate)
- [ ] Cleanup failure not ignored or bypassed
- [ ] No hard-coded shared IDs between tests
- [ ] No dependency on external database state
- [ ] No dependency on previous test runs
- [ ] Test data uses unique identifiers (timestamp-based)
- [ ] Test data marked with "hurl-test-" prefix
- [ ] File can run independently without other files
- [ ] No cross-file capture reuse
- [ ] No execution order assumptions across files

## Assertion Strength

- [ ] No HTTP 200 assertions without body validation
- [ ] No HTTP 4xx assertions without error shape validation
- [ ] Optional fields not treated as required
- [ ] No exact payload snapshots unless contract requires
- [ ] No assertions on internal debugging fields
- [ ] Business-critical changes explicitly asserted
- [ ] Deletion visibility rules asserted
- [ ] Deterministic ordering requirements asserted where relevant

## Error Coverage

- [ ] Validation errors covered (4xx for invalid structure)
- [ ] Business rule errors covered (4xx for constraint violations)
- [ ] Not found errors covered (404 for missing resources)
- [ ] Conflict/idempotency errors covered (explicit success or conflict)
- [ ] Auth errors covered if applicable (401/403)
- [ ] Error responses have stable shape assertions
- [ ] No 500 errors expected for normal missing-resource cases

## File Organization

- [ ] Directory structure follows tests/hurl/{tag}/
- [ ] File naming follows operation mapping (create.hurl, query.hurl, etc.)
- [ ] One operation family per file
- [ ] No unrelated actions mixed in single file
- [ ] No catch-all integration scripts
- [ ] Custom actions have dedicated [action].hurl files

## Test Data Management

- [ ] Test data is short-lived (created and cleaned in same file)
- [ ] Test data is self-contained
- [ ] Test data includes timestamp: "{{$timestamp}}"
- [ ] Test data has clear prefix: "hurl-test-"
- [ ] No dependency on mutable shared state
- [ ] No assumption that database is empty
- [ ] No reliance on leftovers from previous runs

## Captures & Variables

- [ ] Captures have clear, descriptive names (resource_id, not id)
- [ ] Captures used in subsequent requests in same file
- [ ] No captures referenced across unrelated files
- [ ] Capture expressions use valid jsonpath
- [ ] Captures from error responses handled appropriately

## Maintainability

- [ ] Comments explain non-obvious assertions
- [ ] Comments explain business rule test scenarios
- [ ] Consistent formatting across file
- [ ] Consistent naming conventions
- [ ] Test purpose clear from file structure
- [ ] No duplicate test logic within file
- [ ] Reusable patterns extracted appropriately

## Manual Test Preservation

- [ ] Existing manual tests not overwritten
- [ ] User-authored scenarios preserved
- [ ] Domain-specific assertions maintained
- [ ] Custom negative cases kept intact
- [ ] Generated content appended rather than replacing manual content

## Performance & Stability

- [ ] No unnecessary delays or sleeps
- [ ] No retry loops unless eventual consistency is documented
- [ ] Tests are deterministic where contract requires determinism
- [ ] Tests are rerunnable without side effects
- [ ] No timing-based race conditions

## Documentation Alignment

- [ ] Tests match OpenAPI specification
- [ ] Tests match documented API rules
- [ ] Tests match declared status codes
- [ ] Tests match declared error responses
- [ ] Tests match declared pagination contracts
- [ ] Tests do not assert unspecified behavior

## Pre-Commit Verification

- [ ] All tests pass in isolation
- [ ] All tests pass as a suite
- [ ] Tests can be run multiple times sequentially without failures
- [ ] No test data collisions detected
- [ ] Cleanup verification passes
- [ ] No leftover resources after test completion
