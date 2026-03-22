# Deployment Runbook

## Overview

This runbook documents the deployment process and rollback procedures for the OpenClaw Mission Control CI/CD pipeline.

**Current Status**: CI pipeline active, CD deployment stages to be configured.

---

## CI Pipeline Overview

The CI pipeline runs on every push to `master` branch and pull request:

```
check → security-sast → security-deps → installer → e2e
```

### Pipeline Jobs

| Job | Description | Duration |
|-----|-------------|----------|
| `check` | Lint, typecheck, unit tests, build | ~10 min |
| `security-sast` | CodeQL SAST analysis | ~5 min |
| `security-deps` | Dependency vulnerability scan | ~3 min |
| `installer` | Docker/local mode smoke tests | ~15 min |
| `e2e` | Cypress end-to-end tests | ~10 min |

### Security Gates

- **SAST**: CodeQL scans Python and JavaScript code
- **Dependencies**: pip-audit (Python) + npm audit (Node.js)
- **Critical vulnerabilities** block the pipeline

---

## Deployment Process

### Current State

> ⚠️ **Note**: Automated deployment stages are not yet configured in the CI workflow. The following documents the target state.

### Target Deployment Flow (To Be Implemented)

```
PR Merged to master
       ↓
   Build Stage
       ↓
Deploy to Staging
       ↓
  Health Check
       ↓
Production Approval (manual)
       ↓
Deploy to Production
```

### Manual Deployment Steps (When Configured)

1. **Merge PR to master** - Triggers CI pipeline
2. **Wait for CI to pass** - All checks must pass
3. **Deploy to staging** - Automatic on merge
4. **Verify health** - Run health check endpoints
5. **Production approval** - Manual approval via GitHub Environments
6. **Deploy to production** - Automatic after approval

---

## Rollback Procedures

### If Deployment Fails

#### Automated Rollback (Future)
When deployment stages are configured, failed deployments should automatically rollback.

#### Manual Rollback Steps

1. **Identify the failure**:
   - Check GitHub Actions workflow run
   - Check application logs

2. **Rollback to previous version**:
   ```bash
   # Find last good deployment
   git log --oneline -10
   
   # Revert to previous commit
   git revert HEAD
   git push origin master
   ```

3. **Verify rollback**:
   - Check CI pipeline passes
   - Verify application health
   - Confirm previous version is running

### Rollback Decision Matrix

| Issue | Action |
|-------|--------|
| CI failure | Do not deploy, fix issues first |
| Staging health check fails | Investigate before production |
| Production health check fails | Automatic rollback |
| Critical vulnerability found | Block deployment, fix immediately |

---

## Health Check Verification

### Backend Health

```bash
curl -f http://localhost:8000/healthz
```

Expected: `{"ok":true}`

### Frontend Health

```bash
curl -f http://localhost:3000
```

Expected: HTML response with 200 status

### API Health

```bash
curl -f http://localhost:8000/api/v1/health
```

---

## Monitoring Deployment Status

### Where to Monitor

1. **GitHub Actions**: https://github.com/wei6bin/openclaw-mission-control/actions
2. **GitHub Security Tab**: https://github.com/wei6bin/openclaw-mission-control/security

### Checking Deployment Health

```bash
# Check if backend is responding
curl -s http://localhost:8000/healthz | jq

# Check running containers
docker ps

# Check recent logs
docker logs openclaw-mission-control-backend-1
```

### Alerts to Watch For

- ❌ CI check failures
- ⚠️ Security vulnerabilities detected
- 🔴 Health check failures
- 📉 Unusual error rates

---

## Useful Commands

### Trigger CI Manually

```bash
# Via GitHub CLI
gh workflow run ci.yml
```

### Check Workflow Status

```bash
gh run list --workflow=ci.yml --limit=5
```

### View Security Results

```bash
# CodeQL results are in GitHub Security tab
# Navigate to: Repo → Security → CodeQL
```

---

## Configuration

### Environment Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `AUTH_MODE` | Authentication mode | `local` |
| `LOCAL_AUTH_TOKEN` | Local auth token | (generated) |
| `NEXT_PUBLIC_API_URL` | Frontend API URL | `http://localhost:8000` |
| `NEXT_PUBLIC_AUTH_MODE` | Frontend auth mode | `local` |

### GitHub Secrets

Required for deployment (to be configured):
- `DOCKER_USERNAME`
- `DOCKER_PASSWORD`
- `DEPLOY_HOST`
- `DEPLOY_SSH_KEY`

---

## Support

### Common Issues

| Issue | Solution |
|-------|----------|
| CI timeout | Check for slow tests, optimize timeouts |
| Security scan fails | Fix vulnerabilities, update dependencies |
| Installer fails | Check Docker, verify environment |
| E2E tests fail | Check test assertions, verify app state |

### Contacts

- **SDLC Lead**: For deployment issues
- **Dev Team**: For application bugs
- **Security**: For vulnerability questions

---

*Last updated: 2026-03-23*
*Maintained by: SDLC Planner*
