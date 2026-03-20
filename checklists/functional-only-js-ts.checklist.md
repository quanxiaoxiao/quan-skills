# JavaScript / TypeScript Functional Architecture Checklist

Before completing a task, verify:

- [ ] no `class` keyword exists
- [ ] no `abstract class` exists
- [ ] no inheritance (`extends`)
- [ ] no constructor-based services
- [ ] no static utility classes
- [ ] services implemented as functions or factory functions
- [ ] utilities implemented as plain functions
- [ ] dependency injection uses function parameters
- [ ] architecture uses composition
- [ ] business logic is split into small functions with single responsibilities
- [ ] framework context or external dependencies are not pushed through large branching logic
- [ ] orchestration functions mainly coordinate smaller testable helpers

If any violation exists, rewrite the implementation.
