# /plan — Implementation Planning with Progress Tracking

Create an executable plan with emoji progress tracking (🟩🟨🟥⬜).

---

## Purpose

Transform exploration findings into a **step-by-step implementation plan** that:
- Breaks work into testable increments
- Tracks progress visually
- Identifies risks before they bite
- Defines "done" clearly

**This is not a specification** — it's a roadmap for `/build`.

---

## When to Use

- After `/explore` recommends "Proceed to plan"
- When starting a feature that will take more than 1 day
- When multiple people need to coordinate on the same work

**Don't use for**: Trivial changes (1-line fixes, dependency bumps). Just build those.

---

## Inputs Required

**Recommended**:
- Exploration artifact from `/explore` (path to `plans/*-exploration.md`)

**Optional** (reads automatically if present):
- `docs/ARCHITECTURE.md` — Current system design
- `docs/TECH_STACK.md` — Technologies available
- `docs/QUALITY_STANDARDS.md` — Testing/security/performance requirements
- `config/TIER.md` — Process gates and approval requirements

---

## Process

### Step 1: Define Scope

**Based on exploration, what's in and out?**

**In Scope**:
- Specific features/fixes being delivered
- Which acceptance criteria from exploration are covered

**Out of Scope** (explicitly):
- What you're NOT doing this time (even if users asked for it)
- Future work that's related but separate

**Why explicit out-of-scope matters**: Prevents scope creep. When someone says "while you're there, can you also..." you point to this section.

---

### Step 2: Break into Tasks

**Create a task list** with:
- Clear, testable outcomes for each task
- Emoji status: 🟥 Not Started / 🟨 In Progress / 🟩 Done / ⬜ Descoped

**Good task**: "Add user authentication endpoint with JWT tokens — validated by integration test"  
**Bad task**: "Work on auth"

**How small?** Each task should be completable in 2-4 hours. If bigger, break it down.

---

### Step 3: Identify Dependencies

**What has to happen in order?**

- Task A must complete before Task B can start
- External dependency (waiting for API access, design assets, etc.)

**Mark dependencies clearly** so `/build` knows what's blocked.

---

### Step 4: Define Test Plan

**For each task, how will you know it works?**

- Unit tests (specific functions)
- Integration tests (full user flows)
- Manual testing steps (for UI/UX)

**Test-first mindset**: Write the test description now, even if the test code comes later.

---

### Step 5: Risk Assessment

**What could derail this plan?**

- Technical risks (API changes, performance issues)
- Process risks (dependencies on others, unclear requirements)
- Scope risks (feature creep, gold-plating)

**For HIGH risks**: Add mitigation tasks to the plan.

---

### Step 6: Define Acceptance Criteria

**What does "done" look like?**

Acceptance criteria should be:
- ✅ Testable (pass/fail, no ambiguity)
- ✅ User-focused (not "code is written" — "user can log in")
- ✅ Comprehensive (covers happy path + key edge cases)

**Example**:
- ✅ User can sign up with email + password
- ✅ Duplicate emails show clear error message
- ✅ Password must be 8+ characters
- ✅ User receives confirmation email within 30 seconds

---

### Step 7: Rollback Plan

**If this goes wrong in production, how do you undo it?**

- Feature flag to disable? (if yes, which flag?)
- Database rollback needed? (if yes, document the reverse migration)
- API version rollback? (if yes, document steps)

**Tier 2/3**: Rollback plan is mandatory for changes that touch data or public APIs.

---

## Exit Criteria

Plan is **done** when:

1. ✅ Scope (in/out) is explicit
2. ✅ Tasks are defined and sized (<4h each)
3. ✅ Dependencies are mapped
4. ✅ Test plan exists for each task
5. ✅ Risks are identified with mitigations
6. ✅ Acceptance criteria are testable
7. ✅ Rollback plan exists (for risky changes)
8. ✅ If Tier 2/3: Stakeholder approval obtained (per `config/TIER.md`)

