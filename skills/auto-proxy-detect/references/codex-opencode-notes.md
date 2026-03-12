# Codex / OpenCode Notes

This document explains why network diagnosis for Codex / OpenCode may differ from normal shell command behavior.

---

## Key Differences

### 1. Shell env vs GUI env

Exporting proxy vars in a shell does not guarantee a GUI application uses them.

Example:
- terminal `curl` works
- GUI-launched tool still fails

---

### 2. Parent process vs subprocess inheritance

A subprocess may or may not inherit:
- `http_proxy`
- `https_proxy`
- custom CA env
- other runtime settings

So success in one shell command is not proof that the target application will behave identically.

---

### 3. Tool-native network stacks

Different tools use different stacks:
- curl
- Node.js fetch / undici
- package manager internal logic
- Electron / Chromium networking
- docker daemon networking

Proxy behavior may differ even on the same machine.

---

### 4. Proxy works for one target but not another

Examples:
- GitHub works
- npm registry fails
- Docker auth fails
- model provider API fails

This is why target-aware probing is important.

---

## Practical Guidance

When troubleshooting Codex / OpenCode:

1. verify local proxy listener
2. verify proxy HTTP behavior
3. verify proxy HTTPS CONNECT behavior
4. verify target-specific endpoint
5. do not assume shell success equals app success
6. if needed, inspect actual process connections separately

---

## Typical Failure Patterns

- proxy env set in shell, not inherited by app
- GUI app bypasses shell exports
- package manager proxy config differs from curl behavior
- Node TLS trust differs from system trust
- Docker daemon proxy config differs from docker CLI env

---

## Recommended Mindset

Treat this skill as:
- runtime preflight diagnosis
- target reachability validation
- execution path recommendation

Do not treat it as:
- full proof that every process on the machine will use the same route
