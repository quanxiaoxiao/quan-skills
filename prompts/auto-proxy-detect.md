# Auto Proxy Detect

> Authoritative skill: `skills/auto-proxy-detect/`
> Related checklist: `checklists/auto-proxy-detect.checklist.md`

## Task

Diagnose local proxy availability, validate HTTP/HTTPS proxy behavior, probe the actual target, and recommend the safest execution path for a network-dependent command.

## Steps

1. Detect whether a local proxy listener exists on port 7890.
   Use `lsof -i :7890` or `ss -tuln | grep 7890`.

2. If a listener is found, validate basic HTTP proxy behavior.
   `curl -x http://127.0.0.1:7890 -I http://example.com --max-time 5`

3. Validate HTTPS CONNECT behavior.
   `curl -x http://127.0.0.1:7890 -I https://example.com --max-time 8`

4. If the actual target is known, probe it specifically.
   Use HEAD requests, registry pings, or lightweight GET depending on the target (npmjs, docker, GitHub, etc.).

5. Choose the execution mode:
   - **Direct** — no proxy needed, target reachable directly
   - **Proxy Ephemeral** — one-off env var injection
   - **Proxy + Target-Aware** — proxy confirmed and target probe passed
   - **Mirror / Alternate Source** — proxy unavailable, use registry mirror
   - **Verbose Debug Fetch** — all probes failed, run with verbose output for diagnosis

6. Apply the chosen mode to the user's command using ephemeral env injection (not persistent config).

## Output

Return a structured diagnosis:

1. **Proxy Status** — listener detected, HTTP probe, HTTPS probe results
2. **Target Probe** — result for the specific target if applicable
3. **Execution Mode** — which mode was selected and why
4. **Command** — the actual command to run with proxy env if needed
5. **Failure Stage** — if something failed, classify the stage
