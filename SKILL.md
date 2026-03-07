# quan-skills

Global development standards and workflows used across repositories.

This skill set defines rules, prompts, checklists, and workflows for maintaining Node.js projects across macOS and Linux environments.

Primary goals:

- Consistent Node.js runtime practices
- Minimal and runtime-first dependencies
- Standardized verification workflows
- Safe auditing and repair of existing repositories
- Reusable automation across many projects

Supported platforms:

- macOS
- Linux

Windows support is intentionally excluded unless explicitly required.

These skills are intended to be loaded globally via:

`~/.codex/skills/quan-skills`

Projects may inherit these rules automatically or extend them locally.

---

## JavaScript / TypeScript Style Constraint

For JavaScript and TypeScript implementations:

- use functional programming style
- avoid `class` and `abstract class`
- avoid inheritance-based architecture
- prefer functions, closures, and composition
- prefer factory functions instead of constructor-based services
- avoid singleton classes

Default design should use:

- pure functions
- factory functions
- module-based organization

If an existing project contains classes, do not expand that pattern.

New code should default to functional implementations.

---

## OOP Detection and Prevention

JavaScript and TypeScript code generation must prevent object-oriented architecture by default.

If class-based patterns appear, the system must:

1. detect the violation
2. reject the design
3. rewrite it using functional architecture

Preferred architecture:

- functions
- closures
- factory functions
- composition
- module-based design

Forbidden architecture:

- classes
- inheritance
- constructor services
- static utility classes