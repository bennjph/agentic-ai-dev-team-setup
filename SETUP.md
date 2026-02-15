# Setup Guide

Complete configuration in 30-60 minutes.

---

## Overview

**3 configuration steps**:
1. Choose tier (Solo / Team / Enterprise)
2. Configure AI models (cross-model review)
3. Define team conventions (working agreements)

**Time**:
- Solo: 30 minutes
- Team: 60 minutes
- Enterprise: 2-4 hours (includes team alignment)

---

## Step 1: Choose Your Tier (10 min)

### What Are Tiers?

| Tier | Team Size | Commands | Review | Docs |
|------|-----------|----------|--------|------|
| **Solo** | 1 dev | 5 (core) | 1 model | Informal |
| **Team** | 2-10 devs | 7 (all) | 2+ models (cross-provider) | Formal |
| **Enterprise** | 10+ devs | 7 (strict gates) | 3+ models (multi-provider) | Strict |

---

### Decision Guide

**Choose Solo if**:
- ✅ You're the only developer
- ✅ You want lightweight process
- ✅ Speed > formality

**Choose Team if**:
- ✅ You have 2-10 collaborators
- ✅ You need coordination (RFCs, handoffs)
- ✅ Quality > speed

**Choose Enterprise if**:
- ✅ You have 10+ devs or multiple teams
- ✅ Production issues are costly
- ✅ Compliance/audit requirements

---

### Configure Tier

**Edit** `config/TIER.md`:

```markdown
# Project Tier Configuration

**Current Tier**: `team`  # ← CHANGE THIS

**Reasoning**: We have 5 devs collaborating on a B2B product.

**Reviewers configured**: 2 (cross-model)

**Last reviewed**: 2026-02-15
```

**Save and commit**:
```bash
git add config/TIER.md
git commit -m "config: set tier to team"
```

---

## Step 2: Configure AI Models (20-30 min)

### Why This Matters

**Cross-model review** = Builder uses Model A, Reviewer uses Model B.

**Why?** Same model = same blind spots. Different models catch different issues.

---

### Model Selection Guide

**Recommended pairings**:

| Builder Model | Reviewer Model(s) | Why |
|---------------|-------------------|-----|
| **GPT-5.3 Codex** | Claude Sonnet 4.5, Kimi K2 | OpenAI → Anthropic + Moonshot (cross-provider) |
| **Claude Opus 4.6** | GPT-5.2, DeepSeek V3 | Anthropic → OpenAI + DeepSeek |
| **DeepSeek V3** | Claude Sonnet, GPT-5 | DeepSeek → Anthropic + OpenAI |

**Rule**: Builder and Reviewer should be from **different providers**.

---

### Provider Families

| Provider | Models | Strengths | API |
|----------|--------|-----------|-----|
| **OpenAI** | GPT-5.3 Codex, GPT-5.2 | Code generation, API design | https://platform.openai.com |
| **Anthropic** | Claude Opus 4.6, Sonnet 4.5 | Reasoning, long context, safety | https://console.anthropic.com |
| **Moonshot** | Kimi K2 Thinking | Deep reasoning, critique | https://platform.moonshot.cn |
| **DeepSeek** | DeepSeek V3.2 | Math, logic, performance | https://platform.deepseek.com |
| **Google** | Gemini 3 Flash, Pro | Fast inference, multimodal | https://ai.google.dev |

---

### Configuration Methods

**Choose ONE**:

#### Option A: Environment Variables (Recommended)

**Add to `.bashrc` or `.zshrc`**:
```bash
# Builder
export OPENCODE_BUILDER_MODEL="openai/gpt-5-3-codex"

# Reviewer (different provider)
export OPENCODE_REVIEWER_MODEL="anthropic/claude-sonnet-4-5"

# (Optional) Other agents
export OPENCODE_STRATEGIST_MODEL="anthropic/claude-opus-4-6"
export OPENCODE_ANALYST_MODEL="deepseek/deepseek-v3-2-exp"
export OPENCODE_WRITER_MODEL="anthropic/claude-sonnet-4-5"
```

