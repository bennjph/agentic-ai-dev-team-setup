# /explore — Deep Analysis Before Coding

**NO CODE RULE**: This command produces understanding, not implementation. Writing code during exploration is forbidden.

---

## Purpose

Deep analysis of a problem, feature request, or bug **before** any code is written. 

The goal: understand the problem well enough to make good decisions about whether/how to solve it.

---

## When to Use

- Before starting any non-trivial feature or change
- After receiving a bug report that isn't obvious
- When a user request seems simple but might hide complexity
- When you're about to say "I'll just build something and see"

**Don't use for**: Trivial changes where the solution is obvious and low-risk.

---

## Inputs Required

You need **one of**:
1. A backlog item (path to markdown file or GitHub issue URL)
2. Plain-language problem description from user
3. Bug report or unexpected behavior

**Optional context** (reads automatically if present):
- `docs/PROJECT_BRIEF.md` — Business goals and constraints
- `docs/ARCHITECTURE.md` — Current system design
- `docs/TECH_STACK.md` — Technologies in use
- `config/TIER.md` — Process tier and gates

---

## Process

### Step 1: Understand Current State

**Before proposing solutions, map what exists:**

1. What does the system currently do (or not do) related to this?
2. Which files/modules are affected?
3. What assumptions is the current code making?
4. What could break if we change this area?

**Output**: Current-state summary (3-5 bullets)

---

### Step 2: Clarify the Request

**Ask questions to remove ambiguity:**

- What problem is this solving for users? (Not "what feature" — what *pain*)
- What does "done" look like? (Acceptance criteria)
- What are we explicitly NOT doing? (Scope boundaries)
- Are there constraints? (Performance, security, compatibility)

**If the requester is unavailable**: Document assumptions explicitly. Flag them as "ASSUMPTION — needs confirmation."

**Output**: Problem statement + acceptance criteria + assumptions

---

### Step 3: Identify Risks

**What could go wrong?**

- Technical risks (edge cases, performance, data migration)
- Product risks (users won't use it, doesn't solve the real problem)
- Process risks (dependencies on other work, unknown unknowns)

**For each risk**: Rate as HIGH/MEDIUM/LOW and note mitigation or acceptance.

**Output**: Risk list with mitigations

---

### Step 4: Map Impact

**What will this touch?**

- Affected files/modules
- Affected users (all users, subset, admins only?)
- Affected data (database changes, migrations needed?)
- Affected integrations (APIs, third-party services)

**Output**: Impact map

---

### Step 5: Propose Approach (High-Level Only)

**Describe HOW you'd solve it** — but no code yet.

- What's the simplest solution that could work?
- Are there alternatives? (List 2-3 if non-obvious)
- What needs to be tested?
- What's the rollback plan if it doesn't work?

**NO CODE**: Pseudocode is allowed. Actual functions/classes are not.

**Output**: Approach summary + test strategy

---

### Step 6: Recommend Next Step

**Based on exploration, what should happen next?**

Options:
- ✅ **Proceed to `/plan`** — Problem is well-understood, ready to plan implementation
- 🟡 **Need more information** — Specific questions for stakeholders or technical spikes needed
- 🟥 **Don't build** — Risk too high, or doesn't align with project goals (cite `docs/PROJECT_BRIEF.md`)

**Output**: Explicit recommendation with reasoning

---

## Exit Criteria

Exploration is **done** when:

1. ✅ Current state is documented
2. ✅ Problem statement is clear (or assumptions are explicit)
3. ✅ Risks are identified and rated
4. ✅ Impact is mapped
5. ✅ High-level approach is described (no code written)
6. ✅ Next step is recommended with reasoning
7. ✅ All "blocking" questions are resolved (or explicitly deferred with owner)

**If you wrote any code**: You didn't do exploration. Start over.

---

## Output Artifact

Create: `plans/YYYY-MM-DD-<slug>-exploration.md`

**Template**:

```markdown
# Exploration: [Brief Title]

**Date**: YYYY-MM-DD
**Requester**: [Name or "backlog"]
**Status**: [Recommend Proceed / Need Info / Don't Build]

---

## Problem Statement

[What problem are we solving? For whom? Why now?]

**Acceptance Criteria**:
- [ ] [Criterion 1]
- [ ] [Criterion 2]

**Assumptions** (needs confirmation):
- [Assumption 1]
- [Assumption 2]

---

## Current State

[What does the system do today related to this?]

**Affected Areas**:
- `path/to/file.ts` — [Why it's affected]
- `path/to/module/` — [Why it's affected]

---

## Risks

| Risk | Severity | Mitigation |
|------|----------|------------|
| [Risk 1] | HIGH/MED/LOW | [How we'll address it] |
| [Risk 2] | HIGH/MED/LOW | [How we'll address it] |

---

## Impact

**Users Affected**: [All / Subset / Admins / etc.]

**Data Changes**: [Yes/No — if yes, describe]

**Dependencies**: [External systems, APIs, other work]

---

## Proposed Approach

[High-level description — NO CODE]

**Alternatives Considered**:
1. [Option A]: Pros, Cons
2. [Option B]: Pros, Cons

**Test Strategy**:
- [What needs testing and how]

**Rollback Plan**:
- [How to undo if it doesn't work]

---

## Recommendation

**Next Step**: [Proceed to /plan | Need more info | Don't build]

**Reasoning**: [Why this recommendation]

**Open Questions** (if any):
- [Question 1] — Owner: [Name], Due: [Date]
- [Question 2] — Owner: [Name], Due: [Date]
```

---

## Anti-Patterns to Avoid

❌ **"Let me just try something"** — If you're typing code, you're not exploring  
❌ **Copying competitor features** — Understand the *problem*, not just the *solution*  
❌ **Analysis paralysis** — 30-60 minutes is usually enough; if you need more, something is unclear  
❌ **Skipping this step** — "I know what to build" is how you waste a week building the wrong thing  

---

## Integration with Tier System

**Tier 1 (Solo)**:
- Self-exploration is fine; document assumptions

**Tier 2 (Small Team)**:
- If exploration touches areas owned by others, share the artifact before `/plan`

**Tier 3 (Enterprise)**:
- Cross-functional changes require stakeholder review of exploration before proceeding
- Check `config/WORKING_AGREEMENTS.md` for approval gates

---

**Remember**: The best code is the code you don't write. Exploration helps you decide what NOT to build.
