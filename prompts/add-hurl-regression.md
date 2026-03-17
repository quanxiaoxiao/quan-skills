# Prompt: Add Hurl Regression Test

Add regression coverage for fixed bugs following the Hurl Testing Framework rules.

## Context

- Bug description: {bug_description}
- Fix approach: {fix_approach}
- Tag/endpoint affected: {tag_endpoint}
- Related files: {existing_hurl_files}

## Regression Mapping

Map bug type to regression test:

| Bug Type | Regression Test Pattern | Target File |
|----------|------------------------|-------------|
| Duplicate alias/field allowed | Add duplicate create conflict test | create.hurl |
| Deleted resource still visible in queries | Add remove + query 404 verification | remove.hurl |
| Unstable pagination ordering | Add deterministic ordering check | query.hurl |
| Invalid update returns 500 instead of 4xx | Add invalid payload 4xx test | update.hurl |
| Business rule not enforced | Add conflict/validation test | relevant action file |
| Missing required field not caught | Add validation error test | create.hurl |
| Illegal state transition allowed | Add invalid transition test | [action].hurl |
| Repeated operation behavior inconsistent | Add idempotency test | relevant file |
| Filter not working correctly | Add specific filter test | query.hurl |
| Error response missing required fields | Add error envelope assertion | error test cases |

## Steps

### Step 1: Identify Bug Type

From description, determine which category:

1. **Validation bug** - Input validation not working
2. **Business rule bug** - Constraint not enforced
3. **Visibility bug** - Resources visible when shouldn't be
4. **Stability bug** - Non-deterministic behavior
5. **State bug** - Invalid state transitions allowed
6. **Error handling bug** - Wrong status codes or error format

### Step 2: Determine Target File

Choose file based on affected operation:

- **Create flow issues** → create.hurl
- **Query/visibility issues** → query.hurl
- **Update logic issues** → update.hurl
- **Delete/visibility issues** → remove.hurl
- **Custom action issues** → {action}.hurl

### Step 3: Write Regression Test

Regression test must:
- Fail before the fix is applied
- Pass after the fix is applied
- Follow cleanup discipline
- Use unique test data

#### Example: Duplicate Create Conflict

```hurl
# Regression: Bug where duplicate alias was allowed
# Setup: Create first resource
POST {{base_url}}/api/resources
Content-Type: application/json

{
  "alias": "regression-test-{{$timestamp}}"
}

HTTP 201
[Captures]
first_id: jsonpath "$._id"

# Attempt duplicate create with same alias
POST {{base_url}}/api/resources
Content-Type: application/json

{
  "alias": "regression-test-{{$timestamp}}"
}

HTTP 409
[Asserts]
jsonpath "$.code" == "DUPLICATE_ALIAS"

# Cleanup
DELETE {{base_url}}/api/resources/{{first_id}}
HTTP 204
```

#### Example: Delete Visibility

```hurl
# Regression: Bug where deleted resource still returned 200
POST {{base_url}}/api/resources
Content-Type: application/json

{
  "name": "regression-test-{{$timestamp}}"
}

HTTP 201
[Captures]
resource_id: jsonpath "$._id"

# Delete resource
DELETE {{base_url}}/api/resources/{{resource_id}}
HTTP 204

# Verify deleted resource returns 404 (regression test)
GET {{base_url}}/api/resources/{{resource_id}}
HTTP 404
[Asserts]
jsonpath "$.code" exists
```

#### Example: Invalid Update Error Code

```hurl
# Regression: Bug where invalid update returned 500 instead of 400
POST {{base_url}}/api/resources
Content-Type: application/json

{
  "name": "regression-test-{{$timestamp}}"
}

HTTP 201
[Captures]
resource_id: jsonpath "$._id"

# Attempt invalid update
PATCH {{base_url}}/api/resources/{{resource_id}}
Content-Type: application/json

{
  "name": null,
  "invalid_field": "value"
}

HTTP 400
[Asserts]
jsonpath "$.code" == "VALIDATION_ERROR"

# Cleanup
DELETE {{base_url}}/api/resources/{{resource_id}}
HTTP 204
```

#### Example: Pagination Ordering

```hurl
# Regression: Bug where pagination returned unstable ordering
GET {{base_url}}/api/resources?limit=2&offset=0
HTTP 200
[Captures]
first_page_last_id: jsonpath "$.items[-1]._id"

GET {{base_url}}/api/resources?limit=2&offset=2
HTTP 200
[Asserts]
jsonpath "$.items[0]._id" != "{{first_page_last_id}}"
jsonpath "$.items" count <= 2
```

