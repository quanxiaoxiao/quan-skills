# Diagnosis Stages

This document defines the main failure stages for network diagnosis.

---

## Stage: proxy_not_listening

Meaning:
- nothing is listening on `127.0.0.1:7890`

Typical symptoms:
- `curl: (7) Failed to connect to 127.0.0.1 port 7890`
- `ECONNREFUSED 127.0.0.1:7890`

Likely next action:
- start the local proxy service
- verify correct port
- verify the tool is using the intended proxy endpoint

---

## Stage: proxy_protocol_unusable

Meaning:
- something listens on `7890`, but it does not behave like a usable HTTP proxy

Typical symptoms:
- invalid proxy response
- immediate disconnect
- malformed HTTP behavior

Likely next action:
- verify proxy type
- verify whether the listener is actually HTTP proxy and not another protocol
- probe with curl explicitly

---

## Stage: proxy_https_connect_failed

Meaning:
- the proxy accepts requests but HTTPS CONNECT fails

Typical symptoms:
- `Proxy CONNECT aborted`
- CONNECT timeout
- CONNECT refused
- 407-like behavior from proxy

Likely next action:
- verify upstream route
- verify proxy supports HTTPS CONNECT
- verify proxy auth requirements
- debug with `curl -v -x ... https://target`

---

## Stage: dns_failed

Meaning:
- hostname resolution failed

Typical symptoms:
- `Could not resolve host`
- `ENOTFOUND`
- `Temporary failure in name resolution`

Likely next action:
- verify DNS path
- compare direct vs proxy behavior
- verify proxy DNS mode if applicable

---

## Stage: tcp_connect_failed

Meaning:
- TCP connection to target failed

Typical symptoms:
- timeout
- connection refused
- no route to host
- network unreachable

Likely next action:
- verify route
- verify firewall
- verify target host/port
- compare direct vs proxy

---

## Stage: tls_handshake_failed

Meaning:
- TCP connected, but TLS handshake failed

Typical symptoms:
- `SSL routines`
- `certificate verify failed`
- `self signed certificate`
- handshake timeout
- unknown CA

Likely next action:
- inspect cert chain
- verify trust store
- verify MITM behavior if present
- compare curl / Node / GUI behavior

---

## Stage: http_status_error

Meaning:
- request reached target, but the target returned an error status

Typical symptoms:
- 401
- 403
- 404
- 429
- 5xx

Likely next action:
- inspect auth
- inspect rate limit
- inspect endpoint correctness
- inspect mirror or alternate path

---

## Stage: proxy_auth_required

Meaning:
- proxy requires authentication

Typical symptoms:
- 407
- `Proxy Authentication Required`

Likely next action:
- add proxy auth
- verify proxy credentials
- verify whether the local proxy is really intended for unauthenticated local use

---

## Stage: tool_proxy_not_applied

Meaning:
- proxy is healthy, but the actual tool does not seem to be using it

Typical symptoms:
- curl works through proxy
- target tool still fails
- process connections do not show traffic to `127.0.0.1:7890`

Likely next action:
- verify subprocess env inheritance
- verify GUI vs CLI difference
- use one-off tool-native proxy config
- inspect actual process connections

---

## Stage: upstream_blocked_or_rate_limited

Meaning:
- the network path works, but upstream service is blocking, throttling, or unstable

Typical symptoms:
- 403
- 429
- intermittent 5xx
- CDN-specific failures

Likely next action:
- retry later
- use mirror
- use cached artifact
- reduce fetch volume