**Reload shell**:
```bash
source ~/.zshrc  # or source ~/.bashrc
```

---

#### Option B: Project `.env` File

**Create** `.env` in project root:
```bash
# .env (add to .gitignore if it contains API keys)

OPENCODE_BUILDER_MODEL=openai/gpt-5-3-codex
OPENCODE_REVIEWER_MODEL=anthropic/claude-sonnet-4-5
OPENCODE_STRATEGIST_MODEL=anthropic/claude-opus-4-6
OPENCODE_ANALYST_MODEL=deepseek/deepseek-v3-2-exp
OPENCODE_WRITER_MODEL=anthropic/claude-sonnet-4-5
```

**Load before running commands**:
```bash
source .env
opencode /plan "Your feature"
```

---

#### Option C: Runtime Override

**Override per command**:
```bash
OPENCODE_REVIEWER_MODEL="moonshotai/kimi-k2-thinking" opencode /review
```

---

### Verify Configuration

**Check your config**:
```bash
opencode --show-config
```

**Expected output**:
```
Agents:
  builder: openai/gpt-5-3-codex
  reviewer: anthropic/claude-sonnet-4-5 ✅ (cross-provider)
  strategist: anthropic/claude-opus-4-6
```

**If same provider**:
```
  builder: openai/gpt-5-3-codex
  reviewer: openai/gpt-5-2 ⚠️ (same provider — not recommended)
```

---

### Multi-Model Review (Enterprise Tier)

**For 3+ reviewers**, add:

```bash
export OPENCODE_REVIEWER_1_MODEL="anthropic/claude-sonnet-4-5"
export OPENCODE_REVIEWER_2_MODEL="moonshotai/kimi-k2-thinking"
export OPENCODE_REVIEWER_3_MODEL="deepseek/deepseek-v3-2-exp"
```

**Consensus logic**:
- All say SHIP → SHIP
- Any says BLOCK → BLOCK
- Mixed SHIP/REVISE → REVISE

---

### Document Your Config

**Edit** `config/PEER_REVIEW.md`:

```markdown
# Peer Review Configuration

**Level**: enabled  # ← CHANGE THIS (disabled / enabled / multi-model)

**Builder Model**: `openai/gpt-5-3-codex`

**Reviewer Model(s)**:
1. `anthropic/claude-sonnet-4-5`

**Cross-Provider**: YES

**Last Verified**: 2026-02-15
```

**Commit**:
```bash
git add config/PEER_REVIEW.md
git commit -m "config: enable cross-model review (GPT → Claude)"
```

---

## Step 3: Define Team Conventions (10-20 min)

### What Goes Here

**Team-specific rules** that extend the base workflow:
- Code style (Prettier, ESLint, Black)
- Testing standards (coverage target, TDD enforcement)
- Commit format (Conventional Commits, custom)
- Review turnaround (24 hours, 48 hours)
- When to use which command

---

### Edit Working Agreements

**Edit** `config/WORKING_AGREEMENTS.md`:

```markdown
# Working Agreements

**Team**: YourTeam
**Last Updated**: 2026-02-15

---

## Code Standards

**Language**: JavaScript
**Formatter**: Prettier
**Linter**: ESLint
**Run before commit**: `npm run format && npm run lint`

---

## Testing Standards

**Minimum coverage**: 80%
**How to check**: `npm test -- --coverage`
**TDD requirement**: Mandatory (except UI polish)

---

## Commit Standards

**Format**: Conventional Commits
**Example**: `feat: add dark mode toggle [PLAN-001]`
**Size**: < 200 LOC per commit

---

## Review Standards

**Turnaround**: 24 hours
**Priority**:
- 🔴 Critical: 1 hour
- 🟡 High: 4 hours
- 🟢 Normal: 24 hours

---

## Command Usage

**When to use /plan**: Always (for features >100 LOC)
**When to use /rfc**: Required for architecture decisions
**When to use /handoff**: Required for context switch >3 days

---
```

