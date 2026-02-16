# Project Tier Configuration

**Current Tier**: `[CHOOSE: solo / team / enterprise]`

---

## What Are Tiers?

Tiers scale the workflow to match your team size and complexity needs.

| Tier | Team Size | Commands | Review | Docs |
|------|-----------|----------|--------|------|
| **Solo** | 1 dev | 5 (core) | Self-review (1 model) | Informal |
| **Team** | 2-10 devs | 7 (all) | Cross-model (2+ models) | Formal |
| **Enterprise** | 10+ devs | 7 (strict gates) | Multi-model (3+ models) | Strict |

---

## Tier 1: Solo

**Use when**: You're the only developer.

**Commands**:
- `/explore` — Spike without code
- `/plan` — Turn issue into plan
- `/build` — Implement with TDD
- `/review` — Self-review
- `/retro` — Learning (optional)

**Skipped**:
- `/rfc` — Not needed (solo decisions)
- `/handoff` — Not needed (solo context)

**Peer review**:
- 1 AI reviewer (same model as builder is OK)

**Documentation**:
- Informal (quick notes OK)
- Plans can skip some sections
- RFCs optional (use ADR format in `decisions/`)

**Onboarding time**: 30 minutes

---

## Tier 2: Small Team

**Use when**: You have 2-10 developers collaborating.

**Commands**:
- All 7 (explore, plan, build, review, retro, rfc, handoff)

**Peer review**:
- 2+ AI reviewers
- **Cross-model requirement**: Reviewer uses different model than Builder
- Example: Builder = GPT-5.3 → Reviewer = Claude Sonnet 4.5

**Documentation**:
- Formal (all sections required)
- RFCs required for architectural decisions
- Handoffs required for >3 day context switches

**Working agreements**:
- Define in `config/WORKING_AGREEMENTS.md`
- Team reviews RFCs (48-hour window)

**Onboarding time**: 2 hours

---

## Tier 3: Enterprise

**Use when**: You have 10+ developers, multiple teams, or high-stakes production.

**Commands**:
- All 7 (with stricter gates)

**Peer review**:
- 3+ AI reviewers from **different providers**
- Example: Builder = OpenAI GPT → Reviewers = Anthropic Claude + Moonshot Kimi + DeepSeek
- Multi-model consensus required for SHIP

**Documentation**:
- Strict (all fields mandatory)
- Plans include rollback strategy
- RFCs require sign-off from multiple stakeholders
- Handoffs integrate with runbooks

**Feature flags**:
- Required for risky changes
- Gradual rollout (10% → 50% → 100%)

**Workflow phasing**:
- Complex features split into phases (strategy → foundations → features → rollout)

**Onboarding time**: 4-8 hours

---

## How to Choose

### Choose **Solo** if:
- ✅ You're the only dev
- ✅ You want lightweight process
- ✅ Speed > formality

### Choose **Team** if:
- ✅ You have 2-10 collaborators
- ✅ You need coordination (RFCs, handoffs)
- ✅ Quality > speed

### Choose **Enterprise** if:
- ✅ You have 10+ devs or multiple teams
- ✅ Production issues are costly
- ✅ Compliance/audit requirements

---

## Changing Tiers

**You can change tiers** as your project grows.

**Migration path**:
1. Solo → Team: Add `/rfc` and `/handoff`, configure cross-model review
2. Team → Enterprise: Add 3rd reviewer, add feature flags, add runbooks

**Downgrade** (Team → Solo):
1. Archive RFCs and handoffs
2. Simplify peer review to single model
3. Mark tier as `solo` in this file

---

## Current Configuration

**Tier**: `[EDIT THIS: solo / team / enterprise]`

**Reasoning**: `[Why this tier? Add 1-2 sentences]`

**Reviewers configured**: `[1 / 2 / 3+]` (see `config/PEER_REVIEW.md`)

**Last reviewed**: `[YYYY-MM-DD]`

---

## Team-Specific Notes

*Use this section to document tier-specific decisions for your project.*

**Example**:
- We're using Team tier because we have 5 devs.
- RFCs required for backend architecture, optional for frontend.
- Handoffs required for any work >1 week old.

---

*Last Updated: 2026-02-15*
