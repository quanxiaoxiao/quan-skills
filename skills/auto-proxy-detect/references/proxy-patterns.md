# Proxy Configuration Patterns

Reference patterns for various tools and scenarios.

## Quick Reference Cheat Sheet

| Tool | Proxy Config |
|------|--------------|
| **Environment** | `http_proxy`, `https_proxy`, `HTTP_PROXY`, `HTTPS_PROXY` |
| **npm** | `npm config set proxy http://127.0.0.1:7890` |
| **yarn** | `yarn config set proxy http://127.0.0.1:7890` |
| **pnpm** | `pnpm config set proxy http://127.0.0.1:7890` |
| **pip** | `pip install --proxy http://127.0.0.1:7890` |
| **apt** | `apt -o Acquire::http::Proxy="http://127.0.0.1:7890"` |
| **curl** | `curl -x http://127.0.0.1:7890` |
| **wget** | `wget -e http_proxy=http://127.0.0.1:7890` |
| **docker (pull)** | Set env vars before `docker pull` |
| **docker (container)** | `-e http_proxy=http://host.docker.internal:7890` |

## Complete Detection & Setup Script

```bash
#!/bin/bash
# Auto proxy detection and setup

detect_proxy() {
  # Try multiple detection methods
  if command -v lsof &> /dev/null; then
    lsof -i :7890 2>/dev/null
  elif command -v netstat &> /dev/null; then
    netstat -an 2>/dev/null | grep 7890
  elif command -v ss &> /dev/null; then
    ss -tuln 2>/dev/null | grep 7890
  else
    # Fallback: try to connect
    (echo > /dev/tcp/127.0.0.1/7890) 2>/dev/null && echo "127.0.0.1:7890"
  fi
}

setup_proxy_env() {
  export http_proxy=http://127.0.0.1:7890
  export https_proxy=http://127.0.0.1:7890
  export HTTP_PROXY=http://127.0.0.1:7890
  export HTTPS_PROXY=http://127.0.0.1:7890
  export no_proxy=localhost,127.0.0.1,::1
  export NO_PROXY=localhost,127.0.0.1,::1
}

# Main
PROXY_RESULT=$(detect_proxy)
if [ -n "$PROXY_RESULT" ]; then
  setup_proxy_env
  echo "✓ Proxy configured via 127.0.0.1:7890"
else
  echo "✗ No proxy detected on port 7890"
fi
```

## Docker-Specific Patterns

### Pattern 1: Docker Pull with Proxy

```bash
# Detect and set
PROXY_AVAILABLE=$(lsof -i :7890 2>/dev/null)
if [ -n "$PROXY_AVAILABLE" ]; then
  export http_proxy=http://127.0.0.1:7890
  export https_proxy=http://127.0.0.1:7890
fi

docker pull ubuntu:latest
```

### Pattern 2: Docker Run with Proxy

```bash
# For accessing host proxy from container
docker run \
  -e http_proxy=http://host.docker.internal:7890 \
  -e https_proxy=http://host.docker.internal:7890 \
  -e no_proxy=localhost,127.0.0.1 \
  ubuntu:latest apt update
```

### Pattern 3: Docker Build with Proxy

```dockerfile
# Dockerfile with build args
ARG http_proxy
ARG https_proxy
ENV http_proxy=$http_proxy
ENV https_proxy=$https_proxy

RUN apt update && apt install -y curl
```

```bash
# Build with proxy args (if detected)
BUILD_ARGS=""
PROXY_AVAILABLE=$(lsof -i :7890 2>/dev/null)
if [ -n "$PROXY_AVAILABLE" ]; then
  BUILD_ARGS="--build-arg http_proxy=http://127.0.0.1:7890 --build-arg https_proxy=http://127.0.0.1:7890"
fi

docker build $BUILD_ARGS -t myimage .
```

## Node.js Package Managers

### npm

```bash
# One-time config
npm config set proxy http://127.0.0.1:7890
npm config set https-proxy http://127.0.0.1:7890

# Or one-off with env vars
http_proxy=http://127.0.0.1:7890 https_proxy=http://127.0.0.1:7890 npm install
```

### yarn

```bash
yarn config set proxy http://127.0.0.1:7890
yarn config set https-proxy http://127.0.0.1:7890
```

### pnpm

```bash
pnpm config set proxy http://127.0.0.1:7890
pnpm config set https-proxy http://127.0.0.1:7890
```

## Linux Package Managers

### apt/apt-get

```bash
# Method 1: Command line option
apt update -o Acquire::http::Proxy="http://127.0.0.1:7890"
apt install -o Acquire::http::Proxy="http://127.0.0.1:7890" -y git

# Method 2: Environment variables
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890
apt update && apt install -y git

# Method 3: Config file (persistent)
echo 'Acquire::http::Proxy "http://127.0.0.1:7890";' > /etc/apt/apt.conf.d/proxy
```

### dnf/yum

```bash
# /etc/dnf/dnf.conf or /etc/yum.conf
proxy=http://127.0.0.1:7890
```

## Python

### pip

```bash
# Command line
pip install --proxy http://127.0.0.1:7890 requests

# Environment
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890
pip install requests

# Config file (~/.pip/pip.conf)
[global]
proxy = http://127.0.0.1:7890
```

### conda

```bash
conda config --set proxy_servers.http http://127.0.0.1:7890
conda config --set proxy_servers.https http://127.0.0.1:7890
```

## Git

```bash
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890

# Or per-repo
git config http.proxy http://127.0.0.1:7890
```

## One-Liners for Quick Use

```bash
# Detect and export in one line
(lsof -i :7890 || netstat -an | grep 7890) 2>/dev/null && export http_proxy=http://127.0.0.1:7890 https_proxy=http://127.0.0.1:7890

# With fallback to /dev/tcp
({ lsof -i :7890 || netstat -an | grep 7890 || (echo > /dev/tcp/127.0.0.1/7890 && echo "proxy"); } 2>/dev/null) && export http_proxy=http://127.0.0.1:7890 https_proxy=http://127.0.0.1:7890
```
