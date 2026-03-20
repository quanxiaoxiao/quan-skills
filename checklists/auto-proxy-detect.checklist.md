# Auto Proxy Detect Checklist

Before finalizing a proxy diagnosis, verify:

## Diagnosis Steps

- [ ] Port 7890 listener detection was attempted
- [ ] HTTP proxy probe was run (or skipped with reason)
- [ ] HTTPS CONNECT probe was run (or skipped with reason)
- [ ] Target-specific probe was run when the target is known
- [ ] Execution mode was chosen based on probe results, not assumed

## Safety

- [ ] Proxy env vars use ephemeral injection, not persistent config changes
- [ ] `no_proxy` / `NO_PROXY` includes `localhost,127.0.0.1,::1,host.docker.internal`
- [ ] Both lowercase and uppercase env var forms are set (`http_proxy` + `HTTP_PROXY`)

## Output Quality

- [ ] Failure stage is classified (e.g., `proxy_not_listening`, `dns_failed`, `tls_handshake_failed`)
- [ ] Recommended next action is specific and actionable
- [ ] The actual command to run is provided, not just a diagnosis