#### Example: Invalid State Transition

```hurl
# Regression: Bug where published resource could be edited
POST {{base_url}}/api/resources
Content-Type: application/json

{
  "name": "regression-test-{{$timestamp}}"
}

HTTP 201
[Captures]
resource_id: jsonpath "$._id"

# Publish resource
POST {{base_url}}/api/resources/{{resource_id}}/publish
HTTP 200

# Attempt edit on published resource (should fail)
PATCH {{base_url}}/api/resources/{{resource_id}}
Content-Type: application/json

{
  "name": "updated-name"
}

HTTP 409
[Asserts]
jsonpath "$.code" == "CANNOT_MODIFY_PUBLISHED"

# Cleanup
DELETE {{base_url}}/api/resources/{{resource_id}}
HTTP 204
```

### Step 4: Placement Rules

Add regression test to appropriate section:

```hurl
# ============================================
# Regression Tests
# ============================================

# REGRESSION: {bug_id} - {brief_description}
# {detailed explanation of what was broken and how test verifies fix}
{test_case}
```

Place after main test scenarios, before cleanup if shared, or as standalone test with its own cleanup.

### Step 5: Verification

#### Before Committing Fix

1. **Temporarily revert the fix**
   ```bash
   git stash
   # or revert specific commit
   ```

2. **Run regression test** - Must FAIL
   ```bash
   hurl {target_file}
   ```

3. **Re-apply the fix**
   ```bash
   git stash pop
   # or re-apply commit
   ```

4. **Run regression test** - Must PASS
   ```bash
   hurl {target_file}
   ```

5. **Run full test suite** - No regressions
   ```bash
   hurl tests/hurl/{tag}/*.hurl
   ```

## Template Library

### Validation Error Template

```hurl
# REGRESSION: {description}
POST {{base_url}}/api/{resource}
Content-Type: application/json

{
  "{field}": {invalid_value}
}

HTTP 400
[Asserts]
jsonpath "$.code" == "VALIDATION_ERROR"
jsonpath "$.field" == "{field}"
```

### Business Conflict Template

```hurl
# Setup
POST {{base_url}}/api/{resource}
Content-Type: application/json

{
  "{field}": "regression-{{$timestamp}}"
}

HTTP 201
[Captures]
{id}: jsonpath "$._id"

# REGRESSION: {description}
POST {{base_url}}/api/{resource}
Content-Type: application/json

{
  "{field}": "regression-{{$timestamp}}"
}

HTTP 409
[Asserts]
jsonpath "$.code" == "{ERROR_CODE}"

# Cleanup
DELETE {{base_url}}/api/{resource}/{{id}}
HTTP 204
```

### Not Found Template

```hurl
# REGRESSION: {description}
{METHOD} {{base_url}}/api/{resource}/999999999
HTTP 404
[Asserts]
jsonpath "$.code" exists
```

### State Transition Template

```hurl
# Setup resource in initial state
POST {{base_url}}/api/{resource}
Content-Type: application/json

{
  "name": "regression-{{$timestamp}}"
}

HTTP 201
[Captures]
{id}: jsonpath "$._id"

# Move to state that should block action
POST {{base_url}}/api/{resource}/{{id}}/{action}
HTTP 200

# REGRESSION: Attempt illegal action from this state
{METHOD} {{base_url}}/api/{resource}/{{id}}/{blocked_action}
HTTP 409
[Asserts]
jsonpath "$.code" == "{STATE_CONFLICT_CODE}"

# Cleanup
DELETE {{base_url}}/api/{resource}/{{id}}
HTTP 204
```

## Output

Report regression addition:

```
## Regression Test Added

**Bug**: {brief_description}

**Target File**: {file_path}

**Regression Test**:
```hurl
{test_content}
```

**Verification**:
- [ ] Test FAILS before fix
- [ ] Test PASSES after fix
- [ ] Full test suite passes
- [ ] Test is rerunnable

**Coverage**: {what_scenario_this_covers}
```

## Rules

- Every externally visible bug fix must have regression coverage
- Regression tests follow same cleanup rules as regular tests
- Tests must fail before fix, pass after fix
- Document the regression purpose in comments
- Group regression tests in dedicated sections
