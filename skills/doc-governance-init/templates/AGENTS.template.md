# AGENTS.md

Shared rules for all coding and documentation agents working in this repository.

## Scope

These rules apply to every agent (Claude, Codex, OpenCode, or any future tool). Tool-specific instructions belong in `agents/`, not here.

## Code Style

- {LANGUAGE_STYLE_RULES}
- {FORMATTING_RULES}
- {NAMING_CONVENTIONS}

## Commit Conventions

- {COMMIT_FORMAT, e.g., conventional commits}
- {SCOPE_RULES}

## Testing Requirements

- {WHEN_TO_ADD_TESTS}
- {TEST_COMMANDS}

## Documentation Rules

- Project architecture lives in `docs/`. Reference it; do not duplicate it.
- Update documentation when changing behavior described in `docs/`.
- Do not add speculative documentation. Document what exists.
- When creating new docs, classify them by layer (see docs/architecture.md for the layer model).

## Forbidden Patterns

- {PATTERNS_TO_AVOID}

## System Knowledge

For architecture, workflows, and operational constraints, see:

- [Architecture](docs/architecture.md)
- [Workflows](docs/workflows.md)
