# User Story Guide

This guide documents the user story creation process, template, and management practices for the SDLC Engineering team.

## User Story Template

### Standard Format

```
As a [role]
I want [action/feature]
So that [benefit/outcome]

───────────────────────────────────────

Story ID: [ID]
Points: [X]
Priority: [Must/Should/Could/Won't]
Sprint: [Target sprint]

───────────────────────────────────────

## Acceptance Criteria

- [ ] [Criterion 1]
- [ ] [Criterion 2]
- [ ] [Criterion 3]

───────────────────────────────────────

## Tasks

- [Task 1]
- [Task 2]

───────────────────────────────────────

## Notes

- Design reference: [link]
- Dependencies: [ID of blocking stories]
- Test scenarios: [description]
```

### Template Variants

#### Variant A: Simple (for quick capture)
```
As a [role]
I want [action]
So that [benefit]

Acceptance Criteria:
1. [ ]
2. [ ]
```

#### Variant B: Full (for complex stories)
```
As a [role]
I want [action]
So that [benefit]

Context:
[Background, constraints, or edge cases]

Acceptance Criteria:
- Given [precondition]
  When [action]
  Then [expected outcome]
- Given [precondition]
  When [action]
  Then [expected outcome]

Non-functional requirements:
- Performance: [requirement]
- Security: [requirement]
- Accessibility: [requirement]

Out of scope:
- [What this story does NOT cover]
```

---

## Writing Good User Stories

### The "Three Cs" Framework

| Element | Question | Example |
|---------|----------|---------|
| **Card** | What is the story? | Written on index card or ticket |
| **Conversation** | Why does it matter? | Discussed in grooming |
| **Confirmation** | How do we know it's done? | Acceptance criteria |

### Role Examples

| Role | When to use |
|------|-------------|
| "As a user" | Anonymous end users |
| "As a logged-in user" | Authenticated users |
| "As an admin" | Administrative users |
| "As a developer" | Internal tooling |
| "As a system" | Automated integrations |

### Common Mistakes

| Mistake | Why it's bad | Fix |
|---------|--------------|-----|
| "As a user, I want a good experience" | Too vague to test | "As a user, I want password reset within 30 seconds" |
| "As a developer, I want to fix the bug" | Not user-facing | "As a support agent, I want to see full error context" |
| "As an admin, I want everything to work" | Not testable | "As an admin, I want to disable user accounts in one click" |
| "I want a login button" | Missing role | "As a returning user, I want to log in with my email" |
| "So that the app works" | Missing benefit | "So that I can access my personalized dashboard" |

### Story Sizing Rules

| Points | Size | Check |
|--------|------|-------|
| 1 | Trivial | Can be done in a few hours |
| 2 | Small | Half day to 1 day |
| 3 | Medium | Standard — can be done in 1–2 days |
| 5 | Large | 2–3 days — verify no hidden complexity |
| 8 | Very large | 3–5 days — strongly consider breaking down |
| >8 | Too large | Break down immediately |

---

## Acceptance Criteria Best Practices

### Format: Given / When / Then (BDD Style)

```
Given [precondition — state before action]
When [action taken by user or system]
Then [observable outcome]
```

**Example:**
```
Given I am on the login page
When I enter an incorrect password 3 times
Then my account is locked for 15 minutes
And I receive a security alert email
```

### AC Rules

1. **One criterion = one testable statement**
2. **Use concrete language, not vague terms** ("fast" → "< 200ms")
3. **Cover the happy path + key edge cases**
4. **Do not include implementation details** (that's the team's job)
5. **Testability is the test** — if you can't write a test for it, the AC is vague

### AC Checklist

- [ ] Every criterion can be verified with a test
- [ ] Every criterion has one clear pass/fail outcome
- [ ] No criterion relies on undefined terms ("good", "fast", "user-friendly")
- [ ] Edge cases are covered (null, empty, max length, etc.)
- [ ] Non-functional requirements (performance, security) are explicit
- [ ] AC written from user's perspective, not implementation

### Examples of Good vs Bad AC

