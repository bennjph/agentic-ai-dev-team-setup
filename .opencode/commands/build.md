# /build — TDD Implementation, Step-by-Step

Implement the plan using Test-Driven Development (TDD) and incremental progress.

---

## Purpose

Turn a plan into working, tested code by:
- Writing tests first (TDD)
- Implementing in small increments
- Updating plan status as you go
- Verifying after each step

**This is execution** — not exploration, not planning. Follow the plan.

---

## When to Use

- After `/plan` produces an approved plan
- When all blocking questions from exploration are resolved
- When dependencies (external APIs, design assets, etc.) are ready

**Don't use for**: Changes without a plan. If it's non-trivial, plan it first.

---

## Inputs Required

**Required**:
- Plan file path (e.g., `plans/YYYY-MM-DD-feature-plan.md`)

**Reads automatically** (if present):
- `docs/ARCHITECTURE.md` — System design constraints
- `docs/TECH_STACK.md` — Technologies and conventions
- `docs/QUALITY_STANDARDS.md` — Code standards, security rules, performance requirements
- `docs/REPO_MAP.md` — Where things live in the codebase

---

## Process (TDD Loop)

For **each task** in the plan:

### Step 1: Write the Test First

**Before writing any implementation code**, write a test that:
- Describes the desired behavior
- Fails initially (because the feature doesn't exist yet)
- Will pass when the feature is correctly implemented

**Test types**:
- **Unit test**: For isolated functions/methods
- **Integration test**: For user flows across multiple components
- **Manual test**: For UI/UX behavior (document the steps)

**Why test-first?**: Forces you to define "done" before you code. Prevents over-engineering.

---

### Step 2: Implement Minimum Code to Pass

**Write the simplest code** that makes the test pass.

**Don't**:
- Add features not in the plan
- Optimize prematurely
- Refactor unrelated code (unless it's blocking)

**Do**:
- Follow `docs/QUALITY_STANDARDS.md` (security, style, performance floors)
- Add comments for non-obvious decisions
- Update `docs/ARCHITECTURE.md` if you're introducing a new pattern

---

### Step 3: Run the Test

**Verify the test passes**.

If it doesn't:
- Debug and fix
- Don't move to the next task until it's green

If you can't make it pass after 3 attempts:
- Stop and reassess — maybe the approach is wrong
- Update the plan with findings
- Consider `/explore` for this specific issue

---

### Step 4: Refactor (If Needed)

**Now that the test is passing**, clean up:
- Remove duplication
- Improve naming
- Extract helper functions

**Rule**: Tests must still pass after refactoring. If they don't, you broke something.

---

### Step 5: Update Plan Status

**Mark the task** in the plan:
- 🟥 → 🟨 when you start
- 🟨 → 🟩 when test passes and code is clean

**Add a note** with timestamp and PR/commit link (if applicable).

**Example**:
```
- 2026-02-15 14:30 — Task 1: 🟥 → 🟨 (started auth endpoint)
- 2026-02-15 16:45 — Task 1: 🟨 → 🟩 (tests passing, PR #42 merged)
```

---

### Step 6: Repeat for Next Task

**Move to the next task** in the plan.

If tasks have dependencies, follow the order. Don't start Task 2 if it depends on Task 1 being done.

---

## Exit Criteria

Build is **done** when:

1. ✅ All plan tasks marked 🟩 or explicitly descoped (⬜) with reason
2. ✅ All tests (unit + integration) are passing
3. ✅ Code meets `docs/QUALITY_STANDARDS.md` requirements
4. ✅ Plan is updated with current status and timestamps
5. ✅ If new architectural decisions were made: ADR drafted in `decisions/`
6. ✅ No known CRITICAL/HIGH issues left unaddressed

**If acceptance criteria from the plan aren't met**: Build is not done. Fix or descope explicitly.

---

## Handling Issues During Build

### Discovered a Bug (Not in Plan)

1. **Document it** — Add to `backlog/` or create a GitHub issue
2. **Decide urgency**:
   - Blocking this work? Fix it now, update plan
   - Not blocking? Add to backlog, continue with plan
3. **If fixing now**: Update plan with new task, mark risk mitigated

### Plan is Wrong (Approach Doesn't Work)

1. **Stop building**
2. **Document findings** in plan (update "Risks" or add "Learnings" section)
3. **Options**:
   - Minor adjustment: Update plan, continue
   - Major change: Run `/explore` again on this specific issue
   - Abort: Descope remaining tasks, document why

### Scope Creep (New Feature Requests)

1. **Do not build it** — even if it's "just 5 minutes"
2. **Add to backlog** — with context about why it came up
3. **Finish the current plan** first
4. **After completion**: Run `/explore` for the new request

---

## Output

### Code Changes

- Committed to version control (Git)
- Following branch/PR workflow per `config/TIER.md`
- Tests included and passing

### Updated Plan

The plan file (`plans/*-plan.md`) should have:
- Task statuses updated (🟩🟨🟥⬜)
- Progress percentage recalculated
- Timestamp notes for significant changes
- If descoped: Reason documented

### Architecture Decision Records (If Needed)

**When to write an ADR** (see Practice 11):
- You chose a technology or pattern that affects future work
- The decision is expensive to reverse
- Someone will ask "why did we do it this way?" in 6 months

**ADR template**: `decisions/ADR_TEMPLATE.md`

**If you made a decision without an ADR**: You might be wrong, or you didn't think hard enough.

---

## TDD Anti-Patterns to Avoid

❌ **Write all code first, test later** — That's not TDD, that's QA  
❌ **Tests that test nothing** — `expect(true).toBe(true)` is not a test  
❌ **100% coverage obsession** — Coverage doesn't equal quality  
❌ **Giant tests** — If a test does 10 things, it's 10 tests pretending to be one  
❌ **Mocking everything** — Over-mocking makes tests brittle  

✅ **Good TDD**:
- Write test → Fail → Implement → Pass → Refactor → Repeat
- Each test verifies **one behavior**
- Tests read like documentation (clear names, clear assertions)
- When a test fails, you know **exactly** what broke

---

## Integration with Tier System

**Tier 1 (Solo)**:
- Commit to main (trunk-based development)
- Self-review before committing
- Run `/review` before marking plan 🟩

**Tier 2 (Small Team)**:
- Work in short-lived branches (<48 hours)
- Create PR per task or group of related tasks
- Run `/review` before requesting team review
- Update plan with PR links

**Tier 3 (Enterprise)**:
- PR required for all changes
- CI/CD must pass before merge
- Code review from 1+ designated reviewers (see `config/PEER_REVIEW.md`)
- ADRs for architectural changes

---

## When Build Goes Wrong

### Symptom: Tests keep failing

**Diagnose**:
- Is the test correct? (Maybe it's testing the wrong behavior)
- Is the approach sound? (Maybe the plan assumed something wrong)
- Is there missing context? (Read exploration artifact again)

**Action**: Don't force it. Reassess the plan or run `/explore` on this specific blocker.

---

### Symptom: Build is taking 3x longer than planned

**Diagnose**:
- Scope creep? (Are you building things not in the plan?)
- Underestimated complexity? (Common for unfamiliar areas)
- Blocked by dependencies? (External API down, design not ready)

**Action**:
1. Update plan with reality
2. Descope non-critical tasks if needed
3. Communicate status (if Tier 2/3)

---

### Symptom: You're bored and want to refactor unrelated code

**Resist**:
- Refactoring unrelated code during feature work is scope creep
- It adds risk and delays the current plan

**Action**:
- Add refactoring to backlog as a separate item
- Finish current plan first
- Later: Run `/explore` for the refactoring work

---

## After Build is Done

**Next step**: `/review`

The build might feel "done" when tests pass, but it's not done until review validates:
- Correctness (does it solve the problem?)
- Security (no vulnerabilities introduced?)
- Quality (meets standards?)
- Architecture (fits the system?)

**Don't skip review.** Fresh eyes catch what you're blind to.

---

**Remember**: Code is written once and read dozens of times. Make it clear, make it testable, and make it match the plan.
