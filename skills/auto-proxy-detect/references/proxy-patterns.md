# Proxy Configuration Patterns

This document lists practical proxy configuration patterns for runtime command execution.

Default preference:
1. one-off env injection
2. one-off command flags
3. persistent config only when explicitly needed

---

## Quick Reference

| Tool | Recommended Default |
|------|---------------------|
| Environment | `http_proxy`, `https_proxy`, `HTTP_PROXY`, `HTTPS_PROXY` |
| curl | `curl -x http://127.0.0.1:7890 <url>` |
| wget | `wget -e http_proxy=http://127.0.0.1:7890 <url>` |
| npm | `http_proxy=... https_proxy=... npm install` |
| yarn | `http_proxy=... https_proxy=... yarn install` |
| pnpm | `http_proxy=... https_proxy=... pnpm install` |
| pip | `http_proxy=... https_proxy=... pip install <pkg>` |
| apt | `apt -o Acquire::http::Proxy=...` |
| git | `git -c http.proxy=... -c https.proxy=... clone ...` |
| docker pull | `http_proxy=... https_proxy=... docker pull ...` |
| docker build | `docker build --build-arg http_proxy=... --build-arg https_proxy=...` |
| docker run | `-e http_proxy=http://host.docker.internal:7890` |

---

## One-Off Environment Injection

```bash
http_proxy=http://127.0.0.1:7890 https_proxy=http://127.0.0.1:7890 <command>
```

Examples:

```bash
http_proxy=http://127.0.0.1:7890 https_proxy=http://127.0.0.1:7890 npm install
http_proxy=http://127.0.0.1:7890 https_proxy=http://127.0.0.1:7890 pnpm install
http_proxy=http://127.0.0.1:7890 https_proxy=http://127.0.0.1:7890 pip install requests
http_proxy=http://127.0.0.1:7890 https_proxy=http://127.0.0.1:7890 docker pull ubuntu:latest
```

---

## curl / wget

### Standard

```bash
curl -x http://127.0.0.1:7890 <url>
wget -e http_proxy=http://127.0.0.1:7890 <url>
```

### Debug

```bash
curl -v -x http://127.0.0.1:7890 <url>
curl -I -x http://127.0.0.1:7890 <url>
curl -I -x http://127.0.0.1:7890 https://example.com --max-time 8
```

---

## git

### Recommended one-off

```bash
git -c http.proxy=http://127.0.0.1:7890 -c https.proxy=http://127.0.0.1:7890 clone <repo>
```

### Optional persistent

```bash
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890
```

---

## npm / yarn / pnpm

### Recommended one-off

```bash
http_proxy=http://127.0.0.1:7890 https_proxy=http://127.0.0.1:7890 npm install
http_proxy=http://127.0.0.1:7890 https_proxy=http://127.0.0.1:7890 yarn install
http_proxy=http://127.0.0.1:7890 https_proxy=http://127.0.0.1:7890 pnpm install
```

### Optional persistent

```bash
npm config set proxy http://127.0.0.1:7890
npm config set https-proxy http://127.0.0.1:7890

yarn config set proxy http://127.0.0.1:7890
yarn config set https-proxy http://127.0.0.1:7890

pnpm config set proxy http://127.0.0.1:7890
pnpm config set https-proxy http://127.0.0.1:7890
```

---

## Docker

### docker pull

```bash
http_proxy=http://127.0.0.1:7890 https_proxy=http://127.0.0.1:7890 docker pull <image>
```

### docker build

```bash
docker build \
  --build-arg http_proxy=http://127.0.0.1:7890 \
  --build-arg https_proxy=http://127.0.0.1:7890 \
  -t <image> .
```

### docker run

```bash
docker run \
  -e http_proxy=http://host.docker.internal:7890 \
  -e https_proxy=http://host.docker.internal:7890 \
  -e no_proxy=localhost,127.0.0.1,host.docker.internal \
  <image>
```

---

## apt

### Recommended command-scoped

```bash
apt update -o Acquire::http::Proxy="http://127.0.0.1:7890"
apt install -o Acquire::http::Proxy="http://127.0.0.1:7890" -y git
```

### Optional session-scoped

```bash
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890
apt update && apt install -y git
```

---

## pip

### Recommended

```bash
http_proxy=http://127.0.0.1:7890 https_proxy=http://127.0.0.1:7890 pip install requests
```

### Optional

```bash
pip install --proxy http://127.0.0.1:7890 requests
```

---

## Notes

* Prefer one-off usage unless persistence is explicitly needed
* `host.docker.internal` is usually required inside containers
* local listener detection alone is not enough; runtime probing still matters
