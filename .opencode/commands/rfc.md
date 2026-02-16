# /rfc — Request for Comments (Team Decision-Making)

Propose and document significant decisions that affect the team.

**Tier**: Small Team / Enterprise (not needed for solo work)

---

## Purpose

When a decision affects multiple people or has lasting consequences:
- Write it down (RFC)
- Get feedback (review period)
- Make a decision (approval)
- Document the outcome (ADR if architectural)

**RFC (Request for Comments)** forces precision. Writing > talking for alignment.

---

## When to Use

**Use RFC for decisions that**:
- Affect team workflow or processes
- Introduce new technologies or patterns
- Change public APIs or data schemas
- Have significant cost or time implications
- Are expensive to reverse

**Examples**:
- Switching from REST to GraphQL
- Adding a new deployment pipeline
- Changing code review process
- Adopting a new testing framework

**Don't use for**:
- Solo decisions with no team impact
- Trivial choices (naming a variable)
- Decisions already covered by existing ADRs

---

## Inputs Required

**Decision question**:
- What decision needs to be made?
- Why now?

**Context**:
- What problem does this solve?
- Who is affected?

**Reads automatically**:
- `docs/ARCHITECTURE.md` — Current system design
- `config/TIER.md` — Approval requirements
- `config/WORKING_AGREEMENTS.md` — Team decision-making norms
- `decisions/` — Prior ADRs (to check for conflicts)

---

## Process

### Step 1: Frame the Decision

**What are you deciding?**

- Clear decision question (yes/no or choose among options)
- Why this decision matters (impact, urgency)
- Who needs to approve (per `config/TIER.md`)

---

### Step 2: Present Options

**What are the alternatives?**

For each option:
- Description (what it is)
- Pros (benefits)
- Cons (drawbacks)
- Cost (time, money, complexity)
- Risk (what could go wrong)

**Minimum**: 2 options. If there's only one option, you're not deciding — you're announcing.

---

### Step 3: Recommend

**Which option do you recommend and why?**