**Commit**:
```bash
git add config/WORKING_AGREEMENTS.md
git commit -m "config: define team working agreements"
```

---

## Step 4: Install Dependencies (5-10 min)

### OpenCode (if using)

```bash
# NPM
npm install -g opencode

# or Pip
pip install opencode

# Verify
opencode --version
```

---

### Cursor / Claude Code

**No install needed** (built-in).

**Import commands**:
1. Copy `.opencode/commands/` to `.cursor/commands/` or `.claude/commands/`
2. Copy `.opencode/agents/` to `.cursor/agents/` or `.claude/agents/`

---

### API Keys (if needed)

**OpenAI**:
```bash
export OPENAI_API_KEY="sk-..."
```

**Anthropic**:
```bash
export ANTHROPIC_API_KEY="sk-ant-..."
```

**Other providers**: See provider docs.

**Security**: Add to `.bashrc` or use secret manager. **NEVER commit to git.**

---

## Step 5: Create Initial Artifacts (5 min)

### Create First Issue

```bash
cp templates/ISSUE_TEMPLATE.md backlog/issues/ISSUE-001-dark-mode.md
```

**Edit**:
```markdown
# ISSUE-001: Add Dark Mode Toggle

**Type**: Feature
**Priority**: High
**Created**: 2026-02-15
**Status**: Backlog

## Description

Users want to switch between light/dark themes.

## Acceptance Criteria

1. Toggle button in settings
2. Theme persists across sessions
3. All UI components respect theme
```

**Commit**:
```bash
git add backlog/issues/ISSUE-001-dark-mode.md
git commit -m "backlog: add dark mode feature request"
```

---

### Run First Command

```bash
opencode /plan backlog/issues/ISSUE-001-dark-mode.md
```

**Output**: `plans/PLAN-001-dark-mode.md`

**Review the plan**, then:
```bash
git add plans/PLAN-001-dark-mode.md
git commit -m "plan: dark mode implementation"
```

---

## Step 6: Team Onboarding (Team/Enterprise)

### Share with Team

**What to share**:
1. This template (entire `playbooks/product-development/` directory)
2. Configured files (`config/TIER.md`, `config/PEER_REVIEW.md`, `config/WORKING_AGREEMENTS.md`)
3. Quick start guide (`QUICK-START.md`)

**How**:
```bash
# Add to project repo
git add playbooks/product-development/
git commit -m "workflow: add product development template"
git push
```

**Team reads**: `QUICK-START.md` (15 min)

---

### Practice Together

**Week 1 exercise**:
1. Each dev picks a small feature (<100 LOC)
2. Run `/plan` → `/build` → `/review` → Ship
3. Team retro: What worked? What didn't?

**Week 2 exercise**:
1. Practice `/rfc` for a small architecture decision
2. Practice `/handoff` for context transfer
3. Adjust `config/WORKING_AGREEMENTS.md` based on learnings

---

## Step 7: Customize (Optional)

### Add Custom Commands

**Create** `.opencode/commands/custom.md`:

```markdown
# /custom — Your Custom Command

**What**: [Description]

**When to use**: [Trigger]

**Workflow**:
1. [Step 1]
2. [Step 2]

[Implementation details]
```

**Register in** `opencode.json`:
```json
{
  "commands": {
    "custom": {
      "file": ".opencode/commands/custom.md",
      "description": "Your custom command",
      "agent": "orchestrator"
    }
  }
}
```

---

### Add Custom Agents

**Create** `.opencode/agents/custom.md`:

```markdown
# Custom Agent

**Role**: [Purpose]

**Responsibilities**:
1. [Task 1]
2. [Task 2]

[Prompt details]
```