**If you can't explain the plan to someone in 5 minutes**: It's too complex or too vague. Simplify or clarify.

---

## Output Artifact

Create: `plans/YYYY-MM-DD-<slug>-plan.md`

**Template**:

```markdown
# Plan: [Brief Title]

**Date**: YYYY-MM-DD
**Based on**: `plans/YYYY-MM-DD-<slug>-exploration.md` (if applicable)
**Status**: 🟥 Not Started

---

## Scope

### In Scope
- [Feature/fix 1]
- [Feature/fix 2]

### Out of Scope
- [Explicitly NOT doing this]
- [Future work, not now]

---

## Tasks

| # | Task | Status | Owner | Notes |
|---|------|--------|-------|-------|
| 1 | [Task description] | 🟥 | [Name] | Dependencies: none |
| 2 | [Task description] | 🟥 | [Name] | Dependencies: Task 1 |
| 3 | [Task description] | 🟥 | [Name] | Dependencies: none |

**Progress**: 0/3 complete (0%)

---

## Test Plan

### Task 1: [Task name]
- **Unit tests**: [Specific functions to test]
- **Integration tests**: [User flows to test]
- **Manual testing**: [UI/UX scenarios]

### Task 2: [Task name]
- [Same structure]

---

## Risks

| Risk | Severity | Mitigation | Owner |
|------|----------|------------|-------|
| [Risk 1] | HIGH | [How we'll address it] | [Name] |
| [Risk 2] | MEDIUM | [How we'll address it] | [Name] |

---

## Acceptance Criteria

- [ ] [Testable criterion 1]
- [ ] [Testable criterion 2]
- [ ] [Testable criterion 3]

---

## Rollback Plan

**If this fails in production**:
1. [Step to disable or revert]
2. [Data restoration steps if needed]
3. [Communication plan]

**Feature flag**: [Yes/No — if yes, which flag?]

---

## Approval (if Tier 2/3)

**Reviewed by**: [Name(s)]
**Approved**: [Date]
**Conditions**: [Any conditions or follow-up required]

---

## Updates (During /build)

[When tasks change status, update here with timestamp + reason]

- YYYY-MM-DD HH:MM — Task 1: 🟥 → 🟨 (started implementation)
- YYYY-MM-DD HH:MM — Task 1: 🟨 → 🟩 (tests passing, PR merged)
- YYYY-MM-DD HH:MM — Task 2: Descoped ⬜ (external API not ready)
```

---

## Emoji Progress Tracking

| Emoji | Meaning | When to Use |
|-------|---------|-------------|
| 🟥 | Not Started | Task not yet begun |
| 🟨 | In Progress | Work has started but not complete |
| 🟩 | Done | Tests passing, criteria met, ready to ship |
| ⬜ | Descoped | Explicitly removed from plan (with reason) |

**Update the plan during `/build`** — the plan is a living document, not a static spec.

**Progress calculation**: `(🟩 count) / (total tasks - ⬜ count) × 100%`

---

## Anti-Patterns to Avoid

❌ **Tasks too big** — "Build the entire auth system" is not a task  
❌ **Vague acceptance criteria** — "Make it work" is not testable  
❌ **No test plan** — Tests are not "nice to have," they're how you know you're done  
❌ **Plan and forget** — The plan should be updated during `/build`, not written once and ignored  
❌ **Gold-plating** — Stick to the scope; new ideas go to backlog  

---

## Integration with Tier System

**Tier 1 (Solo)**:
- Self-approval is fine
- Document assumptions and rollback plan

**Tier 2 (Small Team)**:
- Share plan with affected team members before `/build`
- Get verbal/async approval from 1 other person

**Tier 3 (Enterprise)**:
- Formal approval required (see `config/WORKING_AGREEMENTS.md`)
- Plans for cross-cutting changes require technical lead sign-off
- Plans affecting data schemas require data owner approval

---

**Remember**: A plan is not about predicting the future — it's about making good decisions when the future surprises you.
