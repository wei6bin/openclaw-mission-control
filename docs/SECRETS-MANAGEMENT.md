# Secrets & Environment Management

## Overview

This guide documents how to manage secrets and environments for the CI/CD pipeline.

---

## Current Status

- **Repository Secrets**: 0 configured
- **Environments**: Not configured yet

---

## Recommended Setup

### 1. Create Environments

Create environments for different deployment stages:

1. **staging** - For staging deployments
2. **production** - For production deployments

**Via GitHub UI**:
1. Repository → Settings → Environments
2. Click "New environment"
3. Configure environment settings

**Required Environment Secrets**:

| Secret | Description | Required For |
|--------|-------------|---------------|
| `DOCKER_USERNAME` | Docker Hub username | Push images |
| `DOCKER_PASSWORD` | Docker Hub token | Push images |
| `DEPLOY_HOST` | SSH host for deployment | SSH deploy |
| `DEPLOY_SSH_KEY` | Private SSH key | SSH deploy |
| `DEPLOY_USER` | Deployment user | SSH deploy |

### 2. Add Repository Secrets

**Via GitHub UI**:
1. Repository → Settings → Secrets and variables → Actions
2. Click "New repository secret"

**Recommended Repository Secrets**:

| Secret | Description |
|--------|-------------|
| `DOCKER_USERNAME` | Docker Hub username |
| `DOCKER_PASSWORD` | Docker Hub access token |
| `SLACK_WEBHOOK_URL` | For CI failure notifications |

### 3. Configure Environment Protection

For production environment:

1. **Required reviewers**: Require approval before deployment
2. **Wait timer**: Add delay before deployment
3. **Branch policy**: Restrict which branches can deploy

---

## Secrets Best Practices

### Do ✅

- Use GitHub secrets for sensitive values
- Rotate secrets regularly
- Use environment-specific secrets
- Require approvals for production deployments

### Don't ❌

- Never commit secrets to git
- Don't use plain text in workflows
- Don't share secrets across repositories
- Don't use personal credentials

---

## Adding Secrets (Guide)

### For CI/CD Pipeline

1. **DOCKER_USERNAME** / **DOCKER_PASSWORD**
   - Create at Docker Hub: Account Settings → Security → New Access Token
   - Add to repository secrets

2. **SLACK_WEBHOOK_URL** (optional)
   - Create incoming webhook in Slack
   - Add to repository secrets

### For Deployment

1. **DEPLOY_HOST**: Server hostname/IP
2. **DEPLOY_USER**: SSH username
3. **DEPLOY_SSH_KEY**: Private key with server access

---

## Environment Variables in Workflows

### Using Secrets

```yaml
- name: Deploy to production
  env:
    DOCKER_USERNAME: ${{ secrets.DOCKER_USERNAME }}
    DOCKER_PASSWORD: ${{ secrets.DOCKER_PASSWORD }}
  run: |
    echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
```

### Using Environments

```yaml
jobs:
  deploy:
    environment: production
    runs-on: ubuntu-latest
```

---

## Current CI Configuration

The CI pipeline currently uses:

| Variable | Source |
|----------|--------|
| `AUTH_MODE` | `local` (hardcoded in workflow) |
| `LOCAL_AUTH_TOKEN` | Generated in workflow |
| `NEXT_PUBLIC_API_URL` | Local URLs (hardcoded) |

**Note**: No external secrets required for current CI pipeline.

---

## Next Steps

1. Create `staging` and `production` environments
2. Add Docker secrets for image publishing
3. Add Slack webhook for notifications (optional)
4. Configure deployment workflows with environment protection

---

*Last updated: 2026-03-23*
*Maintained by: SDLC Planner*
