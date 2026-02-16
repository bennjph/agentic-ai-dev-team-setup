# /retro — Postmortem + Knowledge Capture

Structured retrospective after completing work, fixing bugs, or incidents.

Combines: Incident postmortem + knowledge capture (learning from experience).

---

## Purpose

Transform experience into **reusable knowledge** by:
- Understanding what happened (timeline)
- Analyzing why it happened (5 Whys root cause)
- Capturing what worked and what didn't
- Creating action items to prevent recurrence
- Extracting patterns for the knowledge base

**This is learning**, not blame. Blameless by default.

---

## When to Use

**After completing work**:
- Feature shipped (milestone reached)
- Plan marked 🟩 (all tasks done)
- Significant refactoring completed

**After fixing issues**:
- Bug that took >30 minutes to debug
- Production incident or outage
- User-reported issue that required investigation

**After process issues**:
- Workflow didn't work as expected
- Team coordination broke down
- Tools/config caused problems

**Periodic**:
- Weekly or sprint retrospectives (Tier 2/3)
- After major releases

---

## Inputs Required

**Context**:
- What was worked on? (plan file, issue, or description)
- What was the outcome? (shipped, fixed, blocked, etc.)
- Review findings (if `/review` was run)
- Incidents or surprises (if any)

**Reads automatically**:
- Plan file (to compare plan vs reality)
- Review artifacts (to check findings)
- `knowledge/CRITICAL_PATTERNS.md` (to check for recurring issues)

---

## Process

### Step 1: Timeline Reconstruction

**What happened, when?**

For completed work:
- When did work start?
- When did it complete?
- Were there delays or blockers?

For incidents:
- When did the issue first appear?
- When was it detected?
- When was root cause identified?
- When was it fixed?
- When was fix verified?

**Output**: Timeline table with timestamps and events.

---

### Step 2: 5 Whys Root Cause Analysis

**For issues or surprises, ask "Why?" 5 times to find root cause.**

**Example**:

1. **Why did users see errors?**
   → Database queries timed out

2. **Why did queries time out?**
   → Connection pool exhausted (all 10 connections in use)

3. **Why was pool exhausted?**
   → Traffic spike (3x normal load)

4. **Why did spike exhaust pool?**
   → Pool sized for average load, not peak

5. **Why not sized for peak?**
   → **ROOT CAUSE**: No load testing during development

**Root cause** is usually a process gap, not a person failing.

**Output**: 5 Whys chain with root cause identified.

---

### Step 3: What Went Well

**What prevented worse outcomes?**

- Fast detection (alerts, monitoring)
- Good rollback plan
- Clear documentation
- Effective team coordination
- Resilient architecture (graceful degradation)

**This is important** — recognize what worked so you keep doing it.

**Output**: List of positive aspects.

---

### Step 4: What Went Wrong (If Applicable)

**What made the problem possible?**

**Contributing factors** (beyond root cause):
- Missing tests or coverage gaps
- Missing monitoring or alerts
- Documentation gaps
- Process gaps (no review, no testing, no validation)
- Technical debt
- Time pressure or rushing

**Output**: List of contributing factors.

---

### Step 5: Action Items

**What will you do differently next time?**

**Three categories**:

1. **Immediate** (already done):
   - Fixes applied during incident response
   - Code merged, issues closed

2. **Short-term** (this week/sprint):
   - Add missing tests
   - Add monitoring/alerts
   - Update documentation
   - Create ADR for decisions

3. **Long-term** (backlog):
   - Architectural improvements
   - Process changes
   - Tool/infrastructure upgrades

