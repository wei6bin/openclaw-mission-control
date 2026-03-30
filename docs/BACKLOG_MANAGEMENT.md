# Backlog Management Guide

This guide documents backlog prioritization, grooming, and health metrics for the SDLC Engineering team.

## Backlog Structure

### Hierarchy

| Level | Description | Example | Size |
|-------|-------------|---------|------|
| Epic | Large initiative, may span multiple sprints | "Implement user authentication" | 21–∞ points |
| Feature | Logical grouping of user stories | "Email/password login" | 13–21 points |
| User Story | Single deliverable from user perspective | "User can reset password" | 1–8 points |
| Task | Technical breakdown of a story | "Add password validation regex" | 1–3 points |
| Spike | Time-boxed research/investigation | "Evaluate 2FA libraries" | 1–3 points |

### Story Format (Given/When/Then)

```
As a [role]
I want [action]
So that [benefit]

Acceptance Criteria:
- [ ] [Criterion 1]
- [ ] [Criterion 2]

Effort estimate: [X] points
```

---

## Backlog Grooming Process

### Frequency

Weekly, 1 hour, on Wednesday.

### Participants

- Dev Lead (facilitator)
- Product Owner (prioritization authority)
- Full team (estimation)

### Grooming Agenda

1. **Review top 10 stories** (30 min)
   - Clarify requirements
   - Add acceptance criteria if missing
   - Break down large items

2. **Estimate new stories** (20 min)
   - Planning poker or T-shirt sizing
   - Flag any story >8 points for breakdown

3. **Prioritize** (10 min)
   - Adjust order based on new info
   - Confirm with Product Owner

### Definition of Ready for Grooming

Before a story enters grooming:
- [ ] User story format complete (As a / I want / So that)
- [ ] Acceptance criteria drafted
- [ ] Rough size estimate (>13 points = needs breakdown)
- [ ] Design/UX reference attached (if applicable)
- [ ] Dependencies noted

---

## MoSCoW Prioritization

### Definitions

| Priority | Definition | Target Allocation |
|----------|------------|-------------------|
| **Must have** | Sprint fails without this. Non-negotiable. | 60% of sprint |
| **Should have** | Significant impact. Can defer to next sprint if needed. | 20% of sprint |
| **Could have** | Desirable but not critical. Grab-bag of nice-to-haves. | 15% of sprint |
| **Won't have** | Explicitly excluded from current scope. Revisit later. | 5% of backlog |

### Practical Rules

1. **No sprint should be >80% "Must have"** — leaves no buffer for defects or unexpected work
2. **"Should have" stories promote to "Must" only via Product Owner + Dev Lead agreement**
3. **"Won't have" stories go to a parking lot, not the bottom of the backlog**

### Prioritization Checklist

Before finalizing sprint scope, verify:
- [ ] Top of backlog is 100% "Must have" or "Should have"
- [ ] No critical story is blocked by a lower-priority story
- [ ] Dependencies are respected (if B depends on A, A must precede B)
- [ ] Capacity matches commitment (velocity-based)

---

## Epic & Feature Breakdowns

### When to Break an Epic

Break an epic when:
- It is >21 story points
- It involves multiple teams
- Dependencies span multiple sprints
- It cannot be demo'd in one sprint

### Breaking Down Approach

```
Epic: "User authentication system"
  ├── Feature: "Email/password login"
  │   ├── Story: "User can register with email"
  │   ├── Story: "User can log in with credentials"
  │   ├── Story: "User can log out"
  │   └── Story: "User receives welcome email"
  ├── Feature: "Password recovery"
  │   ├── Story: "User can reset password via email"
  │   ├── Story: "User receives password reset link"
  │   └── Story: "Reset link expires after 1 hour"
  └── Feature: "2FA for admin"
      ├── Story: "Admin can enable 2FA"
      └── Story: "User cannot login without 2FA code"
```

### Theme Tagging

Group stories by theme for better tracking:
- `theme:auth` — authentication and identity
- `theme:payments` — payment processing
- `theme:onboarding` — new user experience
- `theme:performance` — speed and reliability
- `theme:tech-debt` — refactoring and infrastructure

---

## Backlog Health Metrics

### Velocity

```
Sprint Velocity = Total story points completed in a sprint
```

| Metric | Calculation | Healthy Range |
|--------|-------------|---------------|
| Velocity (per sprint) | Sum of done story points | Track trend over 5 sprints |
| Average velocity | Rolling 3-sprint average | ±15% indicates stability |
| Velocity variance | Std deviation of last 5 sprints | <10% = consistent team |

**Use velocity for:** Sprint capacity planning, not pressure.

---

### Backlog Age

```
Story Age = (Current Date) - (Date story entered backlog)
```

| Age | Status | Action |
|-----|--------|--------|
| 0–7 days | Fresh | Normal |
| 8–14 days | Aging | Groom next |
| 15–30 days | Stale | Prioritize or archive |
| >30 days | Zombie | Review: valid or remove |

**Target:** <20% of backlog stories older than 14 days.

---

### Size Distribution

Check story size histogram from last 5 sprints:

| Size | Ideal % | Warning |
|------|---------|---------|
| 1–2 points | 30–40% | Fine-grained, healthy |
| 3–5 points | 40–50% | Normal |
| 8 points | 10–20% | Verify these aren't disguised epics |
| >8 points | <10% | Break down immediately |

**Too many 8+ stories:** Indicative of poor grooming — break down before sprint.

---

### Coverage Ratio

```
Sprint Coverage = Committed points / Average velocity
```

| Coverage | Interpretation |
|-----------|----------------|
| 0.8–1.0x | Comfortable — room for unexpected work |
| 1.0–1.1x | Tight — little buffer |
| >1.2x | Overcommitted — expect spillover |

---

## Backlog Hygiene Rules

### General

- Delete or archive stories that are no longer relevant
- Move blocked stories out of active sprint planning
- Do not carry stories across 3+ sprints without re-grooming
- Keep acceptance criteria specific — not "as a user, I want a good experience"

### Naming Conventions

Stories should follow: `[Verb] [Subject] [Context]`

Good examples:
- "Add password reset flow"
- "Display error message on failed login"
- "Integrate Stripe for payment processing"

Bad examples:
- "Login bug" (too vague)
- "Work on payments" (not a story)
- "Fix things" (not a story)

---

## Tools Reference

| Tool | Use |
|------|-----|
| Azure DevOps | Sprint planning, backlog board |
| GitHub Projects | Lightweight backlog tracking |
| Jira | Enterprise sprint + backlog management |
| Notion | Documentation + lightweight tracking |
| Trello | Simple Kanban for small teams |

---

*Document Version: 1.0*
*Last Updated: 2026-03-30*