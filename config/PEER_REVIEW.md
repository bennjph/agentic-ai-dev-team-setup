# Peer Review Configuration

**Cross-Model Review**: `[CHOOSE: disabled / enabled / multi-model]`

---

## Why Cross-Model Review?

**Problem**: Same AI model = same blind spots.

**Solution**: Use **different models** for builder vs. reviewer.

**Benefit**: Different models catch different issues (security, performance, style).

---

## Configuration Levels

### Disabled (Not Recommended)

**When**: Solo tier, same model for build + review

**Setup**:
- Builder: Any model
- Reviewer: Same model as builder

**Trade-off**: Faster (no model switching), but lower quality

**Example**:
```bash
export OPENCODE_BUILDER_MODEL="anthropic/claude-sonnet-4-5"
export OPENCODE_REVIEWER_MODEL="anthropic/claude-sonnet-4-5"  # Same
```

---

### Enabled (Recommended for Team Tier)

**When**: 2-10 devs, need quality without complexity

**Setup**:
- Builder: One model
- Reviewer: **Different model** (different provider preferred)

**Example**:
```bash
# Builder uses OpenAI
export OPENCODE_BUILDER_MODEL="openai/gpt-5-3-codex"

# Reviewer uses different provider (Moonshot)
export OPENCODE_REVIEWER_MODEL="moonshotai/kimi-k2-thinking"
```

**Verification**: `/review` command checks models differ. If same, it will warn.

---

### Multi-Model (Recommended for Enterprise Tier)

**When**: 10+ devs, high-stakes production, strict quality

**Setup**:
- Builder: One model
- Reviewers: **3+ models from different providers**

**Example**:
```bash
# Builder
export OPENCODE_BUILDER_MODEL="openai/gpt-5-3-codex"

# Reviewer 1 (Anthropic)
export OPENCODE_REVIEWER_1_MODEL="anthropic/claude-sonnet-4-5"

# Reviewer 2 (Moonshot)
export OPENCODE_REVIEWER_2_MODEL="moonshotai/kimi-k2-thinking"

# Reviewer 3 (DeepSeek)
export OPENCODE_REVIEWER_3_MODEL="deepseek/deepseek-v3-2-exp"
```

**Consensus**: All 3 reviewers must agree on verdict (SHIP / REVISE / BLOCK).

---

## Model Selection Guide

### Recommended Pairings

| Builder Model | Reviewer Model(s) | Reasoning |
|---------------|-------------------|-----------|
| **GPT-5.3 Codex** | Claude Sonnet 4.5, Kimi K2 | OpenAI → Anthropic + Moonshot (cross-provider) |
| **Claude Opus 4.6** | GPT-5.2, DeepSeek V3 | Anthropic → OpenAI + DeepSeek |
| **DeepSeek V3** | Claude Sonnet, GPT-5 | DeepSeek → Anthropic + OpenAI |

**Rule**: Builder and reviewer should be from **different providers** (OpenAI vs. Anthropic vs. Moonshot vs. DeepSeek).

---

### Provider Families

| Provider | Models | Strengths |
|----------|--------|-----------|
| **OpenAI** | GPT-5.3 Codex, GPT-5.2 | Code generation, API design |
| **Anthropic** | Claude Opus 4.6, Sonnet 4.5 | Reasoning, long context, safety |
| **Moonshot** | Kimi K2 Thinking | Deep reasoning, critique |
| **DeepSeek** | DeepSeek V3.2 | Math, logic, performance analysis |
| **Google** | Gemini 3 Flash, Pro | Fast inference, multimodal |

**Avoid**: Using models from same family (e.g., GPT-5.2 + GPT-5.3 = weak cross-check).

---

## Setup Instructions

### Step 1: Choose Configuration Level

Based on your tier (see `config/TIER.md`):
- **Solo**: Disabled or Enabled (1 reviewer)
- **Team**: Enabled (2 reviewers, cross-provider)
- **Enterprise**: Multi-Model (3+ reviewers, different providers)

---

### Step 2: Set Environment Variables

**Option A: Shell config** (`.bashrc`, `.zshrc`):
```bash
# Add to your shell config
export OPENCODE_BUILDER_MODEL="openai/gpt-5-3-codex"
export OPENCODE_REVIEWER_MODEL="moonshotai/kimi-k2-thinking"
```

**Option B: Project `.env` file**:
```bash
# .env (don't commit to git if it has API keys)
OPENCODE_BUILDER_MODEL=openai/gpt-5-3-codex
OPENCODE_REVIEWER_MODEL=moonshotai/kimi-k2-thinking
```

**Option C: Runtime override**:
```bash
OPENCODE_REVIEWER_MODEL="anthropic/claude-sonnet-4-5" opencode /review
```

---

### Step 3: Verify Configuration

Run this to check your config:

```bash
opencode --show-config
```

**Expected output**:
```
Agents:
  builder: openai/gpt-5-3-codex
  reviewer: moonshotai/kimi-k2-thinking ✅ (cross-provider)
```

**If same provider**:
```
  builder: openai/gpt-5-3-codex
  reviewer: openai/gpt-5-2 ⚠️ (same provider — not recommended)
```

---

## Current Configuration

**Level**: `[EDIT THIS: disabled / enabled / multi-model]`

**Builder Model**: `[Model ID or env var]`

**Reviewer Model(s)**:
1. `[Model ID or env var]`
2. `[Model ID or env var]` *(if multi-model)*
3. `[Model ID or env var]` *(if multi-model)*

**Cross-Provider**: `[YES / NO]`

**Last Verified**: `[YYYY-MM-DD]`

---

## Troubleshooting

### "Models are the same" warning

**Problem**: Builder and reviewer use same model.

**Fix**: Set different models in env vars.

```bash
export OPENCODE_REVIEWER_MODEL="anthropic/claude-sonnet-4-5"  # Different provider
```

---

### "Model not found" error

**Problem**: Model ID is incorrect or not available.

**Fix**: Check model availability:
```bash
opencode models list
```

Find a valid model and update env var.

---

### Multi-model reviewers disagree

**Problem**: Reviewer 1 says SHIP, Reviewer 2 says BLOCK.

**Resolution**:
- If any reviewer says BLOCK → Overall verdict is BLOCK
- If mixed SHIP/REVISE → Overall verdict is REVISE
- Only if all say SHIP → Overall verdict is SHIP

**Conservative bias**: Favor quality over speed.

---

## Cost Considerations

**Cross-model review costs more** (2-3x API calls).

**Budget tiers**:
- **Solo**: $10-20/month (single model)
- **Team**: $30-50/month (cross-model)
- **Enterprise**: $100+/month (multi-model)

**Optimization**:
- Use cheaper models for reviewer (e.g., Gemini Flash, Haiku)
- Only run `/review` on final code (not drafts)

---

*Last Updated: 2026-02-15*