**Register in** `opencode.json`:
```json
{
  "agents": {
    "custom": {
      "model": "${OPENCODE_CUSTOM_MODEL:-anthropic/claude-sonnet-4-5}",
      "description": "Your custom agent"
    }
  }
}
```

---

### Adjust Tier Settings

**As you grow**:

**Solo → Team**:
1. Update `config/TIER.md` → `team`
2. Enable cross-model review (2+ models)
3. Add `/rfc` and `/handoff` to workflow

**Team → Enterprise**:
1. Update `config/TIER.md` → `enterprise`
2. Enable multi-model review (3+ models)
3. Add feature flags
4. Add workflow phasing

---

## Troubleshooting

### "Command not found"

**Problem**: OpenCode not installed or not in PATH.

**Fix**:
```bash
npm install -g opencode
# or: pip install opencode
```

---

### "Model not configured"

**Problem**: Environment variable not set.

**Fix**:
```bash
export OPENCODE_BUILDER_MODEL="openai/gpt-5-3-codex"
```

**Verify**:
```bash
echo $OPENCODE_BUILDER_MODEL
```

---

### "API key not found"

**Problem**: Provider API key not set.

**Fix**:
```bash
export OPENAI_API_KEY="sk-..."
# or: export ANTHROPIC_API_KEY="sk-ant-..."
```

**Verify**:
```bash
opencode models list  # Should show available models
```

---

### "Cross-model review failed"

**Problem**: Builder and Reviewer use same model.

**Fix**: Set different models from different providers.

```bash
export OPENCODE_BUILDER_MODEL="openai/gpt-5-3-codex"
export OPENCODE_REVIEWER_MODEL="anthropic/claude-sonnet-4-5"  # Different!
```

---

### "Plan file not found"

**Problem**: Typo in file path or plan not created.

**Fix**:
1. Check file exists: `ls plans/PLAN-001-dark-mode.md`
2. Use tab completion for paths
3. Copy path from `/plan` output

---

### "Tests failing during /build"

**Problem**: TDD cycle broken (implementation before tests).

**Fix**: Builder enforces TDD. If tests fail, fix before proceeding.

**Debug**:
```bash
npm test  # Run tests locally
# Fix issues
opencode /build plans/PLAN-001.md  # Resume
```

---

## Verification Checklist

Before claiming "setup complete":

- [ ] Tier configured (`config/TIER.md`)
- [ ] Models configured (`config/PEER_REVIEW.md`)
- [ ] Working agreements defined (`config/WORKING_AGREEMENTS.md`)
- [ ] Commands work (`/plan`, `/build`, `/review`)
- [ ] Cross-model review enabled (if Team/Enterprise)
- [ ] Team onboarded (if Team/Enterprise)
- [ ] First feature shipped using workflow

**If any are missing, setup is not complete.**

---

## Next Steps

### Solo

**Week 1**: Ship 3 features using `/plan` → `/build` → `/review`

**Week 2**: Add `/retro` after each feature (extract learnings)

**Week 3**: Start building `knowledge/` base

---

### Team

**Week 1**: All devs ship 1 feature each using workflow

**Week 2**: Practice `/rfc` and `/handoff`

**Week 3**: Team retro on workflow (what to adjust)

---

### Enterprise

**Month 1**: Core team (5 devs) adopts Tier 2 workflow

**Month 2**: Expand to full team, add multi-model review

**Month 3**: Add feature flags, workflow phasing, architecture review

---

## Support

- **Documentation**: [WORKFLOW.md](WORKFLOW.md), [PRACTICES.md](PRACTICES.md), [AGENTS.md](AGENTS.md)
- **Issues**: https://github.com/bennjph/agentic-ai-dev-team-setup/issues
- **Discussions**: https://github.com/bennjph/agentic-ai-dev-team-setup/discussions

---

*Setup is 20% of success. The other 80% is using it consistently.*
