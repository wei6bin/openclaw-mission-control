# Pipeline Monitoring Guide

## Overview

This document describes the monitoring and observability setup for the CI/CD pipeline.

---

## Current Monitoring Features

### 1. Pipeline Monitor Workflow

A new workflow (`pipeline-monitor.yml`) has been added to provide:

- **Failure Notifications**: Alerts when CI pipeline fails
- **Health Metrics**: Daily metrics on pipeline success rate and duration
- **Daily Health Check**: Scheduled check to verify CI is passing

### 2. GitHub Actions Built-in

| Feature | Location | Description |
|---------|----------|-------------|
| Workflow runs | Actions tab | View all workflow executions |
| Run duration | Each run | Time for each job and step |
| Failure logs | Each run | Detailed error information |
| Artifacts | Each run | Build outputs, test reports |

---

## Acceptance Criteria Coverage

### ✅ Pipeline Failure Alerts

The `notify-failure` job sends alerts when CI fails:
- **Current**: Logs to workflow run output
- **Future**: Can integrate with Slack (commented in workflow)

To enable Slack notifications:
1. Add `SLACK_WEBHOOK_URL` secret to repository
2. Uncomment the Slack action in `notify-failure` job

### ✅ Pipeline Health Dashboard

GitHub provides a built-in dashboard:

**URL**: https://github.com/wei6bin/openclaw-mission-control/actions

Features:
- Filter by workflow
- View success/failure rates
- See run history
- Check individual job durations

### ✅ Deployment Frequency & Success Rate

The `health-metrics` job tracks:
- Total runs in period
- Successful runs
- Success rate percentage

### ✅ Latency Metrics

Pipeline timing is available:
- Per-job duration in each run
- Average duration in health metrics job

---

## Dashboard Access

### GitHub Actions Dashboard

```
https://github.com/wei6bin/openclaw-mission-control/actions
```

### Security Tab (for scanning results)

```
https://github.com/wei6bin/openclaw-mission-control/security
```

### Code Scanning Alerts

```
https://github.com/wei6bin/openclaw-mission-control/security/code-scanning
```

---

## Alert Configuration

### Slack Integration (Optional)

To receive Slack notifications on pipeline failures:

1. Create a Slack webhook:
   - Go to your Slack workspace
   - Create an Incoming Webhook
   - Copy the webhook URL

2. Add secret to GitHub:
   - Repository → Settings → Secrets → Actions
   - Add `SLACK_WEBHOOK_URL`

3. The workflow will automatically send alerts

### Email Notifications (GitHub Default)

Configure in:
- Repository → Settings → Notifications → Actions

---

## Metrics Reference

### Key Metrics

| Metric | Target | Current |
|--------|--------|---------|
| Success Rate | >= 90% | Tracked |
| Avg Duration | < 30 min | Tracked |
| Deployment Frequency | >= 2/sprint | Manual |

### How to View Metrics

1. **Quick check**: GitHub Actions homepage shows pass/fail
2. **Detailed**: Click on individual run for timing
3. **Trends**: Run the `health-metrics` job manually

---

## Troubleshooting

### Pipeline Failing

1. Check GitHub Actions for error details
2. Review the failure job's logs
3. Check recent commits for changes

### Not Receiving Alerts

- Verify Slack webhook is configured
- Check GitHub notification settings
- Check spam/junk folders for emails

### Metrics Missing

- Ensure `pipeline-monitor.yml` is merged to main
- Check workflow runs are completing
- Verify actions:read permissions

---

## Future Enhancements

Potential improvements:

1. **Grafana Dashboard**: Custom visualization
2. **Prometheus Export**: Export metrics for external monitoring
3. **Datadog Integration**: Advanced APM
4. **PagerDuty**: On-call alerting
5. **Deployment Webhooks**: Notify on deploy completion

---

*Last updated: 2026-03-23*
*Maintained by: SDLC Planner*