**Bad:**
- [ ] User can log in (too vague)
- [ ] System is secure (not testable)
- [ ] Page loads fast (undefined "fast")

**Good:**
- [ ] User can log in with email and correct password
- [ ] Login fails with 401 if password is incorrect
- [ ] Account locks after 5 failed attempts
- [ ] Page loads in < 2 seconds on 3G connection
- [ ] User cannot access admin panel without admin role

---

## Story Breakdown Approach

### When to Break a Story

Break a story when any of these apply:
- Estimated >8 story points
- Requires multiple PRs
- Involves multiple teams
- Cannot be demo'd in one sprint
- Has more than 5 acceptance criteria

### Breakdown Patterns

#### Pattern 1: By User Flow Step

```
Epic: "User checkout process"
  ├── Story: "User can add items to cart"
  ├── Story: "User can view cart summary"
  ├── Story: "User can enter shipping address"
  ├── Story: "User can enter payment details"
  └── Story: "User can confirm and pay"
```

#### Pattern 2: By CRUD Operation

```
Feature: "Manage user profiles"
  ├── Story: "Create user profile"
  ├── Story: "Read/view user profile"
  ├── Story: "Update user profile"
  └── Story: "Delete user profile"
```

#### Pattern 3: By Data Scope

```
Story: "Export reports"
  ├── Story: "Export reports as CSV (up to 1,000 rows)"
  ├── Story: "Export reports as CSV (up to 10,000 rows)"
  └── Story: "Export reports as CSV (over 10,000 rows — async)"
```

#### Pattern 4: By Technical Layer

```
Story: "Add search to products page"
  ├── Story: "Backend search API (Elasticsearch query)"
  ├── Story: "Frontend search input and results display"
  └── Story: "Search performance and caching layer"
```

### Technical vs User-Facing Stories

| Type | Audience | Example |
|------|----------|---------|
| User story | Product / PO | "As a user, I can reset my password" |
| Technical story | Dev team | "Add database index on users.email" |
| Spike | Dev team | "Evaluate OAuth libraries for 2 days" |

**Rule:** Aim for ≥70% user-facing stories in the backlog. Too many technical stories = product disconnect.

---

## Definition of Done (DoD)

### Standard DoD

All stories must meet these criteria to be marked done:

- [ ] Code written and linted
- [ ] Unit tests written (≥80% coverage, new code)
- [ ] Integration tests written (if API/DB involved)
- [ ] Code reviewed and approved (1+ reviewer)
- [ ] Deployed to staging environment
- [ ] QA passed (no open critical/high bugs)
- [ ] Acceptance criteria verified
- [ ] Documentation updated
- [ ] No regressions in existing functionality

### Per-Component DoD

| Component | Additional Done Criteria |
|-----------|--------------------------|
| Frontend | Accessible (keyboard + screen reader), responsive |
| Backend | API contract tested, error codes documented |
| Database | Migration reversible, data backed up |
| Infrastructure | Config documented, rollback tested |
| Security | No new vulnerabilities, auth tested |

---

## Story Lifecycle

```
Backlog → Groomed → Ready → In Sprint → In Progress → In Review → Done
   │          │         │         │              │            │
   ▼          ▼         ▼         ▼              ▼            ▼
 Unrefined   Has AC   Has AC    Being        Code review   Shipped
             + Est     + Est    worked on    + tests       to staging
```

### Status Definitions

| Status | Meaning |
|--------|---------|
| Backlog | Captured, not yet groomed |
| Groomed | Has story format, rough estimate, AC |
| Ready | Groomed + estimated + refined |
| In Sprint | Committed to current sprint |
| In Progress | Team actively working |
| In Review | PR open, awaiting review |
| Done | Shipped to staging, QA passed |

### Exit Criteria Per Status

- **Groomed**: Story format complete, AC drafted, rough estimate
- **Ready**: Estimated, AC finalized, no blockers
- **In Progress**: Owner assigned, tasks created
- **In Review**: PR open, tests passing
- **Done**: All DoD criteria met, sprint review demo'd

---

*Document Version: 1.0*
*Last Updated: 2026-03-30*