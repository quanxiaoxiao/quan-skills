# Prompt: Fix Failing Hurl Test

> Authoritative prompt: `quan-skills:prompts/fix-failing-hurl.md`
> Authoritative skill: `quan-skills:skills/hurl-testing/SKILL.md`

Repair failing Hurl tests systematically without weakening contract assertions.

## Context

- Failing test file: {test_file}
- Failure output: {failure_output}
- Recent changes: {recent_changes}

## Step 1: Classify the Failure

Determine failure type:

- [ ] **Outdated OpenAPI** - Spec changed, tests not updated
- [ ] **Outdated assertion** - Contract changed, test stale
- [ ] **Implementation regression** - Behavior broke, contract unchanged
- [ ] **Test data collision** - Leftovers from previous runs
- [ ] **Cleanup failure** - Resource not cleaned properly
- [ ] **Unstable ordering** - Pagination nondeterministic
- [ ] **Auth/environment issue** - Token expired, server down
- [ ] **Flaky behavior** - Timing/race condition
- [ ] **Contract ambiguity** - Unclear expected behavior

## Step 2: Investigation

### Verify OpenAPI and Contract

1. Read OpenAPI spec for the failing endpoint
2. Check documented status codes match assertions
3. Verify response schemas haven't changed
4. Confirm error response contracts

### Inspect Server Behavior

```bash
# Run the specific request with verbose output
hurl --verbose {test_file} 2>&1 | grep -A 20 "Request\|Response"
```

### Check for Collisions

```bash
# Check if test data exists before test runs
# Look for resources with "hurl-test-" prefix
```

### Verify Previous Cleanup

Check if previous test runs left resources behind:
- Query for test data that should have been cleaned
- Check if delete endpoints work correctly

## Step 3: Repair Decision Tree

### IF OpenAPI changed
→ Update Hurl to match new contract
→ Update status codes, fields, error shapes

### IF contract unchanged but behavior changed
→ Flag as implementation regression
→ Do NOT weaken test to match broken behavior
→ File bug report or fix implementation

### IF test data collision detected
→ Strengthen test data uniqueness
→ Add more unique identifiers (random + timestamp)
→ Fix cleanup in this and related files

### IF cleanup failure
→ Repair cleanup logic
→ Add mandatory cleanup verification
→ Ensure delete endpoints work correctly
→ Consider adding cleanup retries if idempotent

### IF unstable ordering
→ Add ordering assertion if contract supports determinism
→ Flag pagination contract issue if ordering not guaranteed
→ Document requirement for stable ordering

### IF auth/environment issue
→ Check server is running
→ Verify authentication tokens
→ Check network connectivity
→ Verify base URL configuration

### IF flaky behavior
→ Identify root cause (timing, race condition)
→ Add explicit synchronization if needed
→ Do not add arbitrary sleeps

### IF contract ambiguity
→ Clarify expected behavior in documentation
→ Align team on contract interpretation
→ Update tests once contract is clear

## Forbidden Repairs

NEVER do the following:

- ❌ Change expected 404 to 200 because deleted resource still returns data
- ❌ Change expected 4xx to 500 because server currently crashes
- ❌ Remove assertions because implementation is incomplete
- ❌ Weaken cleanup requirements because delete is inconvenient
- ❌ Bypass business rule checks to make tests pass
- ❌ Remove post-delete 404 verification
- ❌ Change expected status codes without contract change
- ❌ Assert internal-only fields to match implementation

## Step 4: Apply Repair

### Update Assertions (If Contract Changed)

```hurl
# Old (contract changed)
jsonpath "$.old_field" exists

# New (contract changed)
jsonpath "$.new_field" exists
```

### Strengthen Test Data (If Collision)

```hurl
# Before
"name": "hurl-test-{{$timestamp}}"

# After
"name": "hurl-test-{{$timestamp}}-{{$random}}"
```

### Fix Cleanup (If Cleanup Failure)

```hurl
# Ensure cleanup always runs
DELETE {{base_url}}/api/resources/{{resource_id}}
HTTP 204

# Verify cleanup worked
GET {{base_url}}/api/resources/{{resource_id}}
HTTP 404
```

### Add Missing Coverage (If Gap Found)

```hurl
# Add test case for newly declared error
POST {{base_url}}/api/resources
Content-Type: application/json

{
  "invalid": "data"
}

HTTP 400
[Asserts]
jsonpath "$.code" == "VALIDATION_ERROR"
```

## Step 5: Validation

After repair:

1. **Run failing test in isolation**
   ```bash
   hurl {test_file}
   ```

2. **Run all tests in tag directory**
   ```bash
   hurl tests/hurl/{tag}/*.hurl
   ```

3. **Run tests multiple times to check stability**
   ```bash
   for i in {1..3}; do hurl {test_file}; done
   ```

4. **Verify no new failures introduced**
   - Check previously passing tests still pass
   - Check no regressions in other files

5. **Verify cleanup works correctly**
   - Run test
   - Verify no leftover resources
   - Run test again to confirm rerunnable

6. **Verify test is rerunnable**
   - Run test twice in succession
   - Both runs should pass

## Repair Must Preserve Contract Strictness

Allowed repair actions:

- ✅ Add missing negative tests
- ✅ Strengthen weak assertions
- ✅ Update tests to match real contract changes
- ✅ Add cleanup validation
- ✅ Refine pagination verification
- ✅ Add business conflict coverage
- ✅ Improve test data uniqueness
- ✅ Fix broken cleanup logic

## Output

Report repair results:

```
## Repair Summary

**Root Cause**: {classification}

**Fix Applied**:
- {specific change made}

**Verification**:
- [ ] Test passes in isolation
- [ ] Test passes 3 times sequentially
- [ ] All tests in directory pass
- [ ] No new failures introduced
- [ ] Cleanup verified
- [ ] Test is rerunnable

**Risks**: {any concerns or follow-up needed}
```

## Escalation

If repair requires weakening valid assertions:

1. Stop and document the conflict
2. Escalate to team for contract clarification
3. Do not proceed with weakening test
4. Consider if implementation needs fixing instead
