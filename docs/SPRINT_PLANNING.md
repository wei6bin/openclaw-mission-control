# Sprint Planning Guide

This guide documents the sprint planning process, ceremonies, and artifacts for the SDLC Engineering team.

## Sprint Cadence

| Ceremony | Frequency | Duration | Participants |
|----------|-----------|----------|--------------|
| Sprint Planning | Bi-weekly Monday | 1–2 hours | Full team |
| Daily Standup | Weekdays | 15 min | Full team |
| Sprint Review | Bi-weekly Friday | 1 hour | Team + stakeholders |
| Sprint Retrospective | Bi-weekly Friday | 1 hour | Full team |
| Backlog Grooming | Weekly Wednesday | 1 hour | Dev lead + team |

**Sprint length:** 2 weeks (10 working days)
**Sprint start:** Monday
**Sprint end:** Friday (review) → Friday (retro)

---

## Sprint Planning

### Pre-Planning Checklist

Before the sprint planning meeting:
- [ ] Product Owner has ranked the backlog
- [ ] Stories have acceptance criteria
- [ ] Technical dependencies are identified
- [ ] Capacity known (leave, holidays, other commitments)
- [ ] Previous sprint retrospective action items reviewed

### Planning Meeting Agenda

1. **Review sprint goal proposal** (10 min)
   - Product Owner presents the sprint goal
   - Team confirms it's achievable

2. **Select stories for sprint** (30–45 min)
   - Work from top of backlog
   - Team discusses each story
   - Clarify requirements and acceptance criteria

3. **Estimate stories** (20–30 min)
   - Use planning poker or T-shirt sizing
   - Break down large stories (>8 points)

4. **Identify risks and dependencies** (10 min)
   - Note external dependencies
   - Flag technical risks
   - Assign story point owners

5. **Commit to sprint backlog** (10 min)
   - Team agrees on sprint scope
   - Product Owner confirms scope is acceptable
   - Confirm capacity matches commitment

### Sprint Planning Outputs

- Sprint goal (one sentence)
- Committed backlog (stories + points)
- Assigned story owners
- Identified blockers and dependencies

---

## Sprint Goal Template

```
Sprint [N]: [Goal Statement]

Theme: [One-line theme]

Success criteria:
- [ ] [Outcome 1]
- [ ] [Outcome 2]
- [ ] [Outcome 3]

Committed capacity: [X] story points
Team members: [names]
```

**Example:**
```
Sprint 14: Release user authentication and improve CI/CD pipeline

Theme: Foundation for secure, automated deployments

Success criteria:
- [ ] Users can log in with email/password
- [ ] 2FA available for admin accounts
- [ ] Pipeline triggers on every PR merge
- [ ] Deployment to staging is automated

Committed capacity: 34 story points
Team members: Alice, Bob, Carol, David
```

---

## Backlog Prioritization

### MoSCoW Method

| Priority | Description | Allocation |
|----------|-------------|------------|
| Must have | Critical — sprint fails without this | 60% |
| Should have | Important — significant impact | 20% |
| Could have | Nice to have — nice to have | 15% |
| Won't have | Explicitly out of scope | 5% |

### Prioritization Process

1. Product Owner ranks stories in backlog
2. Dev lead reviews technical feasibility
3. Team provides estimates
4. Adjustments made collaboratively
5. Final backlog confirmed before sprint planning

### Re-prioritization Triggers

- Stakeholder request
- Blocker discovered mid-sprint
- Scope change
- Business priority shift
- Technical debt accumulation

**Note:** Re-prioritization mid-sprint requires Product Owner + Dev Lead agreement. Communicate changes in daily standup.

---

## Story Point Estimation

### Scale (Fibonacci-Based)

| Points | Meaning | Real-world reference |
|--------|---------|-----------------------|
| 1 | Trivial — can be done in a few hours | Fix a typo, add a CSS class |
| 2 | Small — half day to 1 day | Simple form validation |
| 3 | Medium — 1–2 days | Standard CRUD with tests |
| 5 | Large — 2–3 days | Complex feature with integration |
| 8 | Very large — 3–5 days | Multi-component feature |
| 13 | XL — requires breaking down | Epic or full-page feature |
| 21 | XXL — needs epic treatment | Full module, seek approval |

### Estimation Guidelines

**Rules:**
- Estimate complexity, not time
- Consider: code, tests, docs, review, deployment
- Reference previous sprints for calibration
- If a story is >8 points, break it down

**Avoid:**
- Estimating in hours (leads to false precision)
- Including risk buffers in points (handle via buffer in capacity)
- Comparing to other teams' velocities

### Velocity Calibration

After 3 sprints, calculate team velocity:
```
Average velocity = (Sprint 1 + Sprint 2 + Sprint 3) / 3
```

Use velocity to guide sprint capacity, not to pressure the team.

---

## Sprint Review

### Purpose

 демонстрировать completed work to stakeholders and gather feedback.

### Format

1. **Demo** (45 min)
   - Show each completed story
   - Include "happy path" + key edge cases
   - Invite feedback

2. **Discussion** (15 min)
   - Review sprint goal vs outcomes
   - Discuss blockers and learnings
   - Capture stakeholder feedback

### Sprint Review Outputs

- Feedback collected
- Action items documented
- Stakeholder sign-off on outcomes
- Items for next sprint backlog

---

## Sprint Retrospective

### Format: Start / Stop / Continue

| Category | Prompt |
|----------|--------|
| Start | What should we begin doing? |
| Stop | What should we stop doing? |
| Continue | What is working well and should continue? |

### Process

1. Each team member writes 3 items (5 min)
2. Group items by theme (10 min)
3. Vote on top 3 items to address (5 min)
4. Assign owners and deadlines (5 min)

### Retrospective Outputs

| Action | Owner | Due |
|--------|-------|-----|
| [Action 1] | [Name] | [Date] |
| [Action 2] | [Name] | [Date] |

---

## Definition of Ready (DoR)

Stories must meet these criteria before entering sprint planning:

- [ ] Clear user story (who, what, why)
- [ ] Acceptance criteria defined
- [ ] Estimated and sized ≤8 points
- [ ] Dependencies identified
- [ ] Design/UX reviewed (if applicable)
- [ ] Test scenarios identified

---

## Definition of Done (DoD)

Stories are done when:

- [ ] Code written and linted
- [ ] Unit tests written (≥80% coverage)
- [ ] Integration tests written (if applicable)
- [ ] Code reviewed and approved
- [ ] Deployed to staging environment
- [ ] QA signed off
- [ ] Documentation updated
- [ ] No open critical bugs

---

*Document Version: 1.0*
*Last Updated: 2026-03-30*