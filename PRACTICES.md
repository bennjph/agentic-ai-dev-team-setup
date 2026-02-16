# Elite Development Practices

The 15 practices that power this workflow.

---

## Philosophy

**Process-first, not tool-first.** These practices synthesize research from:
- Google SRE (reliability engineering)
- DORA State of DevOps (research-backed metrics)
- Martin Fowler (software architecture)
- Linear Method (product development)
- Elite dev workflows (Zevi, Replit, Vercel)

**Value is in the discipline, not the tooling.**

---

## The 15 Practices (3 Tiers)

### Tier 1: Solo (5 practices)

Foundation practices that work for any developer, any project.

| # | Practice | Why It Matters |
|---|----------|----------------|
| 1 | **Trunk-based development** | Ship faster, reduce merge conflicts |
| 2 | **Self-testing code (TDD)** | Catch bugs early, enable refactoring |
| 3 | **Small batch delivery** | Lower risk, faster feedback |
| 4 | **Strategy-execution split** | Think before coding (plan → build) |
| 5 | **Cross-model review** | Different models catch different bugs |

---

### Tier 2: Small Team (10 practices)

Adds coordination and learning for teams of 2-10.

| # | Practice | Why It Matters |
|---|----------|----------------|
| 6 | **Feature flags** | Ship incomplete features safely, gradual rollout |
| 7 | **Postmortems (5 Whys)** | Learn from incidents, prevent repeats |
| 8 | **Observability** | Understand system behavior in production |
| 9 | **Compound learning** | Build institutional memory, solve faster |
| 10 | **Visual progress tracking** | Glanceable status, reduce status meetings |

---

### Tier 3: Enterprise (15 practices)

Adds rigor and scale for teams of 10+.

| # | Practice | Why It Matters |
|---|----------|----------------|
| 11 | **Architecture Decision Records (ADRs)** | Document why, not just what |
| 12 | **Multi-model validation** | 3+ reviewers from different providers |
| 13 | **Tiered teaching** | Explain at 3 levels (beginner, intermediate, expert) |
| 14 | **Workflow phasing** | Strategy → foundations → features → rollout |
| 15 | **Architecture review** | Prevent tech debt at scale |

---

## Practice Details

### 1. Trunk-Based Development

**What**: All devs commit to main branch. Short-lived feature branches (< 1 day).

**Why**: Reduces merge conflicts, increases deployment frequency.

**How**:
- Feature flags for incomplete work
- Small commits (< 200 LOC)
- CI runs on every commit

**Example**:
```
main (always deployable)
  ├── feature/dark-mode (merged after 4 hours)
  └── fix/api-timeout (merged after 2 hours)
```

