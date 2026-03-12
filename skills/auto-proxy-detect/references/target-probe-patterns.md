# Target Probe Patterns

Use these patterns to probe real targets before expensive network operations.

---

## Principles

- prefer HEAD or lightweight requests
- probe the actual target if known
- do not begin with a large download if a smaller validation can be done first
- compare direct vs proxy when diagnosis is unclear

---

## Generic HTTP Probe

### direct
```bash
curl -I http://example.com --max-time 5
```

### via proxy
```bash
curl -I -x http://127.0.0.1:7890 http://example.com --max-time 5
```

---

## Generic HTTPS Probe

### direct
```bash
curl -I https://example.com --max-time 8
```

### via proxy
```bash
curl -I -x http://127.0.0.1:7890 https://example.com --max-time 8
```

---

## Registry Probes

### npm
```bash
curl -I https://registry.npmjs.org --max-time 8
curl -I -x http://127.0.0.1:7890 https://registry.npmjs.org --max-time 8
```

### docker registry
```bash
curl -I https://registry-1.docker.io/v2/ --max-time 8
curl -I -x http://127.0.0.1:7890 https://registry-1.docker.io/v2/ --max-time 8
```

### GitHub API
```bash
curl -I https://api.github.com --max-time 8
curl -I -x http://127.0.0.1:7890 https://api.github.com --max-time 8
```

### raw.githubusercontent
```bash
curl -I https://raw.githubusercontent.com --max-time 8
curl -I -x http://127.0.0.1:7890 https://raw.githubusercontent.com --max-time 8
```

---

## Small File Probe

Use a small stable file or endpoint instead of a large download.

```bash
curl -L -o /dev/null -s -w "%{http_code} %{time_total}\n" https://example.com/small-file
curl -L -o /dev/null -s -w "%{http_code} %{time_total}\n" -x http://127.0.0.1:7890 https://example.com/small-file
```

---

## Debug Probe

When diagnosis is incomplete:

```bash
curl -v https://target
curl -v -x http://127.0.0.1:7890 https://target
```

Use this when you need to inspect:
- redirects
- CONNECT behavior
- TLS issues
- response headers
- auth failures

---

## Decision Hints

* if direct works and proxy fails: use direct unless policy requires proxy
* if proxy works and direct fails: use proxy
* if both fail but with different errors: classify failure stage, do not guess
* if target-specific probe fails but generic probe works: the issue is likely target-specific
