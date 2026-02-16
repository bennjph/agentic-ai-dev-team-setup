<<<<<<< HEAD
# Product Development Template for OpenCode

> **A 3-tier, process-first template for shipping high-quality software with AI-assisted workflows.**

Model-agnostic. Evidence-based. Copy-paste ready.

---

## What You Get

This template provides a complete **product development workflow** distilled from elite engineering practices at Google, Netflix, Spotify, Linear, and others — adapted for modern AI-assisted development.

**7 workflow commands**:
- `/explore` — Deep analysis before coding (NO CODE rule)
- `/plan` — Implementation planning with emoji progress tracking (🟩🟨🟥)
- `/build` — Test-Driven Development, step-by-step
- `/review` — Configurable peer review (1-3 AI models or humans)
- `/retro` — Postmortem + knowledge capture
- `/rfc` — Team decision-making (Tier 2+)
- `/handoff` — Structured handoffs (Tier 2+)

**15 elite practices** (tiered adoption):
- **Tier 1 (Solo)**: 5 essential practices
- **Tier 2 (Small Team)**: 10 practices (adds coordination)
- **Tier 3 (Enterprise)**: All 15 (organizational scale)

**Complete artifact system**:
- `plans/` — Exploration and implementation plans
- `decisions/` — Architecture Decision Records (ADRs)
- `knowledge/` — Searchable solution vault
- `retros/` — Postmortems and retrospectives
- `rfcs/` — Team decision proposals
- `handoffs/` — Context transfer between teammates

---

## Who Is This For?

**Solo developers** shipping products with AI assistance.

**Small teams** (2-6 people) coordinating on the same codebase.

**Anyone** who wants **process discipline** without heavyweight tools.

---

## Quick Start

### 1. Copy the Template

```bash
# Clone or download from GitHub
git clone https://github.com/bennjph/agentic-ai-dev-team-setup.git

# Copy to your project
cp -r agentic-ai-dev-team-setup/playbooks/product-development/* your-project/

# Or use it as a starting point for a new project
mv agentic-ai-dev-team-setup/playbooks/product-development/ my-new-project/
cd my-new-project/
```

### 2. Run Setup

```bash
# Read the setup guide
cat SETUP.md

# Follow the guided onboarding (5-10 minutes)
# - Select your tier (Solo / Small Team / Enterprise)
# - Configure peer review (0-3 reviewers)
# - Fill in project context
```

### 3. Start Working

```bash
# Open with OpenCode (or compatible tool)
opencode

# Create a backlog item, then:
/explore   # Understand the problem (no code)
/plan      # Create an implementation plan
/build     # Implement with TDD
/review    # Self + peer review
/retro     # Capture learnings
```

**Full workflow**: `explore → plan → build → review → retro`

---

## What Makes This Different?

### 1. Process-First, Not Tool-First

The value is in the **workflow discipline**, not the tooling. These practices work whether you're using:
- OpenCode (primary)
- Cursor
- Claude Code / Codex
- Any tool that reads markdown commands

**Portable by design** — if you switch tools, the workflow stays the same.

---

### 2. Model-Agnostic

**No hardcoded AI providers.** Works with:
- Claude (Anthropic)
- GPT (OpenAI)
- Gemini (Google)
- Open-source models (DeepSeek, Qwen, Llama, etc.)
- Any combination of the above

Configure your models via environment variables. The workflow stays identical.

---

### 3. Tiered Adoption (Not All-or-Nothing)

**Start simple, scale up when needed.**

| Tier | Team Size | Practices | Commands |
|------|-----------|-----------|----------|
| **Solo** | 1 person | 5 essential | 5 core |
| **Small Team** | 2-6 people | 10 practices | 7 (adds RFC + handoff) |
| **Enterprise** | 7+ people | All 15 | 7 (stricter gates) |

You choose your tier during setup. Practices and gates adjust accordingly.

---

### 4. Evidence-Based

Every practice is grounded in published research:
- Google SRE (Site Reliability Engineering)
- DORA (DevOps Research & Assessment)
- Martin Fowler (Continuous Integration, Feature Flags)
- Linear Method (small batch delivery)
- Zevi Arnovitz workflow ("less context" peer review)

**Not opinions — proven patterns from elite teams.**

---

### 5. Artifact-Driven Development

**"If you didn't update the artifacts, you didn't do the work."**

All work produces **version-controlled artifacts**:
- Exploration findings → `plans/*-exploration.md`
- Implementation plans → `plans/*-plan.md`
- Reviews → `plans/*-review.md`
- Postmortems → `retros/*.md`
- Decisions → `decisions/ADR-*.md`
- Knowledge → `knowledge/solutions/*.md`

