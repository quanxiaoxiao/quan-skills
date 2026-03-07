# Forbidden OOP Patterns (JavaScript / TypeScript)

JavaScript and TypeScript code in this environment must follow **functional programming principles**.

Object-oriented patterns are forbidden unless explicitly requested by the user.

---

## Forbidden Keywords

The following patterns must not appear in generated code:

```
class
abstract class
extends
constructor(
private constructor
protected constructor
public constructor
static class
```

---

## Forbidden Architecture Patterns

Reject the following designs:

- service classes
- controller classes
- singleton classes
- inheritance-based hierarchies
- static utility classes

Examples of forbidden code:

```javascript
class UserService {
constructor(repo) {}
}
```

```javascript
class ResourceController extends BaseController {}
```

```javascript
class Logger {
static log() {}
}
```

---

## Required Rewrite Strategy

If any forbidden structure is detected, the implementation must be rewritten.

### Example

OOP code:

```javascript
class UserService {
constructor(repo) {
this.repo = repo
}

async createUser(data) {
return this.repo.create(data)
}
}
```

Rewrite to functional style:

```javascript
export const createUserService = (repo) => {

const createUser = async (data) => {
return repo.create(data)
}

return {
createUser
}
}
```

---

## Controller Rewrite Example

OOP controller:

```javascript
class UserController {
async create(ctx) {}
}
```

Rewrite:

```javascript
export const createUserController = (deps) => {

const create = async (ctx) => {}

return {
create
}

}
```

---

## Utility Rewrite

Static class:

```javascript
class HashUtil {
static sha256(data) {}
}
```

Rewrite:

```javascript
export const sha256 = (data) => {}
```

---

## Enforcement

Before finalizing JavaScript or TypeScript output:

1. Scan for forbidden patterns.
2. If detected, rewrite the code using functions.
3. Ensure no `class` keyword remains.

---

## Exception Rule

Classes are allowed only if:

1. the user explicitly asks for class-based architecture
2. a framework strictly requires classes
3. no functional alternative exists

Otherwise classes must not be generated.