- State your recommendation clearly
- Explain your reasoning (not just gut feeling — data, experience, alignment with goals)
- Acknowledge tradeoffs (what you're giving up)

**If you can't recommend**: The RFC isn't ready. Do more research or run `/explore`.

---

### Step 4: Review Period

**Set a review window**:
- Small Team (2-4 people): 24-48 hours
- Enterprise (5+ people): 3-5 business days

**Notify reviewers**:
- Post RFC in team channel (Slack, Discord, etc.)
- Tag specific people whose input is critical
- Link to the RFC file

---

### Step 5: Gather Feedback

**Reviewers should comment with**:
- Questions (for clarification)
- Concerns (risks you missed)
- Alternative perspectives (different options to consider)
- Approval or objection (with reasoning)

**Author should**:
- Answer questions
- Update RFC with new information
- Revise recommendation if feedback changes your mind

---

### Step 6: Make the Decision

**After review period ends**:

**If consensus**: Decision made, document it.

**If no consensus**: 
- Escalate to decision-maker (tech lead, PM, whoever has authority per `config/TIER.md`)
- Decision-maker makes final call
- Document the decision AND the dissent (minority opinion matters)

---

### Step 7: Document Outcome

**If decision is architectural**:
- Create ADR in `decisions/ADR-XXXX-<slug>.md`
- Link RFC to ADR

**If decision is process/policy**:
- Update `config/WORKING_AGREEMENTS.md`
- Link RFC in the config file

**Always**:
- Mark RFC status as ACCEPTED / REJECTED / DEFERRED
- Add decision date and outcome

---

## Exit Criteria

RFC is **done** when:

1. ✅ Decision question is clear
2. ✅ Options are presented with pros/cons/costs/risks
3. ✅ Recommendation is made with reasoning
4. ✅ Review period completed
5. ✅ Feedback addressed or acknowledged
6. ✅ Decision made (accepted/rejected/deferred)
7. ✅ Outcome documented (ADR or config update)

---

## Output Artifact

Create: `rfcs/YYYY-MM-DD-<slug>.md`

**Template** (also in `rfcs/RFC_TEMPLATE.md`):

```markdown
# RFC-XXXX: [Brief Title]

**Date**: YYYY-MM-DD
**Author**: [Name]
**Status**: [DRAFT / IN REVIEW / ACCEPTED / REJECTED / DEFERRED]
**Review Period**: [Start date] to [End date]
**Reviewers**: [Names of required approvers]

---

## Decision Question

[What are we deciding? Be specific.]

---

## Context

**Problem**: [What problem does this solve?]

**Why now**: [Why is this decision urgent or important now?]

**Who's affected**: [Team members, users, systems]

**Constraints**: [Budget, time, compatibility, etc.]

---

## Options

### Option 1: [Name]

**Description**: [What it is]

**Pros**:
- [Benefit 1]
- [Benefit 2]

**Cons**:
- [Drawback 1]
- [Drawback 2]

**Cost**: [Time, money, complexity]

**Risk**: [What could go wrong]

---

### Option 2: [Name]

[Same structure]

---

### Option 3 (If applicable): [Name]

[Same structure]

---

## Recommendation

**Recommended**: Option [X]

**Reasoning**: [Why this option is best, given context and constraints]

**Tradeoffs**: [What we're accepting/giving up]

**Alternatives considered**: [Why we rejected other options]

---

## Feedback

[During review period, add feedback here]

### [Reviewer Name] — [Date]

**Comment**: [Their feedback]

**Author response**: [How you addressed it]

---

## Decision

**Status**: [ACCEPTED / REJECTED / DEFERRED]

**Decided by**: [Name or "Team consensus"]

**Decision date**: YYYY-MM-DD

**Outcome**: [Final decision and why]

**Dissent** (if any): [Minority opinion recorded]

---

## Follow-up Actions

- [ ] Create ADR: `decisions/ADR-XXXX-<slug>.md` (if architectural)
- [ ] Update config: `config/WORKING_AGREEMENTS.md` (if process)
- [ ] Communicate decision to [stakeholders]
- [ ] Implement decision by [date]

---

## Related

- ADR: [Link to ADR if created]
- Exploration: [Link to `/explore` if relevant]
- Prior RFCs: [Link to related decisions]
```

---

## RFC Numbering

**Sequential**: RFC-0001, RFC-0002, RFC-0003, ...

Check `rfcs/` directory for the last number and increment.

**Tip**: Add a `rfcs/INDEX.md` file that lists all RFCs by number, title, and status.

---

## Anti-Patterns to Avoid

❌ **RFC as announcement** — "We've decided to do X, thoughts?" (decision already made)  
❌ **Too many options** — More than 4 options = analysis paralysis  
❌ **No recommendation** — Forcing the team to decide without your input  
❌ **Ignoring feedback** — If you asked for comments, you must read and respond  
❌ **No follow-through** — RFC approved but decision never implemented  

✅ **Good RFCs**:
- Clear decision question (one question, not five)
- Options with real tradeoffs (not strawman vs real option)
- Explicit recommendation (you did the research, make a call)
- Timely decision (review period ends, decision happens)
- Documented outcome (future you needs to know what was decided)

---

## Integration with Tier System

**Tier 1 (Solo)**:
- RFCs not needed (you're the only decision-maker)
- Use ADRs directly for architectural decisions

**Tier 2 (Small Team)**:
- RFC for decisions affecting >1 person
- 24-48 hour review period
- Approval from at least 1 other team member

**Tier 3 (Enterprise)**:
- RFC for all cross-functional or architectural decisions
- 3-5 day review period
- Formal approval from tech lead / architect / product owner (per `config/WORKING_AGREEMENTS.md`)
- Critical decisions may need 2+ approvals

---

## When RFC Becomes ADR

**If the decision is architectural** (affects system design, tech choices, data models):
- After RFC is ACCEPTED, create an ADR
- ADR format is more concise than RFC
- ADR is immutable (once written, it's history)
- Link RFC in the ADR for full context

**Template**: `decisions/ADR_TEMPLATE.md`

---

## Example RFC

**RFC-0042: Switch from Webpack to Vite**

**Decision**: Should we migrate our build system from Webpack to Vite?

**Context**: Build times are 3+ minutes. Developers wait. Productivity suffers.

**Options**:
1. **Migrate to Vite**: Faster builds (10-20s), simpler config. Cost: 3 days migration, some plugin breakage risk.
2. **Optimize Webpack**: Tune caching, upgrade plugins. Cost: 1 day. Improvement: 30% faster (still 2 min builds).
3. **Do nothing**: Accept current speed. Cost: $0. Risk: Developer frustration.

**Recommendation**: Option 1 (Vite). 10x build speed improvement justifies 3-day migration.

**Outcome**: ACCEPTED. Team consensus. Migration planned for next sprint.

---

**Remember**: Writing is thinking. If you can't write a clear RFC, you don't understand the decision well enough to make it.
