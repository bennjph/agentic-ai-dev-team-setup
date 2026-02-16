# Setup Guide — Product Development Template

**Time required**: 10-15 minutes for first-time setup

This guide walks you through configuring the template for your project.

---

## Prerequisites

Before you start:

✅ **You have**:
- Git installed
- An AI tool (OpenCode, Cursor, Claude Code, or compatible)
- Access to at least 1 AI model (any provider)
- A project directory (new or existing)

✅ **You know**:
- Your team size (solo, 2-6 people, or 7+)
- Which AI models you have access to
- Your project's programming language/framework

---

## Step 1: Copy the Template

### For a New Project

```bash
# Clone the repository
git clone https://github.com/bennjph/agentic-ai-dev-team-setup.git

# Copy template to new project directory
cp -r agentic-ai-dev-team-setup/playbooks/product-development/ my-project/
cd my-project/

# Initialize git
git init
git add .
git commit -m "Initial commit with product development template"
```

### For an Existing Project

```bash
# Clone the repository
git clone https://github.com/bennjph/agentic-ai-dev-team-setup.git

# Copy template files to your project
cp -r agentic-ai-dev-team-setup/playbooks/product-development/* your-project/

# Review and merge if you have existing files
cd your-project/
```

**⚠️ Important**: If you already have an `opencode.json` or similar config, don't overwrite it. Merge the command and agent entries instead (see Step 5).

---

## Step 2: Select Your Tier

**Answer these questions**:

1. **Team size**?
   - Solo (just me)
   - Small team (2-6 people)
   - Enterprise (7+ people)

