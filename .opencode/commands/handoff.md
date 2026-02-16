# /handoff — Structured Handoffs Between Team Members

Create a handoff document that makes another person productive in 5 minutes.

**Tier**: Small Team / Enterprise (not needed for solo work)

---

## Purpose

When you need to hand off work to someone else:
- What's done
- What's next
- What's risky
- How to validate

**Handoffs are where knowledge is lost.** This command prevents that.

---

## When to Use

**Use handoff when**:
- You're ending your work session and someone else continues
- You're blocked and need another person to unblock you
- You're going on vacation and work needs to continue
- A teammate is picking up an unfamiliar area
- Work is split between frontend/backend or design/engineering

**Examples**:
- "I built the API, you build the UI"
- "I debugged down to X, but I'm stuck, you continue"
- "I'm out next week, here's where the feature stands"

---

## Inputs Required

**Current status**:
- What have you completed?
- What's in progress?
- What's not started?

**Context**:
- Why is this work being done? (link to plan or exploration)
- What decisions did you make?
- What assumptions did you make?

**Reads automatically**:
- Plan file (to show progress)
- Recent commits or PRs
- Review findings (if `/review` was run)

---

## Process

### Step 1: Document What's Done

**Completed work**:
- What tasks are finished?
- Where is the code? (branches, PRs, commits)
- What's deployed? (staging, production)
- What's tested and working?

**Evidence**:
- Link to PRs or commits
- Link to deployed URLs (staging previews)
- Link to test results

---

### Step 2: Document What's Next

**Immediate next steps**:
- What should the next person work on?
- What's the priority order?
- What dependencies need to be ready?

**Be specific**:
- ❌ "Finish the feature"
- ✅ "Implement the delete endpoint in `src/api/delete.ts` following the pattern in `create.ts`, then add integration test in `tests/api.test.ts`"

---

### Step 3: Document What's Risky

**Blockers**:
- What's blocking progress?
- Who needs to be contacted to unblock?
- When do you expect blockers to clear?

**Known issues**:
- What's broken or incomplete?
- What workarounds are in place?
- What needs fixing before ship?

**Open questions**:
- What's unclear or needs decision?
- Who can answer these questions?

---

### Step 4: Validation Steps

**How to verify**:
- How do you run/test this locally?
- What command validates it's working?
- What should the next person see if everything is correct?

**Examples**:
```bash
# Run the app
npm run dev

# Test the feature
npm test src/auth.test.ts

# Expected: All tests pass, login page loads at localhost:3000/login
```

---

### Step 5: Key Files and Context

**Where things are**:
- Which files did you touch?
- Which files are important for the next person?
- Where is the documentation?

**Decisions made**:
- What did you decide and why?
- What alternatives did you reject?
- Link to ADRs or RFCs if relevant

**Assumptions**:
- What are you assuming about how this works?
- What needs to be confirmed?

---

## Exit Criteria

Handoff is **done** when:

1. ✅ What's done is documented with evidence (links to PRs, commits)
2. ✅ What's next is specific and actionable
3. ✅ Blockers and risks are identified with owners
4. ✅ Validation steps are clear and executable
5. ✅ Key files and decisions are documented
6. ✅ Another person can continue **without asking you questions**

**Test**: Could someone unfamiliar with this work pick it up and make progress in 5 minutes? If no, add more detail.

---

## Output Artifact

Create: `handoffs/YYYY-MM-DD-<slug>.md`

**Template** (also in `handoffs/HANDOFF_TEMPLATE.md`):

```markdown
# Handoff: [Brief Title]

**From**: [Your name]
**To**: [Recipient name or "Team"]
**Date**: YYYY-MM-DD
**Context**: [Link to plan, issue, or exploration]

---

## What's Done

### Completed Tasks
- [x] [Task 1] — PR: #123 (merged)
- [x] [Task 2] — Commit: abc1234
- [x] [Task 3] — Deployed to staging: [URL]

### Status Summary
[Brief paragraph: Where does this work stand overall?]

**Progress**: X/Y tasks complete

---

## What's Next

### Immediate Next Steps (Priority Order)

1. **[Task name]**
   - What: [Specific description]
   - Where: [File paths or locations]
   - How: [Implementation guidance or pattern to follow]
   - Acceptance: [How to know it's done]

2. **[Task name]**
   - [Same structure]

3. **[Task name]**
   - [Same structure]

---

## What's Risky

### Blockers
- **[Blocker 1]**: [Description]
  - Owner: [Who can unblock]
  - Expected resolution: [When]

### Known Issues
- **[Issue 1]**: [What's broken or incomplete]
  - Workaround: [Temporary solution if any]
  - Fix needed: [What needs to happen]

### Open Questions
- **[Question 1]**: [What's unclear]
  - Who can answer: [Name or role]
  - Impact if not resolved: [What breaks]

---

## Validation Steps

**How to run this locally**:

```bash
# Setup (if first time)
npm install
cp .env.example .env

