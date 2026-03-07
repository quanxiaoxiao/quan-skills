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

## Forbidden

Never generate:

- `class`
- `abstract class`
- inheritance chains
- constructor-based services
- static utility classes
- singleton classes
- `new Service()` patterns

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

## Output Verification

Before finalizing JavaScript or TypeScript output, verify:

- no `class` keyword exists
- no `abstract class` exists
- no inheritance pattern exists
- no constructor-based services exist

All implementations must be function-based.