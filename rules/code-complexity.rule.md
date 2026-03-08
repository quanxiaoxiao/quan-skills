# Code Complexity Rule

All generated JavaScript and TypeScript code must follow strict complexity limits.

The goal is to keep code readable and maintainable.

For JavaScript and TypeScript, these limits also exist to keep business logic easy to unit test.

---

## Function Size

Functions must remain small.

Recommended limits:

- maximum: 40 lines
- preferred: 10–25 lines

If a function grows beyond this size:

- extract helper functions
- move logic to services or utilities
- split decision logic from side effects when possible

Bad example:

```javascript
function handleRequest() {
// 200 lines of logic
}
```

Good example:

```javascript
function handleRequest() {
const input = parseInput()
const result = processInput(input)
return buildResponse(result)
}
```

---

## File Size

Files should stay focused.

Limits:

- maximum: 300 lines
- preferred: 80–200 lines

If a file grows too large:

- split modules
- move utilities to separate files

---

## Nesting Depth

Deep nesting harms readability.

Maximum allowed nesting:

- 3 levels

Bad example:

```javascript
if (a) {
if (b) {
if (c) {
if (d) {
}
}
}
}
```

Good example:

```javascript
if (!a) return

if (!b) return

if (!c) return
```

---

## Single Responsibility

Each module should have one purpose.

Examples:

Good:

- userService.ts → user business logic
- userRepo.ts → database access
- userRoutes.ts → routing

Bad:

- user.ts → routes + services + database logic mixed

---

## Function Responsibilities

Functions should do one thing.

For JavaScript and TypeScript, prefer boundaries that let one function cover one decision, transformation, or validation step so it can be tested directly.

Bad:

```javascript
function createUserAndSendEmailAndLogAndSaveFile() {}
```

Good:

```javascript
createUser()
sendWelcomeEmail()
logUserCreated()
```

Better orchestration shape:

```javascript
const normalizedInput = normalizeInput(input)
const validation = validateInput(normalizedInput)
const decision = decideAction(validation)
return executeAction(decision)
```

---

## Parameter Limits

Avoid functions with too many parameters.

Recommended:

- maximum: 5 parameters

If more are required:

- use an options object

Example:

```javascript
createUser({
name,
email,
role
})
```

---

## Reusability

Extract repeated logic.

If the same pattern appears twice:

- create a helper function

If a function mixes heavy branching with side effects:

- extract pure helpers first
- keep orchestration thin
- prefer smaller functions over larger mocks in tests

---

## Output Verification

Before finishing code generation verify:

- no function exceeds size limits
- files remain modular
- nesting depth ≤ 3
- functions have clear responsibilities
- business logic is split into unit-testable helpers where practical
- side-effect boundaries are separated from pure logic when practical