# Run the app
npm run dev

# Run tests
npm test

# Validate specific feature
[Specific commands or steps]
```

**Expected result**: [What you should see if everything works]

**Common issues**:
- [Problem X]: Solution: [How to fix]
- [Problem Y]: Solution: [How to fix]

---

## Key Files and Context

### Files Modified
- `src/auth/login.ts` — Login endpoint implementation
- `src/middleware/auth.ts` — JWT validation middleware
- `tests/auth.test.ts` — Integration tests for auth flow

### Important Files (Not Modified, But Related)
- `docs/ARCHITECTURE.md` — Auth system design
- `docs/TECH_STACK.md` — JWT library choice
- `decisions/ADR-0012-jwt-auth.md` — Why we chose JWT

### Decisions Made
- **[Decision 1]**: [What you decided]
  - Why: [Reasoning]
  - Alternatives: [What you rejected and why]

- **[Decision 2]**: [What you decided]
  - Why: [Reasoning]

### Assumptions
- [Assumption 1] — Needs confirmation from [person]
- [Assumption 2] — Assumed based on [source]

---

## Resources

**Documentation**:
- Plan: `plans/YYYY-MM-DD-auth-plan.md`
- Exploration: `plans/YYYY-MM-DD-auth-exploration.md`
- Review: `plans/YYYY-MM-DD-auth-review.md`

**PRs/Commits**:
- PR #123: [Title and link]
- Commit abc1234: [Description and link]

**External Resources**:
- [API docs for library X]
- [Design mockups in Figma]
- [Slack thread with context]

---

## Contact

**If you have questions**:
- Slack: @username
- Email: name@example.com
- Available: [Timezone, hours, or "on vacation until DATE"]

**Backup contact** (if I'm unavailable):
- [Name]: Knows the architecture
- [Name]: Can answer product questions
```

---

## Handoff Anti-Patterns to Avoid

❌ **"It's all in the code"** — Code is not documentation  
❌ **Vague next steps** — "Finish it" is not actionable  
❌ **No validation steps** — How does the next person know it works?  
❌ **Assumptions left implicit** — They don't know what you know  
❌ **No contact info** — What if they're stuck and you're unreachable?  

✅ **Good handoffs**:
- Specific next steps (file paths, function names)
- Executable validation (exact commands)
- Explicit assumptions (what you think is true but haven't verified)
- Clear contact path (who to ask if stuck)

---

## Integration with Tier System

**Tier 2 (Small Team)**:
- Handoffs when switching context between team members
- Async handoffs via markdown doc (no meeting required)
- Optional: Quick sync (5-10 min) to walk through handoff doc

**Tier 3 (Enterprise)**:
- Formal handoffs for critical workstreams
- Handoff review required (recipient confirms they understand)
- Handoff docs archived in `handoffs/` for audit trail

---

## Handoff vs Status Update

**Status update** (quick, frequent):
- "I'm working on X, it's 50% done"
- Emoji progress in plan: 🟨
- Daily standups, Slack updates

**Handoff** (detailed, deliberate):
- Complete context transfer
- Actionable next steps
- Another person can continue **without you**
- Happens when work changes hands

---

## Example Handoff Scenarios

### Scenario 1: Vacation

**You**: Build API for user authentication, leave for 1 week vacation  
**Handoff**: What's done (login endpoint), what's next (signup endpoint, password reset), blockers (design pending for reset flow), validation (curl commands to test endpoints)

---

### Scenario 2: Stuck and Need Help

**You**: Debugging performance issue, narrowed it down to database queries but don't know SQL optimization  
**Handoff**: What you've tried (added indexes, no improvement), what you found (slow query logs show `users` table scan), what's next (need SQL expert to optimize query), validation (query should run in <100ms)

---

### Scenario 3: Frontend/Backend Split

**You**: Backend developer, built the API  
**Handoff**: Endpoints documented with request/response examples, Postman collection linked, what's next (frontend needs to integrate), validation (Postman tests pass, staging API is live)

---

**Remember**: The 5-minute rule — if someone can't get productive in 5 minutes with your handoff doc, it's incomplete.