2. **Change risk level**?
   - Low (personal project, can break things)
   - Moderate (users exist, but breakage is recoverable)
   - High (production system, regulated industry, can't afford downtime)

3. **Delivery mode**?
   - Solo trunk (I commit directly to main)
   - Team PRs (we use pull requests for review)
   - Formal approvals (PRs + required approvals from specific people)

---

### Tier Selection Matrix

| Scenario | Recommended Tier |
|----------|------------------|
| Solo dev, personal project, low risk | **Tier 1 (Solo)** |
| Solo dev, users exist, moderate risk | **Tier 1 (Solo)** |
| 2-6 people, any risk level | **Tier 2 (Small Team)** |
| 7+ people OR high-risk regardless of size | **Tier 3 (Enterprise)** |

---

### Configure Your Tier

Edit `config/TIER.md`:

```markdown
# Project Tier Configuration

**Selected Tier**: [Solo / Small Team / Enterprise]

**Rationale**: [Why you chose this tier]

**Team Size**: [Number of people]

**Risk Level**: [Low / Moderate / High]

**Delivery Mode**: [Solo trunk / Team PRs / Formal approvals]

---

## Active Practices (Based on Tier)

[The template will show which practices apply to your tier]

## Active Gates (Based on Tier)

[The template will show which approval gates apply]
```

**Save and commit**:
```bash
git add config/TIER.md
git commit -m "Configure tier: [Your tier]"
```

---

## Step 3: Configure Peer Review

**How many AI models do you have access to?**

### Option 1: One Model Only

**Example**: You only have Claude (Anthropic subscription) or only GPT (ChatGPT Plus).

**Peer review configuration**:
```markdown
# config/PEER_REVIEW.md

**Reviewer Count**: 0 peer reviewers (self-review only)

**Why**: Only 1 model available

**Review Strategy**:
- Self-review with same model (different prompt: "You are a skeptical code reviewer")
- Time-delayed review (wait 24 hours, review with fresh eyes)
- Human review when possible (for critical code)

**Blocking Thresholds**:
- CRITICAL: Always blocks merge
- HIGH: Blocks merge (can self-waive with documented rationale)
- MEDIUM: Does not block
- LOW: Does not block
```

**This is okay!** Self-review + time delay catches 60-70% of issues that peer review would catch.

---

### Option 2: Two Models (Recommended Minimum)

**Example**: Claude + GPT, or GPT + DeepSeek, or any 2 different providers.

**Peer review configuration**:
```markdown
# config/PEER_REVIEW.md

**Reviewer Count**: 1 peer reviewer (different model/provider)

**Independence Rule**:
- If builder uses Model A → reviewer uses Model B
- Different provider strongly preferred (Claude vs GPT vs open-source)

**Review Strategy**:
- Self-review first (same model)
- Peer review second (different model, "less context" — no conversation history)

**Blocking Thresholds**:
- CRITICAL: Always blocks merge
- HIGH: Blocks merge
- MEDIUM: Does not block
- LOW: Does not block
```

**This is the sweet spot** — 1 self + 1 peer = 2 perspectives, catches most issues.

---

### Option 3: Three or More Models (Maximum Coverage)

**Example**: Claude + GPT + DeepSeek + Qwen (or any 3+ from different families).

**Peer review configuration**:
```markdown
# config/PEER_REVIEW.md

**Reviewer Count**: 2-3 peer reviewers (different providers)

**Independence Rule**:
- Builder uses Model A
- Reviewer 1 uses Model B (different provider)
- Reviewer 2 uses Model C (different provider)
- Reviewer 3 uses Model D (optional, for critical changes only)

**Consensus Rule**:
- If 2+ reviewers agree on an issue → likely valid
- If 1 reviewer only → validate carefully (may be false positive)

**Review Strategy**:
- Self-review first
- Parallel peer review (2-3 reviewers get same code, no context)
- Aggregate findings and validate

**Blocking Thresholds**:
- CRITICAL: Always blocks merge
- HIGH: Blocks merge
- MEDIUM: Recommend fix but does not block
- LOW: Does not block
```

**This is comprehensive** — 3-4 perspectives catch provider-specific blind spots.

---

### Save Your Configuration

Edit `config/PEER_REVIEW.md` based on your choice above.

```bash
git add config/PEER_REVIEW.md
git commit -m "Configure peer review: [N] reviewers"
```

---

## Step 4: Fill in Project Context

The AI can't infer your business goals from vibes. **Tell it explicitly.**

### A. Project Brief (`docs/PROJECT_BRIEF.md`)

```markdown
# Project Brief

**Project Name**: [Your project name]

**Problem Statement**: [What problem are you solving? For whom?]

**Users**: [Who uses this? Be specific: "Frontend developers" not "developers"]

**Success Metrics**: [How do you know this is working?]
- [Metric 1]: [Target]
- [Metric 2]: [Target]

**Non-Goals** (What you're NOT building):
- [Explicitly out of scope 1]
- [Explicitly out of scope 2]

**Constraints**:
- Budget: [If any]
- Timeline: [If any]
- Compatibility: [Must work with X, must support Y]
```

---

### B. Tech Stack (`docs/TECH_STACK.md`)

```markdown
# Tech Stack

**Language**: [e.g., TypeScript, Python, Rust]

**Framework**: [e.g., React, Next.js, FastAPI, Django]

**Database**: [e.g., PostgreSQL, MongoDB, Supabase]

**Hosting**: [e.g., Vercel, Railway, AWS, self-hosted]

**Key Libraries**:
- [Library 1]: [Purpose]
- [Library 2]: [Purpose]

**Why These Choices**:
[1-2 sentences explaining major tech decisions]

**How to Run Locally**:
\`\`\`bash
# Setup
[Install commands]

# Run dev server
[Dev command]

# Run tests
[Test command]
\`\`\`
```

---

### C. Quality Standards (`docs/QUALITY_STANDARDS.md`)

```markdown
# Quality Standards

**Testing**:
- All new features must have integration tests
- Minimum coverage: [X%] (if applicable)
- Critical paths (auth, payments, data mutations) must have tests

**Security**:
- No hardcoded secrets (use environment variables)
- All user inputs must be validated
- SQL queries must use parameterization (no string concatenation)
- [Add project-specific security rules]

**Performance**:
- Page load: <[X] seconds
- API response: <[X] ms for [endpoint type]
- [Add project-specific performance floors]

**Code Style**:
- [Linter]: [Config file]
- [Formatter]: [Config file]
- [Project-specific style rules]
```

---

### D. Repository Map (`docs/REPO_MAP.md`)

```markdown
# Repository Map

**Where things live**:

\`\`\`
src/
├── api/          ← API routes and endpoints
├── components/   ← Reusable UI components
├── lib/          ← Utility functions
├── types/        ← TypeScript types
└── ...

tests/
├── integration/  ← Full user flow tests
└── unit/         ← Function-level tests

docs/             ← Project documentation
config/           ← Process configuration
plans/            ← Implementation plans
knowledge/        ← Searchable solutions
\`\`\`

**Key files**:
- \`src/api/auth.ts\` — Authentication logic
- \`src/lib/db.ts\` — Database connection
- [Add your key files]
```

---

### Save Your Context

```bash
git add docs/
git commit -m "Add project context"
```

---

## Step 5: Wire OpenCode (or Your Tool)

### If You Use OpenCode

#### Option A: New Project (No Existing Config)

The template includes `opencode.json`. You're done! Just set environment variables:

```bash
# Create .env file
cp .env.example .env

# Edit .env and add your API keys
ANTHROPIC_API_KEY=your_key_here
OPENAI_API_KEY=your_key_here
# ... etc
```

#### Option B: Existing OpenCode Setup

**Don't overwrite your `opencode.json`**. Instead, merge:

1. **Copy command entries** from template's `opencode.json` to yours:
   ```json
   "command": {
     "explore": {
       "description": "Deep analysis before coding",
       "template": "{file:.opencode/commands/explore.md}"
     },
     // ... copy all 7 commands
   }
   ```

2. **Copy agent entries** from template's `opencode.json` to yours (if you want them):
   ```json
   "agent": {
     "strategist": { ... },
     "builder": { ... },
     // ... etc
   }
   ```

3. **Keep your existing model assignments** — the template is model-agnostic.

---

### If You Use Cursor

Copy commands to Cursor's directory:

```bash
mkdir -p .cursor/commands
cp .opencode/commands/* .cursor/commands/
```

Edit your `.cursorrules` file to reference the template's `AGENTS.md`:

```markdown
# .cursorrules

See AGENTS.md for full system context and workflow.

[Your existing Cursor rules]
```

---

### If You Use Claude Code

Copy commands to Claude Code's directory:

```bash
mkdir -p .claude/commands
cp .opencode/commands/* .claude/commands/
```

Create or edit `CLAUDE.md` to reference the template's `AGENTS.md`:

```markdown
# CLAUDE.md

See AGENTS.md for full system context and workflow.

[Your existing Claude Code config]
```

---

## Step 6: Verification

**Test that everything works**:

### A. Check Commands Are Loaded

```bash
# Open your AI tool in the project directory
opencode  # or cursor, or claude

# Type / to see slash commands
# You should see: /explore, /plan, /build, /review, /retro
# (Plus /rfc and /handoff if Tier 2+)
```

**If commands don't show**: Check that your tool is reading from the right directory (`.opencode/commands/`, `.cursor/commands/`, or `.claude/commands/`).

---

### B. Test `/explore` Command

Create a test backlog item:

```bash
# Create a simple test issue
echo "Add a hello world endpoint" > backlog/test-hello-world.md
```

Run `/explore`:

```
/explore backlog/test-hello-world.md
```

**Expected result**:
- AI analyzes the request
- Produces a file: `plans/YYYY-MM-DD-test-hello-world-exploration.md`
- **NO CODE is written** (this is the NO CODE rule)

**If it works**: Setup is complete! 🎉

**If it doesn't work**: Check:
1. Is the command file in the right location?
2. Is your AI tool configured to read that directory?
3. Are there syntax errors in `opencode.json` or equivalent?

---

## Step 7: Configure Working Agreements (Tier 2/3 Only)

**If you selected Tier 2 or Tier 3**, configure team norms.

Edit `config/WORKING_AGREEMENTS.md`:

```markdown
# Working Agreements

**Team Tier**: [Small Team / Enterprise]

---

## Definition of Done

A task is "done" when:
- [ ] Code is written and tests pass
- [ ] Self-review completed
- [ ] Peer review completed (per PEER_REVIEW.md)
- [ ] Plan updated to 🟩
- [ ] Retro completed (for non-trivial work)

---

## Pull Request Expectations

**Size**: Keep PRs small (<500 lines preferred, <1000 lines max)

**Description**: Link to plan file, summarize what changed and why

**Review SLA**: Team members review within [X hours/days]

**Approval Requirements**:
- Standard changes: 1 approval
- Critical changes (auth, payments, data schema): 2 approvals

---

## Communication Norms

**Status updates**: [How often? Daily standup? Async in Slack?]

**Blockers**: [How to escalate? Who to contact?]

**Decisions**: [When to use /rfc vs just decide?]

---

## Deployment Process

**Who can deploy**: [Everyone / Tech lead only / etc.]

**When to deploy**: [After every PR merge / Daily / Weekly / etc.]

**Rollback authority**: [Who can trigger rollback? What's the process?]
```

Save and commit:

```bash
git add config/WORKING_AGREEMENTS.md
git commit -m "Configure working agreements"
```

---

## Step 8: First Real Workflow

You're ready! Start with a real feature or bug:

### 1. Create a Backlog Item

Option A: Use GitHub Issues (recommended for teams)

Option B: Create markdown file in `backlog/`:

```bash
echo "Add user authentication with JWT" > backlog/2026-02-15-add-auth.md
```

### 2. Run the Workflow

```bash
# Explore (understand the problem)
/explore backlog/2026-02-15-add-auth.md

# Plan (break into tasks)
/plan plans/2026-02-15-add-auth-exploration.md

# Build (TDD implementation)
/build plans/2026-02-15-add-auth-plan.md

# Review (self + peer)
/review

# Retro (capture learnings)
/retro
```

### 3. Review the Artifacts

Check that files were created in:
- `plans/` — Exploration and plan
- `retros/` — Retrospective
- `knowledge/solutions/` — Solution doc (if applicable)

**If artifacts exist**: Setup is successful! 🚀

---

## Troubleshooting

### Commands Don't Show Up

**Symptom**: Typing `/` doesn't show custom commands

**Fix**:
1. Check your tool's config location (OpenCode uses `.opencode/commands/`, Cursor uses `.cursor/commands/`)
2. Verify command files are in that directory
3. Restart your AI tool
4. Check for syntax errors in `opencode.json` or equivalent

---

### Peer Review Not Working

**Symptom**: `/review` doesn't route to different model

**Fix**:
1. Check `config/PEER_REVIEW.md` — is reviewer count >0?
2. Check your tool's agent/model configuration
3. Verify you have access to multiple models (API keys set)
4. Start with 1 peer reviewer (simplest config)

---

### Artifacts Not Created

**Symptom**: Commands run but no files appear in `plans/`, `retros/`, etc.

**Fix**:
1. Check directory permissions (can the tool write files?)
2. Check for errors in command output
3. Verify directories exist (`mkdir -p plans retros decisions knowledge/solutions`)
4. Check `.gitignore` — are you accidentally ignoring these directories?

---

### "Model Not Found" Errors

**Symptom**: Error about missing AI model or API key

**Fix**:
1. Set environment variables (`.env` file or shell):
   ```bash
   export ANTHROPIC_API_KEY=your_key
   export OPENAI_API_KEY=your_key
   ```
2. Check that keys are valid (not expired)
3. Check `opencode.json` model IDs match your available models
4. Use model-agnostic config (environment variable placeholders)

---

## Next Steps

### Learn the Workflow

Read `WORKFLOW.md` for detailed guidance on:
- When to use each command
- What each command produces
- How commands connect
- Common patterns and anti-patterns

### Study the Practices

Read `PRACTICES.md` for:
- Why each of the 15 practices works
- When to adopt each practice
- Real-world examples
- Anti-patterns to avoid

### Join the Community

**GitHub**: https://github.com/bennjph/agentic-ai-dev-team-setup
- Ask questions
- Share your experience
- Contribute improvements

---

## Customization Tips

### Start Minimal, Add Later

Don't feel pressured to use every practice or command immediately.

**Week 1**: Use `/explore`, `/plan`, `/build` only  
**Week 2**: Add `/review`  
**Week 3**: Add `/retro` for significant work  
**Week 4**: Evaluate Tier 2 practices (if working with others)

**The template adapts to you** — not the other way around.

---

### Adapt Commands to Your Workflow

**Feel free to**:
- Edit command prompts (make them shorter/longer)
- Add your own commands
- Remove commands you don't use
- Change artifact formats

**This is YOUR template now** — customize it.

---

### Keep It Under Version Control

**Commit your customizations**:
```bash
git add .
git commit -m "Customize template for [project name]"
```

**When the template updates**, you can:
- Pull new practices/commands
- Merge with your customizations
- Keep what works for you

---

## Summary Checklist

- [ ] Template copied to project directory
- [ ] Tier selected and configured (`config/TIER.md`)
- [ ] Peer review configured (`config/PEER_REVIEW.md`)
- [ ] Project context filled in (`docs/PROJECT_BRIEF.md`, etc.)
- [ ] OpenCode (or tool) wired with commands
- [ ] Verification test passed (`/explore` works, produces artifact)
- [ ] Working agreements configured (Tier 2/3 only)
- [ ] First real workflow completed (explore → plan → build → review → retro)

**When all boxes are checked**: You're ready to ship! 🚀

---

**Questions?** See `WORKFLOW.md` or open an issue on GitHub.

*Setup guide version 1.0*