These artifacts are your **product memory** — searchable, shareable, durable.
=======
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
>>>>>>> bd443ae06242e7408df2a1daa11ef2ca0ce6b30c

---

## The 15 Elite Practices

<<<<<<< HEAD
### Tier 1: Solo Foundation

1. **Trunk-Based Development** — Work on main, no long-lived branches
2. **Self-Testing Code** — Automated tests verify every change
3. **Small Batch Delivery** — Ship in 1-3 week increments
4. **Strategic-Execution Separation** — Plan before you code
5. **Cross-Model Review** — Different AI reviews than created

### Tier 2: Small Team Coordination

6. **Feature Flags** — Deploy hidden, release progressively
7. **Structured Postmortems** — Learn from failures (5 Whys)
8. **Observability-First** — Build logging/metrics from start
9. **Compound Learning** — Capture searchable solutions
10. **Visual Progress Tracking** — Emoji status (🟩🟨🟥)

### Tier 3: Enterprise Scale

11. **Architecture Decision Records** — Document significant decisions
12. **Multi-Model Validation** — 3-4 reviewers for critical code
13. **Tiered Teaching** — 3-level docs (What/How/Why)
14. **Workflow Phasing** — Gated pipeline (explore → plan → build → review → ship)
15. **Continuous Architecture Review** — Quarterly health checks

**See `PRACTICES.md` for full details** (with WHY each practice works).

---

## Example Workflow

### Scenario: Add User Authentication

```bash
# 1. Create backlog item
echo "Add JWT authentication to API" > backlog/2026-02-15-add-auth.md

# 2. Explore (understand the problem)
/explore backlog/2026-02-15-add-auth.md
# Output: plans/2026-02-15-add-auth-exploration.md
# - Current state mapped
# - Risks identified
# - Approach recommended
# - NO CODE written

# 3. Plan (break into tasks)
/plan plans/2026-02-15-add-auth-exploration.md
# Output: plans/2026-02-15-add-auth-plan.md
# - Tasks with emoji status: 🟥🟨🟩
# - Test plan defined
# - Acceptance criteria clear

# 4. Build (TDD implementation)
/build plans/2026-02-15-add-auth-plan.md
# - Write test first
# - Implement minimum code
# - Update plan status
# - Repeat for each task

# 5. Review (self + peer)
/review
# - Self-review finds obvious issues
# - Peer review (different AI model) finds blind spots
# - Verdict: PASS / CONDITIONAL PASS / FAIL

# 6. Retro (capture learnings)
/retro
# - What went well / wrong
# - Root cause analysis (if issues)
# - Action items created
# - Knowledge captured in vault
```

**Total time**: Exploration (30min) + Planning (30min) + Build (varies) + Review (15min) + Retro (15min)

**Result**: Working code + tests + documentation + learnings captured.

---

## File Structure

```
your-project/
├── README.md                    ← This file
├── SETUP.md                     ← Guided onboarding
├── PRACTICES.md                 ← 15 elite practices reference
├── WORKFLOW.md                  ← When to use each command
├── AGENTS.md                    ← Model-agnostic system prompt
├── opencode.json                ← OpenCode config
├── .opencode/
│   ├── agents/                  ← Agent prompts (6 files)
│   └── commands/                ← Workflow commands (7 files)
├── config/
│   ├── TIER.md                  ← Solo / Small Team / Enterprise
│   ├── PEER_REVIEW.md           ← Review policy
│   └── WORKING_AGREEMENTS.md    ← Team norms
├── docs/
│   ├── PROJECT_BRIEF.md         ← Problem, users, goals
│   ├── ARCHITECTURE.md          ← System design
│   ├── TECH_STACK.md            ← Technology choices
│   ├── QUALITY_STANDARDS.md     ← Testing/security rules
│   └── REPO_MAP.md              ← Where things live
├── backlog/                     ← Issues and features
├── plans/                       ← Exploration + implementation plans
├── decisions/                   ← Architecture Decision Records
├── rfcs/                        ← Team decision proposals
├── handoffs/                    ← Context transfers
├── retros/                      ← Postmortems
└── knowledge/
    ├── CRITICAL_PATTERNS.md     ← Recurring issues prevention
    └── solutions/               ← Searchable solution vault
=======
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
>>>>>>> bd443ae06242e7408df2a1daa11ef2ca0ce6b30c
```

---

<<<<<<< HEAD
## Requirements

**Minimum**:
- Git (for version control)
- Any AI tool that reads markdown (OpenCode, Cursor, Claude Code, etc.)
- Access to at least 1 AI model (any provider)

