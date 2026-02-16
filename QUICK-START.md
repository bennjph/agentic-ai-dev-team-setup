# Quick Start

Get up and running in 15 minutes.

---

## 1. Choose Your Tier (2 min)

| Tier | Team Size | Commands | Review |
|------|-----------|----------|--------|
| **Solo** | 1 dev | 5 (core) | 1 model |
| **Team** | 2-10 devs | 7 (all) | 2+ models (cross-provider) |
| **Enterprise** | 10+ devs | 7 (strict) | 3+ models (multi-provider) |

**Most users**: Start with **Solo** or **Team**.

**Edit** `config/TIER.md` and set your tier.

---

## 2. Configure Models (5 min)

Set environment variables for AI models:

```bash
# Example: Team tier (cross-model review)
export OPENCODE_BUILDER_MODEL="openai/gpt-5-3-codex"
export OPENCODE_REVIEWER_MODEL="anthropic/claude-sonnet-4-5"
```

**Or** use `.env` file:
```bash
# .env
OPENCODE_BUILDER_MODEL=openai/gpt-5-3-codex
OPENCODE_REVIEWER_MODEL=anthropic/claude-sonnet-4-5
```

**See** `config/PEER_REVIEW.md` for model pairing recommendations.

---

## 3. Install Dependencies (2 min)

**If using OpenCode**:
```bash
npm install -g opencode
# or: pip install opencode
```

**If using Cursor/Claude Code**: No install needed (built-in).

---

## 4. Run Your First Command (5 min)

### Create a Backlog Issue

```bash
# Copy template
cp templates/ISSUE_TEMPLATE.md backlog/issues/ISSUE-001-dark-mode.md

# Edit the file
# - Add description: "Add dark mode toggle to app"
# - Add acceptance criteria
```

---

### Plan the Work

```bash
opencode /plan backlog/issues/ISSUE-001-dark-mode.md
```

**Output**: `plans/PLAN-001-dark-mode.md`

**What it contains**:
- Problem statement
- Success criteria
- Tasks (< 200 LOC each)
- Testing strategy

---

### Build the Feature

```bash
opencode /build plans/PLAN-001-dark-mode.md
```

**What happens**:
- AI reads plan
- Implements tasks with TDD (RED → GREEN → REFACTOR)
- Updates plan progress (🟩 done / 🟨 in progress)

---

### Review the Code

```bash
opencode /review plans/PLAN-001-dark-mode.md
```

**Output**: `decisions/code-review-2026-02-15-001.md`

**Verdict**: SHIP / REVISE / BLOCK

---

### Ship It

If review says **SHIP**:
```bash
git push origin main
# Deploy to production
```

---

## 5. Learn the Workflow (Optional)

Read these for deeper understanding:

- **WORKFLOW.md** — When to use each command
- **PRACTICES.md** — The 15 elite practices
- **AGENTS.md** — How AI agents work

---

## Common First-Time Mistakes

### ❌ Skipping `/plan`

**Don't**:
```bash
opencode /build "Add dark mode"  # No plan!
```

**Do**:
```bash
opencode /plan "Add dark mode"   # Create plan first
opencode /build plans/PLAN-001-dark-mode.md
```

---

### ❌ Large Commits

**Don't**: Implement entire feature in one commit (800 LOC)

**Do**: Break into small batches (< 200 LOC per commit)

**How**: Plan defines tasks. Each task = 1 commit.

---

### ❌ Skipping Tests

**Don't**: Ship without tests

**Do**: TDD (tests before code)

**How**: `/build` enforces TDD by default

---

### ❌ Ignoring Review Feedback

**Don't**: Ship when review says BLOCK or REVISE

**Do**: Fix issues, re-run `/review`

---

## Next Steps

### Solo Tier

**Master these 5 commands**:
1. `/explore` — Spike without code
2. `/plan` — Turn issue into plan
3. `/build` — Implement with TDD
4. `/review` — Code review
5. `/retro` — Learning (optional)

**Then**: Practice the workflow on 3-5 features.

---

### Team Tier

**Add these 2 commands**:
6. `/rfc` — Architecture decisions
7. `/handoff` — Context transfers

**Setup**:
- Configure cross-model review (see `config/PEER_REVIEW.md`)
- Define working agreements (see `config/WORKING_AGREEMENTS.md`)
- Practice handoffs between team members

---

### Enterprise Tier

**Enable strict gates**:
- Multi-model review (3+ reviewers)
- Feature flags for risky changes
- Workflow phasing (strategy → foundations → features → rollout)

**See** `SETUP.md` for full Enterprise configuration.

---

## Troubleshooting

### "Command not found"

**Problem**: OpenCode not installed or not in PATH

**Fix**:
```bash
npm install -g opencode
# or: pip install opencode
```

---

### "Model not configured"

**Problem**: Environment variable not set

**Fix**:
```bash
export OPENCODE_BUILDER_MODEL="openai/gpt-5-3-codex"
```

---

### "Cross-model review failed"

**Problem**: Builder and reviewer use same model

**Fix**: Set different models (see `config/PEER_REVIEW.md`)

---

### "Plan file not found"

**Problem**: Typo in file path

**Fix**: Use tab completion or copy path from `/plan` output

---

## Getting Help

- **Documentation**: Read WORKFLOW.md, SETUP.md, PRACTICES.md
- **Issues**: Open issue on GitHub
- **Discussions**: Ask in GitHub Discussions

---

## Cheat Sheet

| Command | Purpose | Output |
|---------|---------|--------|
| `/explore` | Research (no code) | `backlog/explorations/EXP-NNN.md` |
| `/plan` | Create executable plan | `plans/PLAN-NNN.md` |
| `/build` | Implement with TDD | Code + tests + updated plan |
| `/review` | Code review | `decisions/code-review-YYYY-MM-DD-NNN.md` |
| `/retro` | Postmortem learning | `retros/RETRO-YYYY-MM-DD.md` |
| `/rfc` | Architecture decision | `rfcs/RFC-NNN-title.md` |
| `/handoff` | Context transfer | `handoffs/HANDOFF-YYYY-MM-DD-feature.md` |

---

*Start with `/plan` → `/build` → `/review`. Master the basics before adding complexity.*
