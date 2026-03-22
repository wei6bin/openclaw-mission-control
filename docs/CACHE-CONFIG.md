# GitHub Actions Cache Configuration

## Overview

This document describes the caching strategy used in the CI/CD pipeline to speed up builds.

---

## Current Cache Configuration

### 1. Python Dependencies (uv)

**Location**: `.github/workflows/ci.yml`

```yaml
- name: Cache uv
  uses: actions/cache@v4
  with:
    path: |
      ~/.cache/uv
      backend/.venv
    key: uv-${{ runner.os }}-${{ hashFiles('backend/uv.lock') }}
```

**Cache key**: `uv-{OS}-{hash of uv.lock}`

**Invalidation**: When `backend/uv.lock` changes

---

### 2. Node.js Dependencies (npm)

**Location**: `.github/workflows/ci.yml`

```yaml
- uses: actions/setup-node@v4
  with:
    node-version: "22"
    cache: npm
    cache-dependency-path: frontend/package-lock.json
```

**Cache key**: Automatic via `setup-node` action

**Invalidation**: When `frontend/package-lock.json` changes

---

### 3. Next.js Build Cache

**Location**: `.github/workflows/ci.yml`

```yaml
- name: Cache Next.js build cache
  uses: actions/cache@v4
  with:
    path: |
      frontend/.next/cache
    key: nextjs-${{ runner.os }}-node-${{ steps.setup-node.outputs.node-version }}-${{ hashFiles('frontend/package-lock.json') }}
    restore-keys: |
      nextjs-${{ runner.os }}-node-${{ steps.setup-node.outputs.node-version }}-
```

**Cache key**: `nextjs-{OS}-node-{version}-{hash of package-lock.json}`

**Invalidation**: When `frontend/package-lock.json` changes

---

### 4. Docker Layer Caching

**Location**: `.github/workflows/container-security.yml`

```yaml
- uses: docker/setup-buildx-action@v3

- uses: docker/build-push-action@v5
  with:
    context: ./backend
    push: false
    load: true
    tags: openclaw-mission-control-backend:scan
    cache-from: type=gha
    cache-to: type=gha,mode=max
```

**Cache type**: GitHub Actions cache (GHA)

**Mode**: `max` - caches all layers

**Invalidation**: When Dockerfiles or dependencies change

---

## Cache Invalidation Strategy

### Primary Rules

1. **Dependency files trigger invalidation**:
   - `backend/uv.lock` → Python cache
   - `frontend/package-lock.json` → npm + Next.js cache
   - `Dockerfile*` → Docker layer cache

2. **OS-specific caches**:
   - Caches are OS-specific (ubuntu-latest, macos-latest, etc.)
   - Each OS maintains separate cache

3. **Node version specific**:
   - Next.js cache includes node version
   - Upgrading Node.js invalidates cache

### Cache Retention

- **Default**: 7 days (GitHub Actions)
- **Maximum**: 90 days (with GitHub Teams/Enterprise)

---

## Cache Performance

### Expected Time Savings

| Job | Without Cache | With Cache | Savings |
|-----|--------------|------------|---------|
| `check` (Python) | ~5 min | ~2 min | 60% |
| `check` (Node) | ~3 min | ~1 min | 66% |
| `installer` (Docker) | ~10 min | ~5 min | 50% |
| `container-security` | ~15 min | ~8 min | 47% |

---

## Troubleshooting

### Cache Misses

If cache is not being used:

1. **Check cache key**: Verify the key in workflow logs
2. **Check cache hit ratio**: Look for "Cache restored" in logs
3. **Verify file paths**: Ensure paths in `cache` action match actual paths

### Common Issues

| Issue | Solution |
|-------|----------|
| Cache not restoring | Check if files exist in key path |
| Cache size too large | Use `.gitignore` or exclude directories |
| Cross-platform issues | Use separate keys per OS |

---

## Future Improvements

Potential enhancements:

1. **Selective cache restoration**: Only restore relevant caches
2. **Cache compression**: Use `compressionLevel` option
3. **Remote cache**: Use GitHub-backed cache with larger storage
4. **BuildKit cache**: Export cache to registry for reuse across machines

---

## Cache Analytics

### Viewing Cache Usage

1. Go to repository → Actions → Cache
2. View cache size and hit rates
3. Identify large caches for optimization

### Metrics to Monitor

- Cache hit rate (target: >80%)
- Average cache size (target: <5GB)
- Cache eviction frequency

---

*Last updated: 2026-03-23*
*Maintained by: SDLC Planner*
