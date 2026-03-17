# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

A personal skill pack (`~/.codex/skills/quan-skills`) containing reusable AI instruction modules for consistent TypeScript/Node.js development.

## Shared Agent Rules

All coding principles, conventions, and the active skill list are defined in [AGENTS.md](AGENTS.md). Read and follow those rules.

## Skill Authoring

For how to create, name, and maintain skills, see [docs/skill-authoring.md](docs/skill-authoring.md).

## Claude-Specific Guidance

- This file (`CLAUDE.md`) is auto-loaded by Claude Code at conversation start.
- `SKILL.md` is the Codex CLI entry point — do not merge its content here.
- `.opencode/` is the OpenCode tool config — do not modify without user request.
- When routing to a skill, check `SKILL.md` for the routing table or `AGENTS.md` for the full skill list.
