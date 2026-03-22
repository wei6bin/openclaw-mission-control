# Manual Workflow Dispatch

## Overview

All CI/CD workflows support manual triggering via GitHub's `workflow_dispatch` feature.

---

## Available Workflows

| Workflow | Manual Trigger | Description |
|----------|---------------|-------------|
| CI | ✅ Yes | Main CI pipeline |
| Container Security | ✅ Yes | Trivy container scanning |
| Integration Tests | ✅ Yes | API/DB integration tests |
| Pipeline Monitor | ✅ Yes | Health metrics & alerts |

---

## How to Run Manually

### Via GitHub UI

1. Go to repository → **Actions** tab
2. Select the workflow from the sidebar
3. Click **"Run workflow"** button
4. Select branch and fill any inputs
5. Click **"Run workflow"**

### Via GitHub CLI

```bash
# Run CI workflow
gh workflow run ci.yml -f branch=master

# Run container security
gh workflow run container-security.yml -f branch=master

# Run integration tests
gh workflow run integration-tests.yml -f branch=master

# Run pipeline monitor
gh workflow run pipeline-monitor.yml --repo wei6bin/openclaw-mission-control
```

---

## Workflow-Specific Inputs

### ci.yml
No required inputs. Runs on default branch.

### container-security.yml
No required inputs. Scans both backend and frontend containers.

### integration-tests.yml
No required inputs. Runs full integration test suite.

### pipeline-monitor.yml

| Input | Type | Required | Description |
|-------|------|----------|-------------|
| `report_type` | choice | No | `summary`, `failures`, or `trends` |

Example:
```bash
gh workflow run pipeline-monitor.yml -f report_type=summary
```

---

## Use Cases

### Manual CI Run
- Trigger CI without pushing code
- Useful for verifying changes to workflow files

### On-Demand Security Scan
- Run Trivy container scan without a PR
- Useful after dependency updates

### Integration Tests
- Run integration tests locally
- Useful for debugging CI issues

### Health Check
- Get current pipeline metrics on demand
- Useful for dashboard updates

---

## Notes

- Manual runs count towards GitHub Actions minutes
- Results appear in Actions tab just like automated runs
- You can cancel manual runs same as automated ones

---

*Last updated: 2026-03-23*
*Maintained by: SDLC Planner*
