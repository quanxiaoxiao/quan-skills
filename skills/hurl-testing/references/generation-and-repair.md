# Generation And Repair

Use this reference for the detailed generation and repair workflow after the target contract is loaded.

## Generation Flow

1. Parse the OpenAPI document by tag, path, method, request schema, and declared responses.
2. Choose the output directory under `tests/hurl/{tag}/`.
3. Generate or update only the files needed for the affected operation families.
4. Preserve manual scenarios already present unless the contract explicitly made them obsolete.

## Minimal Templates

### Create Flow

Typical sequence:

1. send a valid create request
2. assert the status code and required fields
3. capture the created identifier
4. cover validation or conflict cases if the contract declares them
5. clean up the created resource

### Query Flow

Typical sequence:

1. create setup data if the query needs a controlled fixture
2. assert list or detail envelope shape
3. cover not-found or invalid-query behavior
4. clean up setup data

### Update And Remove Flows

Typical sequence:

1. create setup data
2. perform the update or delete
3. assert the caller-visible result
4. cover not-found or invalid-payload paths
5. verify deletion visibility when required

## Repair Order

When a Hurl test fails:

1. classify the failure type
2. compare the assertion with OpenAPI and documented API rules
3. inspect actual server behavior
4. change Hurl only if the contract changed or the existing assertion was too strong or wrong

Never weaken a valid contract assertion just to match a broken implementation.

## Regression Rule

Every externally visible API bug fix should add or strengthen at least one Hurl scenario that proves the bug stays fixed.

Common regression targets:

- duplicate creation conflicts
- invalid payload 4xx handling
- delete visibility after cleanup
- pagination stability or shape
- action-specific business conflicts

## Stability Rules

- use unique test data such as `hurl-test-{{$timestamp}}`
- keep captures local to one file
- avoid retries unless eventual consistency is documented
- treat cleanup failures as real test failures
