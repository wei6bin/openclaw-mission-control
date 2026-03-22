# Branch Protection Rules

## Overview

This document describes the branch protection rules configured for the `master` branch.

---

## Current Configuration

### Status Checks Required

The following CI jobs must pass before merging:

| Check | Description |
|-------|-------------|
| `ci.yml/check` | Lint, typecheck, tests, build |
| `ci.yml/security-sast` | CodeQL SAST scanning |
| `ci.yml/security-deps` | Dependency vulnerability scanning |

**Note**: `require_up_to_date_branch: true` - branch must be up-to-date with master before merge.

---

## Review Requirements

| Setting | Value |
|---------|-------|
| Required approvals | 1 |
| Dismiss stale reviews | Yes |
| Require code owner review | No |
| Admin bypass | No |

---

## Protection Settings

| Setting | Enabled | Description |
|---------|---------|-------------|
| Status checks | ✅ | Requires all checks to pass |
| PR reviews | ✅ | At least 1 approval required |
| Up-to-date branch | ✅ | Must be rebased on latest |
| Admin bypass | ❌ | Admins must also follow rules |
| Force pushes | ❌ | Disabled |
| Branch deletion | ❌ | Disabled |

---

## How to Merge

1. **Create PR** from feature branch to `master`
2. **Wait for CI** - all 3 status checks must pass
3. **Get approval** - at least 1 reviewer must approve
4. **Update branch** - rebase if behind master
5. **Merge** - squash merge or merge commit

---

## Bypass Rules

Currently:
- **No bypass** - all users including admins must follow protection rules
- This can be changed in repository settings if needed

---

## Troubleshooting

### "Required status check failed"

- Check which job failed in GitHub Actions
- Fix the issue and push new commits
- Wait for CI to pass again

### "Branch is out of date"

- Rebase your branch on master: `git rebase master`
- Or merge master into your branch: `git merge master`
- Force push if needed (with care)

### "Reviews required"

- Get at least 1 approval from a reviewer
- Address any review comments

---

*Last updated: 2026-03-23*
*Maintained by: SDLC Planner*