**Source**: [Accelerate (DORA)](https://cloud.google.com/architecture/devops/devops-tech-trunk-based-development)

---

### 2. Self-Testing Code (TDD)

**What**: Write tests before code (RED → GREEN → REFACTOR).

**Why**: Prevents regressions, enables fearless refactoring, acts as living documentation.

**How**:
1. Write failing test (RED)
2. Make test pass (GREEN)
3. Refactor (CLEAN)

**Example**:
```javascript
// RED: Test fails
it('should toggle theme', () => {
  expect(toggleTheme('light')).toBe('dark');
});

// GREEN: Make it pass
function toggleTheme(current) {
  return current === 'light' ? 'dark' : 'light';
}

// CLEAN: Refactor
const THEMES = ['light', 'dark'];
function toggleTheme(current) {
  return THEMES.find(t => t !== current);
}
```

**Source**: [Test-Driven Development (Kent Beck)](https://martinfowler.com/bliki/TestDrivenDevelopment.html)

---

### 3. Small Batch Delivery

**What**: Ship in small increments (< 200 LOC per commit).

**Why**: Faster feedback, lower risk, easier to review.

**How**:
- Break features into vertical slices
- Each slice is independently shippable
- Use feature flags for incomplete flows

**Example** (bad vs. good):

❌ **Bad** (large batch):
- Implement entire auth system (800 LOC, 1 commit)

✅ **Good** (small batches):
- Commit 1: Login form UI (50 LOC)
- Commit 2: Login API endpoint (80 LOC)
- Commit 3: Connect form to API (40 LOC)
- Commit 4: Add error handling (30 LOC)

**Source**: [Lean Software Development](https://www.oreilly.com/library/view/lean-software-development/0321150783/)

---

### 4. Strategy-Execution Split

**What**: Separate planning (/plan) from building (/build).

**Why**: Prevents coding before understanding. Reduces rework.

**How**:
- Exploration phase (no code) → Planning phase (breakdown) → Execution phase (TDD)

**Example**:
```
/explore "How to add notifications?"
  ↓ (research, no code)
/plan "Implement notifications"
  ↓ (break into tasks)
/build "Implement per plan"
  ✅ (code with tests)
```

**Source**: [Zevi workflow](https://x.com/zevi/status/1871649436998160865)

---

### 5. Cross-Model Review

**What**: Use different AI model for review than for building.

**Why**: Same model = same blind spots. Cross-model catches more issues.

**How**:
- Builder: Model A (e.g., GPT-5.3 Codex)
- Reviewer: Model B (e.g., Claude Sonnet 4.5)
- Different providers preferred (OpenAI vs. Anthropic)

**Example**:
- Builder (GPT) misses security issue → Reviewer (Claude) catches it
- Builder (Claude) over-abstracts → Reviewer (GPT) flags complexity

**Source**: [Multi-Model Validation Research](https://arxiv.org/abs/2405.00964)

---

### 6. Feature Flags

**What**: Deploy code behind toggles. Enable gradually (10% → 50% → 100%).

**Why**: De-risk deployments, enable A/B testing, fast rollback.

**How**:
```javascript
if (featureFlags.DARK_MODE) {
  return <ThemeToggle />;
}
```

**Rollout**:
1. Ship code (flag OFF)
2. Enable for 10% users
3. Monitor metrics
4. Gradually increase to 100%

**Source**: [Google SRE](https://sre.google/sre-book/release-engineering/)

---

### 7. Postmortems (5 Whys)

**What**: After incidents, run 5 Whys to find root cause.

**Why**: Prevents repeat incidents, builds learning culture.

**How**:
1. State problem
2. Ask "Why?" 5 times
3. Last answer = root cause
4. Create action items

**Example**:
- Problem: API timeout
- 5 Whys → Root cause: N+1 query pattern
- Action: Fix query + educate team

**Source**: [Google SRE Postmortem Culture](https://sre.google/sre-book/postmortem-culture/)

---

### 8. Observability

**What**: Instrument code to understand behavior (logs, metrics, traces).

**Why**: Debugging in production requires visibility.

**How**:
- Structured logging
- Metrics (request rate, error rate, duration)
- Distributed tracing

**Example**:
```javascript
logger.info('User login', { userId, timestamp, duration });
metrics.increment('login.success');
```

**Source**: [Observability Engineering (Honeycomb)](https://www.honeycomb.io/what-is-observability)

---

### 9. Compound Learning

**What**: Extract learnings from retros → reusable knowledge docs.

**Why**: Don't solve same problem twice. Faster onboarding.

**How**:
1. Solve hard problem
2. Run /retro
3. Extract to knowledge/[topic].md
4. Next time: Search knowledge first

**Example**:
- Bug: N+1 query → Retro → Extract to knowledge/n1-query-pattern.md
- Next dev: Searches knowledge, finds solution in 2 min

**Source**: [Knowledge Management (NASA)](https://www.nasa.gov/offices/oce/functions/knowledge_management.html)

---

### 10. Visual Progress Tracking

**What**: Use emoji in plans (🟩 done / 🟨 in progress / 🟥 blocked / ⬜ pending).

**Why**: Glanceable status. Reduces "What's the status?" meetings.

**How**:
```markdown
### Tasks
1. Add theme context 🟩
2. Create toggle UI 🟨
3. Update styles ⬜
4. Add persistence ⬜

Progress: 1/4 (25%)
```

**Source**: [Linear Method](https://linear.app/method)

---

### 11. Architecture Decision Records (ADRs)

**What**: Document significant decisions (context, decision, consequences).

**Why**: Future devs understand "why" not just "what."

**How**:
- Create RFC for major decisions
- Document: Context, Options, Decision, Consequences

**Example**:
```markdown
# RFC-001: Choose Redux for State Management

Context: App has 50+ components needing shared state
Decision: Redux (over Context API)
Consequence: More boilerplate, but better at scale
```

**Source**: [Michael Nygard ADRs](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions)

---

### 12. Multi-Model Validation

**What**: 3+ AI reviewers from different providers.

**Why**: Consensus improves quality. Different models catch different issues.

**How**:
- Builder: GPT-5.3 Codex (OpenAI)
- Reviewer 1: Claude Sonnet 4.5 (Anthropic)
- Reviewer 2: Kimi K2 (Moonshot)
- Reviewer 3: DeepSeek V3 (DeepSeek)

**Verdict**: All must agree (SHIP / REVISE / BLOCK)

**Source**: [Ensemble Methods in ML](https://en.wikipedia.org/wiki/Ensemble_learning)

---

### 13. Tiered Teaching

**What**: Explain concepts at 3 levels (beginner, intermediate, expert).

**Why**: Accommodates different knowledge levels. Faster onboarding.

**How** (`/learn` command):
- Level 1 (ELI5): Plain language, analogies
- Level 2 (Intermediate): Technical terms, how it works
- Level 3 (Expert): Implementation details, edge cases

**Example** (JWT authentication):
- L1: "Like a ticket that proves you logged in"
- L2: "Signed token with user claims, verified with secret"
- L3: "HMAC-SHA256 signature, base64-encoded JSON payload"

**Source**: [Feynman Technique](https://fs.blog/feynman-technique/)

---

### 14. Workflow Phasing

**What**: Break complex features into phases (strategy → foundations → features → rollout).

**Why**: Each phase is independently reviewable and shippable.

**How**:
1. **Phase 1 (Strategy)**: Explore → RFC → Decision
2. **Phase 2 (Foundations)**: Build infrastructure (behind flag)
3. **Phase 3 (Features)**: Build user-facing features (behind flag)
4. **Phase 4 (Rollout)**: Gradual rollout (10% → 100%)

**Example** (Notifications):
- Phase 1: Decide on SSE vs. WebSockets
- Phase 2: Build event infrastructure
- Phase 3: Build notification UI
- Phase 4: Enable for 10% → 100%

**Source**: [Spotify's "Phases Not Features"](https://engineering.atspotify.com/2014/03/spotify-engineering-culture-part-1/)

---

### 15. Architecture Review

**What**: Senior devs review architectural changes before implementation.

**Why**: Prevents costly mistakes at scale.

**How**:
- RFCs require architect approval (Enterprise tier)
- Review: Scalability, maintainability, consistency with existing patterns

**Example**:
- RFC: "Migrate to microservices"
- Architect review: "Not needed yet. Current monolith scales to 10M users."
- Decision: Reject RFC, defer until needed

**Source**: [Netflix Tech Blog on Architecture](https://netflixtechblog.com/)

---

## Practice Synergies

**These practices reinforce each other**:

- TDD (2) + Small Batches (3) = Low-risk deploys
- Feature Flags (6) + Workflow Phasing (14) = Safe rollouts
- Postmortems (7) + Compound Learning (9) = Faster problem-solving
- Cross-Model Review (5) + Multi-Model Validation (12) = Higher quality

**Combined effect > sum of parts.**

---

## Tier Progression

### Solo → Team

**Add**:
- Feature flags (6)
- Postmortems (7)
- Compound learning (9)
- Visual progress (10)
- Cross-model review (stricter)

**Why**: Coordination and learning become important with teammates.

---

### Team → Enterprise

**Add**:
- ADRs (11)
- Multi-model validation (12)
- Tiered teaching (13)
- Workflow phasing (14)
- Architecture review (15)

**Why**: Scale and rigor needed for 10+ devs and high-stakes production.

---

## Adoption Strategy

**Don't adopt all 15 at once.** Start with Tier 1 (5 practices), master them, then add more.

### Month 1: Tier 1 (Solo)
- Trunk-based dev
- TDD
- Small batches
- Strategy-execution split
- Cross-model review

### Month 2-3: Tier 2 (Team)
- Add feature flags
- Add postmortems
- Add compound learning
- Add visual progress tracking

### Month 4+: Tier 3 (Enterprise)
- Add ADRs
- Add multi-model validation
- Add tiered teaching
- Add workflow phasing
- Add architecture review

**Gradual adoption > big-bang transformation.**

---

## Metrics

**How to know if practices are working**:

| Practice | Metric | Target |
|----------|--------|--------|
| Trunk-based dev | Time to merge PR | < 24 hours |
| TDD | Test coverage | > 80% |
| Small batches | Commits per day | 3-5 |
| Postmortems | Repeat incidents | < 10% |
| Compound learning | Time to solve known issues | < 10 min |
| Feature flags | Rollback time | < 5 min |

**Track monthly. Adjust practices if metrics decline.**

---

## Common Questions

### "Do I need all 15 practices?"

**No.** Start with Tier 1 (5 practices). Add more as your team grows.

### "Can I skip TDD?"

**Not recommended.** TDD (practice 2) is foundational. Without tests, other practices (small batches, refactoring) become risky.

### "What if my team resists?"

**Start small.** Adopt 1 practice at a time. Show value before adding more.

### "How long to see results?"

**Tier 1**: 2-4 weeks (faster shipping, fewer bugs)
**Tier 2**: 2-3 months (better coordination, less rework)
**Tier 3**: 6-12 months (institutional knowledge, sustainable pace)

---

## References

- [Accelerate (DORA)](https://www.oreilly.com/library/view/accelerate/9781457191435/)
- [Google SRE Book](https://sre.google/sre-book/table-of-contents/)
- [Martin Fowler's Bliki](https://martinfowler.com/bliki/)
- [Linear Method](https://linear.app/method)
- [Zevi's Workflow](https://x.com/zevi/status/1871649436998160865)

---

*These practices are distilled from 20+ years of elite software development. Adapt to your context.*
