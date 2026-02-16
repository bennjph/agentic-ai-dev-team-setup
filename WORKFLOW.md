# Workflow Guide

How to use the 7 commands together to ship high-quality features.

---

## Philosophy

**Process-first, tool-agnostic**. These commands work with any AI coding assistant (OpenCode, Cursor, Claude Code, etc.). The value is in the discipline, not the tooling.

**Artifact-driven**. If you didn't update artifacts, you didn't do the work. Every command modifies or creates files in your repo.

**Tier-aware**. Solo devs use 5 commands. Small teams use 7. The workflow scales with your context.

---

## The 7 Commands

| Command | Purpose | Artifact Created | Tier |
|---------|---------|------------------|------|
| `/explore` | Spike without code | `backlog/explorations/EXP-NNN.md` | All |
| `/plan` | Turn issue into executable plan | `plans/PLAN-NNN.md` | All |
| `/build` | Implement with TDD | Code + tests | All |
| `/review` | Cross-model code review | `decisions/code-review-YYYY-MM-DD-NNN.md` | All |
| `/retro` | Postmortem learning | `retros/RETRO-YYYY-MM-DD.md` | All |
| `/rfc` | Architecture decisions | `rfcs/RFC-NNN-title.md` | Team+ |
| `/handoff` | Transfer context | `handoffs/HANDOFF-YYYY-MM-DD-feature.md` | Team+ |

---

## Core Workflow (Solo / Tier 1)

### 1. Exploration Phase

**When**: You have a vague idea but unclear requirements.

**What**: Use `/explore` to spike, research, ask questions. **NO CODE** — only investigation.

**Output**: `backlog/explorations/EXP-001-idea-name.md`

**Example**:
```
/explore "How would we add dark mode to the app?"
```

**Next**: Convert exploration to backlog issue (`backlog/issues/ISSUE-NNN.md`).

---

### 2. Planning Phase

**When**: You have a clear issue and need an executable plan.

**What**: Use `/plan` to break down the work into small batches.

**Output**: `plans/PLAN-001-dark-mode.md` with:
- Problem statement
- Success criteria
- Tasks (🟩 done / 🟨 in progress / 🟥 blocked / ⬜ pending)
- Testing strategy

**Example**:
```
/plan backlog/issues/ISSUE-001-dark-mode.md
```

**Next**: Start implementation.

---

### 3. Build Phase

**When**: You have a plan and are ready to code.

**What**: Use `/build` to implement with TDD. Work in small batches (< 200 LOC per commit).

**Output**: Code + tests + progress updates to `plans/PLAN-NNN.md`

**Example**:
```
/build plans/PLAN-001-dark-mode.md
```

**Next**: Review your work.

---

### 4. Review Phase

**When**: Feature is implemented and tests pass.

**What**: Use `/review` for cross-model code review (1-3 independent reviewers).

**Output**: `decisions/code-review-YYYY-MM-DD-001.md` with:
- What changed
- Review findings (security, performance, maintainability)
- Verdict (SHIP / REVISE / BLOCK)

**Example**:
```
/review plans/PLAN-001-dark-mode.md
```

**Next**: Ship or revise based on verdict.

---

### 5. Retro Phase

**When**: After shipping (success) or incident (failure).

**What**: Use `/retro` to extract learnings. Run 5 Whys for incidents.

**Output**: `retros/RETRO-YYYY-MM-DD.md` with:
- What happened
- What went well / poorly
- Action items

**Example**:
```
/retro "Dark mode shipped successfully"
/retro "Production outage: API timeout cascade"
```

**Next**: Update `knowledge/` with reusable solutions.

---

## Team Workflow (Tier 2+)

### When to Use `/rfc`

**Trigger**: You're about to make a decision that:
- Affects multiple features
- Has long-term consequences
- Involves trade-offs between options
- Needs team buy-in

**Examples**:
- Choosing a state management library
- Defining API versioning strategy
- Deciding database schema migrations approach

**Workflow**:
1. Draft: `/rfc "Proposal: Use Zustand for state management"`
2. Review: Share `rfcs/RFC-001-zustand.md` with team
3. Decide: Update status → `accepted` / `rejected` / `superseded`
4. Implement: Reference RFC in plan (`PLAN-NNN.md`)

**Output**: `rfcs/RFC-001-zustand.md` (ADR format: context, decision, consequences)

---

### When to Use `/handoff`

**Trigger**: You're about to:
- Go on vacation
- Transfer a feature to another dev
- Onboard someone to a codebase area
- Switch context for >1 week

**Workflow**:
1. Create: `/handoff "Dark mode feature → Alex"`
2. Include: What's done, what's pending, gotchas, next steps
3. Review: Walk through with recipient (sync or async)
4. Archive: Move to `handoffs/archive/` when complete

**Output**: `handoffs/HANDOFF-2026-02-15-dark-mode-to-alex.md`

---

## Workflow Patterns

### Pattern 1: New Feature (Happy Path)

```
/explore "Add user notifications" 
  ↓
Convert to backlog/issues/ISSUE-042.md
  ↓
/plan backlog/issues/ISSUE-042.md
  ↓
/build plans/PLAN-042.md (small batches, TDD)
  ↓
/review plans/PLAN-042.md
  ↓
Ship ✅
  ↓
/retro "Notifications shipped"
```

---

### Pattern 2: Architecture Decision (Team)

