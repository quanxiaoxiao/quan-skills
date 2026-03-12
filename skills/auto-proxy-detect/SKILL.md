---
name: auto-proxy-detect
description: Automatically detect and configure HTTP proxy on port 7890 for network operations (docker pull, apt install, npm install, etc.)
---

# Auto Proxy Detect

Automatically detect local HTTP proxy on port 7890 and configure it for all network operations.

## Use This Skill

Use this skill when:
- Running `docker pull` commands
- Running `apt install` or `apt-get` commands inside containers
- Running `npm install`, `yarn install`, or `pnpm install`
- Running `pip install` or other package managers
- Any network operations that might benefit from a proxy
- Working in environments where a proxy (e.g., Clash, V2Ray) is running on localhost:7890

Do NOT use this skill when:
- The network operation is explicitly offline/local only
- You know for certain no proxy is available or needed
- The operation is a simple local file manipulation

## Pre-Read Order

1. Check if the task involves any network operations listed above
2. Review the specific commands to be executed
3. Identify which package managers/tools will be used

## Workflow

### 1. Always detect proxy first

**Before ANY network operation**, first check if port 7890 is listening:

```bash
# For macOS/Linux
lsof -i :7890 || netstat -an | grep 7890 || ss -tuln | grep 7890
```

### 2. If proxy is detected, configure accordingly

Set these environment variables **before** running network commands:

```bash
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890
export HTTP_PROXY=http://127.0.0.1:7890
export HTTPS_PROXY=http://127.0.0.1:7890
export no_proxy=localhost,127.0.0.1,::1
export NO_PROXY=localhost,127.0.0.1,::1
```

### 3. Tool-specific configurations

#### Docker

For `docker pull`:
```bash
# Option 1: Set proxy via environment
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890
docker pull <image>

# Option 2: For Docker daemon (if needed)
# Create/modify /etc/docker/daemon.json
```

For Docker containers (runtime proxy):
```bash
docker run -e http_proxy=http://host.docker.internal:7890 \
           -e https_proxy=http://host.docker.internal:7890 \
           <image>
```

#### npm/yarn/pnpm

```bash
# npm
npm config set proxy http://127.0.0.1:7890
npm config set https-proxy http://127.0.0.1:7890
npm install

# yarn
yarn config set proxy http://127.0.0.1:7890
yarn config set https-proxy http://127.0.0.1:7890
yarn install

# pnpm
pnpm config set proxy http://127.0.0.1:7890
pnpm config set https-proxy http://127.0.0.1:7890
pnpm install
```

#### apt/apt-get (inside containers)

```bash
# For Debian/Ubuntu containers
apt update -o Acquire::http::Proxy="http://127.0.0.1:7890"
apt install -o Acquire::http::Proxy="http://127.0.0.1:7890" -y <packages>

# Or set via environment for the entire session
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890
apt update && apt install -y <packages>
```

#### pip

```bash
pip install --proxy http://127.0.0.1:7890 <package>

# Or set via environment
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890
pip install <package>
```

#### curl/wget

```bash
curl -x http://127.0.0.1:7890 <url>
wget -e http_proxy=http://127.0.0.1:7890 <url>
```

### 4. Detection command pattern

Always run this check **first** in a bash command sequence:

```bash
# Proxy detection one-liner
PROXY_AVAILABLE=$(lsof -i :7890 2>/dev/null || netstat -an 2>/dev/null | grep 7890 || ss -tuln 2>/dev/null | grep 7890 || true)
if [ -n "$PROXY_AVAILABLE" ]; then
  export http_proxy=http://127.0.0.1:7890
  export https_proxy=http://127.0.0.1:7890
  export HTTP_PROXY=http://127.0.0.1:7890
  export HTTPS_PROXY=http://127.0.0.1:7890
  echo "Proxy detected on port 7890, configured environment variables"
else
  echo "No proxy detected on port 7890"
fi
```

### 5. For Docker containers (special case)

When running commands inside Docker containers:
- Use `host.docker.internal` instead of `127.0.0.1` to access host
- Or use `--network=host` (Linux only)

## Implementation Rules

1. **Always detect first**: Never assume proxy is present or absent
2. **Graceful fallback**: If detection fails, proceed without proxy
3. **No silent failures**: If proxy is configured but not working, still attempt the operation
4. **Keep it optional**: Proxy configuration should be additive, not required
5. **Document proxy usage**: Mention in output whether proxy was used
6. **Cleanup not needed**: Environment variables only affect the current session

## Verification

1. Verify proxy detection works:
   ```bash
   lsof -i :7890
   ```

2. Verify environment variables are set when proxy is present:
   ```bash
   echo $http_proxy
   echo $https_proxy
   ```

3. Test a network operation with proxy configured

## Output Contract

When using this skill, include in your response:
1. Whether proxy was detected on port 7890
2. Which proxy configuration method was used
3. The network operation result (success/failure)

## References

- [Proxy Configuration Patterns](./references/proxy-patterns.md)
