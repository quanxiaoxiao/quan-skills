# Node Platform Scope Rule

Defines the supported operating system scope and platform assumptions for Node.js projects.

---

## Scope

Supported environments:

- macOS
- Linux

Out of scope by default:

- Windows

Do not introduce Windows compatibility work unless explicitly requested.

---

## Platform Assumptions

When generating or modifying Node.js code, scripts, or project setup:

- Prefer solutions that work on both macOS and Linux
- Shell examples may assume `bash` or `zsh`
- System tools may assume the availability of:
  - `curl`
  - `openssl`
  - `jq`
  - `grep`
  - `sed`
  - `awk`
  - `find`
  - `xargs`

If a command is macOS-specific or Linux-specific, label it clearly.

---

## Cross-Platform Preference

Within the macOS + Linux scope:

- Prefer Node.js built-in APIs over shell-specific tricks when practical
- Prefer portable shell usage over distro-specific or OS-specific commands
- Avoid unnecessary coupling to one specific Linux distribution
- Avoid platform-specific filesystem assumptions unless required by the task

---

## Disallowed Assumptions

Do not assume:

- PowerShell
- Windows path conventions
- Windows-only package behavior
- CMD or `.bat` scripts as default examples

---

## Guidance

Preferred priority order:

1. Node.js built-in runtime capability
2. Portable shell commands for macOS + Linux
3. Project-local tools already in use
4. New dependencies only when necessary

This project standard optimizes for practical local development and low-maintenance automation in Unix-like environments.
