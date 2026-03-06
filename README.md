# Quan's Skills

A curated collection of AI assistant skills for personal development and project standardization.

## Overview

This repository contains a set of reusable skill modules designed to standardize and streamline development workflows. Each skill is self-contained and can be applied to specific project types or development tasks.

## Skills

### ts-backend-standard

Standardization skill for TypeScript backend development.

**Purpose**: Standardize or review TypeScript backend repositories that use or want to use Hono, Zod, strict TypeScript, and strict ESLint.

**Use Cases**:
- Scaffolding new TypeScript backend projects
- Refactoring existing backends to follow best practices
- Auditing and tightening compiler or lint settings
- Fixing build/lint drift while preserving existing runtime choices

**Key Features**:
- Hono + Zod integration
- Strict TypeScript configuration
- Layered architecture (routes → schemas → services → models)
- Validation-first API design
- Portable, dependency-light approach

See `ts-backend-standard/SKILL.md` for detailed usage instructions.

## License

[MIT](LICENSE)