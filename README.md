# Product Development Workflow

Elite product development with AI assistance — model-agnostic, tier-based, process-first.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![GitHub](https://img.shields.io/badge/GitHub-agentic--ai--dev--team--setup-blue)](https://github.com/bennjph/agentic-ai-dev-team-setup)

---

## What Is This?

A **complete, production-ready workflow** for building software with AI coding assistants.

**Synthesizes elite practices from**:
- Google SRE (reliability engineering)
- DORA State of DevOps (research-backed)
- Martin Fowler (software architecture)
- Linear Method (product development)
- Elite dev workflows (Zevi, Replit, Vercel)

**Works with**: OpenCode, Cursor, Claude Code, or any AI coding assistant.

---

## Quick Start (15 Minutes)

```bash
# 1. Choose your tier (edit config/TIER.md)
# Solo (1 dev) | Team (2-10 devs) | Enterprise (10+ devs)

# 2. Configure AI models (edit config/PEER_REVIEW.md)
export OPENCODE_BUILDER_MODEL="openai/gpt-5-3-codex"
export OPENCODE_REVIEWER_MODEL="anthropic/claude-sonnet-4-5"

# 3. Create your first plan
opencode /plan "Add dark mode to app"

# 4. Build it
opencode /build plans/PLAN-001-dark-mode.md

# 5. Review it
opencode /review plans/PLAN-001-dark-mode.md

# 6. Ship it ✅
```

**Full guide**: [QUICK-START.md](QUICK-START.md)

---

## The 7 Commands

| Command | Purpose | Tier |
|---------|---------|------|
| `/explore` | Spike without code (NO CODE exploration) | All |
| `/plan` | Turn issue into executable plan | All |
| `/build` | Implement with TDD (small batches) | All |
| `/review` | Cross-model code review (SHIP/REVISE/BLOCK) | All |
| `/retro` | Postmortem learning (5 Whys) | All |
| `/rfc` | Architecture decision record (ADR) | Team+ |
| `/handoff` | Context transfer (SBAR format) | Team+ |

**Workflow**: [WORKFLOW.md](WORKFLOW.md)

---

## The 15 Elite Practices

### Tier 1 (Solo — 5 practices)
1. Trunk-based development
2. Self-testing code (TDD)
3. Small batch delivery (< 200 LOC)
4. Strategy-execution split (/plan → /build)
5. Cross-model review (different AI models)

### Tier 2 (Team — 10 practices)
6. Feature flags (gradual rollout)
7. Postmortems (5 Whys root cause)
8. Observability (logs, metrics, traces)
9. Compound learning (reusable knowledge)
10. Visual progress tracking (emoji in plans)

### Tier 3 (Enterprise — 15 practices)
11. Architecture Decision Records (ADRs)
12. Multi-model validation (3+ reviewers)
13. Tiered teaching (3-level explanations)
14. Workflow phasing (strategy → rollout)
15. Architecture review (prevent tech debt)

**Full details**: [PRACTICES.md](PRACTICES.md)

---

## Project Structure

```
playbooks/product-development/
├── README.md                    # This file
├── QUICK-START.md               # 15-min getting started
├── SETUP.md                     # Full configuration guide
├── WORKFLOW.md                  # How to use commands
├── PRACTICES.md                 # The 15 elite practices
├── AGENTS.md                    # AI agent configuration
│
├── .opencode/
│   ├── commands/                # 7 commands
│   │   ├── explore.md
│   │   ├── plan.md
│   │   ├── build.md
│   │   ├── review.md
│   │   ├── retro.md
│   │   ├── rfc.md
│   │   └── handoff.md
│   └── agents/                  # 6 AI agents
│       ├── orchestrator.md
│       ├── strategist.md
│       ├── builder.md
│       ├── reviewer.md
│       ├── analyst.md
│       └── writer.md
│
├── config/                      # Configuration
│   ├── TIER.md                  # Solo / Team / Enterprise
│   ├── PEER_REVIEW.md           # AI model selection
│   └── WORKING_AGREEMENTS.md    # Team conventions
│
├── templates/                   # File templates
│   ├── ISSUE_TEMPLATE.md
│   ├── PLAN_TEMPLATE.md
│   ├── ADR_TEMPLATE.md
│   ├── RFC_TEMPLATE.md
│   ├── HANDOFF_TEMPLATE.md
│   └── RETRO_TEMPLATE.md
│
└── [artifact directories]/      # Where work gets saved
    ├── backlog/                 # Issues + explorations
    ├── plans/                   # Executable plans
    ├── decisions/               # Code reviews + ADRs
    ├── rfcs/                    # Architecture decisions
    ├── handoffs/                # Context transfers
    ├── retros/                  # Postmortems
    └── knowledge/               # Compound learning
```

---

## Key Innovations

### 1. NO CODE Exploration

**Writing code during `/explore` is forbidden.** Exploration is about understanding, not implementation.

**Why?** Code commits you to a solution before you understand the problem.

---

### 2. Cross-Model Review

**Builder uses Model A, Reviewer uses Model B** (different providers preferred).

**Why?** Same model = same blind spots. Cross-model catches more issues.

**Example**:
- Builder (GPT-5.3) → Reviewer (Claude Sonnet 4.5)
- Builder (Claude Opus) → Reviewer (Kimi K2 Thinking)

---

### 3. Emoji Progress Tracking

Plans use emoji for visual status:
- 🟩 Done
- 🟨 In Progress
- 🟥 Blocked
- ⬜ Pending

**Glanceable. No "what's the status?" meetings.**

---

### 4. Artifact-Driven Philosophy

**"If you didn't update artifacts, you didn't do the work."**

Every command creates or modifies files in your repo. No oral tradition.

---

### 5. Tier-Based Scaling

**Solo** (5 practices, 5 commands) → **Team** (10 practices, 7 commands) → **Enterprise** (15 practices, strict gates)

Start simple. Add rigor as you scale.

---

## Use Cases

### Solo Developer

**You**: Building a SaaS product solo.

**Use**:
- `/plan` → `/build` → `/review` loop
- TDD for quality
- Cross-model review for blind spot detection

**Benefit**: Ship faster with fewer bugs.

---

### Small Team (2-10 devs)

**You**: 5 devs building a B2B app.

**Use**:
- All 7 commands
- `/rfc` for architecture decisions
- `/handoff` for context transfers
- Cross-model review (2 AI models)

**Benefit**: Better coordination, less rework.

---

### Enterprise (10+ devs)

**You**: 50 devs, multiple teams, high-stakes production.

**Use**:
- All 15 practices
- Multi-model review (3+ AI models, different providers)
- Feature flags for gradual rollout
- Workflow phasing (strategy → rollout)

**Benefit**: Sustainable pace, institutional knowledge.

---

## Philosophy

### Process-First, Not Tool-First

**Value is in the discipline**, not the tooling.

These commands work with any AI coding assistant. You can even use them manually (without AI) by following the templates.

---

### Model-Agnostic

**No vendor lock-in.** Works with:
- OpenAI (GPT-5.3, GPT-5.2)
- Anthropic (Claude Opus, Sonnet)
- Moonshot (Kimi K2)
- DeepSeek (V3)
- Google (Gemini)
- Any future models

Configure via environment variables. Switch anytime.

---

### Git-Friendly

**Everything tracked in git**:
- Plans (`plans/`)
- Reviews (`decisions/`)
- RFCs (`rfcs/`)
- Retros (`retros/`)
- Knowledge (`knowledge/`)

**Why?** Artifacts are project history. Future devs understand "why" not just "what."

---

## Metrics

**How to know if it's working**:

| Metric | Target |
|--------|--------|
| Time to ship feature | < 1 week |
| Test coverage | > 80% |
| Repeat incidents | < 10% |
| Code review turnaround | < 24 hours |
| Time to onboard new dev | < 1 day (with docs) |

**Track monthly. Adjust process if metrics decline.**

---

## FAQ

### "Do I need all 15 practices?"

**No.** Start with Tier 1 (5 practices). Add more as you grow.

---

### "Can I skip TDD?"

**Not recommended.** TDD is foundational. Without tests, refactoring and small batches become risky.

---

### "What if my team resists?"

**Start small.** Adopt 1 command at a time. Show value before adding more.

**Example**:
1. Week 1: Add `/plan` (planning discipline)
2. Week 2: Add `/review` (quality gate)
3. Week 3: Add `/build` with TDD

---

### "How is this different from X?"

| vs. | Difference |
|-----|------------|
| **GitHub Copilot** | Copilot is code completion. This is full workflow (planning → review). |
| **Cursor Composer** | Cursor is tool-specific. This is model-agnostic (works with any AI). |
| **Linear** | Linear is issue tracking. This includes planning, building, reviewing. |
| **SCRUM** | SCRUM has 2-week sprints. This ships in < 200 LOC batches (daily). |

**Closest comparison**: Combines Linear (planning) + Cursor (building) + GitHub (reviews) + SRE practices (reliability).

---

## Installation

### Copy to Your Project

```bash
# Clone the template
git clone https://github.com/bennjph/agentic-ai-dev-team-setup.git
cd agentic-ai-dev-team-setup/playbooks/product-development

# Copy to your project
cp -r . /path/to/your/project/
```

---

### Configure

1. **Choose tier**: Edit `config/TIER.md`
2. **Set models**: Edit `config/PEER_REVIEW.md` or set env vars
3. **Define conventions**: Edit `config/WORKING_AGREEMENTS.md`

**Full setup**: [SETUP.md](SETUP.md)

---

## Contributing

**Contributions welcome!**

- Bug fixes (typos, broken links)
- Clarity improvements (better examples)
- New tier-specific features

**See**: [CONTRIBUTING.md](CONTRIBUTING.md)

---

## License

MIT License. See [LICENSE](LICENSE).

---

## Credits

**Synthesized from**:
- [Google SRE Book](https://sre.google/sre-book/)
- [Accelerate (DORA)](https://www.oreilly.com/library/view/accelerate/9781457191435/)
- [Martin Fowler's Bliki](https://martinfowler.com/bliki/)
- [Linear Method](https://linear.app/method)
- [Zevi's Workflow](https://x.com/zevi/status/1871649436998160865)

**Created by**: OpenCode community

**Maintained by**: https://github.com/bennjph

---

## Support

- **Documentation**: See [WORKFLOW.md](WORKFLOW.md), [PRACTICES.md](PRACTICES.md)
- **Issues**: https://github.com/bennjph/agentic-ai-dev-team-setup/issues
- **Discussions**: https://github.com/bennjph/agentic-ai-dev-team-setup/discussions

---

*Ship faster. Ship better. Ship together.*
