# Prompt: Generate Hurl Tests from OpenAPI

Generate maintainable Hurl tests from OpenAPI specification following the Hurl Testing Framework rules.

## Context

- OpenAPI spec location: {openapi_path}
- Base URL: {base_url}
- Tags to generate: {tags} (or "all")

## Pre-conditions

1. OpenAPI spec is valid and accessible
2. Tags are identified for generation scope
3. Output directory exists: tests/hurl/

## Generation Steps

### Step 1: Parse OpenAPI Spec

Extract for each tag:
- Paths and HTTP methods
- Request schemas
- Response schemas and status codes
- Error responses
- Query parameters

### Step 2: Create Directory Structure

```
tests/hurl/{tag}/
```

### Step 3: Generate create.hurl

For POST endpoints:

```hurl
# Create success
POST {{base_url}}{path}
Content-Type: application/json

{{
  "field": "hurl-test-{{$timestamp}}"
}}

HTTP 201
[Captures]
resource_id: jsonpath "$._id"
[Asserts]
jsonpath "$._id" exists
jsonpath "$.field" == "hurl-test-{{$timestamp}}"

# Create validation error
POST {{base_url}}{path}
Content-Type: application/json

{{
  "field": null
}}

HTTP 400
[Asserts]
jsonpath "$.code" exists

# Duplicate/business conflict (if applicable)
POST {{base_url}}{path}
Content-Type: application/json

{{
  "field": "duplicate-value"
}}

HTTP 409

# Cleanup
DELETE {{base_url}}{path}/{{resource_id}}
HTTP 204

# Verify deletion
GET {{base_url}}{path}/{{resource_id}}
HTTP 404
```

### Step 4: Generate query.hurl

For GET endpoints:

```hurl
# Setup: Create test resource
POST {{base_url}}{create_path}
Content-Type: application/json

{{
  "name": "hurl-test-{{$timestamp}}"
}}

HTTP 201
[Captures]
resource_id: jsonpath "$._id"

# List with pagination
GET {{base_url}}{path}?limit=2&offset=0
HTTP 200
[Asserts]
jsonpath "$.items" exists
jsonpath "$.total" exists
jsonpath "$.items" count <= 2

# Detail query
GET {{base_url}}{path}/{{resource_id}}
HTTP 200
[Asserts]
jsonpath "$._id" exists

# Query not-found
GET {{base_url}}{path}/999999999
HTTP 404
[Asserts]
jsonpath "$.code" exists

# Cleanup
DELETE {{base_url}}{create_path}/{{resource_id}}
HTTP 204
```

### Step 5: Generate update.hurl

For PATCH/PUT endpoints:

```hurl
# Setup
POST {{base_url}}{create_path}
Content-Type: application/json

{{
  "name": "hurl-test-{{$timestamp}}"
}}

HTTP 201
[Captures]
resource_id: jsonpath "$._id"

# Update success
PATCH {{base_url}}{path}/{{resource_id}}
Content-Type: application/json

{{
  "name": "updated-{{$timestamp}}"
}}

HTTP 200
[Asserts]
jsonpath "$.name" == "updated-{{$timestamp}}"

# Update not-found
PATCH {{base_url}}{path}/999999999
HTTP 404

# Invalid update payload
PATCH {{base_url}}{path}/{{resource_id}}
Content-Type: application/json

{{
  "name": null
}}

HTTP 400

# Cleanup
DELETE {{base_url}}{create_path}/{{resource_id}}
HTTP 204
```

### Step 6: Generate remove.hurl

For DELETE endpoints:

```hurl
# Setup
POST {{base_url}}{create_path}
Content-Type: application/json

{{
  "name": "hurl-test-{{$timestamp}}"
}}

HTTP 201
[Captures]
resource_id: jsonpath "$._id"

# Delete success
DELETE {{base_url}}{path}/{{resource_id}}
HTTP 204

# Post-delete 404 verification
GET {{base_url}}{create_path}/{{resource_id}}
HTTP 404

# Repeated delete behavior
DELETE {{base_url}}{path}/{{resource_id}}
HTTP 404

# Delete not-found
DELETE {{base_url}}{path}/999999999
HTTP 404
```

### Step 7: Generate [action].hurl Files

For custom business operations:

```hurl
# Setup
POST {{base_url}}{create_path}
Content-Type: application/json

{{
  "name": "hurl-test-{{$timestamp}}"
}}

HTTP 201
[Captures]
resource_id: jsonpath "$._id"

# Valid action
POST {{base_url}}{path}/{{resource_id}}/action
HTTP 200
[Asserts]
jsonpath "$.status" == "new_status"

# Invalid action payload
POST {{base_url}}{path}/{{resource_id}}/action
Content-Type: application/json

{{
  "invalid_field": true
}}

HTTP 400

# Action on nonexistent resource
POST {{base_url}}{path}/999999999/action
HTTP 404

# Cleanup
DELETE {{base_url}}{create_path}/{{resource_id}}
HTTP 204
```

## Constraints

- Preserve existing manual tests (read first, append new scenarios)
- Use timestamp-based unique data: "hurl-test-{{$timestamp}}"
- Follow mandatory lifecycle pattern (create → test → cleanup → verify 404)
- No cross-file dependencies
- Don't assert fields not declared in OpenAPI schema
- Don't invent business rules not declared anywhere

## Output

Generate files:
- tests/hurl/{tag}/create.hurl
- tests/hurl/{tag}/query.hurl
- tests/hurl/{tag}/update.hurl
- tests/hurl/{tag}/remove.hurl
- tests/hurl/{tag}/{action}.hurl (if applicable)

Report:
- Files created
- Files modified
- Coverage summary per tag
- Manual tests preserved
- Any schema limitations encountered

## Error Handling

If OpenAPI spec is incomplete:
- Generate based on available information
- Document assumptions made
- Flag areas requiring manual completion