```
Problem: Need to choose API framework
  ↓
/rfc "Proposal: FastAPI vs Flask for new microservice"
  ↓
Team reviews rfcs/RFC-013-api-framework.md
  ↓
Decision: FastAPI (update RFC status → accepted)
  ↓
/plan "Implement auth service with FastAPI"
  ↓
[build → review → ship]
```

---

### Pattern 3: Bug Fix with Learning

```
Bug reported: API timeouts in production
  ↓
/explore "Why are API calls timing out?"
  ↓
/plan "Fix: Add circuit breaker pattern"
  ↓
/build plans/PLAN-055-circuit-breaker.md
  ↓
/review plans/PLAN-055.md
  ↓
Deploy fix
  ↓
/retro "Postmortem: API timeout cascade" (5 Whys)
  ↓
Extract learning → knowledge/circuit-breaker-pattern.md
```

---

### Pattern 4: Context Transfer (Team)

```
Dev A working on dark mode feature
  ↓
Dev A needs to shift to urgent bug
  ↓
/handoff "Dark mode → Dev B"
  ↓
Dev B reads handoffs/HANDOFF-2026-02-15-dark-mode.md
  ↓
Dev B continues from plans/PLAN-001-dark-mode.md
  ↓
(No context loss)
```

---

## Tier Differences

### Tier 1 (Solo)
- **Commands**: explore, plan, build, review, retro (5)
- **Peer review**: Self-review with 1 AI model
- **RFCs**: Optional (use ADR format in `decisions/`)
- **Handoffs**: Not needed (solo context)

### Tier 2 (Small Team)
- **Commands**: All 7
- **Peer review**: 2 AI models (cross-model)
- **RFCs**: Required for architectural decisions
- **Handoffs**: Required for >3 day context switches

### Tier 3 (Enterprise)
- **Commands**: All 7 + stricter gates
- **Peer review**: 3 AI models (multi-provider)
- **RFCs**: Required + mandatory review period
- **Handoffs**: Required + runbook integration

---

## Common Questions

### "When should I skip `/explore`?"

If the problem is well-defined and you know the solution. Example:
- ✅ Skip: "Add a new API endpoint for user profile"
- ❌ Don't skip: "Figure out how to handle real-time notifications"

---

### "Can I combine `/plan` and `/build`?"

**No.** Planning and execution are separate phases for a reason:
1. Planning forces you to think before coding
2. Plans are reviewable (get feedback early)
3. Small batches require pre-planned task breakdown

---

### "How big should a plan be?"

**Target**: Shippable in 1-3 sessions (< 200 LOC per commit).

If a plan grows beyond 10 tasks, split into multiple plans with a parent tracking issue.

---

### "Do I need `/retro` for every feature?"

**Tier 1**: Optional (but recommended for learning)
**Tier 2+**: Required for:
- Incidents (always)
- Features that took >2x estimated time
- First implementation of new patterns

---

### "What if `/review` says BLOCK?"

**Fix the issues** identified by reviewers, then:
1. Update code
2. Run `/review` again
3. Repeat until verdict = SHIP

**Never ship on BLOCK.**

---

## Integration with Git

All artifacts are tracked in git. Recommended workflow:

```bash
# After /explore
git add backlog/explorations/EXP-001.md
git commit -m "exploration: dark mode spike"

# After /plan
git add plans/PLAN-001-dark-mode.md
git commit -m "plan: dark mode implementation"

# During /build (small batches)
git add src/theme.js tests/theme.test.js plans/PLAN-001-dark-mode.md
git commit -m "feat: add theme toggle component [PLAN-001]"

# After /review
git add decisions/code-review-2026-02-15-001.md
git commit -m "review: dark mode code review [SHIP]"

# After /retro
git add retros/RETRO-2026-02-15.md knowledge/theme-toggle-pattern.md
git commit -m "retro: dark mode learnings"
```

**Prefix convention**:
- `exploration:` — Explorations
- `plan:` — Plans
- `feat:` — Feature code
- `fix:` — Bug fixes
- `review:` — Code reviews
- `retro:` — Retrospectives
- `rfc:` — Architecture decisions
- `handoff:` — Context transfers

---

## Workflow Checklist

Before claiming "done":

- [ ] Plan exists (`plans/PLAN-NNN.md`)
- [ ] All tasks marked 🟩 (done)
- [ ] Tests written and passing
- [ ] Code review completed (`/review` → SHIP)
- [ ] Plan updated with final status
- [ ] Retro completed (if warranted)
- [ ] Knowledge extracted (if new pattern)

**If any are missing, the work is not done.**

---

## Advanced: Workflow Phasing (Tier 3)

For complex features, phase the work:

1. **Phase 1 — Strategy**
   - `/explore` → `/rfc` → Decision
   
2. **Phase 2 — Foundations**
   - `/plan` → `/build` (infrastructure only)
   - `/review` → Ship foundational layer
   
3. **Phase 3 — Features**
   - `/plan` → `/build` (user-facing features)
   - `/review` → Ship behind feature flag
   
4. **Phase 4 — Rollout**
   - `/plan` (rollout strategy) → Enable feature flag
   - `/retro` → Extract learnings

**Each phase is independently reviewable and shippable.**

---

*For setup instructions, see [SETUP.md](SETUP.md).*
*For agent configuration, see [AGENTS.md](AGENTS.md).*
*For elite practices reference, see [PRACTICES.md](PRACTICES.md).*
