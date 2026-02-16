# /review — Configurable Peer Review

Self-review + configurable peer review (1-3 independent reviewers).

---

## Purpose

Validate code quality **before** it ships using:
1. **Self-review** (same model/person who wrote the code)
2. **Peer review** (different models/people, per `config/PEER_REVIEW.md`)

The goal: Catch issues that tests miss — logic errors, security vulnerabilities, architectural misalignment, unclear code.

---

## When to Use

- After `/build` completes (tests passing, plan marked 🟩)
- Before merging to main (Tier 1) or before requesting human review (Tier 2/3)
- After fixing issues from a previous review (re-review)

**Don't skip this.** Fresh eyes catch what you're blind to.

---

## Inputs Required

**Required**:
- Changed files (either explicit paths or "review recent changes")

**Reads automatically**:
- `config/PEER_REVIEW.md` — Peer review policy (how many reviewers, independence rules, severity thresholds)
- `docs/QUALITY_STANDARDS.md` — Project-specific quality rules
- `docs/ARCHITECTURE.md` — Architectural constraints
- Plan file (if linked) — To validate code matches intent

---

## Process

### Phase 1: Self-Review

**The same model/person who wrote the code reviews it.**

**Why self-review matters**: Catches obvious mistakes before wasting others' time. You'll find typos, incomplete error handling, and logic errors just by reading your own code with fresh eyes.

**Self-review checklist**:
1. **Correctness**: Does this do what the plan says?
2. **Tests**: Do tests cover the key behaviors?
3. **Error handling**: Are failure cases handled?
4. **Security**: Are inputs validated? Secrets protected?
5. **Readability**: Can someone else understand this in 6 months?

**Output**: List of issues found + fixes applied.

---

### Phase 2: Peer Review (If Configured)

**Read `config/PEER_REVIEW.md`** to determine:
- How many peer reviewers? (0, 1, 2, or 3)
- Independence rule: Different model/provider if possible
- Blocking thresholds: Which severities block merge?

**If peer reviewers = 0**: Skip to Phase 3 (verdict).

**If peer reviewers ≥ 1**: Dispatch code to reviewers.

---

### Peer Review Independence (Critical)

**"Less Context" Rule** (from Zevi workflow):
- Peer reviewers do NOT see the conversation that produced the code
- They see ONLY: the code diff + one-sentence intent

**Why**: A reviewer without context will question assumptions that seem obvious during creation. That's the point.

**How to provide context**:
```
Intent: "Add JWT authentication to the user login endpoint"

Changed files:
- src/auth/login.ts (new file, 87 lines)
- src/middleware/auth.ts (modified, +15 lines)
- tests/auth.test.ts (new file, 45 lines)

[Code diff follows]
```

**Independence rule**:
- If builder used Model A → reviewers use Model B, C, D (different providers if possible)
- If builder was Human → AI reviewers are fine (different perspective)
- If builder was AI → at least 1 human reviewer or different AI family

---

### Phase 3: Aggregate Findings

**For each finding from peer reviewers**:

1. **Verify it exists**: Does the issue actually exist in the code?
2. **Classify severity**:
   - CRITICAL: Security vulnerability, data loss risk, breaks core functionality
   - HIGH: Significant bug, poor architecture, violates standards
   - MEDIUM: Minor bug, code smell, unclear naming
   - LOW: Nitpick, style preference

3. **Validate or reject**:
   - ✅ VALID: Real issue, needs addressing
   - ❌ INVALID: False positive (reviewer lacked context, misunderstood intent)
   - ⚠️ STYLISTIC: Preference, not a bug

**Consensus rule**: If 2+ reviewers report the same issue, it's likely valid.

---

### Phase 4: Validate Findings

**For each finding, check**:

#### Is this actually an issue?
- Read the code at the line referenced
- Check if the concern is real or based on misunderstanding

#### Common false positives:
- ❌ "Missing X" when X exists elsewhere (middleware, parent component)
- ❌ "Should use pattern Y" when current pattern is intentional
- ❌ Framework/library misunderstanding (reviewer unfamiliar with tool)
- ❌ "This function is too long" when length is justified (generated code, state machine)

#### When to accept a finding:
- ✅ Issue actually exists in the code
- ✅ Fix would improve security, correctness, or maintainability
- ✅ Reviewer identified something you missed

#### When to reject a finding:
- ❌ Issue doesn't exist (reviewer misread code)
- ❌ Context is missing (handled elsewhere)
- ❌ Intentional design decision (documented in ADR or comments)
- ❌ Stylistic preference, not a real problem

---

### Phase 5: Produce Verdict

**Based on severity and blocking policy** (from `config/PEER_REVIEW.md`):

**PASS**:
- No CRITICAL or HIGH issues found
- All MEDIUM/LOW issues are optional or fixed

**CONDITIONAL PASS**:
- MEDIUM issues exist but not blocking
- Fix recommended but not required before merge

**FAIL**:
- CRITICAL or HIGH issues must be fixed before merge/deploy

---

## Exit Criteria

Review is **done** when:

1. ✅ Self-review completed and documented
2. ✅ Peer review completed (per policy in `config/PEER_REVIEW.md`)
3. ✅ All findings validated (real issues vs false positives)
4. ✅ Verdict produced (PASS / CONDITIONAL PASS / FAIL)
5. ✅ If FAIL: Issues documented for fixing
6. ✅ If PASS: Ready to merge/deploy

**If FAIL**: Fix issues and re-run `/review`.

---

## Output Artifact

Create or append to: `plans/YYYY-MM-DD-<slug>-review.md`

**Template**:

```markdown
# Review: [Brief Title]

**Date**: YYYY-MM-DD
**Code**: [Paths or PR link]
**Reviewers**: Self + [Peer 1, Peer 2, ...]
**Verdict**: PASS / CONDITIONAL PASS / FAIL

---

## Self-Review Findings

### Issues Found
- [Issue 1] — Fixed: [Yes/No]
- [Issue 2] — Fixed: [Yes/No]

### Changes Made
- [What was fixed during self-review]

---

## Peer Review Findings

### Reviewer 1: [Model/Name]

| Finding | Severity | Validation | Action |
|---------|----------|------------|--------|
| [Finding 1] | CRITICAL | ✅ VALID | Fix required |
| [Finding 2] | LOW | ❌ INVALID | False positive (reason) |

### Reviewer 2: [Model/Name]

[Same structure]

---

## Consensus Findings (2+ Reviewers Agree)

| Finding | Reviewers | Severity | Action |
|---------|-----------|----------|--------|
| [Finding] | 2/3 | HIGH | Fix required |

---

## Validated Issues (Must Fix)

### CRITICAL
1. **[Issue]**
   - Location: `file.ts:42`
   - Why it's critical: [Explanation]
   - Fix: [What needs to change]

### HIGH
2. **[Issue]**
   - Location: `file.ts:88`
   - Why it's high: [Explanation]
   - Fix: [What needs to change]

---

## Rejected Findings

1. ❌ **"Missing error handling in getUserData"** (Reviewer 2)
   - Why rejected: Error boundary exists in parent component at `App.tsx:15`
   - Evidence: [Code reference or link]

2. ❌ **"Should use TypeScript strict mode"** (Reviewer 3)
   - Why rejected: Already enabled in `tsconfig.json:4`

---

## Verdict

**Result**: [PASS / CONDITIONAL PASS / FAIL]

**Reasoning**: [Why this verdict]

**Next steps**:
- [If PASS]: Ready to merge/deploy
- [If CONDITIONAL PASS]: Can merge, but recommend addressing [list]
- [If FAIL]: Fix [critical/high issues], then re-review

---

## Action Items

- [ ] Fix CRITICAL issue 1
- [ ] Fix HIGH issue 2
- [ ] Re-review after fixes
```

---

## Peer Review Configuration

**Stored in**: `config/PEER_REVIEW.md`

**Example policy**:

```markdown
# Peer Review Policy

**Team Tier**: Solo / Small Team / Enterprise

**Reviewer Count**: 1 (minimum)

**Independence Rule**:
- Builder uses one model → Reviewer must use different model/provider
- If multiple reviewers, prefer diversity (different AI families)

**Blocking Thresholds**:
- CRITICAL: Always blocks merge
- HIGH: Blocks merge (can be waived by tech lead with justification)
- MEDIUM: Does not block merge
- LOW: Does not block merge

**Re-Review Required**:
- After fixing CRITICAL or HIGH issues
- After significant scope changes during build

**Approval Process** (Tier 3):
- 1 peer approval required for standard changes
- 2 peer approvals for changes affecting auth, payments, data schemas
```

---

## Anti-Patterns to Avoid

❌ **Rubber-stamping** — "Looks good to me" without reading the code  
❌ **Reviewer as coder** — Reviewer rewrites code instead of pointing out issues  
❌ **Nitpicking without priorities** — Mixing CRITICAL and LOW issues  
❌ **Ignoring all feedback** — "It works, why change it?"  
❌ **Defensive responses** — Reviewer found an issue, don't argue, fix it or explain with evidence  

✅ **Good review**:
- Specific findings with line numbers
- Severity levels that match impact
- Actionable suggestions (not just "this is bad")
- Evidence when rejecting findings (show the code that addresses the concern)

---

## Integration with Tier System

**Tier 1 (Solo)**:
- 1 peer reviewer recommended (different AI model)
- Self + 1 peer = 2 perspectives minimum
- Can self-approve after review (you're accountable)

**Tier 2 (Small Team)**:
- 1-2 peer reviewers (AI or human)
- If AI reviews, consider 1 human spot-check for critical code
- Team member approval recommended (async is fine)

**Tier 3 (Enterprise)**:
- 2-3 peer reviewers for standard changes
- 3-4 for critical changes (auth, payments, data schemas, public APIs)
- Formal approval process per `config/WORKING_AGREEMENTS.md`
- CI/CD gates must pass before human review

---

**Remember**: The goal of review is not perfection — it's **improvement**. If the code is better after review than before, the process worked.
