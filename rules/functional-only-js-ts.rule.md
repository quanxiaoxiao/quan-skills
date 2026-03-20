# Functional-Only JavaScript / TypeScript Rule

JavaScript and TypeScript code must use a **functional programming style**.

## Core Rule

Do not generate or introduce `class`-based implementations in JavaScript or TypeScript.

Allowed structures:

- functions
- arrow functions
- factory functions
- closures
- modules
- plain objects
- composition
- functional helpers

Functional style here also means preferring function boundaries that are easy to unit test.

## Forbidden Keywords

The following patterns must not appear in generated code:

- `class`
- `abstract class`
- `extends` (inheritance)
- `constructor(` / `private constructor` / `protected constructor` / `public constructor`
- `static class`
- `new Service()` patterns

## Forbidden Architecture Patterns

- service classes
- controller classes
- singleton classes
- inheritance-based hierarchies
- static utility classes

## Preferred Alternatives

### Instead of classes

Use functions or factory functions.

### Instead of instance methods

Use plain functions with explicit parameters.

### Instead of private fields

Use closures.

### Instead of inheritance

Use composition.

### Instead of static helpers

Export functions.

---

## Testable Function Boundary Rule

JavaScript and TypeScript code should be split into small functions that are easy to unit test.

Prefer:

- single-purpose functions
- explicit input and output
- pure helpers for branching, mapping, validation, and derived values
- composition where orchestration functions coordinate already-testable helpers
- dependency injection through function parameters

Avoid:

- one function mixing parsing, decision logic, side effects, and formatting
- business branching that depends on hidden mutable state
- pushing framework context, database handles, or logging objects through large decision trees
- keeping logic coupled just to avoid creating helper functions

If a logic block cannot be tested directly without large mocks, first split the function boundary before expanding the test harness.

---

## Service Layer Rule

Service layers must use functions or factory functions.

Example:

```javascript
export const createUserService = (deps) => {

const createUser = async (input) => {
...
}

const updateUser = async (input) => {
...
}

return {
createUser,
updateUser
}
}
```

Avoid:

```javascript
class UserService {
constructor(repo) {}
}
```

---

## State Rule

Prefer:

- explicit parameters
- explicit return values
- immutable updates
- closure-based encapsulation
- testable function boundaries around business logic

Avoid hidden mutable state tied to instances.

---

## Export Style

Prefer:

```javascript
export function createEntry(input) {}
export function updateEntry(input) {}
```

or factory functions:

```javascript
export const createEntryService = (deps) => {
...
}
```

---

## Refactoring Rule

If existing code already contains classes:

- do not expand class usage
- introduce new logic using functions
- refactor only when safe and scoped

---

## Exception Rule

Classes may only be used when:

1. the user explicitly requires class-based architecture
2. the framework requires classes
3. no functional alternative exists

Otherwise classes must not be used.

---

## Rewrite Examples

### Controller

```javascript
// Forbidden
class UserController {
  async create(ctx) {}
}

// Required rewrite
export const createUserController = (deps) => {
  const create = async (ctx) => {}
  return { create }
}
```

### Static Utility

```javascript
// Forbidden
class HashUtil {
  static sha256(data) {}
}

// Required rewrite
export const sha256 = (data) => {}
```

## Enforcement

Before finalizing JavaScript or TypeScript output:

1. Scan for forbidden keywords and architecture patterns.
2. If detected, rewrite the code using functions, factory functions, or closures.
3. Ensure no `class` keyword remains.

Verify:

- no `class` or `abstract class` keyword exists
- no inheritance pattern exists
- no constructor-based services exist
- business logic is split into small functions that can be unit tested directly
- orchestration functions mainly coordinate helpers instead of hiding all logic internally

All implementations must be function-based.
