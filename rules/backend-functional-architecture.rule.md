# Backend Functional Architecture Rule

JavaScript / TypeScript backend services must follow a **functional layered architecture**.

Required architecture flow:

```
routes → handlers → services → repositories → database
```

Each layer must be implemented using functions.

Classes must not be used.

---

## Layer Definitions

### Routes

Responsible for:

- registering endpoints
- connecting handlers

Routes must contain minimal logic.

Example:

```javascript
router.post('/users', createUserHandler)
```

---

### Handlers

Handlers manage:

- HTTP request parsing
- validation
- response formatting

Handlers call services.

Example:

```javascript
export const createUserHandler = async (ctx) => {
const input = ctx.req.valid('json')

const user = await createUserService(input)

return ctx.json(user)
}
```

---

### Services

Services implement business logic.

Services must:

- contain domain logic
- orchestrate repositories
- not depend on HTTP context

Example:

```javascript
export const createUserService = async (input) => {
const user = await userRepo.insert(input)

return user
}
```

---

### Repositories

Repositories manage database operations.

Responsibilities:

- persistence
- queries
- mapping database results

Example:

```javascript
export const insertUser = async (data) => {
return UserModel.create(data)
}
```

---

### Database Layer

Database access should be isolated inside repositories.

Avoid direct database calls inside:

- routes
- handlers
- services

---

# Forbidden Patterns

Do not generate:

- controller classes
- service classes
- repository classes
- static utility classes
- inheritance

Forbidden example:

```javascript
class UserService {}
```

---

# Dependency Injection

Dependencies must be injected via functions.

Example:

```javascript
export const createUserService = (deps) => {

const createUser = async (input) => {
return deps.repo.insert(input)
}

return { createUser }
}
```

---

# Data Flow Rules

Preferred flow:

```
route
↓
handler
↓
service
↓
repository
↓
database
```

Avoid skipping layers.

Bad example:

```
handler → database
```

---

# File Organization

Preferred structure:

```
src/
routes/
handlers/
services/
repositories/
models/
```

---

# Output Requirement

Before finalizing backend code generation:

Verify:

- architecture follows required layers
- no classes exist
- services are functions
- repositories are functions
- handlers are functions
- routes only register endpoints