**Each action item needs**:
- Clear description
- Owner (who's responsible)
- Deadline or target

**Output**: Action items table with owners and deadlines.

---

### Step 6: Knowledge Extraction

**Is this reusable?**

**If the issue could recur** (same class of bug, similar scenario):
- Extract a solution doc for `knowledge/solutions/`
- Tag with searchable keywords
- Link to this retro for context

**If this is the 3rd+ occurrence** of a pattern:
- Update `knowledge/CRITICAL_PATTERNS.md` with prevention checklist
- Mark as "recurring" to prevent future instances

**Solution doc categories**:
- `bug/` — Bugs and fixes
- `performance/` — Performance issues
- `architecture/` — Architectural decisions
- `integration/` — Third-party integrations
- `config/` — Configuration issues
- `security/` — Security vulnerabilities

**Output**: Solution doc (if needed) + pattern update (if recurring).

---

### Step 7: Process/Doc Updates

**Did this retro reveal gaps in your workflow?**

**Update config/docs**:
- `docs/QUALITY_STANDARDS.md` — Add new quality rule
- `config/PEER_REVIEW.md` — Adjust review thresholds
- `config/WORKING_AGREEMENTS.md` — Update team norms
- `docs/ARCHITECTURE.md` — Document new patterns or constraints

**Document what changed** in the retro artifact.

**Output**: List of config/doc updates made.

---

## Exit Criteria

Retro is **done** when:

1. ✅ Timeline is documented (for incidents)
2. ✅ 5 Whys analysis completed (for issues)
3. ✅ What went well is captured
4. ✅ Contributing factors identified
5. ✅ Action items created with owners/deadlines
6. ✅ Knowledge extraction performed (if applicable)
7. ✅ Process/docs updated (if gaps found)

---

## Output Artifact

Create: `retros/YYYY-MM-DD-<slug>-retro.md`

**Template**:

```markdown
# Retro: [Brief Title]

**Date**: YYYY-MM-DD
**Type**: [Milestone / Bug / Incident / Process]
**Status**: [Completed / In Progress]
**Severity** (if incident): CRITICAL / HIGH / MEDIUM / LOW

---

## Summary

[One-paragraph summary: What happened, what we learned, what's changing]

---

## Timeline (If Incident)

| Time | Event |
|------|-------|
| HH:MM | [Event 1] |
| HH:MM | [Event 2] |
| HH:MM | [Event 3] |

**Duration**: [Detection to resolution]
**Impact**: [Users/systems affected]

---

## 5 Whys (If Issue/Incident)

1. **Why did [symptom] happen?**
   → [Immediate cause]

2. **Why did [immediate cause] happen?**
   → [Deeper cause]

3. **Why did [deeper cause] happen?**
   → [System issue]

4. **Why did [system issue] exist?**
   → [Process/design gap]

5. **Why did [gap] exist?**
   → **ROOT CAUSE**: [One sentence]

---

## What Went Well

- ✅ [Positive aspect 1]
- ✅ [Positive aspect 2]
- ✅ [Positive aspect 3]

---

## What Went Wrong (If Applicable)

**Contributing Factors**:
- [ ] Missing test coverage for [area]
- [ ] No monitoring for [metric]
- [ ] Documentation gap in [topic]
- [ ] [Other factors]

---

## Action Items

### Immediate (Done)
- [x] [Action] — Owner: [Name] — Completed: YYYY-MM-DD

### Short-term (This Week/Sprint)
- [ ] [Action] — Owner: [Name] — Due: YYYY-MM-DD
- [ ] [Action] — Owner: [Name] — Due: YYYY-MM-DD

### Long-term (Backlog)
- [ ] [Action] — Owner: [Name] — Due: [Quarter or date]

---

## Knowledge Captured

**Pattern**: [If recurring pattern identified]

**Solution Doc**: [Path to `knowledge/solutions/` doc, if created]

**Critical Pattern Update**: [Yes/No — if updated `knowledge/CRITICAL_PATTERNS.md`]

**Searchable Tags**: [Keywords for future search]

---

## Process/Doc Updates

**Files updated**:
- [ ] `docs/QUALITY_STANDARDS.md` — [What changed]
- [ ] `config/PEER_REVIEW.md` — [What changed]
- [ ] `docs/ARCHITECTURE.md` — [What changed]
- [ ] [Other files]

---

## Metrics (Optional)

**For completed work**:
- Planned duration: [X days]
- Actual duration: [Y days]
- Tasks completed: [N/M]
- Tasks descoped: [K]

**For incidents**:
- Time to detect: [X minutes]
- Time to diagnose: [Y minutes]
- Time to fix: [Z minutes]
- Users affected: [Count or %]

---

## Next Steps

1. [Immediate next action]
2. [Follow-up or validation needed]
3. [When to revisit this retro]
```

---

## Solution Doc Format

**When creating a solution doc** in `knowledge/solutions/<type>/`:

**File**: `knowledge/solutions/<type>/YYYY-MM-DD-<slug>.md`

**Template**:

```markdown
---
module: [Component/area affected]
problem_type: [bug/performance/architecture/integration/config/security]
severity: [critical/high/medium/low]
symptoms: [Searchable description of what was observed]
root_cause: [Underlying issue]
resolution_type: [code_fix/config_change/dependency_update/architecture_change/workaround]
tags: [keyword, array]
date: YYYY-MM-DD
related_files: [paths involved]
recurrence_count: 1
---

# [Brief Title]

## Symptoms

[What was observed — user-visible behavior or error messages]

## Root Cause

[Why it happened — technical explanation]

## Solution

[What fixed it — code changes, config updates, etc.]

## Prevention

[How to avoid in future — tests, monitoring, process changes]

## Related

- Retro: `retros/YYYY-MM-DD-<slug>-retro.md`
- ADR: [If applicable]
- [Other related docs]
```

---

## Anti-Patterns to Avoid

❌ **Blame-oriented retros** — "Who caused this?" is the wrong question  
❌ **Skipping retros** — "We fixed it, move on" loses the learning  
❌ **Action items without owners** — Unowned actions never get done  
❌ **Retros nobody reads** — Keep them concise, link from knowledge base  
❌ **No follow-up** — Action items created but never tracked  

✅ **Good retros**:
- Blameless (focus on systems, not people)
- Factual (observable facts, not opinions)
- Actionable (every finding has a next step)
- Learning-oriented (extract patterns for future use)

---

## Integration with Tier System

**Tier 1 (Solo)**:
- Self-retro after significant work or incidents
- Knowledge extraction is optional but recommended
- Action items can be informal (add to backlog)

**Tier 2 (Small Team)**:
- Team retro for shared work or incidents
- Action items assigned to specific people
- Knowledge docs shared with team

**Tier 3 (Enterprise)**:
- Formal incident postmortems required for production issues
- Action items tracked in project management system
- Retros reviewed by leadership for systemic issues
- Recurring patterns escalate to architecture review

---

## When to Skip Retro

You can skip retro for:
- Trivial changes (typo fixes, dependency bumps)
- Work that went exactly as planned with no surprises
- Routine maintenance with no learnings

**But**: When in doubt, do a quick retro (15 minutes). Future you will thank you.

---

**Remember**: Failures are expensive learning opportunities. Capture the learning so you don't pay twice.