**Recommended**:
- OpenCode (primary target)
- 2+ AI models from different providers (for peer review)
- Node.js / Python / your language runtime (for running tests)

**No complex dependencies** — this is a markdown-based workflow.

---

## Comparison to Other Workflows

| Feature | This Template | Typical Agile | Zevi Workflow |
|---------|---------------|---------------|---------------|
| **Tool dependency** | Any markdown-based tool | Jira, Rally, etc. | Cursor + ChatGPT |
| **Model lock-in** | None (configurable) | N/A | ChatGPT required |
| **Process weight** | Tiered (5→10→15 practices) | Heavy (ceremonies, rituals) | Moderate (8 phases) |
| **Artifact tracking** | Git + markdown files | Separate tools | ChatGPT Projects |
| **Peer review** | Cross-model (1-3 AI reviewers) | Human only | Cross-tool (Cursor + ChatGPT) |
| **Knowledge capture** | Built-in (retro → knowledge vault) | External wiki | Manual |
| **Cost** | $0 (open source) | $$ (tooling subscriptions) | $ (AI subscriptions) |
=======
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
>>>>>>> bd443ae06242e7408df2a1daa11ef2ca0ce6b30c

---

## FAQ

<<<<<<< HEAD
### Can I use this without OpenCode?

**Yes.** The commands are plain markdown. Copy them to:
- Cursor: `.cursor/commands/`
- Claude Code: `.claude/commands/`
- Any tool that reads slash commands

The workflow is **tool-agnostic by design**.

---

### Do I need all 15 practices?

**No.** Start with **Tier 1 (5 practices)** for solo work. Add Tier 2 when you have teammates. Add Tier 3 when you hit scale/complexity.

The template adapts to your tier selection.

---

### Can I customize this?

**Yes — that's the point.** This is a starting template. Customize:
- Commands (add/remove/modify)
- Practices (adopt what fits)
- Tier gates (adjust approval rules)
- Artifact formats (change templates)

Fork it. Make it yours.

---

### What if I only have 1 AI model?

**Peer review still works** with 1 model:
- Self-review (same model, different prompt)
- Time-delayed review (wait 24h, review with fresh eyes)
- Human review (always an option)

See `SETUP.md` for 1-model configuration.

---

### Is this for web apps only?

**No.** This workflow works for:
- Web apps (frontend + backend)
- APIs and microservices
- CLI tools
- Mobile apps (React Native, Flutter, etc.)
- Desktop apps (Electron, etc.)

Any software product benefits from process discipline.
=======
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
>>>>>>> bd443ae06242e7408df2a1daa11ef2ca0ce6b30c

---

## Contributing

<<<<<<< HEAD
Found a bug? Have a suggestion? Want to add a practice?

**GitHub repo**: https://github.com/bennjph/agentic-ai-dev-team-setup

- **Issues**: Report bugs or request features
- **Pull Requests**: Contribute improvements
- **Discussions**: Share your experience using this template

See `CONTRIBUTING.md` for guidelines.
=======
**Contributions welcome!**

- Bug fixes (typos, broken links)
- Clarity improvements (better examples)
- New tier-specific features

**See**: [CONTRIBUTING.md](CONTRIBUTING.md)
>>>>>>> bd443ae06242e7408df2a1daa11ef2ca0ce6b30c

---

## License

<<<<<<< HEAD
**Open Source** — Use freely, modify as needed, attribution appreciated.

See `LICENSE` for details.
=======
MIT License. See [LICENSE](LICENSE).
>>>>>>> bd443ae06242e7408df2a1daa11ef2ca0ce6b30c

---

## Credits

**Synthesized from**:
<<<<<<< HEAD
- Google SRE practices
- DORA research (Forsgren et al.)
- Martin Fowler's Continuous Integration patterns
- Linear Method (small batch delivery)
- Zevi Arnovitz workflow ("less context" peer review)
- OpenCode M1 learnings (multi-agent systems)

**Created by**: [Your name / team]

**Maintained at**: https://github.com/bennjph/agentic-ai-dev-team-setup

---

## Get Started

```bash
# 1. Copy this template to your project
cp -r playbooks/product-development/* your-project/

# 2. Read SETUP.md
cat SETUP.md

# 3. Start your first workflow
/explore "Your first feature"
```

**Questions?** See `WORKFLOW.md` for detailed guidance on each command.

---

*Version 1.0 — Built for developers who ship.*
=======
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
>>>>>>> bd443ae06242e7408df2a1daa11ef2ca0ce6b30c
