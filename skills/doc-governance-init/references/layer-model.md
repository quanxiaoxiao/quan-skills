# Documentation Layer Model

This reference defines the five-layer documentation governance model. Agents must classify every documentation file into exactly one layer.

## Layer Definitions

### Layer 1 — Entry (`README.md`)

- **Purpose**: First thing a human reads. Project identity, quick start, navigation.
- **Content**: Project name, one-line description, setup instructions, link to `docs/` for detail.
- **Rules**:
  - Keep under 200 lines.
  - Do not embed architecture, API details, or agent instructions.
  - Link to deeper docs, never duplicate them.
- **Owner**: Humans (agents may propose edits, humans approve).

### Layer 2 — Collaboration Contract (`AGENTS.md`)

- **Purpose**: Shared behavioral rules that all coding agents must follow.
- **Content**: Code style rules, commit conventions, testing requirements, documentation update rules, forbidden patterns.
- **Rules**:
  - Tool-agnostic. No Claude-specific, Codex-specific, or OpenCode-specific instructions.
  - Reference `docs/` for architecture explanations instead of embedding them.
  - Keep actionable: rules, not essays.
  - One file at repo root. No splitting.
- **Owner**: Humans define rules, agents follow them.

### Layer 3 — System Knowledge (`docs/`)

- **Purpose**: Authoritative source of project architecture, workflows, operational constraints, and system behavior.
- **Content**: Architecture docs, data flow diagrams (as text), API contracts, deployment workflows, operational runbooks, decision records.
- **Rules**:
  - This is the single source of truth for how the system works.
  - Other layers reference into `docs/`, never the reverse.
  - Organize by topic, not by tool or audience.
  - Suggested files:
    - `docs/architecture.md` — system structure, component relationships
    - `docs/workflows.md` — development, deployment, release processes
    - `docs/constraints.md` — operational limits, compliance, SLAs
    - `docs/decisions/` — architecture decision records (ADRs)
- **Owner**: Shared (humans and agents both contribute, humans approve structural changes).

### Layer 4 — Tool-Specific Guidance (`agents/`)

- **Purpose**: Instructions that only apply to a specific tool.
- **Content**: Tool-specific configuration, model parameters, prompt templates, tool-specific workflows.
- **Rules**:
  - Must not contain project architecture or business logic explanations.
  - Must reference `docs/` for any system knowledge.
  - One subdirectory or file per tool:
    - `agents/claude.md` or `CLAUDE.md`
    - `agents/codex.md` or `CODEX.md`
    - `agents/opencode.md` or `.opencode/`
  - If a rule applies to all agents, it belongs in `AGENTS.md`, not here.
- **Owner**: Per-tool maintainer.

### Layer 5 — Reusable Prompts (`prompts/`)

- **Purpose**: Canned prompts for recurring documentation and maintenance tasks.
- **Content**: Audit prompts, alignment check prompts, initialization prompts.
- **Rules**:
  - Prompts are tool-agnostic (any agent can run them).
  - Prompts reference the layer model, not specific file paths that may change.
  - Prompts should be self-contained: include context, instructions, and expected output format.
- **Owner**: Shared.

## Boundary Rules

1. **No upward references**: Lower layers (3–5) never depend on higher layers (1–2) for truth. Layer 3 is the authority.
2. **No cross-layer duplication**: If architecture is explained in `docs/architecture.md`, `AGENTS.md` says "see docs/architecture.md", not a summary.
3. **No tool lock-in**: If removing all `agents/` files would lose project knowledge, that knowledge is in the wrong layer.
4. **Prompt independence**: Prompts in Layer 5 should work regardless of which agent runs them.

## Classification Decision Tree

```
Is it a human-first overview?
  → Layer 1 (README.md)

Is it a behavioral rule for all agents?
  → Layer 2 (AGENTS.md)

Is it knowledge about how the system works?
  → Layer 3 (docs/)

Is it specific to one tool's configuration or behavior?
  → Layer 4 (agents/)

Is it a reusable prompt for maintenance tasks?
  → Layer 5 (prompts/)

None of the above?
  → Likely does not belong in documentation. Consider removing.
```
